DELIMITER $$

CREATE PROCEDURE AddUser(
    IN user_name VARCHAR(50),
    IN role VARCHAR(50),
    IN fname VARCHAR(50),
    IN lname VARCHAR(50),
    IN email VARCHAR(50)
)
BEGIN
    INSERT INTO user (user, role, fname, lname, email, updated_by, updated_ts)
    VALUES (user_name, role, fname, lname, email, 'F2B', NOW());
END $$

DELIMITER ;
