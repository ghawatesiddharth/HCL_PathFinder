# PathFinder AI — System Architecture

**Document ID:** DOC-06  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT  
**Parent Documents:** DOC-01, DOC-02, DOC-03, DOC-04, DOC-05  
**Next Dependent Documents:** DOC-07 Database Design, DOC-08 API Contract, DOC-09 ML/AI Architecture, DOC-10 Frontend Architecture, DOC-11 Backend Architecture  
**Implementation Status:** NOT STARTED  
**Architecture Status:** PROPOSED  

---

# 1. Purpose

This document defines the high-level software architecture for PathFinder AI.

The purpose of this document is to establish a common architectural structure before implementation begins.

The architecture defined here acts as the foundation for:

- frontend implementation
- backend implementation
- database implementation
- authentication
- recommendation engine
- AI/LLM integration
- learning-path generation
- progress tracking
- feedback processing
- deployment
- testing
- GitHub task planning
- integration

All implementation components must conform to the architecture defined in this document unless an approved Architecture Decision Record changes the design.

---

# 2. Architectural Objectives

The PathFinder AI architecture shall achieve the following objectives:

1. Provide a reliable personalized learning experience.
2. Separate frontend, backend, database, AI, and external services.
3. Prevent direct frontend access to sensitive backend resources.
4. Keep deterministic recommendation logic separate from generative AI.
5. Allow the AI provider to be replaced without rewriting the complete application.
6. Support future expansion of the recommendation engine.
7. Make individual components independently testable.
8. Make local development reproducible.
9. Make deployment straightforward.
10. Reduce integration conflicts between team members.
11. Provide clear ownership boundaries between modules.
12. Support graceful failure of external AI and resource services.
13. Provide traceability from requirements to implementation.
14. Support the HCLTech Round 2 demonstration workflow.
15. Keep the MVP simple enough to implement within the competition timeline.

---

# 3. Architectural Principles

The following principles are mandatory architectural guidelines.

## 3.1 Separation of Concerns

Each component shall have a clearly defined responsibility.

Frontend:
- presentation
- user interaction
- client-side state
- API communication

Backend:
- business logic
- authentication
- authorization
- orchestration
- validation
- API delivery

Database:
- persistent application data

AI Layer:
- natural-language processing
- semantic interpretation
- explanations
- AI-assisted reasoning

Recommendation Engine:
- deterministic candidate generation
- scoring
- ranking
- personalization

Path Generator:
- prerequisite ordering
- milestone construction
- roadmap generation

External Integrations:
- external learning resources
- external AI providers

---

# 4. Architecture Style

PathFinder AI shall use a modular layered architecture.

The recommended architecture is:

