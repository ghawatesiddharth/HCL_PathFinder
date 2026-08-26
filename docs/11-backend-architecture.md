# PathFinder AI — Backend Architecture

**Document ID:** DOC-11  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT  
**Parent Documents:** DOC-01, DOC-02, DOC-03, DOC-04, DOC-05, DOC-06, DOC-07, DOC-08, DOC-09, DOC-10  
**Next Documents:** DOC-12 Authentication and Security, DOC-13 Code and Folder Architecture

---

# 1. Purpose

This document defines the backend architecture of PathFinder AI.

The purpose of the backend is to provide a reliable application layer connecting:

- the frontend application
- authentication
- learner profiles
- learner goals
- skills
- skill-gap analysis
- learning resources
- recommendation engine
- learning-path generation
- AI services
- progress tracking
- feedback
- dashboard data
- database persistence
- external services

The backend is responsible for enforcing application rules and maintaining consistency between the different system components.

The backend must not depend on the frontend for business-rule enforcement.

---

# 2. Architectural Position

The backend sits between the frontend and the persistence/intelligence layers.

High-level relationship:

```text
                         PATHFINDER AI

                    ┌───────────────────┐
                    │     FRONTEND      │
                    │                   │
                    │ React Application │
                    └─────────┬─────────┘
                              │
                              │ HTTP/JSON
                              ▼
                    ┌───────────────────┐
                    │    API LAYER      │
                    │                   │
                    │ Routes / Endpoints│
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │ CONTROLLER LAYER  │
                    │                   │
                    │ Request handling  │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │  SERVICE LAYER    │
                    │                   │
                    │ Business Logic    │
                    └──────┬────┬───────┘
                           │    │
              ┌────────────┘    └──────────────┐
              ▼                                ▼
    ┌──────────────────┐              ┌──────────────────┐
    │ DATA ACCESS      │              │ AI / ML SERVICES  │
    │                  │              │                  │
    │ Repository/ORM   │              │ NLP              │
    │                  │              │ Embeddings       │
    └────────┬─────────┘              │ Ranking          │
             │                        │ Path Generation  │
             ▼                        └────────┬─────────┘
    ┌──────────────────┐                       │
    │    DATABASE      │                       │
    │                  │◄──────────────────────┘
    │ PostgreSQL       │
    └──────────────────┘
3. Backend Responsibilities

The backend shall be responsible for:

authentication
authorization
learner management
profile management
goal management
skill management
resource management
skill-gap analysis
recommendation orchestration
learning-path generation
progress tracking
feedback processing
conversational AI orchestration
dashboard aggregation
validation
error handling
logging
persistence
external-service integration
API response formatting
security enforcement
transaction management
health monitoring
4. Backend Design Principles

The backend shall follow these principles.

4.1 Separation of Concerns

Each backend component shall have a clearly defined responsibility.

Example:

Route
  ↓
Controller
  ↓
Service
  ↓
Repository
  ↓
Database

A route shall not directly perform complex database operations.

4.2 Business Logic in Services

Core business rules shall be implemented in service modules.

Examples:

GoalService
SkillService
RecommendationService
LearningPathService
ProgressService
FeedbackService

Business logic shall not be duplicated across controllers.

4.3 Database Isolation

Database access shall be isolated from controllers and API routes.

Preferred architecture:

Controller
    ↓
Service
    ↓
Repository / Data Access
    ↓
ORM / Database Driver
    ↓
Database
4.4 AI Isolation

AI functionality shall be isolated behind an AI service abstraction.

The rest of the backend should not depend directly on a specific LLM provider.

Preferred:

RecommendationService
        ↓
AIService
        ↓
AI Provider

Instead of:

Controller
        ↓
OpenAI API
4.5 Deterministic Core Logic

AI should assist the system but should not control every critical business rule.

Deterministic logic shall handle:

authentication
authorization
progress calculation
prerequisite validation
resource eligibility
database operations
status transitions
validation
access control

AI may assist with:

natural-language goal understanding
skill extraction
semantic matching
explanation generation
conversational interaction
5. Backend Technology Direction

The backend technology shall be finalized in DOC-14.

The architecture is intentionally technology-aware but implementation-framework independent.

The preferred prototype direction is:

Backend Language:
Python

Backend Framework:
FastAPI

Validation:
Pydantic

ORM / Database Access:
SQLAlchemy

Database:
PostgreSQL

Authentication:
JWT-based authentication

Password Hashing:
bcrypt or Argon2-compatible implementation

AI Integration:
Provider abstraction

Testing:
pytest

API Documentation:
OpenAPI / Swagger generated by FastAPI

These choices must be confirmed against DOC-14 before implementation.

6. Backend Architectural Style

PathFinder AI shall use a layered modular architecture.

┌────────────────────────────────────────────┐
│                  API Layer                 │
├────────────────────────────────────────────┤
│               Controller Layer             │
├────────────────────────────────────────────┤
│                Service Layer               │
├────────────────────────────────────────────┤
│              Domain / Logic Layer           │
├────────────────────────────────────────────┤
│          Repository / Data Access Layer    │
├────────────────────────────────────────────┤
│             Infrastructure Layer           │
├────────────────────────────────────────────┤
│                 Database                   │
└────────────────────────────────────────────┘

AI services operate as a specialized infrastructure/domain capability.

7. Backend Layers
7.1 API Layer

Responsibilities:

define API endpoints
parse requests
route requests
attach dependencies
enforce authentication dependencies
return standardized responses

The API layer shall remain thin.

Example:

POST /api/v1/auth/login
POST /api/v1/goals
GET  /api/v1/goals
POST /api/v1/recommendations
GET  /api/v1/learning-paths/{id}
8. Controller Layer

Controllers coordinate requests between the API layer and services.

Responsibilities:

receive validated input
call appropriate service
translate service results into API responses
handle expected application exceptions

Controllers should not contain:

complex recommendation logic
SQL queries
AI prompts
password hashing logic
path-generation algorithms
9. Service Layer

The service layer contains application business logic.

Core services include:

AuthService
UserService
ProfileService
GoalService
SkillService
ResourceService
SkillGapService
RecommendationService
LearningPathService
ProgressService
FeedbackService
DashboardService
ChatService
AIService
10. AuthService

Responsibilities:

registration
login
password verification
password hashing
token generation
token validation
logout-related handling
authentication state management

Example flow:

Registration

User Request
    ↓
AuthController
    ↓
AuthService
    ↓
Validate email
    ↓
Hash password
    ↓
Create user
    ↓
Store user
    ↓
Return safe user response

Login:

Login Request
    ↓
AuthController
    ↓
AuthService
    ↓
Find user
    ↓
Verify password
    ↓
Generate JWT
    ↓
Return authentication response
11. UserService

Responsibilities:

retrieve user
update user information
deactivate user
retrieve user-specific information
enforce user ownership

The service must never return sensitive credentials.

12. ProfileService

Responsibilities:

create learner profile
update profile
retrieve profile
manage experience level
manage interests
manage preferences
manage weekly learning availability

Example:

Profile
├── experience_level
├── interests
├── preferred_resource_types
├── weekly_hours
├── preferred_difficulty
└── learning_preferences
13. GoalService

Responsibilities:

create goal
retrieve goal
update goal
activate goal
pause goal
complete goal
archive goal
determine active goal

Goal lifecycle:

ACTIVE
  │
  ├──► PAUSED
  │
  ├──► COMPLETED
  │
  └──► ARCHIVED

PAUSED
  │
  └──► ACTIVE

Invalid transitions must be rejected.

14. SkillService

Responsibilities:

retrieve skills
create skills where authorized
update skill metadata
retrieve skill relationships
retrieve learner skills
update learner proficiency
validate skill identifiers

Skill proficiency shall use the standardized scale defined by DOC-07.

15. SkillGapService

The skill-gap service compares the learner's current state with the skills required for the selected goal.

Input:

Learner Profile
+
Current Skills
+
Target Goal
+
Required Skills

Output:

Skill Gap List

Example:

{
  "skill": "Python",
  "current_level": 2,
  "required_level": 4,
  "gap": 2,
  "priority": "high"
}
16. Skill Gap Processing

The recommended process is:

1. Load active goal
2. Identify goal-required skills
3. Load learner skills
4. Match learner skills to required skills
5. Compare proficiency
6. Calculate deficiencies
7. Assign priority
8. Return structured gaps
17. ResourceService

Responsibilities:

retrieve resources
search resources
filter resources
validate resource metadata
retrieve resources by skill
retrieve resources by difficulty
retrieve resources by type
retrieve resources by provider
retrieve resources by prerequisite

Resource types may include:

COURSE
VIDEO
ARTICLE
DOCUMENTATION
PROJECT
ASSESSMENT
18. RecommendationService

RecommendationService is one of the most important backend components.

Responsibilities:

build recommendation context
retrieve candidate resources
calculate deterministic relevance
invoke AI/ML ranking where appropriate
combine ranking signals
generate recommendation metadata
generate explanation context
persist recommendation results
19. Recommendation Pipeline

The backend recommendation pipeline shall follow:

Learner
   ↓
Active Goal
   ↓
Learner Profile
   ↓
Current Skills
   ↓
Learning History
   ↓
Skill Gap Analysis
   ↓
Candidate Resource Retrieval
   ↓
Candidate Filtering
   ↓
Candidate Scoring
   ↓
AI/ML Semantic Ranking
   ↓
Prerequisite Validation
   ↓
Final Ranking
   ↓
Recommendation Explanations
   ↓
API Response
20. Recommendation Context

RecommendationService shall construct a structured context object.

Example:

{
  "user_id": "user-123",
  "goal_id": "goal-456",
  "target_role": "Machine Learning Engineer",
  "experience_level": "beginner",
  "current_skills": [
    {
      "skill": "Python",
      "level": 3
    }
  ],
  "skill_gaps": [
    {
      "skill": "Machine Learning",
      "required_level": 4,
      "current_level": 0
    }
  ],
  "preferences": {
    "weekly_hours": 10,
    "preferred_types": [
      "course",
      "project"
    ]
  }
}
21. Candidate Generation

Candidate generation should reduce the total resource search space.

Candidate sources may include:

Skill match
Goal match
Semantic similarity
Prerequisite compatibility
Difficulty compatibility
Resource type preference
Learning history

Example:

10,000 resources
       ↓
Skill filtering
       ↓
2,000 resources
       ↓
Goal filtering
       ↓
800 resources
       ↓
Difficulty filtering
       ↓
300 resources
       ↓
Semantic ranking
       ↓
50 candidates
       ↓
Final ranking
       ↓
10 recommendations
22. Recommendation Scoring

The recommendation system may combine multiple signals.

Conceptual score:

FinalScore =
    SkillMatchScore
    +
    GoalAlignmentScore
    +
    DifficultyScore
    +
    PreferenceScore
    +
    PrerequisiteScore
    +
    SemanticSimilarityScore
    +
    HistoryScore

Exact weights shall be defined in DOC-09 and DOC-21.

The backend shall not hard-code undocumented weights.

23. LearningPathService

Responsibilities:

create learning paths
retrieve learning paths
generate path stages
validate prerequisites
order learning activities
create milestones
calculate estimated effort
identify next action
regenerate paths
24. Learning Path Generation Flow
Goal
 ↓
Required Skills
 ↓
Current Skills
 ↓
Skill Gaps
 ↓
Skill Dependency Graph
 ↓
Candidate Resources
 ↓
Prerequisite Ordering
 ↓
Stage Construction
 ↓
Milestone Construction
 ↓
Effort Estimation
 ↓
Learning Path
25. Learning Path Stages

The backend should support structured stages.

Example:

Stage 1 — Foundation
Stage 2 — Core Skills
Stage 3 — Applied Skills
Stage 4 — Projects
Stage 5 — Assessment
Stage 6 — Advanced Topics

The actual number of stages shall depend on the generated path.

26. Learning Path Data Structure

Conceptual representation:

{
  "path_id": "path-001",
  "goal_id": "goal-001",
  "title": "Machine Learning Engineer Roadmap",
  "status": "active",
  "stages": [
    {
      "stage_number": 1,
      "title": "Python Foundation",
      "activities": [
        {
          "resource_id": "resource-001",
          "order": 1,
          "status": "not_started"
        }
      ]
    }
  ]
}
27. Prerequisite Validation

The backend shall validate prerequisites before accepting a generated path.

Example:

Python
  ↓
NumPy
  ↓
Statistics
  ↓
Machine Learning
  ↓
Deep Learning

If a learner lacks Python fundamentals, the backend should not place an advanced Machine Learning resource before appropriate prerequisites unless the system explicitly determines that the learner already satisfies equivalent knowledge.

28. ProgressService

Responsibilities:

start activity
update activity status
complete activity
calculate progress
calculate milestone progress
calculate path progress
update skill development
determine next action

Supported activity states:

NOT_STARTED
IN_PROGRESS
COMPLETED
SKIPPED
29. Progress Calculation

Example:

Completed Activities
        ÷
Total Required Activities
        ×
100

However, milestone-weighted or effort-weighted calculations may be used if defined by DOC-07 or DOC-09.

The implementation must use one consistent calculation.

30. Next Action Calculation

The backend should determine the next action using:

Current Path Position
+
Activity Status
+
Prerequisites
+
Learner Progress
+
Feedback

Example:

Current:
Python Course → Completed

Next:
NumPy Fundamentals → Not Started

Recommended Action:
Start NumPy Fundamentals
31. FeedbackService

Responsibilities:

receive feedback
validate feedback
store feedback
aggregate feedback
provide feedback signals to recommendation logic

Possible feedback:

USEFUL
NOT_USEFUL
TOO_EASY
TOO_DIFFICULT
ALREADY_KNOWN
NOT_RELEVANT
32. Feedback Processing

Feedback should influence future recommendations.

Example:

Learner rejects resource
        ↓
Feedback stored
        ↓
Recommendation context updated
        ↓
Similar resources receive lower preference
        ↓
Alternative resources considered

The system must avoid overreacting to a single weak feedback signal.

33. DashboardService

DashboardService aggregates information from multiple services.

It may combine:

Profile
Goal
Learning Path
Progress
Skills
Milestones
Recommendations
Next Action

Example:

DashboardService
      │
      ├── ProfileService
      ├── GoalService
      ├── SkillService
      ├── ProgressService
      ├── RecommendationService
      └── LearningPathService

The frontend should receive an optimized dashboard response rather than making excessive independent requests.

34. ChatService

The ChatService coordinates conversational interaction.

Responsibilities:

receive learner message
identify conversation context
retrieve relevant learner state
determine intent
call appropriate application services
call AI service where appropriate
construct response
maintain safe context

Example:

User:
"What should I learn next?"

        ↓

ChatService

        ↓

Retrieve:
Goal
Progress
Current Skills
Skill Gaps
Learning Path

        ↓

RecommendationService

        ↓

AI explanation

        ↓

Response
35. Chat Intent Classification

Potential intents:

GOAL_QUERY
SKILL_QUERY
RECOMMENDATION_QUERY
PROGRESS_QUERY
NEXT_ACTION_QUERY
RESOURCE_QUERY
PATH_QUERY
GENERAL_LEARNING_QUERY

Intent classification may be performed through deterministic rules, an ML model, or an LLM depending on implementation.

Critical application actions must still use backend services.

36. AIService

AIService is the abstraction between backend business logic and external AI providers.

Responsibilities:

goal understanding
skill extraction
semantic analysis
recommendation explanation
conversational generation
structured output generation

Interface concept:

class AIService:
    def understand_goal(...)
    def extract_skills(...)
    def rank_resources(...)
    def explain_recommendation(...)
    def generate_response(...)

Exact implementation shall be defined in DOC-09 and DOC-14.

37. AI Provider Abstraction

The backend shall avoid direct dependency on one AI provider.

Architecture:

Application
    ↓
AIService Interface
    ↓
AI Provider Adapter
    ↓
External Provider

Possible providers:

Provider A
Provider B
Local Model
Mock Provider

Only one provider may be active at a time.

38. AI Fallback

If the external AI provider is unavailable:

AI Request
    ↓
Provider Failure
    ↓
Retry if appropriate
    ↓
Fallback logic
    ↓
Deterministic recommendation

The application should remain usable.

Example fallback:

AI explanation unavailable.

Recommendation selected based on:
- skill gap
- prerequisite match
- difficulty
- learner preference
39. External Service Integration Layer

External services shall be accessed through adapters.

Example:

services/
    external/
        ai/
        learning_resources/

The backend shall not scatter third-party API calls throughout the codebase.

40. Repository Layer

Repositories abstract persistence operations.

Examples:

UserRepository
ProfileRepository
GoalRepository
SkillRepository
ResourceRepository
LearningPathRepository
ProgressRepository
FeedbackRepository
RecommendationRepository

Repositories should focus on data access.

41. Repository Responsibilities

Repositories may perform:

create
read
update
delete
find_by_id
find_by_user
find_by_goal
search
filter
count
exists

Repositories should not contain AI prompting or recommendation business logic.

42. Database Transaction Management

Operations involving multiple related records should use transactions where required.

Example:

Create Learning Path
        ↓
Create Path
        ↓
Create Stages
        ↓
Create Activities
        ↓
Create Milestones
        ↓
Commit

If one critical operation fails:

Rollback
43. Transaction Boundary

Transaction boundaries should generally be controlled at the service layer.

Example:

LearningPathService
        ↓
BEGIN TRANSACTION
        ↓
Create Path
        ↓
Create Stages
        ↓
Create Activities
        ↓
Create Milestones
        ↓
COMMIT
44. API Versioning

The backend API shall use explicit versioning.

Preferred structure:

/api/v1/

Example:

/api/v1/auth/login
/api/v1/users/me
/api/v1/goals
/api/v1/skills
/api/v1/resources
/api/v1/recommendations
/api/v1/learning-paths
/api/v1/progress
/api/v1/feedback
/api/v1/chat
/api/v1/dashboard
45. API Request Flow

Standard request:

HTTP Request
    ↓
Router
    ↓
Authentication
    ↓
Request Validation
    ↓
Controller
    ↓
Service
    ↓
Repository / AI
    ↓
Database / External Service
    ↓
Service Result
    ↓
Response Schema
    ↓
HTTP Response
46. API Response Standards

All API responses should use predictable structures.

Successful response example:

{
  "success": true,
  "data": {},
  "message": "Operation completed successfully"
}

Error response example:

{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "The requested resource was not found"
  }
}

Exact response schemas must remain consistent with DOC-08.

47. HTTP Status Codes

The backend shall use appropriate HTTP status codes.

200 OK
201 Created
204 No Content
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Validation Error
429 Too Many Requests
500 Internal Server Error
502 External Service Error
503 Service Unavailable
48. Error Handling Architecture

Backend errors shall be categorized.

Validation Error
Authentication Error
Authorization Error
Not Found Error
Conflict Error
Business Rule Error
AI Service Error
External Service Error
Database Error
Internal Error
49. Global Exception Handling

A centralized exception-handling mechanism shall convert internal exceptions into safe API responses.

Example:

Service Exception
        ↓
Global Exception Handler
        ↓
Mapped Error Code
        ↓
Safe API Response

Internal stack traces must not be returned to users.

50. Validation

Validation shall occur at multiple levels.

API validation

Validate:

data types
required fields
string lengths
allowed enum values
formats
Service validation

Validate:

business rules
ownership
state transitions
prerequisites
Database validation

Enforce:

foreign keys
unique constraints
non-null requirements
indexes
data integrity
51. Authentication Middleware

Protected endpoints shall require authentication.

Conceptual flow:

Request
   ↓
Authorization Header
   ↓
JWT Validation
   ↓
Extract User ID
   ↓
Attach User Context
   ↓
Controller
52. Authorization

Authentication identifies the user.

Authorization determines what the user may access.

Example:

User A requests Goal B
        ↓
Check ownership
        ↓
If owner → allow
If not owner → reject

The backend must never rely solely on frontend route protection.

53. Ownership Rules

Learners may access only their own:

profile
goals
learning paths
progress
feedback
recommendations
private chat context

Public learning resources may be shared.

Administrative data requires appropriate authorization.

54. Security Boundaries

Security boundaries shall exist at:

Frontend
    ↓
API
    ↓
Authentication
    ↓
Authorization
    ↓
Service
    ↓
Database

Every sensitive operation must be protected.

55. Secrets

Secrets shall be loaded from environment configuration.

Examples:

DATABASE_URL
JWT_SECRET
AI_API_KEY
EXTERNAL_API_KEY

Secrets must never be:

hard-coded
committed to Git
returned in API responses
printed in logs
stored in frontend source
56. Logging Architecture

Backend logs should provide enough information to debug failures without exposing sensitive information.

Useful information:

timestamp
request_id
endpoint
HTTP method
user context where appropriate
status code
execution duration
error category

Never log:

password
JWT token
API key
database password
full sensitive user data
57. Request ID

The backend should support request correlation.

Example:

Request ID:
req-8f9a31

The ID can be used across logs.

Flow:

Frontend Request
      ↓
API
      ↓
Request ID
      ↓
Service
      ↓
Repository
      ↓
Logs
58. Health Check

The backend shall expose a health endpoint.

Example:

GET /api/v1/health

Basic response:

{
  "status": "healthy"
}

A deeper readiness endpoint may verify dependencies.

Example:

GET /api/v1/health/ready
59. Dependency Health

Readiness checks may verify:

Database
AI Provider
External Resource Provider

However, an optional AI provider should not necessarily make the entire application unavailable.

60. Configuration Architecture

Configuration shall be centralized.

Conceptual structure:

config/
    settings
    environment

Configuration should support:

development
testing
production
61. Environment Separation

The backend shall support separate environments.

Development
    ↓
Testing
    ↓
Production

Environment-specific configuration shall not be mixed.

62. Database Connection Management

The backend shall use a managed database connection/session mechanism.

Requirements:

connection pooling
controlled session lifecycle
transaction support
connection cleanup
timeout configuration
63. Database Migrations

Database schema changes shall use versioned migrations.

Preferred architecture:

Migration Tool
      ↓
Migration Files
      ↓
Database Schema

Developers must not manually modify production schemas without corresponding migration files.

64. Seed Data

The prototype shall support controlled seed data.

Seed data may include:

skills
skill relationships
learning resources
sample goals
sample prerequisite mappings

Seed scripts must be reproducible.

65. Resource Search Architecture

Resource search shall support:

keyword matching
skill matching
metadata filtering
semantic similarity
difficulty
resource type

The backend may combine multiple retrieval strategies.

66. Semantic Search

If embeddings are used:

Resource
   ↓
Embedding Generation
   ↓
Vector Representation
   ↓
Vector Storage
   ↓
Query Embedding
   ↓
Similarity Search
   ↓
Candidate Resources

The exact vector database or PostgreSQL vector extension shall be finalized in DOC-14.

67. Recommendation Caching

Caching may be used for expensive operations.

Potential cache candidates:

skill metadata
resource metadata
goal-to-skill mappings
popular resources
AI-generated explanations

Personalized recommendations must be invalidated or regenerated when relevant learner state changes.

68. Recommendation Regeneration Triggers

Recommendation regeneration may occur when:

goal changes
profile changes significantly
skill level changes
resource completion occurs
feedback changes preference
learning path changes

Not every minor event requires full regeneration.

69. Learning Path Regeneration

A path should be regenerated when meaningful learner-state changes occur.

Example:

Learner completes major milestone
        ↓
Skill state changes
        ↓
Skill gap recalculated
        ↓
Existing path evaluated
        ↓
If outdated:
    regenerate
Else:
    continue existing path
70. Idempotency

Critical operations should be designed to avoid unintended duplicate actions.

Examples:

POST learning-path generation
POST progress completion
POST feedback

Where appropriate, idempotency mechanisms should be introduced.

71. Concurrency

The backend must account for concurrent requests.

Potential examples:

Two progress updates
Two path-generation requests
Two profile updates

Database constraints and transactions should protect consistency.

72. Rate Limiting

Rate limiting should be considered for:

login
registration
chat
AI requests
recommendation generation
external resource searches

Exact limits shall be defined in security/deployment documentation.

73. AI Request Management

AI requests should have:

timeout
retry policy
maximum token/input limits
structured output validation
fallback behavior
logging without secrets
74. AI Output Validation

AI output must never be trusted blindly.

Example:

LLM Response
     ↓
Parse
     ↓
Schema Validation
     ↓
Business Rule Validation
     ↓
Accept / Reject

For structured AI output, Pydantic or equivalent schema validation should be used.

75. Prompt Management

AI prompts should not be scattered across controllers.

Preferred:

ai/
    prompts/
        goal_understanding
        skill_extraction
        recommendation_explanation
        chat

Prompt versions should be trackable.

76. AI Context Construction

The backend should construct only the context required by the AI.

Example:

User Goal
+
Current Skills
+
Skill Gaps
+
Relevant Resources
+
Learning Preferences
+
Progress

Sensitive information not required by the AI task must not be included.

77. AI Hallucination Control

The system should reduce unsupported AI-generated information.

For recommendation explanations:

AI receives:
    actual recommendation metadata
    actual skill gaps
    actual resource metadata

The AI should not invent:

course duration
course rating
provider
skills
prerequisites

when those values are not present in trusted data.

78. Source of Truth

The backend shall distinguish between trusted and generated information.

Trusted:

Database resource metadata
Database user data
Database skill relationships
Database progress

Generated:

AI explanation
AI summary
AI natural-language response

Generated information must not overwrite trusted system data without validation.

79. Backend Domain Modules

Recommended modules:

auth
users
profiles
goals
skills
resources
skill_gaps
recommendations
learning_paths
progress
feedback
dashboard
chat
ai
health

Each module should have clearly defined interfaces.

80. Suggested Backend Folder Structure

The final folder architecture will be formally defined in DOC-13.

Initial structure:

backend/
│
├── app/
│   ├── main.py
│   │
│   ├── api/
│   │   └── v1/
│   │       ├── auth.py
│   │       ├── users.py
│   │       ├── profiles.py
│   │       ├── goals.py
│   │       ├── skills.py
│   │       ├── resources.py
│   │       ├── recommendations.py
│   │       ├── learning_paths.py
│   │       ├── progress.py
│   │       ├── feedback.py
│   │       ├── dashboard.py
│   │       ├── chat.py
│   │       └── health.py
│   │
│   ├── controllers/
│   │
│   ├── services/
│   │
│   ├── repositories/
│   │
│   ├── models/
│   │
│   ├── schemas/
│   │
│   ├── core/
│   │
│   ├── ai/
│   │
│   ├── integrations/
│   │
│   ├── db/
│   │
│   └── utils/
│
├── tests/
│
├── migrations/
│
├── scripts/
│
├── requirements.txt
├── .env.example
└── README.md
81. Module Dependency Rule

Dependencies should flow inward.

Preferred:

API
 ↓
Services
 ↓
Repositories
 ↓
Database

AI:

Services
 ↓
AI abstraction
 ↓
Provider adapter

Avoid:

Repository
 ↓
Controller

or:

Database Model
 ↓
Frontend
82. Circular Dependency Prevention

Modules must not create circular imports.

Bad:

GoalService → RecommendationService
RecommendationService → GoalService

Preferred:

Application Orchestrator
       ↓
GoalService
       ↓
RecommendationService

Shared functionality should be extracted into an independent service or utility.

83. Dependency Injection

The backend should use dependency injection for:

database sessions
authentication
repositories
services
AI providers
external clients
configuration

This improves:

testing
maintainability
provider replacement
modularity
84. Testing Architecture

Each backend layer should be testable independently.

Unit Tests
    ↓
Service Tests
    ↓
Repository Tests
    ↓
API Tests
    ↓
Integration Tests
    ↓
End-to-End Tests
85. Unit Testing

Unit tests should cover:

skill-gap calculations
recommendation scoring
progress calculation
goal state transitions
prerequisite validation
input validation
utility functions

These tests should not require a live database or AI provider unless explicitly necessary.

86. Service Testing

Service tests should verify:

business rules
service interactions
error conditions
ownership
state transitions
AI fallback
recommendation orchestration

External dependencies should be mocked where appropriate.

87. Repository Testing

Repository tests should verify:

database queries
relationships
constraints
filters
pagination
transactions

A test database should be used.

88. API Testing

API tests should verify:

status codes
request validation
authentication
authorization
response schemas
error responses
89. Integration Testing

Integration tests should verify interactions between:

API
+
Services
+
Database

And where appropriate:

Backend
+
AI Provider Mock
90. End-to-End Backend Workflow

The primary backend E2E workflow is:

Register
   ↓
Login
   ↓
Create Profile
   ↓
Create Goal
   ↓
Add Skills
   ↓
Analyze Skill Gap
   ↓
Generate Recommendations
   ↓
Generate Learning Path
   ↓
View Path
   ↓
Complete Activity
   ↓
Update Progress
   ↓
Provide Feedback
   ↓
Generate Updated Recommendations

This workflow must be testable before final submission.

91. Backend API-to-Service Mapping
API Capability	Primary Service
Registration	AuthService
Login	AuthService
User profile	ProfileService
Goals	GoalService
Skills	SkillService
Skill gaps	SkillGapService
Resources	ResourceService
Recommendations	RecommendationService
Learning paths	LearningPathService
Progress	ProgressService
Feedback	FeedbackService
Dashboard	DashboardService
Chat	ChatService
AI operations	AIService
Health	HealthService
92. Backend-to-Database Mapping
Backend Module	Main Data
Auth	users
Profile	learner_profiles
Goals	goals
Skills	skills, learner_skills
Skill Gap	goal_skills, skill_dependencies
Resources	learning_resources
Recommendations	recommendations
Learning Path	learning_paths, path_stages, path_activities
Progress	activity_progress, milestone_progress
Feedback	recommendation_feedback
Chat	conversations/messages if persisted

Exact table names must follow DOC-07.

93. Backend-to-AI Mapping
Backend Service	AI Capability
GoalService	Goal understanding
SkillGapService	Skill extraction/support
RecommendationService	Semantic ranking
LearningPathService	Path reasoning support
ChatService	Conversational generation
Explanation layer	Recommendation explanations

AI capabilities shall remain replaceable.

94. Frontend Integration

The frontend shall communicate only with documented API endpoints.

Frontend:

React
  ↓
API Client
  ↓
HTTP

Backend:

HTTP
  ↓
FastAPI
  ↓
Services

Frontend components should not know:

database schema
SQL
AI provider API keys
internal service architecture
95. API Contract Consistency

DOC-08 is the authoritative API contract.

If implementation differs from DOC-08:

Implementation
    ↓
Update DOC-08
    ↓
Update dependent documentation
    ↓
Update tests
    ↓
Commit change

The API must not silently diverge from documentation.

96. Database Contract Consistency

DOC-07 is the authoritative database design document.

Changes to:

table
column
relationship
constraint
enum
index

must be reflected in:

DOC-07
DOC-08
DOC-11
DOC-13
DOC-21
DOC-23

where applicable.

97. AI Contract Consistency

DOC-09 is the authoritative AI architecture document.

Changes to:

model
embedding strategy
ranking algorithm
prompt architecture
AI provider
fallback strategy

must be reflected in relevant documentation.

98. Backend Performance Strategy

Prototype performance should focus on:

fast API validation
efficient database queries
limited unnecessary joins
candidate filtering before AI calls
caching expensive operations
controlled external API calls
pagination
99. Avoiding N+1 Queries

The backend should avoid loading related records individually when bulk retrieval is possible.

Bad:

Get 100 resources
    ↓
100 individual skill queries

Preferred:

Get 100 resources
    ↓
Bulk retrieve related skills
    ↓
Combine in application layer
100. Pagination

Large collections should support pagination.

Potential endpoints:

GET /api/v1/resources?page=1&page_size=20
GET /api/v1/recommendations?page=1&page_size=10
GET /api/v1/feedback?page=1&page_size=20

Maximum page size shall be enforced.

101. Filtering

Filtering should be performed at the database level where practical.

Example:

GET /resources
    ?skill=python
    &difficulty=beginner
    &type=course

The exact query parameters must match DOC-08.

102. Sorting

Supported sorting may include:

relevance
difficulty
duration
rating
created_at

Sorting rules must be deterministic.

103. Backend Caching Strategy

Caching may be introduced after baseline functionality works.

Priority:

Phase 1:
No complex caching

Phase 2:
Cache stable metadata

Phase 3:
Cache expensive AI/resource operations

Phase 4:
Optimize based on measurements

Premature caching should be avoided.

104. Background Processing

Long-running operations may be moved to background processing.

Potential candidates:

large recommendation generation
embedding generation
bulk resource processing
analytics

For MVP, synchronous processing may be used where response times remain acceptable.

105. Backend Startup Flow

Application startup:

Start Process
    ↓
Load Environment
    ↓
Load Configuration
    ↓
Initialize Logging
    ↓
Initialize Database
    ↓
Initialize External Clients
    ↓
Initialize AI Provider
    ↓
Register Routes
    ↓
Start Application
106. Backend Shutdown Flow
Receive Shutdown
    ↓
Stop Accepting Requests
    ↓
Close External Clients
    ↓
Close Database Connections
    ↓
Flush Logs
    ↓
Terminate
107. Backend Main Application

The main application entry point should be responsible for:

application creation
middleware
router registration
startup/shutdown lifecycle
global exception handling

It should not contain business logic.

108. Middleware

Potential middleware:

CORS
request ID
logging
authentication support
rate limiting
security headers

Exact middleware configuration will be finalized in DOC-12 and DOC-14.

109. CORS

CORS must be configured explicitly.

Development example:

Frontend:
http://localhost:5173

Backend:
http://localhost:8000

Production origins must be explicitly configured.

Wildcard origins should not be used in production when credentials are involved.

110. Backend Security Headers

Where appropriate, the backend should provide security-related HTTP headers.

Examples:

X-Content-Type-Options
Content-Security-Policy
Referrer-Policy

Exact configuration will be defined in DOC-12.

111. Authentication Data Flow
User
 ↓
Login
 ↓
Backend validates credentials
 ↓
JWT issued
 ↓
Frontend stores authentication state
 ↓
Frontend sends token
 ↓
Backend validates token
 ↓
User context established

The exact token-storage strategy must be finalized in DOC-12.

112. Authorization Data Flow
Request
 ↓
Authenticate
 ↓
Identify User
 ↓
Check Role
 ↓
Check Resource Ownership
 ↓
Allow / Deny
113. Admin Architecture

Administrative capabilities are not core MVP functionality.

Future admin capabilities may include:

manage skills
manage resources
manage prerequisites
manage datasets
review system metrics
manage AI configuration

Admin functionality shall be isolated from learner functionality.

114. External Learning Resource Integration

If external learning-resource providers are integrated:

ResourceService
      ↓
Provider Adapter
      ↓
External API
      ↓
Normalized Resource
      ↓
Database

The application should normalize external resources into the internal resource schema.

115. External Resource Normalization

External providers may return different fields.

Example:

Provider A:
course_name

Provider B:
title

Internal:
title

The adapter shall map external data to the internal model.

116. External API Failure

If an external provider fails:

External Request
      ↓
Timeout / Error
      ↓
Retry if appropriate
      ↓
Fallback to local resources
      ↓
Return available results

External provider failure must not crash the entire application.

117. Data Ownership

The backend owns application state.

Frontend state:

temporary
UI-oriented
cache

Backend state:

authoritative
persistent
validated
118. Backend State Ownership

The backend is authoritative for:

user identity
goal status
skill state
learning path
progress
feedback
recommendations
resource metadata
119. Business Rule Examples

The backend shall enforce rules such as:

A learner cannot update another learner's goal.

A completed goal cannot automatically become active without an explicit transition.

A path activity cannot be marked completed if the request is unauthorized.

A recommendation must reference a valid resource.

A learning-path activity must reference a valid path.

A resource cannot claim a nonexistent skill.

A prerequisite relationship cannot reference itself.
120. Data Integrity

The backend shall maintain:

referential integrity
ownership integrity
status integrity
resource integrity
skill integrity
recommendation integrity
progress integrity
121. Soft Delete

Soft deletion may be used for selected entities where historical records are important.

Potential candidates:

users
goals
resources
learning paths

The exact policy will be finalized in DOC-07.

122. Auditability

Important changes should be traceable.

Potential auditable events:

goal created
goal updated
path generated
path regenerated
activity completed
feedback submitted
profile changed

Detailed audit logging is not mandatory for all MVP entities unless specified by security requirements.

123. Backend Observability

The backend should expose enough operational information to diagnose:

API failures
database failures
AI failures
external API failures
slow requests
authentication failures
124. Metrics

Future metrics may include:

request count
request latency
error rate
AI request count
AI failure rate
recommendation generation time
database query time
active users

For MVP, basic logging is sufficient unless deployment requirements demand more.

125. Backend Deployment Model

Prototype deployment may use:

Frontend
    ↓
Backend API
    ↓
Managed PostgreSQL

Optional:

Backend
    ↓
External AI Provider

Exact hosting providers shall be finalized in DOC-19.

126. Containerization

Docker may be used to standardize backend execution.

Potential structure:

backend/
    Dockerfile

Production deployment may use:

Docker
+
Cloud Platform

Docker adoption shall be finalized in DOC-14 and DOC-19.

127. Local Development Architecture

Expected local environment:

Browser
  │
  ▼
Frontend :5173
  │
  ▼
Backend :8000
  │
  ▼
PostgreSQL :5432
  │
  └──► AI Provider
128. Local Startup Sequence

Recommended:

1. Start PostgreSQL
2. Configure environment
3. Apply migrations
4. Seed development data
5. Start backend
6. Start frontend
7. Open browser

Exact commands will be defined in DOC-15.

129. Backend Environment Variables

Expected variables may include:

APP_ENV
APP_HOST
APP_PORT
DATABASE_URL
JWT_SECRET
JWT_ALGORITHM
ACCESS_TOKEN_EXPIRE_MINUTES
AI_PROVIDER
AI_API_KEY
CORS_ORIGINS
LOG_LEVEL

Exact variables shall be finalized in DOC-15.

130. Environment File Rules

Repository:

.env.example

Local:

.env

Production:

platform secret manager

.env must not be committed.
131. Dependency Management

Backend dependencies shall be explicitly pinned or constrained.

Example:

requirements.txt

or equivalent package-management mechanism.

The dependency list must be consistent with DOC-14.

132. Dependency Categories

Dependencies should be categorized conceptually.

Web Framework
Validation
Database
Authentication
AI/ML
HTTP Client
Testing
Development Tools
133. Backend Documentation Requirements

The backend shall document:

setup
environment variables
database setup
migration commands
seed commands
server startup
API documentation
testing
deployment
troubleshooting
134. OpenAPI Documentation

The backend should expose automatically generated API documentation where supported.

Expected endpoints:

/docs
/openapi.json

Availability in production shall depend on deployment/security requirements.

135. API Contract Testing

The implementation should be checked against DOC-08.

Validation areas:

endpoint path
HTTP method
request schema
response schema
status code
authentication
error format
136. Backend Integration Contract

The following documents are authoritative dependencies.

Document	Backend Dependency
DOC-01	Product vision
DOC-02	Functional requirements
DOC-03	User journeys
DOC-04	Use cases
DOC-05	MVP scope
DOC-06	System architecture
DOC-07	Database contract
DOC-08	API contract
DOC-09	AI architecture
DOC-10	Frontend integration
137. Backend Change Management

Backend changes must identify affected areas.

Example:

Change:
Add learner weekly learning hours

Affected:
DOC-02
DOC-07
DOC-08
DOC-09
DOC-10
DOC-11
DOC-13
DOC-15
DOC-21
DOC-23

Documentation must be updated before or together with implementation.

138. Backend Implementation Phases

Backend development shall follow controlled phases.

Phase B1 — Foundation
project setup
configuration
database connection
logging
health check
basic application
Phase B2 — Authentication
registration
login
JWT
authorization
Phase B3 — Learner Domain
profile
goals
skills
Phase B4 — Intelligence
skill gaps
resource retrieval
recommendations
Phase B5 — Learning Path
path generation
prerequisites
milestones
Phase B6 — Progress
activity status
progress
next action
Phase B7 — Feedback
feedback
adaptive ranking
Phase B8 — AI Assistant
chat
explanations
context
fallback
Phase B9 — Dashboard
aggregated learner state
Phase B10 — Hardening
testing
security
performance
deployment
139. Backend MVP Boundary

The MVP backend must support:

Authentication
Profile
Goal
Skills
Skill Gap
Resources
Recommendations
Learning Path
Progress
Dashboard
AI Assistant
Explanation

Feedback and adaptation should be implemented if time permits within the MVP schedule.

140. Backend Features That May Be Deferred

Potential future features:

advanced analytics
social learning
team learning
notifications
calendar integration
multiple AI providers simultaneously
advanced recommendation experimentation
real-time collaboration
administrator dashboard
large-scale event processing

These must not destabilize the MVP.

141. Critical Demo Path

The backend must support the competition demo flow.

1. User opens application
2. User registers/logs in
3. User enters learning goal
4. User provides skills
5. Backend analyzes skill gaps
6. Backend retrieves recommendations
7. Backend generates personalized path
8. Frontend displays roadmap
9. AI explains recommendations
10. User completes an activity
11. Progress updates
12. Dashboard updates
13. User asks AI what to learn next
14. Backend returns personalized answer
142. Demo Reliability Requirement

The demo workflow must not depend on fragile external services wherever avoidable.

The system should have:

seeded learning resources
known skill data
fallback recommendation logic
AI fallback
stable database
predictable demo account
143. Demo Data

A controlled demonstration dataset should include:

multiple skills
skill prerequisites
multiple learning resources
different difficulty levels
sample goal mappings
sample projects
sample assessments

The data should be sufficient to demonstrate personalization.

144. Backend Acceptance Criteria

The backend architecture is considered successfully implemented when:

Authentication works
Profile works
Goals work
Skills work
Skill-gap analysis works
Resources can be retrieved
Recommendations can be generated
Learning paths can be generated
Prerequisites are respected
Progress can be updated
Dashboard data can be retrieved
AI assistant can respond
AI failure has fallback behavior
Unauthorized access is blocked
API contracts match DOC-08
Database behavior matches DOC-07
145. Backend Quality Gates

Before moving to final integration:

[ ] No hard-coded secrets
[ ] Environment configuration works
[ ] Database migrations work
[ ] Seed data works
[ ] Authentication tested
[ ] Authorization tested
[ ] API validation tested
[ ] Service logic tested
[ ] Repository tests pass
[ ] AI fallback tested
[ ] Recommendation pipeline tested
[ ] Learning path pipeline tested
[ ] Progress tested
[ ] Error handling tested
[ ] Health endpoint works
[ ] Frontend API integration verified
146. Backend Failure Scenarios

The backend must handle:

invalid credentials
duplicate registration
expired token
invalid token
unauthorized resource access
invalid goal
missing skill
missing resource
no recommendations
AI timeout
AI provider unavailable
external API unavailable
database unavailable
invalid progress update
invalid feedback
duplicate requests
147. Failure Recovery Principles

When a failure occurs:

Detect
 ↓
Classify
 ↓
Log safely
 ↓
Recover if possible
 ↓
Fallback if available
 ↓
Return controlled response
148. Recommendation Failure Example

If AI ranking fails:

Candidate resources
        ↓
AI ranking unavailable
        ↓
Deterministic scoring
        ↓
Prerequisite validation
        ↓
Final recommendations

This preserves core functionality.

149. Chat Failure Example

If AI chat fails:

User question
      ↓
AI unavailable
      ↓
Intent recognized
      ↓
Backend retrieves trusted learner state
      ↓
Predefined/contextual response

Example:

"Your next recommended activity is NumPy Fundamentals because it is the next incomplete prerequisite in your current learning path."
150. Database Failure Example

If database connectivity fails:

Database Error
      ↓
Exception Handler
      ↓
Log error
      ↓
Return 503

The response must not expose database credentials or internal stack traces.

151. Backend Security Checklist

Before deployment:

[ ] Password hashing enabled
[ ] JWT secret configured
[ ] CORS restricted
[ ] Secrets removed from code
[ ] Input validation enabled
[ ] Authorization checks enabled
[ ] Ownership checks enabled
[ ] Rate limiting considered
[ ] Secure error responses
[ ] Sensitive logging removed
[ ] Database credentials secured
[ ] AI API key secured
152. Backend Code Quality Rules

Code should follow:

PEP 8
type hints
clear naming
small functions
single responsibility
documented public interfaces
consistent error handling
no duplicated business logic
153. Naming Conventions

Recommended:

Python files:
snake_case.py

Classes:
PascalCase

Functions:
snake_case

Constants:
UPPER_SNAKE_CASE

Database:
snake_case

API paths:
kebab-case or resource-oriented convention defined by DOC-08

The repository shall follow one consistent convention.

154. Type Safety

Type annotations should be used for:

function parameters
return values
service interfaces
schemas
repository interfaces
AI responses

This reduces integration errors.

155. Schema Separation

Request and response schemas should be separated from database models.

Example:

UserDBModel
UserCreateSchema
UserResponseSchema

The database model must not automatically become the public API response.

156. Sensitive Data Filtering

API responses must explicitly select returned fields.

Example:

Never return:

password_hash
JWT secret
internal tokens
private configuration
157. Service Interface Stability

Services should expose stable interfaces.

Example:

RecommendationService.generate_recommendations(
    learner_id,
    goal_id
)

Internal implementation may change without requiring frontend changes.

158. AI Interface Stability

The rest of the application should depend on:

AIService

rather than:

OpenAIClient
GeminiClient
ClaudeClient

This allows provider replacement.

159. Mock Services

Development and testing should support mock implementations.

Example:

MockAIService
MockResourceProvider

This allows tests to run without external API dependencies.

160. Test Environment Architecture
Tests
  ↓
Test Application
  ↓
Test Database
  ↓
Mock AI Provider

External production AI APIs should not be required for normal unit tests.

161. Backend Integration Contract With Frontend

The frontend depends on:

Authentication API
Profile API
Goal API
Skill API
Recommendation API
Learning Path API
Progress API
Feedback API
Dashboard API
Chat API

Any breaking change requires coordinated frontend and backend updates.

162. Backend Integration Contract With Database

The backend depends on:

tables
columns
foreign keys
indexes
constraints
enumerations
relationships

All schema changes must use migrations.

163. Backend Integration Contract With AI

The backend depends on structured AI outputs.

Example:

{
  "skills": [
    {
      "name": "Python",
      "confidence": 0.94
    }
  ]
}

The backend must validate the structure before using it.

164. Backend Integration Contract With External Resources

External resource data must be normalized.

Required internal fields may include:

resource_id
title
description
url
provider
resource_type
difficulty
duration
skills
prerequisites
165. Backend Architecture Decision Summary

The backend architecture establishes:

Layered architecture
Modular services
Repository pattern
API versioning
JWT authentication
Database abstraction
AI abstraction
External provider adapters
Centralized error handling
Structured validation
Transaction management
Testing layers
Environment configuration
Observability
166. Architecture Constraints

The backend must not:

couple frontend directly to database
expose database credentials
hard-code AI API keys
place SQL in controllers
place AI prompts in routes
trust raw AI output
allow cross-user data access
depend completely on AI availability
duplicate business rules
167. Implementation Order

The recommended backend implementation order is:

1. Repository initialization
2. Python environment
3. FastAPI setup
4. Configuration
5. Database connection
6. Models
7. Migrations
8. Schemas
9. Authentication
10. User/Profile
11. Goals
12. Skills
13. Resources
14. Skill-gap engine
15. Recommendation engine
16. Learning path engine
17. Progress
18. Feedback
19. Dashboard
20. AI service
21. Chat
22. Error handling
23. Testing
24. Security hardening
25. Deployment

The exact implementation task order will be defined in DOC-17.

168. Backend-to-Git Workflow

Each meaningful backend feature should be developed through a dedicated branch.

Example:

main
 │
 ├── feature/backend-auth
 ├── feature/backend-profile
 ├── feature/backend-goals
 ├── feature/backend-skills
 ├── feature/backend-recommendations
 └── feature/backend-learning-path

The exact Git workflow is defined in DOC-16.

169. Backend Pull Request Requirements

Before merging backend changes:

[ ] Requirement identified
[ ] Documentation checked
[ ] Tests added
[ ] Tests passing
[ ] API contract checked
[ ] Database impact checked
[ ] Security impact checked
[ ] AI impact checked
[ ] Error handling checked
[ ] Code reviewed
170. Backend Definition of Done

A backend task is complete only when:

Implementation complete
+
Tests complete
+
Documentation updated
+
API contract verified
+
Database impact handled
+
Security checked
+
Integration verified
+
Git commit created
171. Backend Dependency Map
DOC-02
  ↓
DOC-06
  ↓
DOC-07
  ↓
DOC-08
  ↓
DOC-09
  ↓
DOC-10
  ↓
DOC-11
  ↓
DOC-12
  ↓
DOC-13
  ↓
DOC-14
  ↓
DOC-15

DOC-11 therefore acts as the bridge between the architecture documents and actual backend implementation.

172. Traceability Mapping
Requirement	Backend Component
FR-001	AuthService
FR-002	AuthService
FR-003	Authentication middleware
FR-004	AuthService
FR-005–FR-011	ProfileService
FR-012–FR-015	GoalService
FR-016–FR-019	ChatService
FR-020–FR-024	SkillService / SkillGapService
FR-025–FR-028	ResourceService
FR-029–FR-035	RecommendationService
FR-036–FR-039	SkillGapService / LearningPathService
FR-040–FR-046	LearningPathService
FR-047–FR-050	AIService / RecommendationService
FR-051–FR-055	ProgressService
FR-056–FR-058	FeedbackService
FR-059–FR-062	RecommendationService
FR-063–FR-067	DashboardService
AI-001–AI-009	AIService
SEC-001–SEC-006	Security / Core
INT-001–INT-004	API / Integration Layer
NFR-001–NFR-008	Entire Backend
173. Backend Acceptance Test Scenarios

The following scenarios must eventually be implemented as automated tests.

Scenario 1 — Registration
Given a new valid learner
When registration is submitted
Then a user account is created
And the password is securely hashed
And sensitive data is not returned
Scenario 2 — Duplicate Registration
Given an existing email
When registration is attempted
Then the backend returns a controlled conflict response
Scenario 3 — Login
Given valid credentials
When login is submitted
Then a valid authentication response is returned
Scenario 4 — Unauthorized Access
Given user A
When user A requests user B's private goal
Then access is denied
Scenario 5 — Goal Creation
Given an authenticated learner
When a valid goal is submitted
Then the goal is stored
And belongs to that learner
Scenario 6 — Skill Gap
Given current learner skills
And goal-required skills
When skill-gap analysis runs
Then missing skills are returned
Scenario 7 — Recommendation
Given a valid learner goal
When recommendations are generated
Then relevant resources are returned
Scenario 8 — Learning Path
Given a valid goal and skill gaps
When path generation runs
Then a structured path is returned
And prerequisites are respected
Scenario 9 — Progress
Given an active learning activity
When the learner completes it
Then progress is updated
Scenario 10 — AI Failure
Given AI provider unavailable
When recommendation generation occurs
Then deterministic fallback logic is used
174. Backend Readiness Checklist

Before backend implementation begins:

[ ] DOC-01 approved
[ ] DOC-02 approved
[ ] DOC-03 approved
[ ] DOC-04 approved
[ ] DOC-05 approved
[ ] DOC-06 approved
[ ] DOC-07 approved
[ ] DOC-08 approved
[ ] DOC-09 approved
[ ] DOC-10 approved
[ ] DOC-11 approved

Then continue with:

DOC-12
DOC-13
DOC-14
DOC-15

before writing production code.

175. Document Change Control

Any architectural change must identify:

Change ID
Date
Reason
Affected component
Affected requirement
Affected database
Affected API
Affected frontend
Affected AI architecture
Affected tests
Affected deployment

The change shall be recorded in DOC-22 where appropriate.

176. Known Architectural Decisions Pending

The following are not fully frozen yet:

Exact FastAPI project organization
Exact ORM configuration
Exact JWT implementation
Exact AI provider
Exact embedding model
Exact vector-storage strategy
Exact caching technology
Exact deployment platform
Exact background-job technology
Exact monitoring platform

These decisions will be finalized in:

DOC-12
DOC-13
DOC-14
DOC-19
DOC-22
177. Architectural Freeze Rule

Once DOC-14 and DOC-22 are approved, implementation should not introduce major technology changes without an architecture decision record.

Example:

Chosen:
PostgreSQL

Developer wants:
MongoDB

Required:
ADR
+
Impact Analysis
+
Documentation Update
+
Approval
178. Backend Prototype Philosophy

The backend should prioritize:

correctness
simplicity
demonstrability
maintainability
integration reliability

over unnecessary enterprise complexity.

The goal is a professional prototype that can demonstrate the complete personalized-learning journey within the competition timeline.

179. Final Backend Architecture

The complete architecture can be summarized as:

                    ┌──────────────────────┐
                    │       FRONTEND       │
                    │    React / Client    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │       API v1         │
                    │       FastAPI        │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │    CONTROLLERS       │
                    └──────────┬───────────┘
                               │
                               ▼
        ┌─────────────────────────────────────────────┐
        │                SERVICES                     │
        │                                             │
        │ Auth │ Profile │ Goal │ Skill │ Resource   │
        │                                             │
        │ SkillGap │ Recommendation │ LearningPath   │
        │                                             │
        │ Progress │ Feedback │ Dashboard │ Chat     │
        └───────────────┬─────────────────────────────┘
                        │
              ┌─────────┴─────────┐
              ▼                   ▼
    ┌──────────────────┐  ┌────────────────────┐
    │   REPOSITORIES   │  │      AI SERVICE    │
    │                  │  │                    │
    │ Database Access  │  │ Goal Understanding │
    │                  │  │ Skill Extraction   │
    │                  │  │ Ranking            │
    └────────┬─────────┘  │ Explanations       │
             │            │ Chat               │
             │            └─────────┬──────────┘
             ▼                      ▼
    ┌──────────────────┐  ┌────────────────────┐
    │   PostgreSQL     │  │   AI Provider      │
    │                  │  │                    │
    │ Users            │  │ External / Local   │
    │ Goals            │  └────────────────────┘
    │ Skills           │
    │ Resources        │
    │ Paths            │
    │ Progress         │
    │ Feedback         │
    └──────────────────┘
180. Final End-to-End Backend Flow

The complete backend flow is:

Learner
   │
   ▼
Authentication
   │
   ▼
Learner Profile
   │
   ▼
Learning Goal
   │
   ▼
Current Skills
   │
   ▼
Goal Understanding
   │
   ▼
Required Skills
   │
   ▼
Skill Gap Analysis
   │
   ▼
Candidate Resource Retrieval
   │
   ▼
Recommendation Ranking
   │
   ▼
Prerequisite Validation
   │
   ▼
Personalized Learning Path
   │
   ▼
Milestones
   │
   ▼
Learning Activities
   │
   ▼
Progress Tracking
   │
   ▼
Feedback
   │
   ▼
Recommendation Adaptation
   │
   ▼
AI Assistant
   │
   ▼
Updated Personalized Learning Experience
181. DOC-11 Completion Criteria

DOC-11 is considered complete when:

[✓] Backend responsibilities defined
[✓] Backend layers defined
[✓] Service architecture defined
[✓] Repository architecture defined
[✓] AI abstraction defined
[✓] API integration defined
[✓] Database integration defined
[✓] Authentication integration defined
[✓] Authorization defined
[✓] Recommendation pipeline defined
[✓] Learning-path pipeline defined
[✓] Progress architecture defined
[✓] Feedback architecture defined
[✓] Chat architecture defined
[✓] Error handling defined
[✓] Validation defined
[✓] Testing architecture defined
[✓] Configuration defined
[✓] Deployment relationship defined
[✓] Frontend integration defined
[✓] Traceability defined
[✓] Implementation order defined
[✓] Acceptance criteria defined
182. Document Status

Document ID: DOC-11

Document Name: Backend Architecture

Project: PathFinder AI

Version: 1.0

Status: DRAFT

Implementation Status: NOT STARTED

Architecture Status: PENDING FINAL FREEZE

Parent Documents:

DOC-01 Project Charter
DOC-02 Software Requirements Specification
DOC-03 User Roles and Journeys
DOC-04 Use Cases and User Stories
DOC-05 MVP Scope and Acceptance Criteria
DOC-06 System Architecture
DOC-07 Database Design
DOC-08 API Contract
DOC-09 ML/AI Architecture
DOC-10 Frontend Architecture

Next Document:

DOC-12 Authentication and Security
183. Final Rule

No backend production implementation shall begin until the backend architecture is reconciled with:

DOC-07 Database Design
DOC-08 API Contract
DOC-09 ML/AI Architecture
DOC-10 Frontend Architecture
DOC-12 Authentication and Security
DOC-13 Code and Folder Architecture
DOC-14 Technology Stack and Dependencies
DOC-15 Environment and Configuration

This rule exists to minimize integration failures when the project is implemented by multiple team members or generated with AI coding assistants.