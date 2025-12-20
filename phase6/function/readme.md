# calculation function

CREATE OR REPLACE FUNCTION calculate_average(

    p_total_score NUMBER,
    
    p_total_questions NUMBER
    
) RETURN NUMBER IS

BEGIN

    RETURN p_total_score / p_total_questions;
    
END;

/

<img width="788" height="585" alt="funct calcu" src="https://github.com/user-attachments/assets/5b34b7b5-a565-4836-b524-39ebfad53eb1" />


# validate function

CREATE OR REPLACE FUNCTION validate_age(

    p_age NUMBER
    
) RETURN BOOLEAN IS

BEGIN

    IF p_age >= 18 THEN
    
        RETURN TRUE;
        
    ELSE
    
        RETURN FALSE;
        
    END IF;
    
END;

/

<img width="710" height="598" alt="funct validate" src="https://github.com/user-attachments/assets/26a419f7-ca8d-4914-a4c0-2dc8556a6e73" />


# lookup functions

CREATE OR REPLACE FUNCTION get_user_name(

    p_user_id NUMBER
    
) RETURN VARCHAR2 IS

    v_name VARCHAR2(100);
    
BEGIN

    SELECT full_name
    
    INTO v_name
    
    FROM users
    
    WHERE user_id = p_user_id;

    RETURN v_name;
    
END;

/

<img width="845" height="605" alt="lookup function" src="https://github.com/user-attachments/assets/25d889e4-0943-4dca-a787-375b93354bad" />

# proper return type

| Function Type | Return Type          | Reason                    |
| ------------- | -------------------- | ------------------------- |
| Calculation   | `NUMBER`             | Returns numerical results |
| Validation    | `BOOLEAN`            | Returns TRUE or FALSE     |
| Lookup        | `VARCHAR2`, `NUMBER` | Returns table values      |

