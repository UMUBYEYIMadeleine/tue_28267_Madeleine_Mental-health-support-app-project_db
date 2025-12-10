# testing query command
# select

SELECT * FROM USERS;

SELECT * FROM EXERCISES;

# join

SELECT u.full_name, e.title, p.start_time, p.completion_time
FROM USER_EXERCISE_PROGRESS p
JOIN USERS u ON p.user_id = u.user_id
JOIN EXERCISES e ON p.exercise_id = e.exercise_id;

# aggregate

SELECT assessment_type, AVG(score) AS avg_score
FROM SELF_ASSESSMENTS
GROUP BY assessment_type;
