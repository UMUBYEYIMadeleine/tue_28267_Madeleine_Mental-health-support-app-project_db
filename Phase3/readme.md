### PHASE III: LOGICAL MODEL DESIGN
##  Mental Health Support App - Rwanda Context

## ENTITY-RELATIONSHIP MODEL:
# Core Entities with Attributes:
 
# 1.user

user_id (NUMBER(10), PK)

full_name (VARCHAR2(100), NOT NULL)

email (VARCHAR2(100), UNIQUE, NOT NULL)

password_hash (VARCHAR2(255), NOT NULL)

phone_number (VARCHAR2(20))

date_of_birth (DATE)

gender (VARCHAR2(20))

status (VARCHAR2(20))

language_preference (VARCHAR2(20))

# 2.SELF_ASSESSMENTS ( assessment_id NUMBER(10) PRIMARY KEY,

user_id NUMBER(10) REFERENCES USERS(user_id),

assessment_date DATE,

assessment_type VARCHAR2(50),

score NUMBER(5,2));

# 3.EXERCISES (exercise_id (PK) NUMBER(10),

title VARCHAR2(100), NOT NULL

description VARCHAR2(500),

duration_minutes - NUMBER(3), NOT NULL

category - VARCHAR2(50));

# 4.USER_EXERCISE_PROGRESS

( progress_id (PK) NUMBER(10),

user_id (FK) NUMBER(10) REFERENCES users(user_id),

exercise_id (FK) NUMBER(10) REFERENCES exercises(exercise_id),

start_time TIMESTAMP,

completion_time TIMESTAMP,

duration_seconds NUMBER(6));

# 5.COUNSELORS

(counselor_id (PK) NUMBER(10),

counselor_name VARCHAR2(100), NOT NULL

contact_number VARCHAR2(20),NOT NULL

email VARCHAR2(100), UNIQUE NOT NULL

specialization VARCHAR2(200),

qualifications VARCHAR2(500),

working_hours - VARCHAR2(100));

# 6.MESSAGES

message_id (PK) NUMBER(10),

user_id (FK) NUMBER(10) REFERENCES users(user_id),

counselor_id (FK) NUMBER(10) REFERENCES counselors(counselor_id),

message_text varchar(100));

# 7.RESOURCES

resource_id (PK) NUMBER(10),

title - VARCHAR2(200),

description - VARCHAR2(1000),

content_type - VARCHAR2(30),

category - VARCHAR2(50),

language - VARCHAR2(20),

publication_date - DATE,

author - VARCHAR2(100),

publisher - VARCHAR2(100));

## 8.APPOINTMENTS
appointment_id (PK) NUMBER(10),

user_id (FK) - NUMBER(10) REFERENCES users(user_id),

counselor_id (FK) - NUMBER(10) REFERENCES counselors(counselor_id),

appointment_date - DATE,

appointment_type - VARCHAR2(20));



Cardinalities:

USERS (1) ---- (M) SELF_ASSESSMENTS

USERS (1) ---- (M) USER_EXERCISE_PROGRESS

USERS (1) ---- (M) MESSAGES

USERS (1) ---- (M) APPOINTMENTS

COUNSELORS (1) ---- (M) MESSAGES

COUNSELORS (1) ---- (M) APPOINTMENTS

EXERCISES (1) ---- (M) USER_EXERCISE_PROGRESS

### Data Dictionary

## 1.user table

| Column Name         | Data Type     | Description                             | Constraints      |
| ------------------- | ------------- | --------------------------------------- | ---------------- |
| user_id             | NUMBER(10)    | Unique ID for each user                 | Primary Key      |
| full_name           | VARCHAR2(100) | Full name of the user                   | NOT NULL         |
| email               | VARCHAR2(100) | User’s email                            | UNIQUE, NOT NULL |
| password_hash       | VARCHAR2(255) | Encrypted password                      | NOT NULL         |
| phone_number        | VARCHAR2(20)  | User’s phone number                     | Optional         |
| date_of_birth       | DATE          | User’s date of birth                    | Optional         |
| gender              | VARCHAR2(20)  | Gender description                      | Optional         |
| status              | VARCHAR2(20)  | Account status (Active, Disabled, etc.) | Optional         |
| language_preference | VARCHAR2(20)  | Preferred language                      | Optional         |








ALL tables are in 1NF no one group is repeating

2. second normal form (2NF)-Eliminate Partial Dependencies
All tables are in 2NF

3.Third Normal Form (3NF) – Eliminate Transitive Dependencies
All table are in 3NF

Justification of Normalization Approach
Reduces data redundancy: Information is stored only once, e.g., user data in USERS table.
Ensures data integrity: Foreign keys maintain consistent relationships.
Supports scalability: Adding new users, exercises, messages, or appointments does not require table restructuring.
Improves query efficiency: Structured tables simplify joins and reporting.
Supports BI and analytics: Normalized tables enable accurate aggregations and data analysis.
