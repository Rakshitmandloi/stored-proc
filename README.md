DELIMITER //

CREATE DEFINER=`f2buser`@`%` PROCEDURE `createProgramFlowMap_proc`(
    IN in_program_id INT, 
    IN in_flow_ids VARCHAR(255) -- Pass a comma-separated list of flow_ids
)
BEGIN
    DECLARE flowMapCount INT;
    DECLARE out_flow_map_id INT;
    DECLARE flow_id INT;
    DECLARE done INT DEFAULT 0;

    -- Prepare the list of flow_ids by splitting the input string
    DECLARE flow_id_list CURSOR FOR 
    SELECT CAST(SUBSTRING_INDEX(SUBSTRING_INDEX(in_flow_ids, ',', numbers.n), ',', -1) AS UNSIGNED) AS flow_id
    FROM 
    (SELECT @rownum := @rownum + 1 AS n 
     FROM (SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5 
           UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9 UNION SELECT 10) numbers 
     CROSS JOIN (SELECT @rownum := 0) r) num
    WHERE n <= CHAR_LENGTH(in_flow_ids) - CHAR_LENGTH(REPLACE(in_flow_ids, ',', '')) + 1;

    -- Handle the case when no more rows are found in the cursor
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;

    -- Open the cursor
    OPEN flow_id_list;

    read_loop: LOOP
        -- Fetch the next flow_id from the cursor
        FETCH flow_id_list INTO flow_id;

        -- Check if there are no more rows
        IF done THEN
            LEAVE read_loop;
        END IF;

        -- Ensure flow_id is not null before proceeding
        IF flow_id IS NOT NULL THEN
            -- Check if the flow_id already exists for the given program_id
            SELECT COUNT(*) INTO flowMapCount 
            FROM program_flow_map 
            WHERE program_id = in_program_id AND flow_id = flow_id;

            -- If it doesn't exist, insert it
            IF flowMapCount = 0 THEN
                INSERT INTO program_flow_map (program_id, flow_id, flow_version_id)
                VALUES (in_program_id, flow_id, 1);
            END IF;
        END IF;
    END LOOP;

    -- Close the cursor
    CLOSE flow_id_list;
END //

DELIMITER ;
