# package specification

CREATE OR REPLACE PACKAGE user_management_pkg AS

    PROCEDURE add_user(p_name VARCHAR2, p_email VARCHAR2);
    
    PROCEDURE deactivate_user(p_user_id NUMBER);
    
    FUNCTION get_user_status(p_user_id NUMBER) RETURN VARCHAR2;
    
END user_management_pkg;

<img width="721" height="622" alt="image" src="https://github.com/user-attachments/assets/e682d883-49e3-440d-82e7-4d21a5faa1a0" />

# package body

CREATE OR REPLACE PACKAGE BODY user_management_pkg AS

    PROCEDURE add_user(p_name VARCHAR2, p_email VARCHAR2) IS
    
    BEGIN
    
        INSERT INTO users(full_name, email, status)
        
        VALUES (p_name, p_email, 'ACTIVE');
        
        COMMIT;
        
    END add_user;

    PROCEDURE deactivate_user(p_user_id NUMBER) IS
    
    BEGIN
    
        UPDATE users
        
        SET status = 'DISABLED'
        
        WHERE user_id = p_user_id;
        
        COMMIT;
        
    END deactivate_user;

    FUNCTION get_user_status(p_user_id NUMBER) RETURN VARCHAR2 IS
    
        v_status VARCHAR2(20);
        
    BEGIN
    
        SELECT status INTO v_status
        
        FROM users
        
        WHERE user_id = p_user_id;
        
        RETURN v_status;
        
    END get_user_status;

END user_management_pkg;

<img width="852" height="761" alt="image" src="https://github.com/user-attachments/assets/c24b4803-db5f-4a45-9a0b-24b358cc99c7" />
