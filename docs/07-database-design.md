File: docs/07-database-design.md
Document ID: DOC-07
Project: PathFinder AI
Competition: HCLTech Round 2 — PathFinder Prototype
Team: AlgoX
Version: 1.0
Status: DRAFT
Parent Documents: DOC-01, DOC-02, DOC-03, DOC-04, DOC-05, DOC-06
Next Document: DOC-08 API Contract

1. Purpose

This document defines the database architecture and data model for PathFinder AI.

The purpose of DOC-07 is to translate the requirements defined in the previous project documents into a concrete, implementation-ready data model.

This document defines:

database technology
database architecture
entities
tables
columns
data types
primary keys
foreign keys
relationships
indexes
constraints
enumerations
JSON structures
audit information
progress storage
feedback storage
recommendation storage
learning-path storage
skill relationships
resource relationships
AI-related data
seed data requirements
migration requirements
database security
backup considerations
data lifecycle
consistency rules
integration requirements

The database design must remain consistent with:

DOC-01 Project Charter
        ↓
DOC-02 Software Requirements Specification
        ↓
DOC-03 User Roles and Journeys
        ↓
DOC-04 Use Cases and User Stories
        ↓
DOC-05 MVP Scope and Acceptance Criteria
        ↓
DOC-06 System Architecture
        ↓
DOC-07 Database Design
        ↓
DOC-08 API Contract

The database is the system-of-record for persistent application state.

AI-generated text must not become the authoritative source for user state, progress, skills, or resource identity.

2. Database Design Principles

The following principles govern the database design.

DB-PRINCIPLE-001 — Single Source of Truth

Persistent learner information shall be stored in the application database.

The AI service shall not be treated as the source of truth for:

learner identity
goals
skills
resources
learning paths
progress
feedback
DB-PRINCIPLE-002 — Relational Integrity

Relationships between major application entities shall be represented using foreign keys wherever practical.

DB-PRINCIPLE-003 — Stable Identifiers

Every major entity shall have a stable unique identifier.

The preferred identifier strategy for the MVP is UUID.

Example:

550e8400-e29b-41d4-a716-446655440000
DB-PRINCIPLE-004 — Explicit Relationships

Important relationships shall not depend only on free-form text.

For example:

User → Goal
Goal → Learning Path
Skill → Skill Dependency
Resource → Skill
Learning Path → Path Item
Path Item → Resource

shall be represented explicitly.

DB-PRINCIPLE-005 — Normalized Core Data

Core relational information shall be normalized to avoid unnecessary duplication.

JSON fields may be used for flexible AI metadata or configuration where appropriate.

DB-PRINCIPLE-006 — Auditability

Important records shall contain timestamps.

At minimum:

created_at
updated_at

where applicable.

DB-PRINCIPLE-007 — Security

Passwords shall never be stored in plaintext.

Only secure password hashes shall be persisted.

Secrets such as:

JWT_SECRET
DATABASE_URL
LLM_API_KEY

shall never be stored inside database records unless explicitly required by a secure secret-management system.

3. Recommended Database Technology
3.1 Primary Database

The MVP shall use:

PostgreSQL

PostgreSQL is selected because the application contains strongly related entities such as:

users
profiles
goals
skills
resources
learning paths
progress
feedback
recommendations

A relational database provides:

referential integrity
transactions
constraints
indexing
structured querying
reliable joins
predictable application behavior
4. Database Architecture

The logical architecture is:

                    ┌──────────────────────┐
                    │      Frontend        │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │       Backend        │
                    │      REST API        │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Service Layer      │
                    └──────────┬───────────┘
                               │
                ┌──────────────┴──────────────┐
                ▼                             ▼
       ┌────────────────┐           ┌─────────────────┐
       │  PostgreSQL    │           │   AI Service    │
       │   Database     │           │  / LLM Layer    │
       └────────────────┘           └─────────────────┘

The AI layer may read application data through backend services.

The AI layer must not directly modify critical database state unless explicitly controlled through backend application services.

5. Logical Data Domains

The database is divided conceptually into the following domains.

Identity
   │
   ├── Users
   └── User Profiles

Learning Goals
   │
   ├── Goals
   └── Goal Skills

Skill Intelligence
   │
   ├── Skills
   ├── Skill Relationships
   └── User Skills

Learning Resources
   │
   ├── Resources
   ├── Resource Skills
   └── Resource Prerequisites

Recommendation
   │
   ├── Recommendations
   └── Recommendation Feedback

Learning Paths
   │
   ├── Learning Paths
   ├── Path Stages
   └── Path Items

Progress
   │
   ├── Activity Progress
   └── Milestone Progress

Conversation
   │
   ├── Conversations
   └── Messages

Feedback
   │
   └── Learner Feedback
6. Entity Overview

The MVP database shall contain the following major entities.

Entity	Purpose
users	Authentication and identity
user_profiles	Learner personalization data
goals	Learner objectives
skills	Structured skill catalog
user_skills	Learner skill state
goal_skills	Skills required for goals
skill_relationships	Skill prerequisite graph
resources	Learning resources
resource_skills	Skills developed by resources
resource_prerequisites	Resource prerequisites
learning_paths	Generated learning roadmaps
path_stages	Logical roadmap stages
path_items	Individual learning activities
recommendations	Recommended resources
recommendation_feedback	Recommendation feedback
activity_progress	Resource/activity progress
milestone_progress	Milestone completion
conversations	AI conversations
messages	Chat messages
learner_feedback	General learner feedback
7. Naming Conventions

Database naming shall follow these rules.

7.1 Tables

Use:

snake_case

Examples:

user_profiles
learning_paths
path_items
7.2 Columns

Use:

snake_case

Examples:

created_at
updated_at
experience_level
resource_type
7.3 Primary Keys

Use:

id

with UUID type.

7.4 Foreign Keys

Use:

<entity>_id

Examples:

user_id
goal_id
skill_id
resource_id
path_id
8. USERS Table
8.1 Purpose

Stores authentication identity for registered users.

8.2 Table
users
8.3 Columns
Column	Type	Null	Key	Description
id	UUID	NO	PK	Unique user identifier
name	VARCHAR(100)	NO		User display name
email	VARCHAR(255)	NO	UNIQUE	Login email
password_hash	TEXT	NO		Secure password hash
role	VARCHAR(30)	NO		User role
is_active	BOOLEAN	NO		Account status
created_at	TIMESTAMP	NO		Creation time
updated_at	TIMESTAMP	NO		Last update
8.4 Role Values

MVP roles:

LEARNER
ADMIN
8.5 Constraints
email UNIQUE
email NOT NULL
password_hash NOT NULL
role NOT NULL
9. USER_PROFILES Table
9.1 Purpose

Stores personalization information about a learner.

