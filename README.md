# UMUBYEYI-Gahamanyi-Madeleine-pl-sql-final-project-
mental health support app project

project title:MENTAL HEALTH SUPPORT APP

Name:UMUBYEYI GAHAMANYI Madeleine

ID: 28267 | Group B

course:PL/SQL Database Development

date:7/12/2025

### PHASE I:PROBLEM STATEMENT AND PRESENTATION
## Problem definition
Problem statement

• High mental health burden in Rwanda

• Limited access to professional counselors

• Cultural stigma preventing help-seeking

• No immediate support available 24/7

CONTEXT
• Mobile-based digital platform available 24/7

• Available nationwide across Rwanda

• Integrated with community health systems

• Supports Kinyarwanda language content

TARGET USERS

• Individuals with stress/anxiety/depression

• Community Health Workers (CHWs)

• Professional counselors & psychologists

• Youth and young adults

PROJECT GOALS

• Provide immediate anonymous support

• Reduce mental health stigma

• Enable early intervention

• Connect users with professionals

• Support Rwanda's mental health strategy

BI Potential and analysis

• Track user engagement patterns

• Analyze assessment trends over time

• Monitor counselor response efficiency

• Identify regional mental health needs

• Measure exercise effectiveness

• Support national health planning

• Generate usage analytics reports

Core Features Overview

.Self-assessment tools with instant feedback

.Educational resources in multiple languages

.Guided meditation and breathing exercises

.Anonymous chat with certified counselors

.Crisis support with emergency contacts

.User progress tracking and analytics
### PHASE 2:Business process modeling
 ## SCOPE DEFINITION
 
This business process model outlines the complete operational workflow of the Mental Health Support App, covering user registration, self-assessment, resource access, counselor interaction, and administrative oversight. The scope ensures comprehensive coverage of all digital mental health service delivery aspects.

## KEY ENTITIES & RESPONSIBILITIES

User/Patient:

Downloads and installs mobile application

Completes registration with personal details

Engages with self-assessment tools

Accesses educational resources and exercises

Initiates and maintains chat sessions

Uses emergency support features

Manages personal profile and settings

App System:

Handles user authentication and security

Processes and stores assessment data

Delivers multimedia educational content

Tracks user progress and engagement

Routes messages between users and counselors

Provides emergency contact information

Maintains data privacy and integrity

Counselor:

Manages professional dashboard access

Responds to user messages promptly

Provides evidence-based mental health guidance

Maintains professional boundaries and ethics

Documents session interactions

Manages workload and availability

Admin:

Monitors overall system performance

Generates analytical reports and insights

Manages counselor accounts and assignments

Updates educational content and resources

Ensures regulatory compliance

Handles system troubleshooting

## MIS RELEVANCE

.Service Management: Real-time tracking of counseling sessions

.Resource Optimization: Efficient allocation of counselor time

.Performance Analytics: User engagement and counselor effectiveness

.Decision Support: Data-driven mental health program planning

.Quality Assurance: Continuous service improvement monitoring

## LOGICAL FLOW CHARACTERISTICS

.Start Points: User registration, Counselor login, Admin access

.Parallel Pathways: Multiple support options available simultaneously

.Decision Points: Assessment routing, Emergency level determination

.Data Collection: Continuous metrics gathering at each interaction

.End Points: Session completion, Logout, Report generation

## ORGANIZATIONAL IMPACT

.Increases mental health service accessibility nationwide

.Reduces stigma through anonymous digital platform

.Provides scalable solution for growing mental health needs

.Supports data-informed public health strategy development

.Enhances community health worker effectiveness

## ANALYTICS OPPORTUNITIES

.User engagement patterns and feature popularity

.Assessment score trends and improvement metrics

.Counselor response times and session quality

.Regional mental health need identification

.Resource effectiveness and content engagement
### PHASE III: LOGICAL MODEL DESIGN
Mental Health Support App - Rwanda Context

 ## ENTITY-RELATIONSHIP MODEL:
 
