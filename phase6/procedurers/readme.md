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

# procedurer delete

CREATE OR REPLACE PROCEDURE delete_assessment (

    p_assessment_id IN SELF_ASSESSMENTS.assessment_id%TYPE
)
IS

BEGIN

    DELETE FROM SELF_ASSESSMENTS WHERE assessment_id = p_assessment_id;

    IF SQL%ROWCOUNT = 0 THEN
    
        RAISE_APPLICATION_ERROR(-20020, 'Assessment not found: '||p_assessment_id);
        
    END IF;

    COMMIT;

EXCEPTION

    WHEN OTHERS THEN
    
        ROLLBACK;
        
        RAISE;
        
END delete_assessment;
/

<img width="861" height="666" alt="image" src="https://github.com/user-attachments/assets/c52c26ea-dc97-4237-8e0b-c46c78c4e5bd" />

# procedurer insert

CREATE OR REPLACE PROCEDURE bulk_insert_resources (

    p_titles       IN SYS.ODCIVARCHAR2LIST,
    
    p_descriptions IN SYS.ODCIVARCHAR2LIST,
    
    p_categories   IN SYS.ODCIVARCHAR2LIST
)
IS
    TYPE t_num_tab IS TABLE OF NUMBER;
    
    v_ids t_num_tab := t_num_tab();
    
    v_count PLS_INTEGER;
    
BEGIN

    IF p_titles.COUNT != p_descriptions.COUNT OR p_titles.COUNT != p_categories.COUNT THEN
    
        RAISE_APPLICATION_ERROR(-20010, 'Collections must be same size');
        
    END IF;

    v_count := p_titles.COUNT;
    
    v_ids.EXTEND(v_count);

    FOR i IN 1..v_count LOOP
    
        SELECT NVL(MAX(resource_id),0)+1 INTO v_ids(i) FROM RESOURCES;
        
    END LOOP;

    FORALL i IN 1..v_count
    
        INSERT INTO RESOURCES(resource_id, title, description, category)
        
        VALUES(v_ids(i), p_titles(i), p_descriptions(i), p_categories(i));

    COMMIT;

EXCEPTION

    WHEN OTHERS THEN
    
        ROLLBACK;
        
        RAISE;
        
END bulk_insert_resources;
/

<img width="1011" height="810" alt="image" src="https://github.com/user-attachments/assets/17fd5c5d-ae59-4ae6-b870-539b10bb2f45" />