9.2 Table
user_profiles
9.3 Columns
Column	Type	Null	Description
id	UUID	NO	Profile identifier
user_id	UUID	NO	Related user
experience_level	VARCHAR(30)	NO	Learner experience
weekly_learning_hours	DECIMAL(5,2)	YES	Available weekly time
preferred_resource_type	VARCHAR(30)	YES	Preferred resource
preferred_learning_format	VARCHAR(50)	YES	Learning preference
bio	TEXT	YES	Optional learner description
created_at	TIMESTAMP	NO	Creation time
updated_at	TIMESTAMP	NO	Update time
9.4 Relationship
users 1 ───── 1 user_profiles

One user has one profile.

10. GOALS Table
10.1 Purpose

Stores learner objectives.

10.2 Table
goals
10.3 Columns
Column	Type	Null	Description
id	UUID	NO	Goal identifier
user_id	UUID	NO	Goal owner
title	VARCHAR(200)	NO	Goal title
description	TEXT	YES	Detailed goal
natural_language_input	TEXT	YES	Original goal input
target_role	VARCHAR(150)	YES	Target career role
target_domain	VARCHAR(150)	YES	Target domain
status	VARCHAR(30)	NO	Goal status
target_date	DATE	YES	Optional target
created_at	TIMESTAMP	NO	Creation time
updated_at	TIMESTAMP	NO	Update time
11. GOAL STATUS

Allowed values:

ACTIVE
PAUSED
COMPLETED
ARCHIVED
12. SKILLS Table
12.1 Purpose

Stores the canonical skill catalog.

12.2 Table
skills
12.3 Columns
Column	Type	Null	Description
id	UUID	NO	Skill identifier
name	VARCHAR(150)	NO	Skill name
slug	VARCHAR(150)	NO	URL/system identifier
description	TEXT	YES	Skill description
category	VARCHAR(100)	YES	Skill category
parent_skill_id	UUID	YES	Optional parent skill
created_at	TIMESTAMP	NO	Creation time
updated_at	TIMESTAMP	NO	Update time
13. SKILL HIERARCHY

Skills may optionally form a hierarchy.

Example:

Programming
    │
    ├── Python
    │     ├── NumPy
    │     └── Pandas
    │
    └── JavaScript

This allows future expansion into skill taxonomies.

14. USER_SKILLS Table
14.1 Purpose

Stores the learner's current proficiency for a skill.

14.2 Table
user_skills
14.3 Columns
Column	Type	Null	Description
id	UUID	NO	Record identifier
user_id	UUID	NO	Learner
skill_id	UUID	NO	Skill
proficiency_level	INTEGER	NO	Skill level
evidence_source	VARCHAR(50)	YES	Source of evidence
confidence_score	DECIMAL(5,2)	YES	Confidence
last_assessed_at	TIMESTAMP	YES	Assessment time
created_at	TIMESTAMP	NO	Creation
updated_at	TIMESTAMP	NO	Update
15. PROFICIENCY SCALE

The MVP shall use:

0 = Not Known
1 = Beginner
2 = Basic
3 = Intermediate
4 = Advanced
5 = Expert

Constraint:

proficiency_level >= 0
AND proficiency_level <= 5
16. GOAL_SKILLS Table
16.1 Purpose

Stores skills required by a particular goal.

16.2 Table
goal_skills
16.3 Columns
Column	Type	Null	Description
id	UUID	NO	Record identifier
goal_id	UUID	NO	Goal
skill_id	UUID	NO	Required skill
required_level	INTEGER	NO	Required proficiency
importance	VARCHAR(30)	NO	Skill importance
priority_score	DECIMAL(5,2)	YES	Numerical priority
created_at	TIMESTAMP	NO	Creation time
17. SKILL_RELATIONSHIPS Table
17.1 Purpose

Represents dependencies between skills.

Example:

Python
   ↓
NumPy
   ↓
Machine Learning
17.2 Table
skill_relationships
17.3 Columns
Column	Type	Null	Description
id	UUID	NO	Relationship ID
source_skill_id	UUID	NO	Existing prerequisite
target_skill_id	UUID	NO	Dependent skill
relationship_type	VARCHAR(30)	NO	Relationship
strength	DECIMAL(5,2)	YES	Relationship strength
created_at	TIMESTAMP	NO	Creation
18. RELATIONSHIP TYPES

MVP:

PREREQUISITE
RELATED
COMPLEMENTARY

Primary path-generation logic shall use:

PREREQUISITE
19. RESOURCES Table
19.1 Purpose

Stores learning resources.

19.2 Table
resources
19.3 Columns
Column	Type	Null	Description
id	UUID	NO	Resource ID
title	VARCHAR(300)	NO	Resource title
description	TEXT	YES	Description
url	TEXT	NO	Resource URL
provider	VARCHAR(150)	YES	Provider
resource_type	VARCHAR(40)	NO	Resource category
difficulty	VARCHAR(30)	YES	Difficulty
duration_minutes	INTEGER	YES	Estimated duration
language	VARCHAR(50)	YES	Language
rating	DECIMAL(3,2)	YES	Rating
is_active	BOOLEAN	NO	Availability
metadata	JSONB	YES	Additional metadata
created_at	TIMESTAMP	NO	Creation
updated_at	TIMESTAMP	NO	Update
20. RESOURCE TYPES

MVP resource types:

COURSE
VIDEO
ARTICLE
DOCUMENTATION
PROJECT
ASSESSMENT
21. DIFFICULTY VALUES
BEGINNER
INTERMEDIATE
ADVANCED
22. RESOURCE_SKILLS Table
22.1 Purpose

Maps learning resources to skills they develop.

22.2 Table
resource_skills
22.3 Columns
Column	Type	Null	Description
id	UUID	NO	Mapping ID
resource_id	UUID	NO	Resource
skill_id	UUID	NO	Skill
skill_level_gain	INTEGER	YES	Expected improvement
relevance_score	DECIMAL(5,2)	YES	Relevance
created_at	TIMESTAMP	NO	Creation
23. RESOURCE_PREREQUISITES Table
23.1 Purpose

Defines prerequisite skills for learning resources.

23.2 Table
resource_prerequisites
23.3 Columns
Column	Type	Null	Description
id	UUID	NO	Mapping ID
resource_id	UUID	NO	Resource
skill_id	UUID	NO	Required skill
minimum_level	INTEGER	YES	Minimum proficiency
created_at	TIMESTAMP	NO	Creation
24. LEARNING_PATHS Table
24.1 Purpose

Stores generated personalized learning paths.

