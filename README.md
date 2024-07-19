# stored-proc
# stored-proc

CREATE DEFINER=`f2buser`@`%` PROCEDURE `createProgramFlowMap_proc`(
    IN in_program_id INT, 
    IN in_flow_ids VARCHAR(255) -- Pass a comma-separated list of flow_ids
)
BEGIN
    DECLARE flowMapCount INT;
    DECLARE out_flow_map_id INT;
    DECLARE flow_id INT;
    DECLARE done INT DEFAULT 0;
    DECLARE cur CURSOR FOR SELECT flow_id FROM (
        SELECT CAST(SUBSTRING_INDEX(SUBSTRING_INDEX(in_flow_ids, ',', numbers.n), ',', -1) AS UNSIGNED) flow_id
        FROM
        (SELECT @rownum := @rownum + 1 AS n FROM
        (SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 UNION
         SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9 UNION SELECT 10) numbers 
         CROSS JOIN (SELECT @rownum := 0) r) numbers
        WHERE n <= CHAR_LENGTH(in_flow_ids) - CHAR_LENGTH(REPLACE(in_flow_ids, ',', '')) + 1
    ) AS flow_ids;

    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

    OPEN cur;
    
    read_loop: LOOP
        FETCH cur INTO flow_id;
        IF done THEN
            LEAVE read_loop;
        END IF;

        SELECT COUNT(*) INTO flowMapCount 
        FROM program_flow_map 
        WHERE program_id = in_program_id AND flow_id = flow_id;

        IF flowMapCount = 0 THEN
            INSERT INTO program_flow_map (program_id, flow_id, flow_version_id)
            VALUES (in_program_id, flow_id, 1);

            SET out_flow_map_id = LAST_INSERT_ID();
        ELSE
            SELECT id INTO out_flow_map_id 
            FROM program_flow_map 
            WHERE program_id = in_program_id AND flow_id = flow_id;
        END IF;
    END LOOP;

    CLOSE cur;
END
