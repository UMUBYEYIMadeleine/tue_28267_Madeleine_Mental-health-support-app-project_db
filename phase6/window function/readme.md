# row num()

SELECT *

FROM (

    SELECT 
    
        sa.assessment_id,
        
        sa.user_id,
        
        sa.assessment_date,
        
        sa.score,
        
        ROW_NUMBER() OVER(PARTITION BY sa.user_id ORDER BY sa.assessment_date DESC) AS rn
        
    FROM SELF_ASSESSMENTS sa
) 

WHERE rn = 1;

<img width="928" height="650" alt="image" src="https://github.com/user-attachments/assets/0bbf3db1-ad5f-486a-a76b-4cbba7260a4d" />

# rank and danse

SELECT 

    u.user_id,
    
    u.full_name,
    
    SUM(uep.duration_seconds) AS total_duration,
    
    RANK() OVER(ORDER BY SUM(uep.duration_seconds) DESC) AS rank_r,
    
    DENSE_RANK() OVER(ORDER BY SUM(uep.duration_seconds) DESC) AS dense_rank_r
    
FROM USER_EXERCISE_PROGRESS uep

JOIN USERS u ON u.user_id = uep.user_id

GROUP BY u.user_id, u.full_name;

<img width="767" height="667" alt="image" src="https://github.com/user-attachments/assets/988aadbd-ad43-4c9c-8784-a11b851e4b08" />

# lag and lead

SELECT 

    sa.user_id,
    
    sa.assessment_date,
    
    sa.score,
    
    LAG(sa.score) OVER(PARTITION BY sa.user_id ORDER BY sa.assessment_date) AS prev_score,
    
    LEAD(sa.score) OVER(PARTITION BY sa.user_id ORDER BY sa.assessment_date) AS next_score
    
FROM SELF_ASSESSMENTS sa;

<img width="935" height="616" alt="image" src="https://github.com/user-attachments/assets/ad54cda7-7ede-4127-bf72-e438b428354c" />

# aggregation

SELECT 

    sa.user_id,
    
    sa.assessment_date,
    
    sa.score,
    
    AVG(sa.score) OVER(PARTITION BY sa.user_id) AS avg_score
    
FROM SELF_ASSESSMENTS sa;

<img width="757" height="628" alt="image" src="https://github.com/user-attachments/assets/96ac0d4d-e531-4dbf-8f04-e1b2a38b26a9" />