24.2 Table
learning_paths
24.3 Columns
Column	Type	Null	Description
id	UUID	NO	Path ID
user_id	UUID	NO	Learner
goal_id	UUID	NO	Related goal
title	VARCHAR(300)	NO	Path title
description	TEXT	YES	Path description
status	VARCHAR(30)	NO	Path status
estimated_hours	DECIMAL(8,2)	YES	Estimated effort
progress_percentage	DECIMAL(5,2)	NO	Overall progress
generation_version	VARCHAR(50)	YES	Generator version
generated_by	VARCHAR(50)	YES	Generation source
created_at	TIMESTAMP	NO	Creation
updated_at	TIMESTAMP	NO	Update
25. LEARNING PATH STATUS

Allowed values:

ACTIVE
PAUSED
COMPLETED
ARCHIVED
26. PATH_STAGES Table
26.1 Purpose

Represents logical phases of the roadmap.

Example:

1. Foundation
2. Core Skills
3. Applied Skills
4. Projects
5. Assessment
26.2 Table
path_stages
26.3 Columns
Column	Type	Null	Description
id	UUID	NO	Stage ID
path_id	UUID	NO	Learning path
title	VARCHAR(200)	NO	Stage title
description	TEXT	YES	Stage description
sequence_order	INTEGER	NO	Stage order
milestone	BOOLEAN	NO	Whether stage is milestone
created_at	TIMESTAMP	NO	Creation
updated_at	TIMESTAMP	NO	Update
27. PATH_ITEMS Table
27.1 Purpose

Stores individual activities inside a learning path.

27.2 Table
path_items
27.3 Columns
Column	Type	Null	Description
id	UUID	NO	Item ID
stage_id	UUID	NO	Parent stage
resource_id	UUID	YES	Related resource
title	VARCHAR(300)	NO	Activity title
description	TEXT	YES	Activity description
item_type	VARCHAR(40)	NO	Activity type
sequence_order	INTEGER	NO	Item order
estimated_minutes	INTEGER	YES	Estimated time
is_required	BOOLEAN	NO	Required activity
created_at	TIMESTAMP	NO	Creation
updated_at	TIMESTAMP	NO	Update
28. PATH ITEM TYPES
COURSE
VIDEO
ARTICLE
DOCUMENTATION
PROJECT
ASSESSMENT
CUSTOM
29. RECOMMENDATIONS Table
29.1 Purpose

Stores recommendations generated for learners.

29.2 Table
recommendations
29.3 Columns
Column	Type	Null	Description
id	UUID	NO	Recommendation ID
user_id	UUID	NO	Learner
goal_id	UUID	YES	Related goal
resource_id	UUID	NO	Recommended resource
skill_id	UUID	YES	Main skill addressed
rank	INTEGER	NO	Recommendation rank
score	DECIMAL(8,4)	NO	Recommendation score
reason_code	VARCHAR(100)	YES	Machine-readable reason
explanation	TEXT	YES	Human-readable explanation
algorithm_version	VARCHAR(50)	YES	Ranking version
status	VARCHAR(30)	NO	Recommendation status
created_at	TIMESTAMP	NO	Creation
30. RECOMMENDATION STATUS
GENERATED
VIEWED
ACCEPTED
REJECTED
COMPLETED
EXPIRED
31. RECOMMENDATION_FEEDBACK Table
31.1 Purpose

Stores explicit feedback about recommendations.

31.2 Table
recommendation_feedback
31.3 Columns
Column	Type	Null	Description
id	UUID	NO	Feedback ID
recommendation_id	UUID	NO	Recommendation
user_id	UUID	NO	Learner
feedback_type	VARCHAR(40)	NO	Feedback category
comment	TEXT	YES	Optional comment
created_at	TIMESTAMP	NO	Feedback time
32. FEEDBACK TYPES
USEFUL
NOT_USEFUL
TOO_EASY
TOO_DIFFICULT
ALREADY_KNOWN
NOT_RELEVANT
33. ACTIVITY_PROGRESS Table
33.1 Purpose

Tracks learner progress for path activities.

33.2 Table
activity_progress
33.3 Columns
Column	Type	Null	Description
id	UUID	NO	Progress ID
user_id	UUID	NO	Learner
path_item_id	UUID	NO	Path activity
status	VARCHAR(30)	NO	Progress status
progress_percentage	DECIMAL(5,2)	NO	Completion percentage
started_at	TIMESTAMP	YES	Start time
completed_at	TIMESTAMP	YES	Completion
time_spent_minutes	INTEGER	YES	Time spent
updated_at	TIMESTAMP	NO	Last update
34. ACTIVITY STATUS
NOT_STARTED
IN_PROGRESS
COMPLETED
SKIPPED
35. MILESTONE_PROGRESS Table
35.1 Purpose

Tracks milestone completion.

35.2 Columns
Column	Type	Null	Description
id	UUID	NO	Milestone progress ID
user_id	UUID	NO	Learner
stage_id	UUID	NO	Milestone stage
status	VARCHAR(30)	NO	Status
progress_percentage	DECIMAL(5,2)	NO	Progress
completed_at	TIMESTAMP	YES	Completion time
updated_at	TIMESTAMP	NO	Last update
36. CONVERSATIONS Table
36.1 Purpose

Stores learner AI conversations.

36.2 Columns
Column	Type	Null	Description
id	UUID	NO	Conversation ID
user_id	UUID	NO	Learner
goal_id	UUID	YES	Relevant goal
title	VARCHAR(200)	YES	Conversation title
created_at	TIMESTAMP	NO	Creation
updated_at	TIMESTAMP	NO	Last activity
37. MESSAGES Table
37.1 Purpose

Stores messages belonging to conversations.

37.2 Columns
Column	Type	Null	Description
id	UUID	NO	Message ID
conversation_id	UUID	NO	Conversation
sender_type	VARCHAR(30)	NO	Sender
content	TEXT	NO	Message
metadata	JSONB	YES	AI metadata
created_at	TIMESTAMP	NO	Creation
38. MESSAGE SENDER TYPES
USER
ASSISTANT
SYSTEM
39. LEARNER_FEEDBACK Table
39.1 Purpose

Stores general learner feedback.

39.2 Columns
Column	Type	Null	Description
id	UUID	NO	Feedback ID
user_id	UUID	NO	Learner
category	VARCHAR(50)	NO	Feedback category
rating	INTEGER	YES	Rating
comment	TEXT	YES	Comment
context	JSONB	YES	Additional context
created_at	TIMESTAMP	NO	Creation
40. Entity Relationship Model

The major relationships are:

