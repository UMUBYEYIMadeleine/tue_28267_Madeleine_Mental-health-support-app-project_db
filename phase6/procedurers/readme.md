# procedurer add

CREATE OR REPLACE PROCEDURE add_user (

    p_full_name     IN  USERS.full_name%TYPE,
    
    p_email         IN  USERS.email%TYPE,
    
    p_password_hash IN  USERS.password_hash%TYPE,
    
    p_phone_number  IN  USERS.phone_number%TYPE,
    
    p_date_of_birth IN  USERS.date_of_birth%TYPE,
    
    p_gender        IN  USERS.gender%TYPE,
    
    p_status        IN  USERS.status%TYPE,
    
    p_language_pref IN  USERS.language_preference%TYPE,
    
    p_new_id        OUT USERS.user_id%TYPE
    
)

IS

BEGIN
    
    SELECT NVL(MAX(user_id),0)+1 INTO p_new_id FROM USERS;

    INSERT INTO USERS (
    
        user_id, full_name, email, password_hash, phone_number,
        
        date_of_birth, gender, status, language_preference
        
    ) VALUES (
    
        p_new_id, p_full_name, p_email, p_password_hash, p_phone_number,
        
        p_date_of_birth, p_gender, p_status, p_language_pref
    );

    COMMIT;

EXCEPTION

    WHEN DUP_VAL_ON_INDEX THEN
    
        ROLLBACK;
        
        RAISE_APPLICATION_ERROR(-20001, 'Email already exists: ' || p_email);
        
    WHEN OTHERS THEN
    
        ROLLBACK;
        
        RAISE;
        
END add_user;
/

<img width="957" height="732" alt="image" src="https://github.com/user-attachments/assets/e83d4f53-2502-436e-8236-d21130d260b3" />


# procedurer update

CREATE OR REPLACE PROCEDURE update_user_status (

    p_user_id    IN  USERS.user_id%TYPE,
    
    p_new_status IN  USERS.status%TYPE,
    
    p_old_status OUT USERS.status%TYPE
)

IS

BEGIN

    SELECT status INTO p_old_status
    
    FROM USERS
    
    WHERE user_id = p_user_id
    
    FOR UPDATE;

    UPDATE USERS
    
    SET status = p_new_status
    
    WHERE user_id = p_user_id;

    COMMIT;

EXCEPTION

    WHEN NO_DATA_FOUND THEN
    
        ROLLBACK;
        
        RAISE_APPLICATION_ERROR(-20002, 'User not found: ' || p_user_id);
        
    WHEN OTHERS THEN
    
        ROLLBACK;
        
        RAISE;
        
END update_user_status;
/

<img width="726" height="752" alt="image" src="https://github.com/user-attachments/assets/96ccc88c-8514-4bb8-94d2-d9977c2464e0" />

# procedurer add

<img width="1260" height="806" alt="image" src="https://github.com/user-attachments/assets/6037c40a-9068-4e1f-b00b-d6961d282334" />

 # out
<img width="748" height="406" alt="image" src="https://github.com/user-attachments/assets/aa8a69c0-6a90-476f-86f9-27ff2d0bd27b" />
