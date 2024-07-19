CREATE DEFINER=`f2buser`@`%` PROCEDURE `createProgramFlowMap_proc`(
    IN in_program_id INT, 
    IN in_flow_ids VARCHAR(255) -- Pass a comma-separated list of flow_ids
)
BEGIN
    -- Declare variables
    DECLARE flowMapCount INT;
    DECLARE out_flow_map_id INT;
    DECLARE flow_id INT;
    DECLARE done INT DEFAULT 0;

    -- Declare cursor for splitting comma-separated flow_ids into individual flow_id
    DECLARE cur CURSOR FOR SELECT flow_id FROM (
        SELECT CAST(SUBSTRING_INDEX(SUBSTRING_INDEX(in_flow_ids, ',', numbers.n), ',', -1) AS UNSIGNED) flow_id
        FROM
        (SELECT @rownum := @rownum + 1 AS n FROM
        (SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION
         SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9 UNION SELECT 10) numbers 
         CROSS JOIN (SELECT @rownum := 0) r) numbers
        WHERE n <= CHAR_LENGTH(in_flow_ids) - CHAR_LENGTH(REPLACE(in_flow_ids, ',', '')) + 1
    ) AS flow_ids;

    -- Declare handler for cursor to continue on NOT FOUND
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

            -- Set the output id to the last inserted id
            SET out_flow_map_id = LAST_INSERT_ID();
        ELSE
            -- If it exists, select the existing id
            SELECT id INTO out_flow_map_id 
            FROM program_flow_map 
            WHERE program_id = in_program_id AND flow_id = flow_id;
        END IF;
    END LOOP;

    -- Close cursor
    CLOSE cur;
END;