USERS
  │
  ├────────────── USER_PROFILES
  │
  ├────────────── GOALS
  │                   │
  │                   ├──── GOAL_SKILLS ──── SKILLS
  │                   │
  │                   └──── LEARNING_PATHS
  │                              │
  │                              └──── PATH_STAGES
  │                                        │
  │                                        └──── PATH_ITEMS
  │                                                   │
  │                                                   └──── RESOURCES
  │
  ├────────────── USER_SKILLS ───────────── SKILLS
  │
  ├────────────── RECOMMENDATIONS ───────── RESOURCES
  │                       │
  │                       └──── RECOMMENDATION_FEEDBACK
  │
  ├────────────── ACTIVITY_PROGRESS
  │
  ├────────────── MILESTONE_PROGRESS
  │
  ├────────────── CONVERSATIONS
  │                       │
  │                       └──── MESSAGES
  │
  └────────────── LEARNER_FEEDBACK


SKILLS
  │
  ├──── SKILL_RELATIONSHIPS
  │
  └──── RESOURCE_SKILLS ──── RESOURCES
                                │
                                └──── RESOURCE_PREREQUISITES
41. Detailed Relationship Definitions
REL-001
users 1 : 1 user_profiles
REL-002
users 1 : N goals
REL-003
users N : N skills

implemented through:

user_skills
REL-004
goals N : N skills

implemented through:

goal_skills
REL-005
skills N : N skills

implemented through:

skill_relationships
REL-006
resources N : N skills

implemented through:

resource_skills
REL-007
resources N : N skills

for prerequisites through:

resource_prerequisites
REL-008
goals 1 : N learning_paths
REL-009
learning_paths 1 : N path_stages
REL-010
path_stages 1 : N path_items
REL-011
resources 1 : N path_items
REL-012
users 1 : N recommendations
REL-013
recommendations 1 : N recommendation_feedback
REL-014
path_items 1 : N activity_progress
REL-015
conversations 1 : N messages
42. Foreign Key Rules

Foreign keys shall use cascading behavior carefully.

Recommended behavior:

User deletion

Do not automatically delete critical historical records unless explicitly required.

Preferred MVP behavior:

users
  ↓
soft delete / deactivate

using:

is_active

rather than physically deleting the user.

Goal deletion

Goals referenced by learning paths should not be physically deleted.

Use:

ARCHIVED

status.

Resource deletion

Resources referenced by paths should not be physically deleted.

Use:

is_active = false
43. Unique Constraints

The following constraints shall exist.

Users
users.email UNIQUE
Skills
skills.slug UNIQUE
User Skills
UNIQUE(user_id, skill_id)
Goal Skills
UNIQUE(goal_id, skill_id)
Resource Skills
UNIQUE(resource_id, skill_id)
Resource Prerequisites
UNIQUE(resource_id, skill_id)
Skill Relationships
UNIQUE(source_skill_id, target_skill_id, relationship_type)
Activity Progress
UNIQUE(user_id, path_item_id)
44. Indexing Strategy

Indexes shall be created for frequently queried fields.

Users
INDEX users.email
Goals
INDEX goals.user_id
INDEX goals.status
User Skills
INDEX user_skills.user_id
INDEX user_skills.skill_id
Goal Skills
INDEX goal_skills.goal_id
INDEX goal_skills.skill_id
Resources
INDEX resources.resource_type
INDEX resources.difficulty
INDEX resources.provider
INDEX resources.is_active
Resource Skills
INDEX resource_skills.resource_id
INDEX resource_skills.skill_id
Learning Paths
INDEX learning_paths.user_id
INDEX learning_paths.goal_id
INDEX learning_paths.status
Path Items
INDEX path_items.stage_id
INDEX path_items.resource_id
Recommendations
INDEX recommendations.user_id
INDEX recommendations.goal_id
INDEX recommendations.resource_id
INDEX recommendations.rank
Progress
INDEX activity_progress.user_id
INDEX activity_progress.path_item_id
INDEX activity_progress.status
Conversations
INDEX conversations.user_id
Messages
INDEX messages.conversation_id
INDEX messages.created_at
45. Timestamp Standard

All timestamps shall use UTC.

Database fields:

created_at
updated_at

Application services shall convert timestamps to the user's preferred timezone for display.

The database shall not store local browser time as the authoritative timestamp.

46. JSONB Usage

JSONB may be used for flexible metadata.

Appropriate examples:

resources.metadata
messages.metadata
learner_feedback.context

Example:

{
  "source": "seed_dataset",
  "tags": ["python", "machine-learning"],
  "difficulty_score": 0.65
}

JSONB shall not be used as a replacement for relational fields that are frequently queried or required for referential integrity.

47. AI Metadata

AI-generated information may be stored for traceability.

Example:

{
  "model": "configured-llm",
  "prompt_version": "v1",
  "generation_timestamp": "2026-08-26T10:00:00Z",
  "confidence": 0.86
}

AI metadata must never contain:

API keys
passwords
authentication tokens
unnecessary personal information
48. Recommendation Scoring Storage

Recommendation scores may be stored as numeric values.

Example:

score = 0.8742

The score is not itself the explanation.

The system should additionally store:

reason_code
explanation
algorithm_version

Example:

reason_code:
SKILL_GAP_MATCH

algorithm_version:
v1.0
49. Recommendation Reason Codes

Initial reason codes:

SKILL_GAP_MATCH
GOAL_MATCH
PREREQUISITE_MATCH
DIFFICULTY_MATCH
INTEREST_MATCH
LEARNING_HISTORY_MATCH
PREFERENCE_MATCH
PROGRESS_ADAPTATION

Multiple reason signals may be stored inside metadata if necessary.

50. Learning Path Generation Record

Every generated learning path shall identify the generation version.

Example:

generation_version = "path-generator-v1"

This allows future debugging when the algorithm changes.

51. Path Generation Inputs

The backend may use:

user profile
current skills
goal
goal skills
skill gaps
skill relationships
resource catalog
resource prerequisites
learning history
feedback

The generated path is then persisted.

52. Skill Gap Persistence

The MVP does not require a separate permanent skill_gaps table.

Skill gaps may initially be calculated dynamically from:

user_skills
+
goal_skills

Example:

Required Python = 4
Current Python = 2

Gap = 2

This reduces unnecessary duplication.

A dedicated skill_gaps table may be introduced later if historical gap tracking becomes necessary.

53. Skill Gap Calculation

Conceptually:

gap = required_level - current_level

Rules:

if gap <= 0:
    no gap

if gap > 0:
    skill gap exists

Example:

Required = 4
Current = 1

Gap = 3
54. Skill Gap Priority

Initial classification:

gap >= 3
    HIGH

gap = 2
    MEDIUM

gap = 1
    LOW

gap <= 0
    NONE

Importance from goal_skills shall also influence recommendation ranking.

55. Progress Calculation

Activity progress:

completed activities / total activities

Example:

Total = 10
Completed = 6

Progress = 60%

The backend shall calculate progress rather than trusting frontend-calculated values.

56. Stage Progress

For each stage:

completed stage items
/
total stage items

The result shall be used to calculate milestone status.

57. Path Progress

Path progress shall be calculated from path-item progress.

