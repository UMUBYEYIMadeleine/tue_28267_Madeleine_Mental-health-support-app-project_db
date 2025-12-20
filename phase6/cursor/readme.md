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