## Core Entities with Attributes:
1.user

(user_id (PK) - NUMBER(10),

full_name  VARCHAR2(100), NOT NULL

email  VARCHAR2(100), UNIQUE NOT NULL

password_hash  VARCHAR2(255), NOT NULL

phone_number VARCHAR2(20),

date_of_birth DATE,

gender - VARCHAR2(20),

status - VARCHAR2(20),

language_preference - VARCHAR2(20));

2.SELF_ASSESSMENTS 

(assessment_id (PK) NUMBER(10),

user_id (FK) NUMBER(10) REFERENCES users(user_id),

assessment_date  date,

assessment_type  VARCHAR2(50),

score - NUMBER(5,2));

3.EXERCISES
(exercise_id (PK) NUMBER(10),

title VARCHAR2(100), NOT NULL

description  VARCHAR2(500),

duration_minutes - NUMBER(3), NOT NULL

category - VARCHAR2(50));

 4.USER_EXERCISE_PROGRESS
 
( progress_id (PK) NUMBER(10),

user_id (FK) NUMBER(10) REFERENCES users(user_id),

exercise_id (FK) NUMBER(10) REFERENCES exercises(exercise_id),

start_time  TIMESTAMP,

completion_time TIMESTAMP,

duration_seconds NUMBER(6));

5.COUNSELORS 

(counselor_id (PK)  NUMBER(10),

counselor_name VARCHAR2(100), NOT NULL

contact_number VARCHAR2(20),NOT NULL

email VARCHAR2(100), UNIQUE NOT NULL

specialization  VARCHAR2(200),

qualifications VARCHAR2(500),

working_hours - VARCHAR2(100));

6. MESSAGES
 
   message_id (PK)  NUMBER(10),
   
user_id (FK) NUMBER(10) REFERENCES users(user_id),

counselor_id (FK)  NUMBER(10) REFERENCES counselors(counselor_id),

message_text  varchar(20),

message_type  VARCHAR2(20),

session_id  NUMBER(10);

7.RESOURCES

resource_id (PK)  NUMBER(10),

title - VARCHAR2(200),

description - VARCHAR2(1000),

content_type - VARCHAR2(30), 

category - VARCHAR2(50),

language - VARCHAR2(20),

publication_date - DATE,

author - VARCHAR2(100),

publisher - VARCHAR2(100));

8. APPOINTMENTS

appointment_id (PK)  NUMBER(10),

user_id (FK) - NUMBER(10) REFERENCES users(user_id),

counselor_id (FK) - NUMBER(10) REFERENCES counselors(counselor_id),

appointment_date - DATE,

appointment_type - VARCHAR2(20)); 

9. CRISIS_EVENTS
    
    crisis_id (PK) NUMBER(10),
   
user_id (FK) - NUMBER(10) REFERENCES users(user_id),

crisis_type - VARCHAR2(50), 

counselor_id (FK) - NUMBER(10) REFERENCES counselors(counselor_id));

10.EMERGENCY_CONTACTS

contact_id (PK) NUMBER(10),

user_id (FK) NUMBER(10) REFERENCES users(user_id),

contact_name  VARCHAR2(100),

contact_number  VARCHAR2(20), 

contact_email - VARCHAR2(100));

Cardinalities:

USERS (1) ---- (M) SELF_ASSESSMENTS

USERS (1) ---- (M) USER_EXERCISE_PROGRESS

USERS (1) ---- (M) MESSAGES

USERS (1) ---- (M) APPOINTMENTS

USERS (1) ---- (M) CRISIS_EVENTS

USERS (1) ---- (M) EMERGENCY_CONTACTS

COUNSELORS (1) ---- (M) MESSAGES

COUNSELORS (1) ---- (M) APPOINTMENTS

EXERCISES (1) ---- (M) USER_EXERCISE_PROGRESS

Each message belongs to one user and one counselor

Each appointment involves one user and one counselor