The exact weighted calculation shall be implemented in the backend service layer and tested independently.

The frontend shall consume the calculated result through the API.

58. Data Validation Rules

The backend must validate data before database insertion.

Examples:

Email
valid email format
Proficiency
0–5
Progress
0–100
Rating
1–5
Duration
>= 0
Sequence
>= 1
59. Database Transactions

Transactions shall be used for multi-step operations that must remain consistent.

Example:

Creating a learning path:

BEGIN
   create learning_path
   create stages
   create path_items
COMMIT

If any required operation fails:

ROLLBACK

The system must not leave a partially created path.

60. Example Transaction
Generate Path
     │
     ▼
Create learning_paths record
     │
     ▼
Create path_stages
     │
     ▼
Create path_items
     │
     ▼
Commit transaction

If path-item creation fails:

ROLLBACK
61. Seed Data

The MVP shall contain deterministic seed data.

Seed data should include:

Skills

Examples:

Python
SQL
Git
Statistics
NumPy
Pandas
Machine Learning
Deep Learning
Data Visualization
FastAPI
React
Resources

Examples:

Python fundamentals course
SQL fundamentals course
Machine learning course
Pandas tutorial
NumPy tutorial
Git documentation
FastAPI documentation
React documentation
Skill Relationships

Examples:

Python → NumPy
Python → Pandas
NumPy → Machine Learning
Statistics → Machine Learning
Machine Learning → Deep Learning
62. Seed Data Requirements

Seed scripts must be:

repeatable
deterministic
version controlled
safe to execute on a fresh database

Seed data must not depend on manually entered production records.

63. Migration Strategy

Database schema changes shall be managed through migrations.

The team shall not rely on manually editing production database tables.

Migration workflow:

Change schema
      ↓
Create migration
      ↓
Review migration
      ↓
Run migration locally
      ↓
Run tests
      ↓
Commit migration
64. Database Migration Location

The final folder structure shall be defined consistently with DOC-13.

The expected backend structure is:

backend/
├── app/
├── migrations/
└── ...

The exact migration framework will be finalized in DOC-14.

65. ORM Strategy

The backend shall use an ORM or structured database-access layer.

The ORM must provide:

models
relationships
migrations
validation support
transaction support
query abstraction

The exact ORM shall be finalized in DOC-14.

66. Repository Layer

Database access should be separated from business logic.

Conceptual structure:

API
 ↓
Service
 ↓
Repository
 ↓
ORM
 ↓
PostgreSQL

The frontend must never directly connect to PostgreSQL.

67. Service Layer Responsibilities

The service layer handles:

business rules
skill-gap calculation
recommendation generation
path generation
progress calculation
feedback processing
68. Repository Responsibilities

The repository layer handles:

database queries
inserts
updates
deletes
relationship retrieval
transaction participation
69. Database Error Handling

Database exceptions shall not be exposed directly to users.

Bad:

psycopg2.errors.UniqueViolation:
duplicate key value violates unique constraint...

Good API response:

{
  "success": false,
  "error": {
    "code": "RESOURCE_ALREADY_EXISTS",
    "message": "The requested resource already exists."
  }
}
70. Database Security

Database credentials shall be supplied through environment variables.

Example:

DATABASE_URL

The following shall never be committed:

password
database credentials
API keys
JWT secrets
production connection strings
71. Local Development Database

Developers shall be able to run PostgreSQL locally.

The recommended approach is containerized PostgreSQL.

Conceptual environment:

Application
     │
     ▼
Local PostgreSQL

All team members shall use the same schema migrations and seed process.

72. Development Database Initialization

A fresh developer environment should follow:

Install dependencies
       ↓
Configure .env
       ↓
Start PostgreSQL
       ↓
Run migrations
       ↓
Run seed script
       ↓
Start backend

Exact commands are defined in later environment and setup documents.

73. Database Environment Separation

At minimum:

development
testing
production

Each environment shall have a separate database.

Production data must not be used casually for development.

74. Test Database

Automated tests shall use a dedicated test database.

Test execution must not destroy developer or production data.

75. Test Data Isolation

Tests shall use controlled test data.

Example:

test_user
test_goal
test_skill
test_resource

Tests should clean up or reset their database state.

76. Backup Strategy

For a prototype:

scheduled database backup

is recommended for any deployed persistent environment.

For local development, source-controlled migrations and deterministic seed data are the primary recovery mechanism.

77. Data Lifecycle
User
ACTIVE
INACTIVE
Goal
ACTIVE
PAUSED
COMPLETED
ARCHIVED
Learning Path
ACTIVE
PAUSED
COMPLETED
ARCHIVED
Resource
ACTIVE
INACTIVE

Records should preferably be archived or deactivated rather than physically deleted when historical relationships exist.

78. Data Ownership

Each learner-owned record must be associated with the appropriate user.

Examples:

goals.user_id
user_skills.user_id
recommendations.user_id
learning_paths.user_id
conversations.user_id
feedback.user_id

The backend must validate ownership before returning or modifying learner-specific data.

79. Multi-User Isolation

User A must never access User B's:

profile
goals
skills
recommendations
learning paths
progress
conversations
feedback

unless an authorized administrative workflow explicitly permits it.

80. API and Database Boundary

The API shall expose domain objects rather than raw database tables.

For example:

The API should return:

{
  "id": "...",
  "title": "Machine Learning Engineer Roadmap",
  "progress": 42,
  "next_action": "Complete Python Fundamentals"
}

rather than exposing internal database implementation details.

81. Database-to-API Mapping
Database Entity	API Domain
users	Authentication
user_profiles	Profile
goals	Goals
skills	Skills
user_skills	Learner Skills
goal_skills	Goal Requirements
resources	Resources
recommendations	Recommendations
learning_paths	Learning Paths
path_stages	Path Stages
path_items	Path Activities
activity_progress	Progress
conversations	Chat
messages	Chat Messages
feedback tables	Feedback
82. Database-to-AI Mapping

The AI system may consume:

user profile
current skills
goal
goal skills
learning history
feedback
resource metadata
skill relationships

The AI system may produce:

goal interpretation
skill extraction
candidate ranking signals
recommendation explanation
path-generation suggestions
chat responses

The backend validates and persists the final application state.

83. AI Data Flow
Learner Input
      │
      ▼
Backend
      │
      ▼
AI Processing
      │
      ▼
Structured AI Output
      │
      ▼
Validation
      │
      ▼
Business Logic
      │
      ▼
PostgreSQL

The AI must not bypass validation.

84. Resource URL Handling

Resources shall store canonical URLs.

Example:

https://example.com/course/python

The application shall validate URL format before persistence.

85. External Resource Reliability

An external resource may become unavailable.

The database therefore stores:

is_active

