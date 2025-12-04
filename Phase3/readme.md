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

# 2.SELF_ASSESSMENTS 
( assessment_id NUMBER(10) PRIMARY KEY,

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


## 2.self_assessiment table

| Column Name     | Data Type    | Description            | Constraints                  |
| --------------- | ------------ | ---------------------- | ---------------------------- |
| assessment_id   | NUMBER(10)   | Unique assessment ID   | Primary Key                  |
| user_id         | NUMBER(10)   | ID of the user         | Foreign Key → users(user_id) |
| assessment_date | DATE         | Date of the assessment | Optional                     |
| assessment_type | VARCHAR2(50) | Type of assessment     | Optional                     |
| score           | NUMBER(5,2)  | Assessment score       | Optional                     |

## 3.exercises table

| Column Name      | Data Type     | Description                     | Constraints |
| ---------------- | ------------- | ------------------------------- | ----------- |
| exercise_id      | NUMBER(10)    | Unique ID for each exercise     | Primary Key |
| title            | VARCHAR2(100) | Exercise title                  | NOT NULL    |
| description      | VARCHAR2(500) | Exercise details                | Optional    |
| duration_minutes | NUMBER(3)     | Duration of exercise in minutes | NOT NULL    |
| category         | VARCHAR2(50)  | Exercise category               | Optional    |

## 4.USER_EXERCISE_PROGRESS Table

| Column Name      | Data Type  | Description                  | Constraints                          |
| ---------------- | ---------- | ---------------------------- | ------------------------------------ |
| progress_id      | NUMBER(10) | Unique ID for progress       | Primary Key                          |
| user_id          | NUMBER(10) | User performing the exercise | Foreign Key → users(user_id)         |
| exercise_id      | NUMBER(10) | Exercise performed           | Foreign Key → exercises(exercise_id) |
| start_time       | TIMESTAMP  | When the exercise started    | Optional                             |
| completion_time  | TIMESTAMP  | When the exercise ended      | Optional                             |
| duration_seconds | NUMBER(6)  | Time taken to complete       | Optional                             |

 ## 5.COUNSELORS Table
 
 | Column Name    | Data Type     | Description                 | Constraints      |
| -------------- | ------------- | --------------------------- | ---------------- |
| counselor_id   | NUMBER(10)    | Unique ID for counselor     | Primary Key      |
| counselor_name | VARCHAR2(100) | Name of counselor           | NOT NULL         |
| contact_number | VARCHAR2(20)  | Counselor phone             | NOT NULL         |
| email          | VARCHAR2(100) | Counselor email             | UNIQUE, NOT NULL |
| specialization | VARCHAR2(200) | Area of expertise           | Optional         |
| qualifications | VARCHAR2(500) | Education or certifications | Optional         |
| working_hours  | VARCHAR2(100) | Available working hours     | Optional         |

 ## 6.MESSAGES Table

 | Column Name  | Data Type    | Description                    | Constraints                            |
| ------------ | ------------ | ------------------------------ | -------------------------------------- |
| message_id   | NUMBER(10)   | Unique message ID              | Primary Key                            |
| user_id      | NUMBER(10)   | User who sent/received message | Foreign Key → users(user_id)           |
| counselor_id | NUMBER(10)   | Counselor involved             | Foreign Key → counselors(counselor_id) |
| message_text | VARCHAR2(20) | The message content            | Optional                               |
| message_type | VARCHAR2(20) | Type (sent/received)           | Optional                               |
| session_id   | NUMBER(10)   | Counseling session ID          | Optional                               |

## 7.RESOURCES Table

| Column Name      | Data Type      | Description               | Constraints |
| ---------------- | -------------- | ------------------------- | ----------- |
| resource_id      | NUMBER(10)     | Unique ID for resource    | Primary Key |
| title            | VARCHAR2(200)  | Resource title            | Optional    |
| description      | VARCHAR2(1000) | Summary of resource       | Optional    |
| content_type     | VARCHAR2(30)   | Format (video, PDF, etc.) | Optional    |
| category         | VARCHAR2(50)   | Category of content       | Optional    |
| language         | VARCHAR2(20)   | Language of resource      | Optional    |
| publication_date | DATE           | Publication date          | Optional    |
| author           | VARCHAR2(100)  | Content author            | Optional    |
| publisher        | VARCHAR2(100)  | Resource publisher        | Optional    |

## 8.APPOINTMENTS Table

| Column Name      | Data Type    | Description            | Constraints                            |
| ---------------- | ------------ | ---------------------- | -------------------------------------- |
| appointment_id   | NUMBER(10)   | Unique appointment ID  | Primary Key                            |
| user_id          | NUMBER(10)   | User attending         | Foreign Key → users(user_id)           |
| counselor_id     | NUMBER(10)   | Counselor assigned     | Foreign Key → counselors(counselor_id) |
| appointment_date | DATE         | Scheduled date         | Optional                               |
| appointment_type | VARCHAR2(20) | Type (online/physical) | Optional                               |

### Normalization
## 1. First Normal Form (1NF) – Eliminate Repeating Groups
ALL tables are in 1NF no one group is repeating
## 2. second normal form (2NF)-Eliminate Partial Dependencies
 All tables are in 2NF 
## 3.Third Normal Form (3NF) – Eliminate Transitive Dependencies
  All table are in 3NF
  ## Justification of Normalization Approach
  1. Reduces data redundancy: Information is stored only once, e.g., user data in USERS table.
2. Ensures data integrity: Foreign keys maintain consistent relationships.
 3. Supports scalability: Adding new users, exercises, messages, or appointments does not require table restructuring.
  4. Improves query efficiency: Structured tables simplify joins and reporting.
  5. Supports BI and analytics: Normalized tables enable accurate aggregations and data analysis.

  | Aspect                     | Consideration                                                                                                                  |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Fact vs Dimension Tables   | Fact: SELF_ASSESSMENTS, USER_EXERCISE_PROGRESS, APPOINTMENTS, MESSAGES <br> Dimension: USERS, COUNSELORS, EXERCISES, RESOURCES |
| Slowly Changing Dimensions | USERS, COUNSELORS profiles tracked via Type 2 SCD to maintain historical changes                                               |
| Aggregation Levels         | Daily, weekly, monthly user activity for reporting and analytics                                                               |
| Audit Trails               | Include created_at and updated_at timestamps for all tables                                                                    |

