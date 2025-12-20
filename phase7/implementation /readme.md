# hillyday management

CREATE TABLE holidays (

  holiday_date DATE PRIMARY KEY,
  
  description  VARCHAR2(100)
);

<img width="552" height="297" alt="image" src="https://github.com/user-attachments/assets/11a603fc-941e-4461-a55a-4fa199237bbf" />

# audity log table

CREATE TABLE audit_log (

  audit_id      NUMBER GENERATED ALWAYS AS IDENTITY,
  
  username      VARCHAR2(50),
  
  action_type   VARCHAR2(10),
  
  table_name    VARCHAR2(50),
  
  action_date   DATE,
  
  status        VARCHAR2(10),
  
  message       VARCHAR2(200)
);

<img width="732" height="377" alt="image" src="https://github.com/user-attachments/assets/50f5d0be-539a-415a-89af-136ab4fb0691" />

# Audit Logging Function

CREATE OR REPLACE PROCEDURE log_audit(

  p_action  VARCHAR2,
  
  p_table   VARCHAR2,
  
  p_status  VARCHAR2,
  
  p_message VARCHAR2
  
) IS

BEGIN

  INSERT INTO audit_log
  
  VALUES (
  
    DEFAULT,
    
    USER,
    
    p_action,
    
    p_table,
    
    SYSDATE,
    
    p_status,
    
    p_message
    
  );
  
END;

/

<img width="523" height="592" alt="image" src="https://github.com/user-attachments/assets/44984b34-59f6-476f-a420-62e417b206c4" />

 # Restriction Check Function

 CREATE OR REPLACE FUNCTION is_restricted_day
 
RETURN BOOLEAN IS

  v_day VARCHAR2(10);
  
  v_cnt NUMBER;
  
BEGIN

  v_day := TO_CHAR(SYSDATE, 'DY', 'NLS_DATE_LANGUAGE=ENGLISH');

  IF v_day IN ('MON','TUE','WED','THU','FRI') THEN
  
    RETURN TRUE;
    
  END IF;

  SELECT COUNT(*) INTO v_cnt
  
  FROM holidays
  
  WHERE holiday_date = TRUNC(SYSDATE);

  IF v_cnt > 0 THEN
  
    RETURN TRUE;
    
  END IF;

  RETURN FALSE;
  
END;
/

<img width="705" height="695" alt="image" src="https://github.com/user-attachments/assets/2b77df68-4b2a-47ff-bd87-89cfe98c6275" />

# Simple Trigger (Example: INSERT)

CREATE OR REPLACE TRIGGER trg_users_insert

BEFORE INSERT ON users

FOR EACH ROW

BEGIN

  IF is_restricted_day = TRUE THEN
  
    log_audit(
    
      'INSERT',
      
      'USERS',
      
      'DENIED',
      
      'Insert not allowed on weekdays or public holidays'
    );

    RAISE_APPLICATION_ERROR(
    
      -20001,
      
      'INSERT not allowed on weekdays or public holidays'
    );
    
  ELSE
  
    log_audit(
    
      'INSERT',
      
      'USERS',
      
      'ALLOWED',
      
      'Insert allowed'
    );
    
  END IF;
END;
/

<img width="732" height="738" alt="image" src="https://github.com/user-attachments/assets/5298df21-7cc0-46b1-b98e-f9282200783c" />

# compound triger

CREATE OR REPLACE TRIGGER trg_users_dml

FOR INSERT OR UPDATE OR DELETE ON users

COMPOUND TRIGGER

  v_restricted BOOLEAN;

  BEFORE STATEMENT IS
  
  BEGIN
  
    v_restricted := is_restricted_day;
    
  END BEFORE STATEMENT;

  BEFORE EACH ROW IS
  
  BEGIN
  
    IF v_restricted = TRUE THEN
    
      log_audit(
      
        ORA_SYSEVENT,
        
        'USERS',
        
        'DENIED',
        
        'DML operation not allowed on weekday or public holiday'
      );

      RAISE_APPLICATION_ERROR(
      
        -20002,
        
        ORA_SYSEVENT || ' not allowed on weekdays or public holidays'
      );
      
    END IF;
    
  END BEFORE EACH ROW;

  AFTER STATEMENT IS
  
  BEGIN
  
    IF v_restricted = FALSE THEN
    
      log_audit(
      
        ORA_SYSEVENT,
        
        'USERS',
        
        'ALLOWED',
        
        'DML operation completed successfully'
      );
      
    END IF;
    
  END AFTER STATEMENT;

END trg_users_dml;
/

<img width="742" height="793" alt="image" src="https://github.com/user-attachments/assets/c501945a-008a-4a98-8d5b-6dcb6626ce8d" />

