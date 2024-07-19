DELIMITER //

CREATE DEFINER=`f2buser`@`%` PROCEDURE `createProgramFlowMap_proc`(
    IN in_program_id INT, 
    IN in_flow_ids VARCHAR(255) -- Pass a comma-separated list of flow_ids
)
BEGIN
    -- Declare variables
    DECLARE flowMapCount INT;
    DECLARE flow_id INT;
    DECLARE done INT DEFAULT 0;

    -- Create a temporary table to store flow_ids
    CREATE TEMPORARY TABLE temp_flow_ids (flow_id INT NOT NULL);

    -- Split the comma-separated flow_ids and insert them into the temporary table
    SET @sql = CONCAT('INSERT INTO temp_flow_ids (flow_id) VALUES (', REPLACE(in_flow_ids, ',', '), ('), ')');
    PREPARE stmt FROM @sql;
    EXECUTE stmt;
    DEALLOCATE PREPARE stmt;

    -- Declare cursor for iterating through the temporary table
    DECLARE cur CURSOR FOR SELECT flow_id FROM temp_flow_ids;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

    -- Open cursor
    OPEN cur;

    -- Loop to read each flow_id from cursor
    read_loop: LOOP
        FETCH cur INTO flow_id;
        IF done THEN
            LEAVE read_loop;
        END IF;

        -- Check if the flow_id already exists for the given program_id
        SELECT COUNT(*) INTO flowMapCount 
        FROM program_flow_map 
        WHERE program_id = in_program_id AND flow_id = flow_id;

        -- If it doesn't exist, insert it
        IF flowMapCount = 0 THEN
            INSERT INTO program_flow_map (program_id, flow_id, flow_version_id)
            VALUES (in_program_id, flow_id, 1);
        END IF;
    END LOOP;

    -- Close cursor
    CLOSE cur;

    -- Drop the temporary table
    DROP TEMPORARY TABLE temp_flow_ids;
END //

DELIMITER ;