The recommendation engine should avoid inactive resources.

86. Resource Deduplication

Resources should be deduplicated using canonical URLs where possible.

Recommended constraint strategy:

canonical_url UNIQUE

If the final implementation uses a separate canonical URL field, it shall be defined consistently across:

DOC-07
DOC-08
DOC-11
DOC-14
87. Recommendation Persistence Strategy

Recommendations should be persisted when they are generated.

This provides:

reproducibility
debugging
analytics
feedback tracking
explanation history
88. Recommendation Regeneration

When learner state changes significantly:

profile update
skill update
goal update
progress update
feedback

the backend may generate a new recommendation set.

Old recommendations should not be silently overwritten.

Instead, they should remain available through their status/version information.

89. Learning Path Versioning

A learner may receive multiple generated paths over time.

Example:

Path v1
Path v2
Path v3

The database shall preserve sufficient information to identify the generation version.

90. Conversation Storage Strategy

Conversation history should be stored separately from application logic.

The AI assistant can retrieve relevant context through the backend.

The database should not store an unbounded amount of unnecessary conversation history indefinitely.

Future retention policies may be introduced.

91. Chat Context

Relevant context may include:

active goal
current path
current skill gaps
recent progress
recent recommendations
recent conversation messages

The backend should construct AI context rather than blindly sending the entire database.

92. Privacy

The system shall minimize unnecessary personal data collection.

MVP learner data should be limited to what is necessary for:

authentication
personalization
recommendation
progress tracking
feedback
93. Sensitive Information

The database shall not store unnecessary:

financial information
government IDs
health information
private credentials

The MVP does not require such information.

94. Database Performance Targets

Prototype target:

Normal CRUD API:
< 500 ms preferred

Recommendation generation may take longer because it may involve AI processing.

Target:

Interactive AI/recommendation response:
< several seconds preferred

Exact benchmarks will be defined in:

DOC-18 Testing and QA Strategy
95. Query Optimization

The backend shall avoid:

N+1 queries

when loading:

learning paths
stages
path items
resources
skills

Appropriate joins/eager loading/batched queries should be used.

96. Pagination

Large collections should support pagination.

Potential candidates:

resources
recommendations
messages
learning paths

Example:

page
page_size

or cursor-based pagination where appropriate.

97. Sorting

API-level sorting must use approved database fields.

Examples:

created_at
rank
score
sequence_order
title

The backend must validate sort fields to prevent unsafe dynamic queries.

98. Search

Resource search may initially use PostgreSQL text search.

Potential fields:

title
description
provider

Future semantic/vector search may be introduced if required by the AI architecture.

99. Vector Search — Future Extension

The MVP does not require a vector database unless the implementation demonstrates measurable benefit.

Future architecture may introduce:

embeddings
vector similarity
semantic retrieval

Possible architecture:

PostgreSQL
+
pgvector

This decision must be recorded in DOC-22 if introduced.

100. Embedding Storage

If embeddings are introduced later, they must be linked to the relevant entity.

Example:

resource_id
embedding
embedding_model
embedding_version
created_at

The embedding model version must be tracked.

101. Recommendation Explainability Storage

Each recommendation should support explanation fields:

reason_code
explanation

Example:

reason_code:
SKILL_GAP_MATCH

explanation:
"This resource was selected because Python is a high-priority skill gap for your target role."
102. Deterministic vs AI Data

The following should be deterministic:

user identity
resource IDs
skill IDs
progress
path ordering
database relationships
permissions

The following may be AI-generated:

natural-language explanation
goal interpretation
skill extraction suggestions
chat responses

AI-generated values must pass application validation.

103. Database State Ownership
Data	Owner
Authentication	Backend
User profile	Backend
Goals	Backend + AI assistance
Skills	Backend
Skill relationships	Backend/Admin
Resources	Backend/Admin
Recommendations	Recommendation Service
Learning paths	Path Generator
Progress	Backend
Feedback	Backend
Chat messages	Backend
AI explanations	AI Service + Backend persistence
104. Database Integrity Rules

The following must always remain true.

Rule 1

Every goal belongs to an existing user.

Rule 2

Every user skill references an existing skill.

Rule 3

Every goal skill references an existing goal and skill.

Rule 4

Every path belongs to an existing goal and user.

Rule 5

Every path stage belongs to an existing path.

Rule 6

Every path item belongs to an existing stage.

Rule 7

Every resource reference points to an existing resource.

Rule 8

Every recommendation belongs to an existing learner.

Rule 9

Every progress record belongs to an existing path item.

Rule 10

Every message belongs to an existing conversation.

105. Orphan Prevention

The database must prevent orphan records through:

foreign keys
constraints
transaction management
application validation
106. Circular Skill Dependencies

The skill dependency graph should avoid circular prerequisite relationships.

Invalid:

A → B
B → C
C → A

The path-generation service should detect cycles before generating a learning path.

107. Path Ordering Integrity

Every path stage shall contain:

sequence_order

Every path item shall contain:

sequence_order

Ordering must be deterministic.

Example:

Stage 1
  Item 1
  Item 2
  Item 3

Stage 2
  Item 1
  Item 2
108. Milestone Integrity

A milestone stage should not be marked completed unless its required activities satisfy the completion criteria defined by the backend.

Frontend state alone must not determine milestone completion.

109. Progress Integrity

The backend must calculate:

overall_progress
stage_progress
activity_progress

where applicable.

The frontend may display these values but must not be considered authoritative.

110. Feedback Integrity

Feedback must be associated with the correct:

user
recommendation
resource

where applicable.

A user must not be able to submit feedback for another user's recommendation.

111. Database Logging

Application logs may record:

query failures
transaction failures
migration failures
connection failures

Logs must not contain:

passwords
JWT tokens
API keys
database passwords
112. Connection Management

The backend shall use a database connection pool.

The application shall not create a new uncontrolled database connection for every request.

Connection pool configuration will be defined in:

DOC-15 Environment and Configuration
113. Database Health Check

The backend shall provide an internal database health check.

Conceptual behavior:

API
 ↓
Database connection test
 ↓
Healthy / Unhealthy

The health endpoint shall not expose credentials or internal connection details.

114. Database Initialization Flow

Fresh environment:

Clone repository
      ↓
Install backend dependencies
      ↓
Configure environment
      ↓
Start PostgreSQL
      ↓
Create database
      ↓
Run migrations
      ↓
Run seed data
      ↓
Verify health
      ↓
Start backend
115. Database Development Workflow

Developer workflow:

Modify model
      ↓
Review impact
      ↓
Create migration
      ↓
Run migration
      ↓
Run tests
      ↓
Review generated schema
      ↓
Commit migration
      ↓
Push branch
116. Schema Change Rules

Any schema change must update all relevant documents.

Potential affected documents:

