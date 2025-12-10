# 1.Verify data with SELECT queries

SELECT COUNT(*) AS total_users FROM USERS;

SELECT * FROM USERS WHERE ROWNUM <= 10;

SELECT title, duration_minutes FROM EXERCISES;

# 2.constraint enforced

check if not null,unique,ckeck is working

SELECT * FROM USERS WHERE gender NOT IN ('M','F','OTHER');

SELECT email, COUNT(*) FROM USERS GROUP BY email HAVING COUNT(*) > 1;

SELECT * FROM USERS WHERE status NOT IN ('ACTIVE','DISABLED','DELETED');
   

# 3.test foreign key relationship

SELECT a.appointment_id, u.full_name AS user_name, c.counselor_name
FROM APPOINTMENTS a
JOIN USERS u ON a.user_id = u.user_id
JOIN COUNSELORS c ON a.counselor_id = c.counselor_id;

SELECT sa.assessment_id
FROM SELF_ASSESSMENTS sa
LEFT JOIN USERS u ON sa.user_id = u.user_id WHERE u.user_id IS not NULL;

# 4.data completeness

SELECT * FROM USERS WHERE full_name IS NULL OR email IS NULL OR password_hash IS NULL;

SELECT * FROM EXERCISES WHERE title IS NULL OR duration_minutes IS NULL;

# 5.validate

SELECT u.user_id, u.full_name
FROM USERS u
LEFT JOIN USER_EXERCISE_PROGRESS p ON u.user_id = p.user_id
WHERE p.exercise_id IS not NULL;


SELECT * FROM SELF_ASSESSMENTS
WHERE user_id NOT IN (SELECT user_id FROM USERS);



