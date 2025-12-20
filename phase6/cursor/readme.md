# explicity cursor

DECLARE
    CURSOR c_users IS
        SELECT user_id, full_name
        FROM users;

    v_user_id users.user_id%TYPE;
    v_full_name users.full_name%TYPE;
BEGIN
    OPEN c_users;

    LOOP
        FETCH c_users INTO v_user_id, v_full_name;
        EXIT WHEN c_users%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            'User ID: ' || v_user_id || ' Name: ' || v_full_name
        );
    END LOOP;

    CLOSE c_users;
END;
/

<img width="747" height="730" alt="exp cursor" src="https://github.com/user-attachments/assets/3c7141c5-567f-4486-8aa5-74931443de65" />


# bulk 

DECLARE

    TYPE t_user_id IS TABLE OF users.user_id%TYPE;
    
    v_user_ids t_user_id;

BEGIN

    SELECT user_id
    
    BULK COLLECT INTO v_user_ids
    
    FROM users
    
    WHERE status = 'ACTIVE' AND MONTHS_BETWEEN(SYSDATE, date_of_birth)/12 > 50;
    
    FORALL i IN v_user_ids.FIRST .. v_user_ids.LAST
    
        UPDATE users
        
        SET status = 'DISABLED'
        
        WHERE user_id = v_user_ids(i);

    COMMIT;
END;

<img width="815" height="747" alt="image" src="https://github.com/user-attachments/assets/5f45df5f-567c-450a-ba90-40d510eaa245" />

# open,fetch and close

DECLARE
    
    CURSOR c_users IS
    
        SELECT user_id, full_name, date_of_birth
        
        FROM users
        
        WHERE status = 'ACTIVE';

    v_user_id users.user_id%TYPE;
    
    v_name users.full_name%TYPE;
    
    v_dob  users.date_of_birth%TYPE;

BEGIN
    
    OPEN c_users;

    LOOP
    
        FETCH c_users INTO v_user_id, v_name, v_dob;
        
        EXIT WHEN c_users%NOTFOUND;  

        IF MONTHS_BETWEEN(SYSDATE, v_dob)/12 > 50 THEN
        
            UPDATE users
            
            SET status = 'DISABLED'
            
            WHERE user_id = v_user_id;
            
        END IF;
        
    END LOOP;
    
    CLOSE c_users;
    
    COMMIT;
    
END;

<img width="717" height="780" alt="image" src="https://github.com/user-attachments/assets/620969e3-7fd3-4d73-b7fc-eb772e858b2d" />