```text
                    ┌───────────────────────────────┐
                    │           USER                │
                    │                               │
                    │  Learner / Administrator      │
                    └───────────────┬───────────────┘
                                    │
                                    ▼
                    ┌───────────────────────────────┐
                    │       FRONTEND CLIENT         │
                    │                               │
                    │ React-based Web Application   │
                    │                               │
                    │ Pages                         │
                    │ Components                    │
                    │ State Management               │
                    │ API Client                    │
                    └───────────────┬───────────────┘
                                    │ HTTPS / REST
                                    ▼
                    ┌───────────────────────────────┐
                    │          BACKEND API          │
                    │                               │
                    │ Authentication                │
                    │ User Management               │
                    │ Goal Management               │
                    │ Profile Management            │
                    │ Learning Path API             │
                    │ Recommendation API            │
                    │ Progress API                  │
                    │ Feedback API                  │
                    │ Chat API                     │
                    └───────────────┬───────────────┘
                                    │
                     ┌──────────────┼──────────────┐
                     │              │              │
                     ▼              ▼              ▼
             ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
             │   BUSINESS   │ │ RECOMMENDATION│ │ AI SERVICE   │
             │    LOGIC     │ │    ENGINE     │ │    LAYER     │
             │              │ │              │ │              │
             │ Services     │ │ Candidate Gen │ │ LLM Adapter  │
             │ Validation   │ │ Scoring       │ │ Prompting    │
             │ Orchestration│ │ Ranking       │ │ Extraction   │
             └──────┬───────┘ └──────┬───────┘ │ Explanation  │
                    │                 │         └──────┬───────┘
                    │                 │                │
                    └────────┬────────┴────────────────┘
                             │
                             ▼
                    ┌───────────────────────────────┐
                    │       PATH GENERATION         │
                    │                               │
                    │ Skill Gap Analysis            │
                    │ Prerequisite Resolution       │
                    │ Ordering                      │
                    │ Milestone Generation          │
                    │ Effort Estimation             │
                    └───────────────┬───────────────┘
                                    │
                                    ▼
                    ┌───────────────────────────────┐
                    │          DATA LAYER            │
                    │                               │
                    │ Repository / ORM Layer         │
                    │ Validation                    │
                    │ Persistence                   │
                    └───────────────┬───────────────┘
                                    │
                                    ▼
                    ┌───────────────────────────────┐
                    │           DATABASE            │
                    │                               │
                    │ Users                         │
                    │ Profiles                      │
                    │ Skills                        │
                    │ Resources                     │
                    │ Goals                         │
                    │ Paths                         │
                    │ Progress                      │
                    │ Feedback                      │
                    └───────────────────────────────┘
5. High-Level Component Architecture

PathFinder AI shall contain the following major components.

Component ID	Component	Responsibility
CMP-001	Frontend	User interface
CMP-002	API Gateway/Application API	Backend entry point
CMP-003	Authentication Module	Login, registration, authorization
CMP-004	Profile Module	Learner profile
CMP-005	Goal Module	Learning goals
CMP-006	Skill Module	Skill representation
CMP-007	Resource Module	Learning resources
CMP-008	Skill Gap Engine	Gap identification
CMP-009	Recommendation Engine	Candidate generation and ranking
CMP-010	Prerequisite Engine	Dependency resolution
CMP-011	Path Generator	Personalized roadmap
CMP-012	Progress Module	Progress tracking
CMP-013	Feedback Module	Learner feedback
CMP-014	AI Service Layer	LLM integration
CMP-015	Conversation Module	AI assistant
CMP-016	Dashboard Module	Progress visualization
CMP-017	Data Access Layer	Database interaction
CMP-018	External Resource Integration	External learning resources
CMP-019	Logging/Monitoring	Application observability
6. System Context

The system context describes PathFinder AI and its external actors.

                    ┌──────────────────────┐
                    │      LEARNER         │
                    └──────────┬───────────┘
                               │
                               │
                               ▼
                    ┌──────────────────────┐
                    │    PATHFINDER AI     │
                    │                      │
                    │ Personalized         │
                    │ Learning Assistant    │
                    └───────┬───────┬──────┘
                            │       │
                 ┌──────────┘       └──────────┐
                 ▼                             ▼
        ┌─────────────────┐          ┌──────────────────┐
        │ External AI/LLM │          │ Learning Resource │
        │ Provider        │          │ Providers        │
        └─────────────────┘          └──────────────────┘
7. Primary System Boundary

The following components belong inside the PathFinder AI system boundary:

frontend
backend
authentication
user management
profile management
goal management
skill management
skill-gap analysis
recommendation engine
learning-path generator
progress tracking
feedback processing
dashboard
conversation orchestration
AI abstraction layer
database
logging

The following are external:

third-party AI providers
external learning-resource websites
optional authentication providers
cloud infrastructure
deployment services
8. Frontend Architecture

The frontend shall be responsible for presenting the application to the learner.

The frontend shall not contain authoritative business logic.

The frontend shall communicate with the backend using documented APIs.

8.1 Frontend Responsibilities

The frontend shall handle:

registration interface
login interface
onboarding
learner profile
goal input
skill input
learning path visualization
recommendation display
recommendation explanations
progress dashboard
AI chat interface
feedback controls
loading states
error states
navigation
client-side validation
9. Backend Architecture

The backend shall act as the primary application control layer.

The backend shall:

authenticate users
authorize requests
validate data
execute business logic
access database
call recommendation services
call AI services
generate learning paths
track progress
process feedback
expose APIs

The frontend shall not directly communicate with the database.

The frontend shall not directly communicate with private AI API keys.

10. Backend Layering

The backend shall use the following logical layers:

Request
   │
   ▼
Router / Controller
   │
   ▼
Validation
   │
   ▼
Service Layer
   │
   ├──────────────► Recommendation Engine
   │
   ├──────────────► AI Service
   │
   ├──────────────► Path Generator
   │
   ├──────────────► Progress Service
   │
   └──────────────► Feedback Service
   │
   ▼
Repository / Data Access
   │
   ▼
Database
11. Controller Layer

Controllers or route handlers shall be responsible for:

receiving requests
extracting parameters
validating request structure
invoking services
returning standardized responses

Controllers shall not contain complex recommendation algorithms.

Controllers shall not contain database-heavy business logic.

Controllers shall not contain large AI prompts.

12. Service Layer

The service layer shall contain application business logic.

Examples:

AuthService
ProfileService
GoalService
SkillService
ResourceService
RecommendationService
SkillGapService
PathService
ProgressService
FeedbackService
ChatService

Services may call:

repositories
recommendation engines
AI adapters
path-generation modules
external integrations
13. Repository Layer

The repository layer shall abstract database access.

Example responsibilities:

UserRepository
ProfileRepository
GoalRepository
SkillRepository
ResourceRepository
LearningPathRepository
ProgressRepository
FeedbackRepository
ConversationRepository

Application services should not construct raw database queries unless explicitly required by the selected data-access technology.

14. Database Architecture

The database shall be responsible for persistent application state.

The database shall store:

users
profiles
goals
skills
learner skills
resources
resource skills
prerequisites
learning paths
path items
milestones
progress
feedback
conversations where required

The exact schema will be defined in:

DOC-07 Database Design

15. Data Ownership Principle

Each data entity shall have one authoritative owner.

For example:

User
    ↓
Authentication/Profile module

Goal
    ↓
Goal module

Skill
    ↓
Skill module

Resource
    ↓
Resource module

Learning Path
    ↓
Path module

Progress
    ↓
Progress module

Other modules may read data through defined interfaces but should not directly mutate another module's data without authorization.

16. Authentication Architecture

Authentication shall occur through the backend.

Basic flow:

User
 │
 ▼
Frontend Login
 │
 ▼
POST /auth/login
 │
 ▼
Backend
 │
 ├── Validate credentials
 │
 ├── Verify password hash
 │
 └── Generate authentication token/session
 │
 ▼
Frontend
 │
 ▼
Authenticated Requests

Passwords shall never be returned to the frontend after registration.

Passwords shall never be stored in plaintext.

Authentication implementation details are defined further in:

DOC-12 Authentication and Security

17. Goal Processing Architecture

Goal processing begins when the learner enters a natural-language objective.

Example:

"I want to become a machine learning engineer."

Processing flow:

Learner Goal
     │
     ▼
Frontend
     │
     ▼
Backend Goal API
     │
     ▼
Goal Processing Service
     │
     ▼
AI/NLP Layer
     │
     ├── Target Role
     ├── Domain
     ├── Skills
     └── Learning Objective
     │
     ▼
Structured Goal Representation
     │
     ▼
Skill Requirement Resolution
18. AI Architecture Principle

AI shall be treated as an assisting intelligence layer.

AI shall not be the only source of truth for:

authentication
authorization
database integrity
progress calculation
prerequisite enforcement
deterministic ranking constraints
security decisions

AI may assist with:

natural-language understanding
skill extraction
goal interpretation
recommendation explanations
conversational responses
semantic matching
19. AI Abstraction Layer

The application shall not tightly couple business logic to a single LLM provider.

Recommended architecture:

Application
    │
    ▼
AIService Interface
    │
    ├──────────────► Provider A
    │
    ├──────────────► Provider B
    │
    └──────────────► Local/Fallback Model

The actual provider will be finalized in:

DOC-09 ML/AI Architecture

20. AI Request Flow

Example:

Learner
   │
   ▼
Chat Interface
   │
   ▼
Backend Chat API
   │
   ▼
ChatService
   │
   ▼
Context Builder
   │
   ├── Learner profile
   ├── Active goal
   ├── Skills
   ├── Progress
   └── Current path
   │
   ▼
AI Service Adapter
   │
   ▼
LLM Provider
   │
   ▼
Validated AI Response
   │
   ▼
ChatService
   │
   ▼
Frontend
21. AI Response Validation

AI-generated structured information must be validated before being used by deterministic application logic.

Example:

LLM Output
    │
    ▼
Schema Validation
    │
    ├── Valid
    │     │
    │     ▼
    │   Application
    │
    └── Invalid
          │
          ▼
       Retry/Fallback

The system must not blindly trust arbitrary LLM output.

22. Skill Architecture

Skills are central to PathFinder AI.

A skill may represent:

programming language
framework
technical concept
tool
methodology
domain capability

Example:

Python
NumPy
Pandas
Statistics
Machine Learning
Deep Learning
TensorFlow
Model Deployment

Skills shall be represented using structured identifiers.

23. Skill Relationship Architecture

Skills may have relationships.

Example:

Python
   │
   ▼
NumPy
   │
   ▼
Pandas
   │
   ▼
Data Analysis
   │
   ▼
Machine Learning
   │
   ▼
Deep Learning

Relationships may represent:

prerequisite
dependency
related skill
specialization
alternative skill

The initial MVP shall prioritize prerequisite relationships.

24. Skill Gap Analysis Architecture

Skill-gap analysis shall compare learner capabilities with target requirements.

Flow:

Learner Profile
      │
      ├── Current Skills
      │
      └── Experience
              │
              ▼
        Current Skill Set
              │
              │
Target Goal ──┤
              ▼
       Required Skill Set
              │
              ▼
       Skill Gap Engine
              │
              ▼
       Gap Classification

Possible output:

Skill                  Current   Required   Gap
------------------------------------------------
Python                    3          4       1
Statistics                1          4       3
Machine Learning          0          4       4
Deep Learning             0          3       3
25. Skill Gap Classification

The system may classify gaps as:

NONE
LOW
MEDIUM
HIGH
CRITICAL

The final calculation method will be defined in DOC-09.

26. Resource Architecture

Learning resources shall be represented as structured objects.

Example:

Resource
├── ID
├── Title
├── Description
├── URL
├── Provider
├── Resource Type
├── Difficulty
├── Duration
├── Skills
├── Prerequisites
├── Rating
└── Metadata

Resource types may include:

COURSE
VIDEO
ARTICLE
DOCUMENTATION
PROJECT
ASSESSMENT
BOOK
27. Resource Source Architecture

The MVP should primarily use a controlled resource dataset.

Possible future sources:

Internal Dataset
       │
       ├── Curated Resources
       │
       └── External Resource APIs

The MVP should avoid excessive dependence on live web scraping.

This reduces:

broken URLs
unpredictable response formats
rate limits
API failures
demonstration instability
28. Recommendation Engine Architecture

The recommendation engine shall use a multi-stage process.

Recommended flow:

Learner Context
      │
      ▼
Candidate Generation
      │
      ▼
Candidate Filtering
      │
      ▼
Feature Calculation
      │
      ▼
Scoring
      │
      ▼
Ranking
      │
      ▼
Diversity / Constraint Check
      │
      ▼
Final Recommendations
29. Recommendation Inputs

The recommendation engine may consider:

Learner Profile
Current Skills
Target Goal
Skill Gaps
Experience Level
Learning History
Learning Preferences
Progress
Feedback
Resource Metadata
Prerequisites
30. Recommendation Candidate Generation

Candidate generation shall identify potentially relevant resources.

Candidate sources may include:

Resources matching target skills
Resources matching missing skills
Resources matching learner interests
Resources related to goal
Resources satisfying prerequisites
31. Recommendation Filtering

Candidates may be removed if:

already completed
inappropriate difficulty
prerequisite unavailable
unrelated to goal
incompatible with learner preferences
duplicate resource
invalid resource URL
unavailable resource
32. Recommendation Scoring

A recommendation score may combine multiple factors.

Example conceptual model:

Recommendation Score =
    Goal Alignment
    +
    Skill Gap Alignment
    +
    Difficulty Fit
    +
    Preference Fit
    +
    Prerequisite Fit
    +
    Resource Quality
    +
    Progress Context
    +
    Feedback Signal

The exact mathematical formula shall be finalized in DOC-09.

33. Deterministic Recommendation Principle

Where possible, recommendation ranking shall use deterministic logic.

This provides:

reproducibility
testability
explainability
predictable demos

AI may contribute semantic signals, but the final recommendation pipeline should remain controllable.

34. Recommendation Explanation Architecture

Each recommendation should contain structured explanation information.

Example:

{
  "resource_id": "RES-102",
  "reason_codes": [
    "SKILL_GAP",
    "GOAL_ALIGNMENT",
    "PREREQUISITE_READY"
  ],
  "explanation": "This resource strengthens your Python foundation required before progressing to machine learning."
}

The exact response format will be defined in DOC-08.

35. Prerequisite Engine

The prerequisite engine ensures logical learning order.

Example:

Python
   ↓
NumPy
   ↓
Pandas
   ↓
Statistics
   ↓
Machine Learning

The engine shall prevent a path from placing a dependent skill before its required prerequisite unless an explicit override exists.

36. Prerequisite Resolution Flow
Required Skills
      │
      ▼
Dependency Lookup
      │
      ▼
Dependency Graph
      │
      ▼
Cycle Detection
      │
      ▼
Topological Ordering
      │
      ▼
Learning Sequence
37. Cycle Detection

Prerequisite relationships must not create impossible cycles.

Example of invalid graph:

A → B
B → C
C → A

The system shall detect such cycles.

Invalid prerequisite graphs must not be silently accepted.

38. Learning Path Generator

The learning-path generator combines:

learner state
target goal
skill gaps
resource candidates
prerequisites
recommendations
learning preferences

to produce a structured roadmap.

39. Learning Path Flow
Goal
 │
 ▼
Required Skills
 │
 ▼
Current Skills
 │
 ▼
Skill Gap
 │
 ▼
Prerequisite Graph
 │
 ▼
Candidate Resources
 │
 ▼
Recommendation Ranking
 │
 ▼
Learning Path Generator
 │
 ▼
Milestones
 │
 ▼
Learning Activities
 │
 ▼
Estimated Effort
 │
 ▼
Personalized Roadmap
40. Learning Path Structure

A path should conceptually contain:

Learning Path
│
├── Goal
├── Summary
├── Estimated Duration
│
├── Milestone 1
│   ├── Activity
│   ├── Activity
│   └── Assessment
│
├── Milestone 2
│   ├── Activity
│   ├── Project
│   └── Assessment
│
└── Milestone 3
    ├── Advanced Activity
    ├── Project
    └── Assessment
41. Path Stages

The MVP may use the following generic stages:

Stage 1 — Foundation
Stage 2 — Core Skills
Stage 3 — Applied Skills
Stage 4 — Project Practice
Stage 5 — Assessment

The actual stages should be dynamically adapted when required.

42. Milestone Architecture

Each milestone should contain:

title
description
objective
required skills
learning activities
completion criteria
estimated effort
status
43. Progress Architecture

Progress shall be tracked at multiple levels.

Activity Progress
       │
       ▼
Milestone Progress
       │
       ▼
Path Progress
       │
       ▼
Goal Progress
44. Activity Status

The architecture shall support:

NOT_STARTED
IN_PROGRESS
COMPLETED
SKIPPED
45. Progress Update Flow
Learner
   │
   ▼
Mark Activity Complete
   │
   ▼
Backend
   │
   ▼
Progress Service
   │
   ├── Update Activity
   ├── Recalculate Milestone
   ├── Recalculate Path
   └── Update Skill Progress
   │
   ▼
Dashboard
46. Feedback Architecture

Feedback shall be captured as structured signals.

Examples:

USEFUL
NOT_USEFUL
TOO_EASY
TOO_DIFFICULT
ALREADY_KNOWN
NOT_RELEVANT

Feedback may influence:

recommendation ranking
difficulty selection
resource filtering
future path generation
47. Adaptive Recommendation Flow
Learner State
     │
     ├── Progress
     ├── Feedback
     ├── Completed Resources
     └── Performance
             │
             ▼
      Updated Learner State
             │
             ▼
       Recommendation Engine
             │
             ▼
          Re-ranking
             │
             ▼
      Updated Recommendations
48. Conversational Assistant Architecture

The conversational assistant shall be context-aware.

Example:

Learner:
"What should I learn next?"

        │
        ▼

Conversation Service

        │
        ├── Current Goal
        ├── Current Path
        ├── Current Skills
        ├── Progress
        └── Recent Feedback

        │
        ▼

Recommendation/Path Context

        │
        ▼

AI Service

        │
        ▼

Response
49. Conversation Context

The system should provide the AI with only relevant context.

Possible context:

User Profile Summary
Active Goal
Current Skill Gaps
Current Milestone
Completed Activities
Next Recommended Activity
Recent Feedback

Sensitive or unnecessary data should not be passed to the AI provider.

50. AI Prompt Architecture

Prompts should be centralized.

Recommended structure:

Prompt Templates
│
├── goal_extraction
├── skill_extraction
├── recommendation_explanation
├── path_explanation
├── chatbot_response
└── fallback_response

Prompts shall not be scattered throughout frontend components.

51. AI Fallback Architecture

If the external AI service becomes unavailable:

AI Request
    │
    ▼
AI Provider
    │
    ├── SUCCESS
    │     │
    │     ▼
    │   Response
    │
    └── FAILURE
          │
          ▼
       Fallback
          │
          ├── Deterministic response
          ├── Cached explanation
          └── Controlled error

Core recommendation and path generation should remain functional when possible.

52. Dashboard Architecture

The dashboard shall aggregate information from backend services.

Dashboard sections may include:

Welcome / Goal
       │
       ├── Current Goal
       ├── Overall Progress
       ├── Current Skills
       ├── Skill Gaps
       ├── Current Milestone
       ├── Recommended Next Action
       └── Recent Activity
53. Dashboard Data Flow
Frontend Dashboard
       │
       ├── GET /profile
       ├── GET /goals
       ├── GET /paths/current
       ├── GET /progress
       ├── GET /recommendations
       └── GET /skills
               │
               ▼
            Backend
               │
               ▼
            Services
               │
               ▼
            Database
54. API Architecture

The backend shall expose versioned APIs.

Recommended base:

/api/v1

Example:

/api/v1/auth
/api/v1/users
/api/v1/profile
/api/v1/goals
/api/v1/skills
/api/v1/resources
/api/v1/recommendations
/api/v1/paths
/api/v1/progress
/api/v1/feedback
/api/v1/chat

Exact endpoints will be defined in:

DOC-08 API Contract

55. API Versioning

The initial MVP shall use:

v1

Future breaking API changes should use:

v2

Existing clients should not unexpectedly break because of internal implementation changes.

56. Request Flow

Standard request flow:

HTTP Request
     │
     ▼
API Router
     │
     ▼
Authentication
     │
     ▼
Authorization
     │
     ▼
Validation
     │
     ▼
Controller
     │
     ▼
Service
     │
     ▼
Repository / Engine / AI
     │
     ▼
Response Model
     │
     ▼
HTTP Response
57. Error Response Architecture

API errors shall use a consistent structure.

Conceptual example:

{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid goal input.",
    "details": []
  }
}

Internal stack traces shall not be exposed in production responses.

58. Success Response Architecture

Where appropriate:

{
  "success": true,
  "data": {}
}

The exact API envelope will be finalized in DOC-08.

59. Validation Architecture

Validation shall occur at appropriate boundaries.

Validation levels:

Frontend Validation
        │
        ▼
API Request Validation
        │
        ▼
Business Validation
        │
        ▼
Database Constraints

Frontend validation shall improve user experience but shall not replace backend validation.

60. Security Boundary

The security architecture shall follow:

Internet
   │
   ▼
Frontend
   │
   ▼
HTTPS
   │
   ▼
Backend
   │
   ├── Authentication
   ├── Authorization
   ├── Validation
   ├── Rate Protection
   └── Secret Protection
   │
   ▼
Database
61. Secret Management

Secrets shall be stored in environment configuration.

Examples:

DATABASE_URL
JWT_SECRET
AI_API_KEY
EXTERNAL_API_KEY

Secrets shall never be hardcoded in source code.

Secrets shall never be committed to Git.

62. Environment Separation

The system shall support at least:

Development
Testing
Production

Example:

.env
.env.example

The actual .env file shall remain local.

63. Configuration Architecture

Configuration shall be centralized.

Recommended conceptual structure:

config/
│
├── environment
├── database
├── authentication
├── ai
├── application
└── logging

Exact implementation will be defined in DOC-15.

64. Logging Architecture

The backend should produce structured logs for:

startup
authentication failures
API failures
AI failures
external API failures
database failures
important recommendation operations

Logs must not contain:

passwords
tokens
API keys
sensitive user information
65. Observability

The MVP shall provide basic observability.

Minimum requirements:

Application Logs
Error Logs
Request Errors
AI Errors
Database Errors

Advanced monitoring may be added later.

66. External AI Integration

External AI integration shall occur only through the backend.

Incorrect architecture:

Frontend ─────► AI Provider

Correct architecture:

Frontend
   │
   ▼
Backend
   │
   ▼
AI Adapter
   │
   ▼
AI Provider

This protects API keys and allows provider replacement.

67. External Resource Integration

External learning-resource integrations shall be isolated.

Recommended:

ResourceService
      │
      ▼
ResourceProvider Interface
      │
      ├── Internal Dataset
      ├── Provider A
      └── Provider B

A provider failure shall not crash the entire recommendation system.

68. Caching Strategy

Caching may be introduced for:

resource metadata
static skill data
prerequisite graphs
repeated AI responses where safe

Caching is not mandatory for the initial MVP unless performance testing indicates a need.

69. Recommendation Caching

Recommendation results should not be blindly cached indefinitely because learner state changes.

A recommendation cache may depend on:

user_id
goal_id
profile_version
skill_state_version
path_version
feedback_version

When learner state changes, the cached recommendation may become invalid.

70. Database Transaction Principle

Operations that update multiple related entities should use transactional behavior where supported.

Example:

Complete Activity
      │
      ├── Update Progress
      ├── Update Milestone
      ├── Update Path
      └── Record Event

These operations should not leave inconsistent state.

71. Data Consistency

The system shall maintain consistency between:

Goal
   │
   ▼
Learning Path
   │
   ▼
Path Items
   │
   ▼
Progress

Deleted or invalid entities shall not silently create broken paths.

72. Concurrency Considerations

The MVP should protect against obvious concurrent-update problems.

Example:

Two requests simultaneously mark
the same learning activity complete.

The backend should avoid creating duplicate progress records or inconsistent counters.

73. State Management

The frontend may maintain:

Authentication State
User Profile State
Goal State
Learning Path State
Dashboard State
Chat State

Server-authoritative data shall remain on the backend.

74. Navigation Architecture

Recommended application routes:

/login
/register
/onboarding
/dashboard
/profile
/goals
/goals/:id
/learning-path
/learning-path/:id
/recommendations
/chat
/progress

Exact frontend routes will be finalized in DOC-10.

75. Frontend Component Architecture

Recommended component categories:

components/
│
├── common/
├── layout/
├── auth/
├── onboarding/
├── profile/
├── goals/
├── skills/
├── recommendations/
├── learning-path/
├── progress/
├── dashboard/
└── chat/

Exact folder architecture will be finalized in DOC-13.

76. State Flow

The preferred data direction is:

User Interaction
       │
       ▼
Frontend Event
       │
       ▼
API Request
       │
       ▼
Backend
       │
       ▼
Database / Engine
       │
       ▼
API Response
       │
       ▼
Frontend State
       │
       ▼
UI Update
77. No Direct Database Access from Frontend

The frontend shall never:

Frontend → Database

Instead:

Frontend → Backend API → Database

This is mandatory.

78. No Secret AI Key in Frontend

The frontend shall never contain:

OPENAI_API_KEY
GEMINI_API_KEY
ANTHROPIC_API_KEY
DATABASE_PASSWORD
JWT_SIGNING_SECRET

AI credentials belong to the backend environment.

79. End-to-End Core Workflow

The primary system workflow shall be:

1. User Registration
        ↓
2. Login
        ↓
3. Profile Creation
        ↓
4. Goal Creation
        ↓
5. Current Skills Input
        ↓
6. Goal Understanding
        ↓
7. Required Skill Identification
        ↓
8. Skill Gap Analysis
        ↓
9. Resource Candidate Generation
        ↓
10. Recommendation Ranking
        ↓
11. Prerequisite Resolution
        ↓
12. Learning Path Generation
        ↓
13. Roadmap Display
        ↓
14. Recommendation Explanation
        ↓
15. Learner Starts Activity
        ↓
16. Progress Update
        ↓
17. Feedback
        ↓
18. Recommendation Re-ranking
        ↓
19. Updated Next Action
80. Core Demo Workflow

The HCLTech demonstration shall prioritize the following workflow:

Landing Page
    ↓
Create Account / Demo Login
    ↓
Onboarding
    ↓
Enter Career Goal
    ↓
Enter Current Skills
    ↓
AI Goal Analysis
    ↓
Skill Gap Visualization
    ↓
Generate Personalized Path
    ↓
Show Roadmap
    ↓
Show Why Recommendations Were Made
    ↓
Mark Activity Complete
    ↓
Dashboard Updates
    ↓
Ask AI Assistant
    ↓
Receive Context-Aware Answer

This workflow represents the minimum integrated demonstration.

81. Demo Stability Principle

The competition demo must not depend entirely on unpredictable external services.

The system should maintain a controlled dataset and deterministic fallback behavior.

Recommended:

Primary:
AI + Recommendation Engine

Fallback:
Controlled Dataset + Deterministic Logic
82. MVP Data Strategy

The MVP shall use a curated learning-resource dataset.

The dataset should contain enough resources to demonstrate:

multiple career goals
multiple skills
different difficulty levels
prerequisites
different resource types
multiple learning paths
83. Recommended Initial Demo Domains

The MVP may initially focus on selected technology career paths.

Example:

Data Analyst
Machine Learning Engineer
Full Stack Developer
Data Scientist
Cloud Engineer

The final supported goals should remain manageable for the competition prototype.

84. Architecture for Multiple Career Paths

The system should not hardcode an entire roadmap for only one career.

Instead:

Career Goal
     │
     ▼
Required Skills
     │
     ▼
Skill Graph
     │
     ▼
Resources
     │
     ▼
Personalized Path

This allows the same architecture to support multiple goals.

85. Recommendation Engine Independence

The recommendation engine shall be independently callable.

Conceptual interface:

recommend(
    learner_profile,
    goal,
    skills,
    progress,
    resources
)

It should return structured recommendations.

This makes it easier to test recommendation quality independently of the frontend.

86. Skill Gap Engine Independence

The skill-gap engine shall be independently testable.

Conceptual interface:

analyze_skill_gap(
    current_skills,
    required_skills
)

Output:

skill
current_level
required_level
gap
severity
87. Path Generator Independence

The path generator shall be independently testable.

Conceptual interface:

generate_path(
    goal,
    skill_gaps,
    resources,
    prerequisites
)

Output:

path
milestones
activities
estimated_effort
88. AI Service Independence

The AI service shall be independently replaceable.

Conceptual interface:

extract_goal()
extract_skills()
explain_recommendation()
answer_question()

This prevents provider-specific code from spreading across the application.

89. Dependency Direction

Dependencies should generally point inward toward business logic.

Recommended:

Frontend
   ↓
API
   ↓
Services
   ↓
Domain / Engines
   ↓
Repositories
   ↓
Database

External providers should be accessed through adapters.

90. Dependency Inversion

Business logic should not depend directly on a specific external provider.

Incorrect:

RecommendationService
       ↓
SpecificLLMProvider

Preferred:

RecommendationService
       ↓
AIService Interface
       ↓
Provider Adapter
91. Module Communication

Modules should communicate through defined service interfaces.

Example:

GoalService
     ↓
SkillGapService
     ↓
RecommendationService
     ↓
PathService

Modules should avoid uncontrolled circular dependencies.

92. Circular Dependency Prevention

The architecture shall avoid:

A → B
B → C
C → A

Dependencies should follow a predictable direction.

93. Background Processing

The MVP should keep most operations synchronous for simplicity.

Potential future background jobs:

Resource ingestion
Recommendation precomputation
Analytics processing
Large AI requests
Dataset updates

These are not mandatory for the initial prototype.

94. API and AI Timeout Strategy

External AI calls shall have explicit timeout handling.

Example conceptual flow:

AI Request
   │
   ▼
Timeout
   │
   ├── Response received → Continue
   │
   └── Timeout → Fallback

The application must not hang indefinitely waiting for an external provider.

95. Retry Strategy

Retries may be used for transient external failures.

Retries shall:

have a maximum attempt count
use controlled delays
avoid infinite loops

Authentication failures and invalid requests should generally not be retried.

96. Rate Limiting

The architecture should allow rate limiting for:

login attempts
chat requests
AI requests
expensive recommendation operations

Exact rate limits will be finalized in security documentation.

97. File Storage

If the application requires uploaded profile documents or learning materials in the future, file storage should be separated from the relational application database.

The MVP does not require user file uploads unless explicitly approved.

98. Search Architecture

The MVP may use database filtering and structured search.

Future versions may introduce:

Vector Database
Semantic Search
Embedding Search
Hybrid Search

These are not mandatory for the first prototype.

99. Semantic Search Architecture

If semantic search is introduced:

Resource
   │
   ▼
Embedding Model
   │
   ▼
Vector Representation
   │
   ▼
Vector Store
   │
   ▼
Semantic Query
   │
   ▼
Candidate Resources

This architecture shall be documented in DOC-09 if implemented.

100. Analytics Architecture

The MVP may collect basic product metrics.

Potential metrics:

Goals Created
Paths Generated
Resources Completed
Recommendations Accepted
Recommendation Rejections
Feedback Signals
Chat Questions

Analytics should not interfere with core workflows.

101. Privacy Architecture

Only information necessary for personalization should be processed.

AI providers should receive minimum necessary context.

Sensitive information should not be included in prompts unless explicitly required.

102. AI Data Minimization

The AI context should preferably use a summarized learner representation.

Example:

Learner:
Experience: Intermediate
Goal: Machine Learning Engineer
Skills:
- Python: 4
- Statistics: 2
- ML: 1

Current Gap:
- Statistics
- Machine Learning

rather than sending unnecessary personal information.

103. Error Isolation

A failure in one subsystem should not unnecessarily crash the entire application.

Example:

AI Failure
   ↓
Recommendation Engine still works

External Resource Failure
   ↓
Other resources still work

Analytics Failure
   ↓
Core learning flow still works
104. Failure Domains

Major failure domains:

Frontend
Backend
Database
AI Provider
External Resource Provider
Network
Authentication

Each shall have controlled error handling.

105. Database Failure Behavior

If the database becomes unavailable:

Request
   ↓
Database Failure
   ↓
Controlled Backend Error
   ↓
User-Friendly Response

The system must not expose database credentials or internal stack traces.

106. AI Failure Behavior

If the AI provider becomes unavailable:

AI Failure
   ↓
Fallback
   ↓
Deterministic Recommendation
   ↓
User continues learning

Where a conversational answer cannot be generated, the UI should clearly communicate the limitation.

107. External Resource Failure

If an external resource URL becomes unavailable:

Invalid Resource
     ↓
Resource Validation
     ↓
Mark Resource Unavailable
     ↓
Remove/Replace Candidate

The entire learning path should not fail.

108. Security Architecture Summary

Security controls include:

HTTPS
Password Hashing
Authentication
Authorization
Input Validation
Secret Management
CORS Configuration
Rate Limiting
Secure Cookies/Tokens
Error Sanitization
Dependency Updates

Exact implementation is defined in DOC-12.

109. CORS Architecture

CORS shall allow only approved frontend origins.

Development may use:

http://localhost:<frontend-port>

Production shall use the deployed frontend domain.

Wildcard origins should not be used in production when credentials are involved.

110. API Security

Protected endpoints shall require authentication.

Public endpoints may include:

/register
/login
/health

Protected endpoints may include:

/profile
/goals
/paths
/progress
/recommendations
/chat
/feedback

Exact endpoint classification will be defined in DOC-08.

111. Health Check Architecture

The backend should expose a basic health endpoint.

Conceptual:

GET /health

It may return:

{
  "status": "ok"
}

A deeper health endpoint may verify database connectivity.

112. Deployment Architecture

The deployment architecture should separate:

Frontend
Backend
Database
AI Provider

Conceptual production structure:

                 INTERNET
                    │
             ┌──────┴──────┐
             ▼             ▼
        Frontend         Backend
             │             │
             │             ├──────► AI Provider
             │             │
             │             └──────► Database
             │
             └──────── HTTPS
113. Local Development Architecture

Developers should be able to run:

Frontend
Backend
Database

locally.

Conceptual:

localhost
│
├── frontend
│
├── backend
│
└── database

The exact commands will be defined in DOC-15.

114. Containerization

Docker may be used to simplify reproducible environments.

Possible future structure:

docker-compose
│
├── frontend
├── backend
└── database

Docker is optional for the MVP if local setup remains reproducible without it.

115. Source Code Repository Architecture

The repository shall contain:

HCL_PathFinder/
│
├── README.md
│
├── docs/
│
├── frontend/
│
├── backend/
│
├── data/
│
├── tests/
│
├── scripts/
│
├── .gitignore
│
├── .env.example
│
└── LICENSE

The exact implementation folder structure will be finalized in DOC-13.

116. Documentation Architecture

Documentation shall remain under:

docs/

Current documents:

01-project-charter.md
02-software-requirements-specification.md
03-user-roles-and-journeys.md
04-use-cases-and-user-stories.md
05-mvp-scope-and-acceptance-criteria.md
06-system-architecture.md
07-database-design.md
08-api-contract.md
09-ml-ai-architecture.md
10-frontend-architecture.md
11-backend-architecture.md
12-authentication-and-security.md
13-code-and-folder-architecture.md
14-technology-stack-and-dependencies.md
15-environment-and-configuration.md
16-git-github-development-workflow.md
17-github-task-breakdown.md
18-testing-and-qa-strategy.md
19-deployment-architecture.md
20-integration-checklist.md
21-master-ai-implementation-spec.md
22-architecture-decision-records.md
23-requirements-traceability-matrix.md
117. Architecture Dependency Map

The documentation dependency chain is:

DOC-01
Project Charter
   │
   ▼
DOC-02
Software Requirements
   │
   ▼
DOC-03
User Roles & Journeys
   │
   ▼
DOC-04
Use Cases & User Stories
   │
   ▼
DOC-05
MVP Scope & Acceptance
   │
   ▼
DOC-06
System Architecture
   │
   ├───────────────┬───────────────┬───────────────┐
   ▼               ▼               ▼               ▼
DOC-07          DOC-08          DOC-09          DOC-10
Database        API             AI/ML           Frontend
   │               │               │               │
   └───────────────┴───────┬───────┴───────────────┘
                           ▼
                         DOC-11
                         Backend
                           │
                           ▼
                         DOC-12
                    Security/Auth
                           │
                           ▼
                         DOC-13
                  Code/Folder Architecture
                           │
                           ▼
                         DOC-14
                Technology & Dependencies
                           │
                           ▼
                         DOC-15
              Environment & Configuration
                           │
                           ▼
                         DOC-16
                Git/GitHub Development
                           │
                           ▼
                         DOC-17
                    GitHub Tasks
                           │
                           ▼
                         DOC-18
                     Testing / QA
                           │
                           ▼
                         DOC-19
                       Deployment
                           │
                           ▼
                         DOC-20
                  Integration Checklist
                           │
                           ▼
                         DOC-21
                Master AI Implementation
                           │
                           ▼
                         DOC-22
                   Architecture Decisions
                           │
                           ▼
                         DOC-23
                Requirements Traceability
118. Architecture-to-Requirement Mapping
Architecture Component	Requirement Coverage
Frontend	FR-016, FR-063–FR-067, UX-001–UX-006
Authentication	FR-001–FR-004, SEC-001–SEC-004
Profile	FR-005–FR-011
Goal	FR-012–FR-015
Chat	FR-016–FR-019, AI-001, AI-007
Skill Engine	FR-020–FR-024, AI-004, AI-005
Resource Module	FR-025–FR-028
Recommendation Engine	FR-029–FR-035, AI-003, AI-006
Prerequisite Engine	FR-036–FR-039
Path Generator	FR-040–FR-046
Explainability	FR-047–FR-050, AI-007
Progress	FR-051–FR-055
Feedback	FR-056–FR-058
Adaptation	FR-059–FR-062, AI-008
Dashboard	FR-063–FR-067
Security	SEC-001–SEC-006
Integration	INT-001–INT-004
Quality	NFR-001–NFR-008
119. Architecture-to-Document Mapping
Architecture Area	Primary Document
Requirements	DOC-02
User journeys	DOC-03
Use cases	DOC-04
MVP	DOC-05
System architecture	DOC-06
Database	DOC-07
API	DOC-08
AI/ML	DOC-09
Frontend	DOC-10
Backend	DOC-11
Security	DOC-12
Code structure	DOC-13
Dependencies	DOC-14
Environment	DOC-15
Git/GitHub	DOC-16
Tasks	DOC-17
Testing	DOC-18
Deployment	DOC-19
Integration	DOC-20
AI implementation	DOC-21
Architecture decisions	DOC-22
Traceability	DOC-23
120. Integration Contract Principle

No module should be implemented in isolation without considering its integration contract.

For every module, the following must be defined before implementation:

Inputs
Outputs
Dependencies
Errors
Data Models
API Contract
Authentication
Testing
121. Module Contract Example

Recommendation Engine:

INPUT
    learner_id
    goal_id
    skill_state
    progress
    feedback

DEPENDENCIES
    Skill Service
    Resource Repository
    Goal Service

OUTPUT
    ranked recommendations

ERRORS
    invalid goal
    insufficient resource data
    unavailable AI semantic service

TESTING
    unit tests
    ranking tests
    edge-case tests
122. Integration Testing Principle

Integration testing shall verify:

Frontend
   ↕
Backend
   ↕
Database
   ↕
Recommendation Engine
   ↕
AI Layer

Individual unit tests are not sufficient.

The complete end-to-end learner workflow must also be tested.

123. Architecture Freeze

The architecture shall not be considered fully frozen until:

DOC-07 is approved
DOC-08 is approved
DOC-09 is approved
DOC-10 is approved
DOC-11 is approved
DOC-12 is approved
DOC-13 is approved
DOC-14 is approved

Changes after freeze must be documented through DOC-22.

124. Architecture Change Process

If a team member proposes an architectural change:

Identify Problem
      ↓
Propose Change
      ↓
Evaluate Impact
      ↓
Check Requirements
      ↓
Check Database
      ↓
Check API
      ↓
Check Frontend
      ↓
Check Backend
      ↓
Check Testing
      ↓
Record ADR
      ↓
Update Affected Documents
      ↓
Implement
125. Architecture Decision Record Requirement

Any significant architectural decision shall be recorded in:

docs/22-architecture-decision-records.md

Examples:

database technology
authentication mechanism
AI provider
embedding model
vector database
deployment provider
frontend framework
backend framework
API architecture
caching strategy
126. Team Development Boundaries

To minimize Git conflicts, team members should work within defined areas.

Example:

Member A
Frontend

Member B
Backend / API

Member C
Database / Data

Member D
AI / Recommendation

Member E
Testing / Integration / Deployment

The exact ownership will be defined in DOC-17.

127. Git Integration Principle

Team members shall not modify the same files unnecessarily.

Feature branches should be used.

Example:

main
 │
 ├── feature/frontend-dashboard
 ├── feature/backend-auth
 ├── feature/recommendation-engine
 ├── feature/database-schema
 └── feature/testing
128. Pull Request Principle

Code should be merged into main through reviewed changes where practical.

A pull request should contain:

Feature
Requirements addressed
Tests
Integration impact
Screenshots where relevant
Known limitations
129. Definition of Architecture-Ready

The architecture is implementation-ready when:

component boundaries are defined
major data flows are defined
frontend/backend boundaries are defined
AI boundaries are defined
database boundary is defined
external integrations are defined
failure behavior is defined
security boundaries are defined
documentation dependencies are established
module responsibilities are defined
integration principles are established
130. Architecture Risks
RISK-ARCH-001 — Overengineering

Risk:

The team builds a complex enterprise architecture unsuitable for the competition timeline.

Mitigation:

Use modular architecture without unnecessary microservices.

RISK-ARCH-002 — AI Overdependence

Risk:

The system fails when the AI provider is unavailable.

Mitigation:

Use deterministic recommendation and fallback logic.

RISK-ARCH-003 — Integration Conflicts

Risk:

Frontend and backend implementations use incompatible assumptions.

Mitigation:

Freeze API contracts before implementation.

RISK-ARCH-004 — Database/API Mismatch

Risk:

Frontend expects fields that do not exist in the database.

Mitigation:

Define canonical data models in DOC-07 and API schemas in DOC-08.

RISK-ARCH-005 — Unstable External Resources

Risk:

External resource websites or APIs fail during the demo.

Mitigation:

Use a controlled curated dataset for MVP.

RISK-ARCH-006 — LLM Hallucination

Risk:

AI generates invalid skills, resources, or learning sequences.

Mitigation:

Validate AI output and use deterministic application logic.

RISK-ARCH-007 — Scope Expansion

Risk:

The team attempts to support too many career paths.

Mitigation:

Focus on a controlled MVP dataset and expand only after the core journey works.

131. Architecture Quality Attributes

The architecture prioritizes:

1. Correctness
2. Integration reliability
3. Maintainability
4. Testability
5. Explainability
6. Security
7. Demonstrability
8. Performance
9. Scalability

For the competition MVP, correctness and integration reliability take priority over unnecessary scalability.

132. Architecture Tradeoffs

The architecture intentionally chooses:

Modular Monolith
over
Microservices

because the project is a prototype with a small development team and limited implementation time.

It chooses:

Controlled Dataset
over
Heavy Live Scraping

for demo reliability.

It chooses:

Hybrid AI + Deterministic Logic
over
AI-only Recommendation

for reliability and explainability.

It chooses:

REST APIs
over
Complex Event-Driven Architecture

for implementation simplicity.

133. Why Not Microservices

Microservices would introduce additional:

deployment complexity
network failures
service discovery
monitoring
CI/CD complexity
debugging overhead

The PathFinder MVP does not require this complexity.

A modular monolith provides sufficient separation while remaining easy to develop and deploy.

134. Why Hybrid AI

AI is valuable for:

natural language
semantic interpretation
explanations
conversational assistance

Deterministic logic is better for:

prerequisite validation
progress calculations
authentication
authorization
ranking constraints
database integrity

Therefore:

AI
+
Deterministic Algorithms
=
Reliable Personalized Learning System
135. Architecture Success Criteria

The architecture will be considered successful if the team can demonstrate:

User
 ↓
Goal
 ↓
Profile
 ↓
Skills
 ↓
Skill Gap
 ↓
Recommendations
 ↓
Learning Path
 ↓
Explanation
 ↓
Progress
 ↓
Feedback
 ↓
Updated Recommendation

without manual database manipulation during the normal demo workflow.

136. Final MVP Architecture

The final MVP architecture is:

                           USER
                            │
                            ▼
                    ┌───────────────┐
                    │   FRONTEND    │
                    │               │
                    │ React Web UI  │
                    └───────┬───────┘
                            │
                         HTTPS
                            │
                            ▼
                    ┌───────────────┐
                    │   BACKEND     │
                    │               │
                    │ REST API      │
                    │ Auth          │
                    │ Services      │
                    └───────┬───────┘
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
          ▼                 ▼                 ▼
   ┌────────────┐   ┌──────────────┐   ┌────────────┐
   │ Skill Gap  │   │Recommendation│   │ AI Service │
   │ Engine     │   │ Engine       │   │ Adapter    │
   └──────┬─────┘   └──────┬───────┘   └─────┬──────┘
          │                │                  │
          └────────────────┼──────────────────┘
                           ▼
                  ┌─────────────────┐
                  │ Path Generator  │
                  │                 │
                  │ Prerequisites   │
                  │ Milestones      │
                  │ Ordering        │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ Data Access     │
                  │ Layer           │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │   DATABASE      │
                  │                 │
                  │ Users           │
                  │ Profiles        │
                  │ Goals           │
                  │ Skills          │
                  │ Resources       │
                  │ Paths           │
                  │ Progress        │
                  │ Feedback        │
                  └─────────────────┘

                           │
                           │
                  ┌────────▼────────┐
                  │ External AI/LLM │
                  │ Provider        │
                  └─────────────────┘
137. Implementation Order

Implementation shall follow this architectural order:

PHASE 1
Repository + Environment
        ↓
PHASE 2
Database + Data Models
        ↓
PHASE 3
Backend Foundation
        ↓
PHASE 4
Authentication
        ↓
PHASE 5
Profile + Goals + Skills
        ↓
PHASE 6
Skill Gap Engine
        ↓
PHASE 7
Resource Dataset + Resource Service
        ↓
PHASE 8
Recommendation Engine
        ↓
PHASE 9
Prerequisite Engine
        ↓
PHASE 10
Learning Path Generator
        ↓
PHASE 11
Frontend Foundation
        ↓
PHASE 12
Dashboard + Learning Path UI
        ↓
PHASE 13
AI Integration
        ↓
PHASE 14
Chat Assistant
        ↓
PHASE 15
Progress + Feedback
        ↓
PHASE 16
Adaptive Recommendation
        ↓
PHASE 17
Testing
        ↓
PHASE 18
Integration
        ↓
PHASE 19
Deployment
        ↓
PHASE 20
Demo Preparation
138. Important Implementation Rule

The team shall not begin writing production application code until the dependent architectural documents are sufficiently defined.

At minimum, before feature implementation:

DOC-06
System Architecture

DOC-07
Database Design

DOC-08
API Contract

DOC-09
ML/AI Architecture

DOC-10
Frontend Architecture

DOC-11
Backend Architecture

DOC-12
Authentication & Security

DOC-13
Code & Folder Architecture

DOC-14
Technology Stack

DOC-15
Environment Configuration

must be reviewed.

139. Integration-First Development Principle

The project shall be developed with integration in mind from the beginning.

Each feature must define:

Frontend
   ↕
API
   ↕
Service
   ↕
Database

where applicable.

AI features must additionally define:

Service
   ↕
AI Adapter
   ↕
AI Provider
140. No Hidden Contracts

No developer should create undocumented assumptions such as:

Frontend assumes field = "userName"
Backend returns field = "username"

or:

Frontend expects:
progress = 0.75

Backend returns:
progress = 75

All contracts must be documented.

141. Canonical Data Principle

When multiple modules need the same information, one canonical representation shall be established.

Examples:

User ID
Goal ID
Skill ID
Resource ID
Path ID
Milestone ID
Activity ID

IDs shall remain consistent across:

frontend
backend
database
APIs
tests
documentation
142. Canonical Naming Principle

The same entity should use the same terminology across all documents.

For example:

Learning Path
not alternately:
Roadmap
Course Plan
Learning Journey

"Learning Path" shall be the canonical system entity.

Similarly:

Learning Resource
Learner
Goal
Skill
Milestone
Activity
Recommendation
Feedback

shall be used consistently.

143. Documentation Consistency Rule

If an architecture decision changes:

DOC-06
DOC-07
DOC-08
DOC-09
DOC-10
DOC-11
DOC-13
DOC-14
DOC-15
DOC-17
DOC-18
DOC-19
DOC-20
DOC-21
DOC-22
DOC-23

must be reviewed for impact.

Only affected documents need modification, but the impact must be recorded.

144. Architecture Approval Checklist

Before implementation, the team should confirm:

 System components identified
 Frontend boundary defined
 Backend boundary defined
 Database boundary defined
 AI boundary defined
 External integration boundary defined
 Data flow defined
 Authentication flow defined
 Recommendation flow defined
 Skill-gap flow defined
 Path-generation flow defined
 Progress flow defined
 Feedback flow defined
 Error handling defined
 Security boundaries defined
 Deployment direction defined
 Team ownership can be assigned
 API contract can be created
 Database schema can be created
 AI architecture can be created
 Frontend architecture can be created
 Backend architecture can be created
145. Architecture Status

Document: DOC-06
Status: DRAFT
Architecture: PROPOSED
Implementation: NOT STARTED
Database: NOT FROZEN
API: NOT FROZEN
AI Architecture: NOT FROZEN
Frontend Architecture: NOT FROZEN
Backend Architecture: NOT FROZEN
Security Architecture: PROPOSED

146. Next Document

The next document is:

DOC-07 — Database Design

DOC-07 shall define:

complete entity model
database choice
tables/collections
primary keys
foreign keys
indexes
relationships
constraints
enums
skill representation
prerequisite representation
learning resources
goals
learning paths
milestones
activities
progress
feedback
conversations
timestamps
audit fields
seed data
migration strategy
database validation
database-to-API mapping

DOC-07 must be completed before finalizing DOC-08 API Contract.

147. Final Architecture Statement

PathFinder AI shall be implemented as a modular, integration-first personalized learning platform.

The architecture combines:

Structured Learner Data
        +
Skill Intelligence
        +
Deterministic Recommendation
        +
Prerequisite Reasoning
        +
AI/Natural Language
        +
Personalized Learning Path
        +
Progress Tracking
        +
Feedback

to produce:

PERSONALIZED
LEARNING
JOURNEYS

The architecture intentionally avoids unnecessary complexity while preserving clear boundaries between the user interface, application logic, recommendation intelligence, AI services, data persistence, and external integrations.

The architecture is designed specifically so that each team member can implement an assigned module independently while maintaining clearly defined integration contracts.

No production implementation should begin without reviewing the dependent architecture and contract documents.