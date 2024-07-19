DELIMITER //

CREATE DEFINER=`f2buser`@`%` PROCEDURE `createProgramFlowMap_proc`(
    IN in_program_id INT, 
    IN in_flow_ids VARCHAR(255) -- Pass a comma-separated list of flow_ids
)
BEGIN
    DECLARE flowMapCount INT;
    DECLARE flow_id INT;
    DECLARE done INT DEFAULT 0;
    DECLARE flow_ids CURSOR FOR
        SELECT CAST(SUBSTRING_INDEX(SUBSTRING_INDEX(in_flow_ids, ',', numbers.n), ',', -1) AS UNSIGNED) AS flow_id
        FROM
        (SELECT 1 n UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION
         SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9 UNION SELECT 10) numbers
        WHERE numbers.n <= LENGTH(in_flow_ids) - LENGTH(REPLACE(in_flow_ids, ',', '')) + 1;
    
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

    OPEN flow_ids;
    read_loop: LOOP
        FETCH flow_ids INTO flow_id;
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

    CLOSE flow_ids;
END //

DELIMITER ;
