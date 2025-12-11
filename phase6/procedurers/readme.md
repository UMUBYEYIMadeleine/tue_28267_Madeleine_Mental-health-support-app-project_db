# procedurer update

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
    
    )IS
    
BEGIN
 SELECT NVL(MAX(user_id),0)+1 INTO p_new_id FROM USERS;
 
 INSERT INTO USERS (
   user_id, full_name, email, password_hash, phone_number,
        
        date_of_birth, gender, status, language_preference
    ) VALUES (
        p_new_id, p_full_name, p_email, p_password_hash, p_phone_number,
        
        p_date_of_birth, p_gender, p_status, p_language_pref );
        
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

<img width="836" height="768" alt="image" src="https://github.com/user-attachments/assets/65d39c21-eb62-4939-b560-3b453393945a" />


# procedurer delete


# procedurer add

<img width="1260" height="806" alt="image" src="https://github.com/user-attachments/assets/6037c40a-9068-4e1f-b00b-d6961d282334" />

 # out
<img width="748" height="406" alt="image" src="https://github.com/user-attachments/assets/aa8a69c0-6a90-476f-86f9-27ff2d0bd27b" />