DOC-07 Database Design
DOC-08 API Contract
DOC-09 ML/AI Architecture
DOC-11 Backend Architecture
DOC-13 Code and Folder Architecture
DOC-14 Technology Stack
DOC-18 Testing Strategy
DOC-20 Integration Checklist
DOC-21 Master AI Implementation Specification
DOC-23 Traceability Matrix
117. Backward Compatibility

When changing database structures after APIs have been implemented, developers should prefer backward-compatible migrations.

Example:

Add new column
      ↓
Deploy code supporting both
      ↓
Migrate existing data
      ↓
Switch application behavior
      ↓
Remove obsolete field later
118. Database Testing Requirements

Tests must cover:

model creation
constraints
foreign keys
relationships
validation
transactions
progress calculation
skill-gap calculation
path persistence
recommendation persistence
ownership
119. Required Database Test Cases

At minimum:

DB-TEST-001
Create user successfully

DB-TEST-002
Reject duplicate email

DB-TEST-003
Create learner profile

DB-TEST-004
Create goal

DB-TEST-005
Create user skill

DB-TEST-006
Calculate skill gap

DB-TEST-007
Create resource

DB-TEST-008
Create learning path

DB-TEST-009
Create path stages

DB-TEST-010
Create path items

DB-TEST-011
Track activity progress

DB-TEST-012
Create recommendation

DB-TEST-013
Store recommendation feedback

DB-TEST-014
Store chat message

DB-TEST-015
Reject unauthorized data access
120. Example User Data Flow

A learner registers:

users

Then creates profile:

user_profiles

Then enters:

"I want to become a Machine Learning Engineer."

Stored in:

goals.natural_language_input

AI processing identifies:

Machine Learning
Python
Statistics
SQL
Data Processing

Required skills are stored in:

goal_skills

Learner skills are stored in:

user_skills

Skill gaps are calculated.

Recommendations are stored in:

recommendations

Generated roadmap:

learning_paths
path_stages
path_items

Progress:

activity_progress
milestone_progress

Feedback:

recommendation_feedback
121. Complete Database Flow
                 USER
                  │
                  ▼
             USER PROFILE
                  │
                  ▼
                GOAL
                  │
                  ▼
            GOAL SKILLS
                  │
                  ▼
           SKILL GAP ENGINE
                  │
          ┌───────┴────────┐
          ▼                ▼
      USER SKILLS       SKILLS
          │                │
          └───────┬────────┘
                  ▼
          RECOMMENDATION
              ENGINE
                  │
                  ▼
         RECOMMENDATIONS
                  │
                  ▼
          PATH GENERATOR
                  │
                  ▼
          LEARNING PATH
                  │
            ┌─────┴─────┐
            ▼           ▼
        PATH STAGES  PATH ITEMS
                            │
                            ▼
                        RESOURCES
                            │
                            ▼
                     USER PROGRESS
                            │
                            ▼
                        FEEDBACK
                            │
                            ▼
                    ADAPTIVE ENGINE
                            │
                            ▼
                   NEW RECOMMENDATIONS
122. MVP Database Scope

The following entities are mandatory for MVP:

users
user_profiles
goals
skills
user_skills
goal_skills
skill_relationships
resources
resource_skills
resource_prerequisites
learning_paths
path_stages
path_items
recommendations
recommendation_feedback
activity_progress
milestone_progress
conversations
messages
123. Optional MVP Entity
learner_feedback

This may be included if the UI supports general feedback.

124. Future Entities

Possible future entities:

assessments
assessment_attempts
certificates
achievements
notifications
learning_sessions
resource_reviews
resource_embeddings
skill_assessments
analytics_events
admin_audit_logs

These are outside the mandatory MVP database unless required by later design decisions.

125. Database Technology Decision

Current decision:

Primary DB:
PostgreSQL

Rationale:

Strong relational model
Reliable transactions
Excellent Python support
Scalable
Good JSONB support
Suitable for recommendation metadata
Suitable for future pgvector extension
126. ORM Decision

The exact ORM is intentionally deferred to:

DOC-14 Technology Stack and Dependencies

The selected ORM must support:

PostgreSQL
UUID
relationships
migrations
transactions
JSON/JSONB
indexes
constraints
127. Database Decision Record

This decision shall be recorded in:

DOC-22 Architecture Decision Records

Decision:

ADR-DB-001
PostgreSQL selected as primary application database.
128. Consistency With DOC-06

The database architecture must remain consistent with the system architecture.

The expected flow is:

Frontend
   ↓
API
   ↓
Backend Services
   ↓
Repository/Data Access Layer
   ↓
PostgreSQL

The frontend must not bypass the backend.

129. Consistency With DOC-02

DOC-02 requires persistence for:

learner data
goal data
skill data
skill relationships
resource data
learning paths
progress
feedback

DOC-07 provides database structures for each required domain.

130. Consistency With DOC-03

The learner journey requires:

Registration
Profile
Goal
Skills
Recommendations
Path
Progress
Feedback
Adaptation

Each state has corresponding database entities.

131. Consistency With DOC-04

User stories involving:

profile creation
goal creation
skill tracking
recommendation
path generation
progress
feedback
chat

must map to database operations defined by the backend.

132. Consistency With DOC-05

Only MVP-required entities should be implemented first.

Future database features must not delay the prototype.

133. Consistency With DOC-06

The database must remain behind the backend service layer.

No direct database connection from:

React
browser
frontend JavaScript
134. Consistency With DOC-08

DOC-08 must use these database entities as the source for API request/response design.

API names may differ from table names.

Example:

Database:
learning_paths

API:
GET /api/v1/learning-paths
135. Consistency With DOC-09

AI/ML services must consume structured data from the backend.

The AI system should not invent database IDs.

For example, when recommending a resource:

resource_id

must correspond to an existing resource.

136. Consistency With DOC-11

Backend architecture must define:

models
repositories
services
schemas
database configuration
migrations

consistently with this document.

137. Consistency With DOC-13

The code and folder architecture must contain the database implementation in a predictable location.

The exact folder naming must be frozen in DOC-13.

138. Consistency With DOC-14

DOC-14 must specify the exact versions of:

PostgreSQL
ORM
migration framework
database drivers
139. Consistency With DOC-15

DOC-15 must define environment variables such as:

DATABASE_URL

without exposing secrets in Git.

140. Consistency With DOC-18

DOC-18 must define database integration tests and transaction tests.

141. Consistency With DOC-19

Deployment architecture must provide a PostgreSQL-compatible persistent database.

142. Consistency With DOC-20

Integration checklist must verify:

database connectivity
migrations
seed data
foreign keys
API/database integration
AI/database integration
143. Consistency With DOC-21

The Master AI Implementation Specification must treat this database design as authoritative for entity names and relationships unless a documented change is approved.

144. Consistency With DOC-23

Every database-backed requirement must eventually map to:

Requirement
↓
Database entity
↓
API
↓
Service
↓
Implementation
↓
Test
145. Database Freeze Criteria

The database design can be considered frozen when:

all MVP entities are defined
all relationships are defined
primary keys are defined
foreign keys are defined
constraints are defined
indexes are defined
status values are defined
data validation rules are defined
migration strategy is defined
seed strategy is defined
API mapping is confirmed
AI mapping is confirmed
146. Database Implementation Readiness Checklist

Before coding:

 PostgreSQL confirmed
 ORM confirmed
 Migration framework confirmed
 UUID strategy confirmed
 Timestamp strategy confirmed
 All MVP tables defined
 Relationships reviewed
 Constraints reviewed
 Indexes reviewed
 Seed data identified
 Environment variables identified
 Test database strategy defined
 Database ownership rules defined
 API mapping reviewed
 AI mapping reviewed
147. Implementation Order

Database implementation shall follow this order:

1. Database configuration
        ↓
2. Base model configuration
        ↓
3. Users
        ↓
4. User profiles
        ↓
5. Skills
        ↓
6. User skills
        ↓
7. Goals
        ↓
8. Goal skills
        ↓
9. Skill relationships
        ↓
10. Resources
        ↓
11. Resource skills
        ↓
12. Resource prerequisites
        ↓
13. Learning paths
        ↓
14. Path stages
        ↓
15. Path items
        ↓
16. Recommendations
        ↓
17. Recommendation feedback
        ↓
18. Progress
        ↓
19. Conversations
        ↓
20. Messages
148. First Database Migration

The first migration shall establish:

users
user_profiles
skills
goals

Subsequent migrations may add dependent entities.

This reduces migration complexity during initial implementation.

149. Seed Execution Order

Seed data should be inserted in dependency order:

Skills
   ↓
Skill Relationships
   ↓
Resources
   ↓
Resource Skills
   ↓
Resource Prerequisites

User-specific demo data may then be created.

150. Demo User

The development environment may include a demo learner.

Example:

Name:
Demo Learner

Email:
demo@pathfinder.local

The password must be documented only through secure local development instructions and must never be reused in production.

151. Demo Goal

Example:

Machine Learning Engineer

Demo profile:

Experience:
Beginner

Current Skills:
Python = 2
SQL = 1
Statistics = 1
Git = 2
152. Demo Goal Skills

Example:

Python = 4
SQL = 3
Statistics = 3
Machine Learning = 4
NumPy = 3
Pandas = 3

This allows the prototype to demonstrate skill-gap analysis.

153. Demo Learning Path

Example:

Foundation
    ↓
Python Fundamentals
    ↓
NumPy
    ↓
Pandas
    ↓
Statistics
    ↓
Machine Learning
    ↓
Machine Learning Project
    ↓
Assessment
154. Database Demo Validation

The seeded demo must demonstrate:

profile
goal
skills
skill gaps
resources
recommendations
learning path
progress
feedback
155. Final Database Architecture

The final conceptual database is:

┌─────────────────────────────────────────────────────────┐
│                     PATHFINDER AI                       │
│                     PostgreSQL                          │
├─────────────────────────────────────────────────────────┤
│                                                         │
│ Identity                                                │
│ ├── users                                               │
│ └── user_profiles                                       │
│                                                         │
│ Goals                                                   │
│ ├── goals                                               │
│ └── goal_skills                                         │
│                                                         │
│ Skills                                                  │
│ ├── skills                                              │
│ ├── user_skills                                         │
│ └── skill_relationships                                 │
│                                                         │
│ Resources                                               │
│ ├── resources                                           │
│ ├── resource_skills                                     │
│ └── resource_prerequisites                              │
│                                                         │
│ Recommendation                                          │
│ ├── recommendations                                     │
│ └── recommendation_feedback                             │
│                                                         │
│ Learning Paths                                          │
│ ├── learning_paths                                      │
│ ├── path_stages                                         │
│ └── path_items                                          │
│                                                         │
│ Progress                                                │
│ ├── activity_progress                                   │
│ └── milestone_progress                                  │
│                                                         │
│ Conversation                                            │
│ ├── conversations                                       │
│ └── messages                                            │
│                                                         │
│ Feedback                                                │
│ └── learner_feedback                                    │
│                                                         │
└─────────────────────────────────────────────────────────┘
156. Final End-to-End Data Model
USER
 │
 ├── PROFILE
 │
 ├── SKILLS
 │      │
 │      └──────────────┐
 │                     │
 └── GOAL ── GOAL SKILLS
             │
             ▼
        SKILL GAP
             │
             ▼
        RECOMMENDATION
             │
             ▼
        LEARNING PATH
             │
        ┌────┴─────┐
        ▼          ▼
      STAGES     RESOURCES
        │
        ▼
      ITEMS
        │
        ▼
     PROGRESS
        │
        ▼
     FEEDBACK
        │
        ▼
   ADAPTATION
        │
        ▼
 NEW RECOMMENDATIONS
157. Approval Criteria

DOC-07 shall be considered approved when:

 Database technology is approved
 All MVP entities are approved
 Relationships are approved
 Primary keys are approved
 Foreign keys are approved
 Constraints are approved
 Index strategy is approved
 Progress model is approved
 Recommendation model is approved
 Learning-path model is approved
 Conversation model is approved
 Seed strategy is approved
 Migration strategy is approved
 Security requirements are approved
 API mapping is reviewed
 AI mapping is reviewed
158. Document Status
Document ID: DOC-07
Document: Database Design
Version: 1.0
Status: DRAFT
Parent: DOC-06 System Architecture
Next: DOC-08 API Contract
Implementation Status: NOT STARTED
Database Status: NOT FROZEN
159. Change Control

Any approved database change must identify:

Changed entity
Changed field
Reason
Affected API
Affected service
Affected AI logic
Affected frontend
Affected tests
Affected migration
Affected documentation
Affected GitHub issue

All approved changes must be reflected in:

DOC-07 Database Design
DOC-08 API Contract
DOC-22 Architecture Decision Records
DOC-23 Requirements Traceability Matrix
160. Final Database Design Statement

PathFinder AI shall use PostgreSQL as the authoritative persistence layer for the MVP.

The database shall maintain structured learner, goal, skill, resource, recommendation, learning-path, progress, feedback, and conversation data.

The architecture shall enforce clear separation between:

Frontend
    ↓
API
    ↓
Backend Services
    ↓
Repository/Data Access
    ↓
PostgreSQL

AI services shall assist with intelligence and personalization but shall not bypass backend validation or database integrity rules.

This database design is the authoritative baseline for implementation of the PathFinder AI MVP.

END OF DOC-07