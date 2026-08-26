# PathFinder AI — Code and Folder Architecture

**Document ID:** DOC-13  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT  
**Parent Documents:** DOC-01, DOC-02, DOC-03, DOC-04, DOC-05, DOC-06, DOC-07, DOC-08, DOC-09, DOC-10, DOC-11, DOC-12  
**Next Documents:** DOC-14 Technology Stack and Dependencies, DOC-15 Environment and Configuration  
**Implementation Status:** NOT STARTED  
**Architecture Status:** CONTROLLED / NOT YET FROZEN

---

# 1. Purpose

This document defines the physical codebase structure of PathFinder AI.

The purpose of this document is to ensure that:

1. every implementation component has a known location;
2. frontend and backend responsibilities remain separated;
3. AI/ML logic remains modular;
4. database access is isolated;
5. API contracts can be implemented consistently;
6. authentication and security logic is not duplicated;
7. tests have predictable locations;
8. configuration and secrets remain outside source code;
9. multiple team members can work independently;
10. future AI-assisted code generation can follow one consistent structure.

This document is binding for implementation unless an Architecture Decision Record explicitly changes it.

---

# 2. Architectural Principles

The codebase shall follow these principles.

## 2.1 Separation of Concerns

Frontend, backend, AI, database, infrastructure, and documentation shall remain logically separated.

## 2.2 Single Responsibility

Each module should have one primary responsibility.

## 2.3 API Boundary

Frontend code shall communicate with backend functionality through documented API contracts.

Frontend components shall not directly access the database.

## 2.4 Service Boundary

Business logic shall remain outside HTTP route handlers wherever practical.

## 2.5 AI Abstraction

AI providers shall be accessed through an abstraction layer.

Frontend components shall never directly call an external LLM provider.

## 2.6 Database Abstraction

Database operations shall be centralized through repositories/data-access modules.

## 2.7 Configuration Separation

Environment-specific values shall be provided through environment variables or configuration files.

Secrets shall never be hardcoded.

## 2.8 Testability

Business logic should be testable independently of HTTP, UI, external APIs, and production databases where practical.

## 2.9 Traceability

Implementation modules must be traceable to requirements in DOC-02 and the Requirements Traceability Matrix in DOC-23.

## 2.10 Controlled Dependencies

A new dependency should not be introduced without confirming:

- purpose;
- compatibility;
- security implications;
- license;
- maintenance status;
- whether an existing dependency already solves the problem.

---

# 3. Repository Structure

The target repository structure is:

```text
HCL_PathFinder/
│
├── README.md
├── .gitignore
├── .env.example
├── docker-compose.yml
├── LICENSE
│
├── docs/
│   ├── 01-project-charter.md
│   ├── 02-software-requirements-specification.md
│   ├── 03-user-roles-and-journeys.md
│   ├── 04-use-cases-and-user-stories.md
│   ├── 05-mvp-scope-and-acceptance-criteria.md
│   ├── 06-system-architecture.md
│   ├── 07-database-design.md
│   ├── 08-api-contract.md
│   ├── 09-ml-ai-architecture.md
│   ├── 10-frontend-architecture.md
│   ├── 11-backend-architecture.md
│   ├── 12-authentication-and-security.md
│   ├── 13-code-and-folder-architecture.md
│   ├── 14-technology-stack-and-dependencies.md
│   ├── 15-environment-and-configuration.md
│   ├── 16-git-github-development-workflow.md
│   ├── 17-github-task-breakdown.md
│   ├── 18-testing-and-qa-strategy.md
│   ├── 19-deployment-architecture.md
│   ├── 20-integration-checklist.md
│   ├── 21-master-ai-implementation-spec.md
│   ├── 22-architecture-decision-records.md
│   └── 23-requirements-traceability-matrix.md
│
├── frontend/
│   ├── package.json
│   ├── package-lock.json
│   ├── vite.config.js
│   ├── index.html
│   ├── public/
│   └── src/
│       ├── main.jsx
│       ├── App.jsx
│       ├── routes/
│       ├── pages/
│       ├── components/
│       ├── layouts/
│       ├── features/
│       ├── services/
│       ├── hooks/
│       ├── context/
│       ├── utils/
│       ├── constants/
│       ├── schemas/
│       ├── assets/
│       └── styles/
│
├── backend/
│   ├── requirements.txt
│   ├── .env.example
│   ├── app/
│   │   ├── main.py
│   │   ├── config/
│   │   ├── api/
│   │   ├── core/
│   │   ├── models/
│   │   ├── schemas/
│   │   ├── repositories/
│   │   ├── services/
│   │   ├── ai/
│   │   ├── recommendation/
│   │   ├── path_generator/
│   │   ├── integrations/
│   │   ├── utils/
│   │   └── middleware/
│   │
│   ├── tests/
│   │   ├── unit/
│   │   ├── integration/
│   │   └── e2e/
│   │
│   └── scripts/
│
├── data/
│   ├── raw/
│   ├── processed/
│   ├── seed/
│   └── README.md
│
├── ml/
│   ├── notebooks/
│   ├── experiments/
│   ├── models/
│   ├── embeddings/
│   ├── evaluation/
│   └── README.md
│
├── tests/
│   ├── api/
│   ├── integration/
│   └── fixtures/
│
└── deployment/
    ├── docker/
    ├── nginx/
    └── README.md
4. Repository-Level Responsibilities
4.1 docs/

Contains project specifications.

No application runtime code should be placed in this directory.

4.2 frontend/

Contains the complete learner-facing web application.

4.3 backend/

Contains API, business logic, authentication, database access, recommendation orchestration, AI orchestration, and integrations.

4.4 data/

Contains controlled datasets and seed data.

Sensitive production data must never be committed.

4.5 ml/

Contains experimentation and model-development artifacts.

Production backend code must not depend directly on exploratory notebooks.

4.6 tests/

Contains cross-module tests where they cannot logically belong inside backend or frontend.

4.7 deployment/

Contains deployment-specific configuration.

5. Frontend Architecture

The frontend shall use a feature-oriented structure while maintaining shared infrastructure.

frontend/
└── src/
    ├── main.jsx
    ├── App.jsx
    │
    ├── routes/
    │
    ├── pages/
    │
    ├── components/
    │
    ├── layouts/
    │
    ├── features/
    │
    ├── services/
    │
    ├── hooks/
    │
    ├── context/
    │
    ├── utils/
    │
    ├── constants/
    │
    ├── schemas/
    │
    ├── assets/
    │
    └── styles/
6. Frontend Entry Point
6.1 main.jsx

Responsibilities:

initialize React;
mount application;
initialize global providers;
initialize router;
initialize global state providers where required.

It shall not contain business logic.

Example responsibility:

Browser
   ↓
main.jsx
   ↓
App.jsx
   ↓
Router
   ↓
Page
7. App.jsx

Responsibilities:

define top-level application structure;
register routes;
configure application-level providers.

It should not contain recommendation logic or database logic.

8. Frontend Routes

Directory:

frontend/src/routes/

Responsibilities:

route definitions;
protected route handling;
public route handling;
route-level access control.

Potential routes:

/
 /login
 /register
 /onboarding
 /dashboard
 /profile
 /goals
 /goals/:goalId
 /learning-path/:pathId
 /skills
 /resources
 /assistant
 /progress
 /settings

Final routes must remain consistent with DOC-10 and DOC-08.

9. Frontend Pages

Directory:

frontend/src/pages/

Suggested structure:

pages/
├── LandingPage/
├── LoginPage/
├── RegisterPage/
├── OnboardingPage/
├── DashboardPage/
├── ProfilePage/
├── GoalsPage/
├── GoalDetailsPage/
├── LearningPathPage/
├── SkillsPage/
├── ResourcesPage/
├── AssistantPage/
├── ProgressPage/
├── SettingsPage/
└── NotFoundPage/

Pages are responsible for composing components and invoking feature-level services.

Pages shall not contain complex business logic.

10. Frontend Components

Directory:

frontend/src/components/

Shared components include:

components/
├── common/
├── forms/
├── navigation/
├── feedback/
├── charts/
├── cards/
├── modals/
├── loading/
└── errors/

Examples:

Button
Input
Select
Modal
Card
ProgressBar
SkillBadge
ResourceCard
RecommendationCard
MilestoneCard
LoadingSpinner
ErrorMessage
11. Frontend Layouts

Directory:

frontend/src/layouts/

Potential layouts:

layouts/
├── PublicLayout.jsx
├── AuthLayout.jsx
└── DashboardLayout.jsx

Layouts control common page structure.

12. Frontend Features

Directory:

frontend/src/features/

Feature modules shall correspond to business capabilities.

Recommended structure:

features/
├── auth/
├── onboarding/
├── profile/
├── goals/
├── skills/
├── recommendations/
├── learningPath/
├── progress/
├── feedback/
├── assistant/
└── dashboard/

Each feature may contain:

feature/
├── components/
├── hooks/
├── services/
├── schemas/
├── utils/
└── index.js
13. Authentication Frontend
features/auth/
├── components/
├── hooks/
├── services/
├── schemas/
└── index.js

Responsibilities:

login;
registration;
logout;
authentication state;
token/session handling;
protected routes.

Authentication behavior must remain consistent with DOC-12.

14. Onboarding Frontend

Responsibilities:

collect learner profile;
collect experience;
collect interests;
collect existing skills;
collect learning preferences;
collect initial goal.

Flow:

Register
   ↓
Onboarding
   ↓
Profile
   ↓
Goal
   ↓
Skills
   ↓
Generate Path
   ↓
Dashboard
15. Goal Feature
features/goals/

Responsibilities:

create goal;
update goal;
list goals;
activate goal;
pause goal;
complete goal;
archive goal.
16. Skill Feature

Responsibilities:

display current skills;
add skills;
edit proficiency;
display required skills;
display skill gaps.

The frontend shall consume backend skill-gap results rather than independently implementing the authoritative skill-gap algorithm.

17. Recommendation Feature

Responsibilities:

display recommended resources;
display recommendation score where appropriate;
display recommendation reason;
allow feedback;
allow refresh/re-ranking.

The frontend shall not calculate authoritative recommendation rankings.

18. Learning Path Feature

Responsibilities:

display roadmap;
display stages;
display milestones;
display activities;
display prerequisites;
show estimated effort;
identify next action;
track completion.

Example:

Learning Path
│
├── Stage 1 — Foundation
│   ├── Activity
│   └── Activity
│
├── Stage 2 — Core Skills
│   ├── Activity
│   └── Activity
│
├── Stage 3 — Applied Skills
│
├── Stage 4 — Project
│
└── Stage 5 — Assessment
19. Assistant Feature

Responsibilities:

chat interface;
conversation history;
learner-context display where appropriate;
recommendation questions;
path questions;
skill questions;
progress questions.

The frontend assistant calls backend assistant APIs.

It does not call the LLM provider directly.

20. Dashboard Feature

The dashboard shall aggregate:

active goal;
overall path progress;
current milestone;
skill-gap summary;
next recommendation;
recent activity;
feedback-based updates.
21. Frontend Services

Directory:

frontend/src/services/

Suggested:

services/
├── apiClient.js
├── authService.js
├── profileService.js
├── goalService.js
├── skillService.js
├── recommendationService.js
├── learningPathService.js
├── progressService.js
├── feedbackService.js
└── assistantService.js

The exact implementation shall follow DOC-08.

22. API Client

apiClient.js shall centralize:

base URL;
HTTP configuration;
authorization headers;
common error handling;
request configuration.

Feature services shall use the API client.

Example:

Component
   ↓
Feature Hook
   ↓
Feature Service
   ↓
apiClient
   ↓
Backend API
23. Frontend Hooks

Directory:

frontend/src/hooks/

Hooks may manage:

authentication state;
API loading;
API errors;
goal data;
recommendations;
learning path;
progress;
assistant conversations.

Hooks shall not duplicate backend business rules.

24. Frontend Context

Directory:

frontend/src/context/

Potential contexts:

AuthContext
NotificationContext
ThemeContext

Only truly global state should be stored here.

25. Frontend Schemas

Directory:

frontend/src/schemas/

Responsibilities:

form validation;
request validation where appropriate;
response shape assumptions.

Schemas must remain consistent with DOC-08 API contracts.

26. Frontend Utilities

Directory:

frontend/src/utils/

Examples:

formatDate.js
formatDuration.js
formatProgress.js
formatSkillLevel.js
validation.js

Utilities must remain generic.

27. Backend Architecture

The backend structure is:

backend/
├── requirements.txt
├── app/
│   ├── main.py
│   ├── config/
│   ├── api/
│   ├── core/
│   ├── models/
│   ├── schemas/
│   ├── repositories/
│   ├── services/
│   ├── ai/
│   ├── recommendation/
│   ├── path_generator/
│   ├── integrations/
│   ├── utils/
│   └── middleware/
│
├── tests/
└── scripts/
28. Backend Entry Point

File:

backend/app/main.py

Responsibilities:

create application;
register middleware;
register routers;
initialize required services;
configure exception handlers;
expose health endpoint.

It shall not contain large business functions.

29. Backend API Layer

Directory:

backend/app/api/

Suggested:

api/
├── dependencies.py
├── router.py
└── routes/
    ├── auth.py
    ├── users.py
    ├── profiles.py
    ├── goals.py
    ├── skills.py
    ├── resources.py
    ├── recommendations.py
    ├── learning_paths.py
    ├── progress.py
    ├── feedback.py
    ├── assistant.py
    └── health.py

Routes are responsible for:

receiving HTTP requests;
validating request schemas;
invoking services;
returning response schemas;
mapping expected errors.

Routes should not contain complex business algorithms.

30. Backend Core

Directory:

backend/app/core/

Potential files:

security.py
database.py
exceptions.py
logging.py
constants.py

Responsibilities:

security primitives;
database initialization;
application-wide exceptions;
logging;
shared constants.
31. Backend Configuration

Directory:

backend/app/config/

Potential files:

settings.py
environment.py

Configuration shall read environment variables.

Secrets shall never be hardcoded.

32. Database Models

Directory:

backend/app/models/

Models shall represent persistent database entities defined in DOC-07.

Potential model groups:

models/
├── user.py
├── profile.py
├── goal.py
├── skill.py
├── resource.py
├── learning_path.py
├── progress.py
├── feedback.py
└── conversation.py

Exact model names must match DOC-07.

33. Pydantic/API Schemas

Directory:

backend/app/schemas/

Suggested:

schemas/
├── auth.py
├── user.py
├── profile.py
├── goal.py
├── skill.py
├── resource.py
├── recommendation.py
├── learning_path.py
├── progress.py
├── feedback.py
└── assistant.py

Schemas define request and response structures.

They must remain consistent with DOC-08.

34. Repository Layer

Directory:

backend/app/repositories/

Repositories isolate database access.

Potential files:

repositories/
├── user_repository.py
├── profile_repository.py
├── goal_repository.py
├── skill_repository.py
├── resource_repository.py
├── learning_path_repository.py
├── progress_repository.py
├── feedback_repository.py
└── conversation_repository.py

Repositories shall:

query database;
insert records;
update records;
delete records where allowed;
return persistence-layer objects.

Repositories should not contain AI reasoning.

35. Service Layer

Directory:

backend/app/services/

Services contain business workflows.

Potential services:

services/
├── auth_service.py
├── profile_service.py
├── goal_service.py
├── skill_service.py
├── resource_service.py
├── progress_service.py
├── feedback_service.py
└── assistant_service.py

Services coordinate repositories and domain modules.

36. AI Layer

Directory:

backend/app/ai/

Recommended:

ai/
├── __init__.py
├── provider.py
├── llm_client.py
├── prompt_manager.py
├── goal_parser.py
├── skill_extractor.py
├── explanation_generator.py
├── fallback.py
└── validators.py

Responsibilities:

external LLM communication;
prompt management;
structured extraction;
AI explanation generation;
fallback behavior;
response validation.
37. AI Provider Abstraction

The application shall not directly depend on a single LLM provider throughout the codebase.

Recommended conceptual interface:

AIProvider
   │
   ├── generate_text()
   ├── generate_structured()
   └── health_check()

Possible implementation:

LLMProvider
MockAIProvider
FallbackAIProvider

This enables:

testing without external APIs;
provider replacement;
local development;
graceful degradation.
38. AI Response Validation

AI-generated structured output shall be validated before being used by core business logic.

Flow:

User Input
   ↓
AI Provider
   ↓
Raw AI Response
   ↓
Parser
   ↓
Schema Validation
   ↓
Business Logic

Invalid AI output must not directly enter the database.

39. Recommendation Engine

Directory:

backend/app/recommendation/

Suggested structure:

recommendation/
├── __init__.py
├── candidate_generator.py
├── feature_builder.py
├── scorer.py
├── ranker.py
├── filters.py
├── explanations.py
├── personalization.py
└── pipeline.py
40. Recommendation Pipeline

The authoritative recommendation flow shall be:

Learner Profile
      ↓
Active Goal
      ↓
Required Skills
      ↓
Current Skills
      ↓
Skill Gap
      ↓
Candidate Resources
      ↓
Hard Filters
      ↓
Feature Generation
      ↓
Scoring
      ↓
Ranking
      ↓
Explanation
      ↓
Top Recommendations
41. Candidate Generation

candidate_generator.py shall identify potentially relevant resources.

Candidate sources may include:

skill matches;
semantic similarity;
goal tags;
prerequisite compatibility;
resource type;
difficulty.
42. Recommendation Scoring

scorer.py shall calculate a recommendation score.

The exact scoring method is defined in DOC-09.

The implementation must not invent a different scoring system without updating DOC-09 and DOC-22.

43. Recommendation Ranking

ranker.py shall:

sort candidates;
remove invalid candidates;
enforce limits;
apply ranking rules;
return final recommendations.
44. Recommendation Explanation

explanations.py shall generate structured explanation data.

Example:

{
  "reason_codes": [
    "SKILL_GAP",
    "GOAL_ALIGNMENT",
    "PREREQUISITE_MATCH"
  ],
  "summary": "Recommended because it addresses a key missing skill."
}

The exact response structure must match DOC-08.

45. Path Generator

Directory:

backend/app/path_generator/

Suggested:

path_generator/
├── __init__.py
├── prerequisite_graph.py
├── stage_builder.py
├── milestone_builder.py
├── activity_selector.py
├── effort_estimator.py
├── path_validator.py
└── generator.py
46. Learning Path Pipeline

The path generation flow shall be:

Goal
 ↓
Required Skills
 ↓
Current Skills
 ↓
Skill Gap
 ↓
Prerequisite Graph
 ↓
Required Skill Ordering
 ↓
Resource Selection
 ↓
Stage Construction
 ↓
Milestone Construction
 ↓
Effort Estimation
 ↓
Path Validation
 ↓
Learning Path
47. Path Validation

The generated path must be validated for:

prerequisite ordering;
duplicate activities;
missing required skills;
invalid resources;
unreachable stages;
invalid references;
malformed data.

Invalid paths shall not be persisted as final paths.

48. Integrations

Directory:

backend/app/integrations/

Suggested:

integrations/
├── ai/
├── learning_resources/
└── external/

External integrations must remain isolated from core business logic.

49. External Resource Integration

Potential structure:

integrations/learning_resources/
├── base_provider.py
├── provider_a.py
├── provider_b.py
└── normalizer.py

All external resource data must be normalized into the internal resource schema.

50. Middleware

Directory:

backend/app/middleware/

Potential middleware:

request_logging.py
error_handling.py
request_id.py
security_headers.py

Middleware must not contain feature-specific business logic.

51. Backend Utilities

Directory:

backend/app/utils/

Potential utilities:

date_utils.py
pagination.py
text_utils.py
validators.py
serialization.py

Only reusable utility logic belongs here.

52. Database Access Rule

The following architecture is mandatory:

API Route
   ↓
Service
   ↓
Repository
   ↓
Database

The following is prohibited:

API Route
   ↓
Database

and:

Frontend
   ↓
Database
53. AI Access Rule

The following is mandatory:

API Route
   ↓
Service
   ↓
AI Module
   ↓
AI Provider

The frontend must never directly call:

OpenAI
Gemini
Anthropic
Other LLM Provider
54. Recommendation Access Rule

The recommendation engine shall be called through backend services.

API
 ↓
Recommendation Service
 ↓
Recommendation Pipeline
 ↓
Candidate Generator
 ↓
Scorer
 ↓
Ranker
 ↓
Explanation
55. Learning Path Access Rule
API
 ↓
Learning Path Service
 ↓
Path Generator
 ↓
Path Validator
 ↓
Repository
 ↓
Database
56. Authentication Access Rule
Login Request
 ↓
Auth Route
 ↓
Auth Service
 ↓
Password Verification
 ↓
Token/Session Creation
 ↓
Response

Protected request:

Request
 ↓
Authentication Middleware/Dependency
 ↓
Current User
 ↓
Authorization
 ↓
Service
57. Error Handling Architecture

Errors shall flow through controlled layers.

Repository Error
      ↓
Service Error
      ↓
Application Exception
      ↓
API Exception Handler
      ↓
Safe HTTP Response

Internal implementation details shall not be exposed.

58. Error Categories

Recommended application error categories:

ValidationError
AuthenticationError
AuthorizationError
NotFoundError
ConflictError
ExternalServiceError
AIServiceError
DatabaseError
RecommendationError
PathGenerationError
InternalServerError
59. HTTP Status Mapping

Recommended mapping:

400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Validation Error
429 Rate Limited
500 Internal Server Error
502 External Service Failure
503 Service Unavailable

Exact mappings must remain consistent with DOC-08.

60. Data Flow

The complete application data flow shall be:

                    ┌─────────────────┐
                    │     Learner     │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │    Frontend     │
                    └────────┬────────┘
                             │
                         REST API
                             │
                             ▼
                    ┌─────────────────┐
                    │     Backend     │
                    └───────┬─────────┘
                            │
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
         ┌─────────┐   ┌──────────┐  ┌───────────┐
         │ Services│   │ AI Layer │  │Recommend. │
         └────┬────┘   └────┬─────┘  └─────┬─────┘
              │             │               │
              │             ▼               │
              │        External AI          │
              │                             │
              └──────────────┬──────────────┘
                             ▼
                       Repositories
                             │
                             ▼
                         Database
61. Authentication Data Flow
User
 ↓
Login Form
 ↓
POST /auth/login
 ↓
Auth Route
 ↓
Auth Service
 ↓
User Repository
 ↓
Password Verification
 ↓
Token Generation
 ↓
HTTP Response
 ↓
Frontend Auth State
62. Onboarding Data Flow
Learner
 ↓
Registration
 ↓
Profile
 ↓
Experience
 ↓
Interests
 ↓
Current Skills
 ↓
Goal
 ↓
POST /onboarding or equivalent APIs
 ↓
Backend
 ↓
Database
63. Goal Analysis Flow
Natural Language Goal
        ↓
Goal API
        ↓
Goal Service
        ↓
Goal Parser
        ↓
Structured Goal
        ↓
Skill Extraction
        ↓
Required Skills
        ↓
Database
64. Skill Gap Flow
Current Skills
       +
Required Skills
       ↓
Skill Gap Analyzer
       ↓
Missing Skills
       ↓
Gap Severity
       ↓
Recommendation Engine
65. Recommendation Flow
Learner State
     ↓
Candidate Generation
     ↓
Filtering
     ↓
Feature Construction
     ↓
Scoring
     ↓
Ranking
     ↓
Explanation
     ↓
API Response
     ↓
Frontend
66. Learning Path Flow
Goal
 ↓
Required Skills
 ↓
Skill Gap
 ↓
Prerequisite Graph
 ↓
Ordered Skills
 ↓
Resource Selection
 ↓
Stages
 ↓
Milestones
 ↓
Activities
 ↓
Validation
 ↓
Persist Path
67. Feedback Flow
Learner
 ↓
Feedback UI
 ↓
Feedback API
 ↓
Feedback Service
 ↓
Feedback Repository
 ↓
Database
 ↓
Future Recommendation State
 ↓
Re-ranking
68. Progress Flow
Learner completes activity
 ↓
Progress API
 ↓
Progress Service
 ↓
Progress Repository
 ↓
Database
 ↓
Milestone Calculation
 ↓
Overall Progress
 ↓
Dashboard
69. Backend Dependency Direction

Dependencies shall generally point inward toward business logic.

Recommended:

API
 ↓
Services
 ↓
Domain / AI / Recommendation / Path Generator
 ↓
Repositories / Integrations
 ↓
Infrastructure

Avoid circular dependencies.

70. Dependency Restrictions

The following restrictions apply.

Frontend shall not import backend code.

Backend API modules shall not import frontend code.

Repositories shall not import API routes.

Models shall not import API routes.

AI provider modules shall not import frontend code.

External integration modules shall not control core application flow directly.

71. Naming Convention — Python

Use:

snake_case

Examples:

user_service.py
goal_service.py
recommendation_engine.py
path_generator.py

Classes use:

PascalCase

Examples:

UserService
GoalService
RecommendationEngine
LearningPathGenerator

Functions use:

snake_case
72. Naming Convention — React

Components:

PascalCase.jsx

Examples:

DashboardCard.jsx
RecommendationCard.jsx
LearningPath.jsx

Hooks:

useSomething.js

Examples:

useAuth.js
useRecommendations.js
useLearningPath.js

Services:

camelCase.js

Examples:

authService.js
goalService.js
recommendationService.js
73. File Naming Rule

File names shall clearly communicate responsibility.

Bad:

helper.py
misc.py
stuff.js
temp.py
new_service.py
final_final.py

Good:

skill_gap_analyzer.py
recommendation_ranker.py
learning_path_service.py
74. Environment Files

Allowed:

.env.example

Local-only:

.env

Production secrets shall be managed by deployment infrastructure.

.env shall be ignored by Git.

75. Required Root .gitignore

The project shall ignore at minimum:

.env
.env.*
!.env.example

node_modules/
__pycache__/
*.pyc
.venv/
venv/

dist/
build/

.pytest_cache/
.coverage
htmlcov/

.idea/
.vscode/

.DS_Store

logs/
*.log

data/local/

Additional entries may be added according to technology decisions.

76. Data Directory Rules

The data/ directory shall be divided into:

data/
├── raw/
├── processed/
├── seed/
└── README.md

Raw external datasets shall not be modified directly.

Processing scripts shall produce processed datasets.

77. Seed Data

Seed data may contain:

skills;
skill relationships;
learning resources;
sample users;
sample goals.

Production user information shall never be stored in seed files.

78. ML Directory Rules

The ml/ directory is for:

experimentation;
model evaluation;
embedding generation;
offline analysis;
notebooks.

Production inference code should live in the backend AI/recommendation architecture where required.

79. Notebook Rule

Notebooks shall not become production dependencies.

A notebook may be used to:

Experiment
 ↓
Evaluate
 ↓
Validate
 ↓
Extract production logic
 ↓
Implement in backend/ml module
80. Test Directory

Backend tests:

backend/tests/
├── unit/
├── integration/
└── e2e/

Frontend tests may remain close to feature modules or under:

frontend/src/

depending on the final testing framework.

81. Unit Tests

Unit tests shall cover:

services;
validators;
recommendation scoring;
skill-gap logic;
path generation;
prerequisite ordering;
utilities;
AI response parsing.
82. Integration Tests

Integration tests shall cover:

API + service;
service + repository;
database interaction;
authentication;
recommendation workflow;
learning-path workflow.
83. End-to-End Tests

E2E tests should cover the main learner journey:

Register
 ↓
Login
 ↓
Onboarding
 ↓
Create Goal
 ↓
Enter Skills
 ↓
Generate Path
 ↓
View Recommendations
 ↓
Complete Activity
 ↓
View Progress
 ↓
Provide Feedback
84. Documentation-to-Code Mapping

The following mapping is mandatory.

Document	Implementation Area
DOC-01	Entire project
DOC-02	Requirements
DOC-03	User journeys
DOC-04	Use cases
DOC-05	MVP
DOC-06	System architecture
DOC-07	Models/repositories
DOC-08	API routes/schemas
DOC-09	AI/recommendation
DOC-10	Frontend
DOC-11	Backend
DOC-12	Authentication/security
DOC-13	Repository structure
DOC-14	Dependencies
DOC-15	Configuration
DOC-16	Git workflow
DOC-17	GitHub tasks
DOC-18	Testing
DOC-19	Deployment
DOC-20	Integration
DOC-21	AI implementation
DOC-22	Architecture decisions
DOC-23	Traceability
85. Requirement-to-Module Mapping
Requirement	Primary Module
FR-001–FR-004	auth
FR-005–FR-011	profile
FR-012–FR-015	goals
FR-016–FR-019	assistant
FR-020–FR-024	skills
FR-025–FR-028	resources
FR-029–FR-035	recommendation
FR-036–FR-039	path_generator
FR-040–FR-046	path_generator
FR-047–FR-050	ai / recommendation
FR-051–FR-055	progress
FR-056–FR-058	feedback
FR-059–FR-062	recommendation
FR-063–FR-067	dashboard
86. API-to-Module Mapping
API Capability	Route	Service
Authentication	auth.py	auth_service.py
Profile	profiles.py	profile_service.py
Goals	goals.py	goal_service.py
Skills	skills.py	skill_service.py
Resources	resources.py	resource_service.py
Recommendations	recommendations.py	recommendation service
Learning Paths	learning_paths.py	learning path service
Progress	progress.py	progress_service.py
Feedback	feedback.py	feedback_service.py
Assistant	assistant.py	assistant_service.py

Exact endpoints are controlled by DOC-08.

87. Database-to-Code Mapping

Database entities defined in DOC-07 shall have corresponding:

Model
Schema
Repository
Service where required
Tests

Example:

Goal
 ├── models/goal.py
 ├── schemas/goal.py
 ├── repositories/goal_repository.py
 ├── services/goal_service.py
 └── tests
88. AI-to-Code Mapping

AI capabilities defined in DOC-09 shall map to:

backend/app/ai/
backend/app/recommendation/
backend/app/path_generator/
ml/

No AI requirement should be implemented only inside a frontend component.

89. Frontend-to-Backend Contract

For every frontend API call:

Frontend Service
      ↓
HTTP Request
      ↓
DOC-08 Endpoint
      ↓
Backend Route
      ↓
Backend Service

The request and response shapes must match DOC-08.

90. API Contract Change Rule

If an API contract changes:

update DOC-08;
update backend schemas;
update backend route;
update frontend service;
update frontend schemas;
update affected components;
update tests;
update DOC-23;
create/update ADR if architecture is affected.
91. Database Change Rule

If the database schema changes:

update DOC-07;
update model;
update repository;
update service;
update API schema if required;
update API contract;
update tests;
update DOC-23;
create ADR if architectural behavior changes.
92. AI Logic Change Rule

If recommendation or AI logic changes:

update DOC-09;
update implementation;
update evaluation tests;
update explanation logic;
verify API response compatibility;
update DOC-21;
update DOC-23;
create ADR if architectural behavior changes.
93. Authentication Change Rule

If authentication changes:

update DOC-12;
update backend security code;
update frontend auth;
update API contract;
update tests;
verify all protected endpoints;
update DOC-20.
94. Feature Development Pattern

Every new feature should follow:

Requirement
   ↓
Use Case
   ↓
API Contract
   ↓
Database Design
   ↓
Backend Service
   ↓
Frontend Service
   ↓
Frontend UI
   ↓
Tests
   ↓
Integration
95. Standard Feature Package

For a new feature, the team should identify:

Requirement IDs
Use Case IDs
API endpoints
Database entities
Backend modules
Frontend modules
Tests
Acceptance criteria
GitHub issue
96. Example — Goal Feature
Requirement:
FR-012

Use Case:
UC-GOAL-001

Backend:
api/routes/goals.py
schemas/goal.py
services/goal_service.py
repositories/goal_repository.py
models/goal.py

Frontend:
features/goals/
pages/GoalsPage/

Tests:
backend/tests/unit/test_goal_service.py
backend/tests/integration/test_goal_api.py
97. Example — Recommendation Feature
Requirements:
FR-029–FR-035
AI-003
AI-006
AI-007

Backend:
api/routes/recommendations.py
services/
recommendation/
ai/

Frontend:
features/recommendations/

Tests:
recommendation unit tests
recommendation integration tests
API tests
98. Example — Learning Path Feature
Requirements:
FR-040–FR-046

Backend:
api/routes/learning_paths.py
services/
path_generator/

Frontend:
features/learningPath/
pages/LearningPathPage/

Tests:
path generation tests
prerequisite tests
API tests
E2E tests
99. Git Branch Structure

Development branches shall follow:

main
│
├── feature/*
├── fix/*
├── docs/*
├── test/*
└── chore/*

Examples:

feature/authentication
feature/goal-management
feature/recommendation-engine
feature/learning-path
feature/dashboard

fix/login-validation
fix/recommendation-ranking

docs/update-api-contract
100. Commit Scope

Commits should represent one logical change.

Good:

feat: add goal creation API
feat: add recommendation ranking service
feat: add learning path dashboard
fix: validate duplicate goal
docs: update API contract
test: add skill gap unit tests

Avoid:

final changes
updated everything
all work done
changes
101. Pull Request Structure

Each PR should contain:

Title
Summary
Requirements
Changes
Files Changed
Tests
Screenshots if UI
Known Limitations
Related Issue
102. Team Parallel Development

Because the team contains five members, work should be divided by modules.

Recommended initial ownership model:

Member 1:
Frontend / UI

Member 2:
Backend / API

Member 3:
Database / Authentication

Member 4:
AI / Recommendation

Member 5:
Integration / Testing / Deployment

Ownership may change according to the final GitHub task assignment.

103. Shared Contract Rule

Parallel development is allowed only when shared contracts are defined.

The most important shared contracts are:

DOC-07 Database
DOC-08 API
DOC-09 AI
DOC-10 Frontend
DOC-11 Backend
DOC-12 Security
104. Integration Order

Integration shall happen in this order:

1. Environment
2. Database
3. Backend health
4. Authentication
5. Profile
6. Goals
7. Skills
8. Resources
9. Recommendation engine
10. Learning path
11. Progress
12. Feedback
13. Assistant
14. Dashboard
15. E2E workflow

This ordering minimizes integration failures.

105. Development Startup Order

During implementation:

Database
   ↓
Backend
   ↓
API
   ↓
Frontend API Client
   ↓
Frontend Features

AI integrations shall be added after deterministic backend foundations are working.

106. Mocking Rule

External services shall be mockable.

At minimum:

AI provider
External learning-resource provider

must have mock/test implementations.

This allows the project to run without external service availability during development.

107. Local Development Modes

The project should support:

Development
Testing
Production

Development may use:

Local database
Mock AI provider
Seed resources
Debug logging

Testing should use:

Test database
Mock external APIs
Deterministic test data

Production should use:

Production database
Configured AI provider
Production secrets
Production logging
108. Code Generation Rules for AI Assistants

When using ChatGPT, Claude, or another AI coding assistant, the assistant should first read:

README.md
DOC-01
DOC-02
DOC-05
DOC-06
DOC-07
DOC-08
DOC-09
DOC-10
DOC-11
DOC-12
DOC-13

before generating implementation code.

For a specific feature, the assistant should additionally read:

DOC-03
DOC-04
DOC-17
DOC-18
DOC-20
DOC-21
DOC-23

The assistant must not invent:

API endpoints;
database fields;
authentication behavior;
recommendation scores;
folder locations;
environment variables.

If information is missing, the assistant should identify the missing specification instead of silently inventing it.

109. AI Coding Prompt Context

The standard implementation context should be:

Project: PathFinder AI

Read the project documentation before modifying code.

Source of truth:
DOC-01 Project Charter
DOC-02 SRS
DOC-05 MVP Scope
DOC-06 System Architecture
DOC-07 Database Design
DOC-08 API Contract
DOC-09 ML/AI Architecture
DOC-10 Frontend Architecture
DOC-11 Backend Architecture
DOC-12 Authentication and Security
DOC-13 Code and Folder Architecture

Rules:
1. Do not invent API contracts.
2. Do not invent database fields.
3. Do not bypass repository/service layers.
4. Do not expose secrets.
5. Do not call external AI services directly from frontend.
6. Preserve existing folder architecture.
7. Maintain backwards compatibility unless explicitly changing a contract.
8. Add tests for new business logic.
9. Update documentation if implementation changes a defined contract.
10. Report conflicts between documents before implementation.
110. Documentation Conflict Resolution

If two documents conflict:

DOC-22 Architecture Decision Records
        ↓
Latest approved architectural decision
        ↓
Specific architecture document
        ↓
General project documentation

Conflicts shall not be silently ignored.

111. Source of Truth Hierarchy

For implementation:

Approved ADR
      ↓
Relevant Architecture Document
      ↓
API / Database Contract
      ↓
SRS
      ↓
Project Charter
112. No Silent Architecture Changes

Developers shall not make major architecture changes merely to make code easier.

Examples requiring documentation:

changing database technology;
changing authentication model;
changing API architecture;
replacing recommendation approach;
introducing a new major external service;
moving responsibilities between frontend and backend.

Such changes require DOC-22.

113. Production Code Quality Rules

Production code shall:

use clear naming;
avoid duplicated business logic;
validate external input;
handle expected errors;
avoid secrets;
include appropriate logging;
have tests for critical logic;
follow the documented architecture.
114. Duplicate Logic Rule

The same business rule must not be implemented independently in:

Frontend
Backend Service
AI Prompt
Database Trigger

The authoritative implementation location must be defined.

For example:

Recommendation Ranking
→ Backend Recommendation Engine

Frontend only displays the result.

115. Frontend Calculation Rule

The frontend may calculate presentation-only values such as:

progress bar width
display formatting
date formatting
UI state

The frontend shall not independently calculate authoritative:

skill gaps
recommendation rankings
learning-path ordering
user authorization
116. Backend Calculation Rule

Backend services are responsible for authoritative:

business rules
skill gap
recommendation ranking
path generation
progress calculations
authorization
117. AI Calculation Rule

AI may assist with:

natural-language understanding
semantic extraction
explanation generation
semantic similarity

AI shall not be the only authority for:

authentication
authorization
database integrity
prerequisite validation
security decisions
118. Deterministic Core Principle

The project should maintain a deterministic core.

                    AI Layer
                       │
                       ▼
             ┌──────────────────┐
             │ Business Rules   │
             │ Validation       │
             │ Ranking Rules    │
             │ Path Validation  │
             └────────┬─────────┘
                      │
                      ▼
                  Database

This ensures the application remains functional even if AI output is unavailable or inconsistent.

119. Fallback Architecture

If the external AI provider fails:

AI Request
   ↓
Provider
   ↓
Failure
   ↓
Fallback
   ↓
Deterministic Logic / Safe Response

The user should receive a controlled message rather than an application crash.

120. Health Checks

Backend shall expose a health endpoint.

Example:

GET /health

It should verify application availability.

Optional checks:

database
AI provider
external resource provider

Detailed endpoint contract is defined by DOC-08.

121. Logging Structure

Logs should capture:

timestamp
request ID
event
module
severity
safe contextual information

Logs must not contain:

passwords
JWT secrets
API keys
database passwords
private tokens
122. Request ID

Requests should have a request/correlation ID where practical.

Example:

Frontend
 ↓
Request-ID
 ↓
API
 ↓
Service
 ↓
Repository

This assists debugging during integration.

123. Configuration Structure

Configuration should be centralized.

Example conceptual structure:

config/
├── settings.py
└── environment.py

Configuration categories:

Application
Database
Authentication
AI
External APIs
Logging
CORS
Deployment

Exact environment variables are defined in DOC-15.

124. CORS Architecture

CORS shall be configured in backend middleware.

Allowed frontend origins must come from configuration.

Development:

localhost frontend

Production:

deployed frontend domain

Wild-card production CORS should not be used unless explicitly justified.

125. API Versioning

The API should use a versioning strategy defined in DOC-08.

Preferred structure:

/api/v1/

All frontend services must use the documented version.

126. Module Import Rules

Imports should follow:

API → Services
Services → Repositories / Domain Modules
Repositories → Database
AI → AI Provider
Recommendation → supporting services/data

Avoid:

Repository → API
Model → API
Frontend → Backend Python
AI Provider → Frontend
127. Circular Dependency Prevention

If a circular dependency appears:

identify shared responsibility;
extract shared abstraction;
move shared utility to appropriate module;
update imports;
add tests.

Do not solve circular imports through arbitrary runtime hacks.

128. Shared Constants

Constants shared across modules should be centralized where appropriate.

Examples:

experience levels
goal statuses
activity statuses
feedback types
skill levels

Their authoritative definitions must match DOC-07 and DOC-08.

129. Status Enumerations

Potential statuses:

Goal:
ACTIVE
PAUSED
COMPLETED
ARCHIVED

Activity:
NOT_STARTED
IN_PROGRESS
COMPLETED
SKIPPED

Exact values shall follow the database and API contracts.

130. Skill Levels

The documented skill-level representation must be centralized.

Example:

0 Not Known
1 Beginner
2 Basic
3 Intermediate
4 Advanced
5 Expert

The final implementation must match DOC-07 and DOC-09.

131. Resource Types

Potential values:

COURSE
VIDEO
ARTICLE
DOCUMENTATION
PROJECT
ASSESSMENT

Exact enum representation shall match DOC-07 and DOC-08.

132. Recommendation Response Structure

The backend recommendation response should conceptually contain:

resource
score
rank
reason
matched_skills
gap_alignment
difficulty
estimated_effort

The authoritative JSON schema remains DOC-08.

133. Learning Path Response Structure

Conceptually:

path
 ├── goal
 ├── summary
 ├── stages
 │    ├── stage
 │    ├── milestones
 │    └── activities
 ├── estimated_effort
 └── next_action

Exact schema remains DOC-08.

134. Dashboard Data Aggregation

The dashboard may consume multiple endpoints or a dedicated aggregation endpoint.

The final choice shall follow DOC-08.

The frontend shall not perform excessive API calls if a dedicated backend aggregation endpoint is defined.

135. Performance Considerations

Potentially expensive operations include:

embedding generation
semantic search
recommendation ranking
learning-path generation
LLM calls

These operations should not unnecessarily block unrelated API requests.

Caching and asynchronous processing may be introduced later if required.

136. MVP Simplification Rule

The prototype shall prioritize:

correctness
integration
demonstrability
explainability
maintainability

over unnecessary infrastructure complexity.

The architecture should remain production-oriented without overengineering the MVP.

137. MVP Architecture

The MVP should be implementable with:

One frontend application
One backend application
One primary database
One recommendation pipeline
One AI provider abstraction
Seed learning resources

Additional microservices are not required for the MVP unless justified by an ADR.

138. Monolith Rule

The backend should initially use a modular monolith architecture.

One Backend
│
├── Auth
├── Profile
├── Goals
├── Skills
├── Resources
├── Recommendation
├── Learning Path
├── Progress
├── Feedback
└── Assistant

This reduces integration complexity for the competition prototype.

139. Future Scalability

The modular boundaries should allow future extraction of:

Recommendation Service
AI Service
Search Service
Analytics Service

without requiring the MVP to deploy them independently.

140. Deployment Structure

Deployment should conceptually contain:

Frontend
   ↓
Backend API
   ↓
Database

Backend
   ├── AI Provider
   └── External Resource Providers

Deployment-specific details are defined in DOC-19.

141. Docker Structure

If Docker is used:

deployment/
└── docker/
    ├── frontend/
    ├── backend/
    └── database/

Root-level docker-compose.yml may orchestrate local development.

142. Local Development Architecture
Browser
  ↓
Frontend localhost
  ↓
Backend localhost
  ↓
Database localhost

External services:

Backend
  ├── AI provider
  └── External resource provider
143. Production Development Difference

Development may use:

localhost
mock services
seed data
debug logs

Production must use:

configured domain
secure credentials
production database
restricted CORS
production logging
144. Build Order

Implementation should follow:

Phase 1

Repository and environment.

Phase 2

Database.

Phase 3

Backend skeleton.

Phase 4

Authentication.

Phase 5

Learner profile.

Phase 6

Goal management.

Phase 7

Skill management.

Phase 8

Learning resources.

Phase 9

Skill-gap analysis.

Phase 10

Recommendation engine.

Phase 11

Learning-path generator.

Phase 12

Progress tracking.

Phase 13

Feedback.

Phase 14

AI assistant.

Phase 15

Dashboard.

Phase 16

Testing.

Phase 17

Deployment.

Phase 18

Final integration.

145. Vertical Slice Principle

Where practical, development should validate complete vertical slices rather than building every frontend and backend layer independently.

Example:

Goal Creation

Frontend
 ↓
API
 ↓
Service
 ↓
Repository
 ↓
Database
 ↓
Test

This catches integration errors early.

146. First Vertical Slice

The first complete slice should be:

Health Check

Flow:

Browser/API Client
 ↓
GET /health
 ↓
Backend
 ↓
Response

This proves the backend can run.

147. Second Vertical Slice

Authentication:

Register
 ↓
Database
 ↓
Login
 ↓
Authentication
 ↓
Protected Endpoint
148. Third Vertical Slice

Goal:

Create Goal
 ↓
Store Goal
 ↓
Retrieve Goal
 ↓
Display Goal
149. Fourth Vertical Slice

Skills:

Current Skills
+
Goal
 ↓
Required Skills
 ↓
Skill Gap
 ↓
Display Gap
150. Fifth Vertical Slice

Recommendation:

Skill Gap
 ↓
Candidate Resources
 ↓
Ranking
 ↓
Recommendation
 ↓
Explanation
151. Sixth Vertical Slice

Learning Path:

Goal
 ↓
Skill Gap
 ↓
Prerequisites
 ↓
Ordered Activities
 ↓
Milestones
 ↓
Path
152. Seventh Vertical Slice

Progress:

Activity
 ↓
Complete
 ↓
Progress
 ↓
Milestone
 ↓
Dashboard
153. Final Vertical Slice

Complete:

Register
 ↓
Onboarding
 ↓
Goal
 ↓
Skills
 ↓
Skill Gap
 ↓
Recommendations
 ↓
Learning Path
 ↓
Progress
 ↓
Feedback
 ↓
Updated Recommendations

This is the primary competition demonstration workflow.

154. Code Review Checklist

Before merging code:

[ ] Correct folder
[ ] Correct module responsibility
[ ] Requirement identified
[ ] API contract respected
[ ] Database contract respected
[ ] Authentication rules respected
[ ] No secrets
[ ] Error handling
[ ] Logging
[ ] Tests
[ ] No duplicate business logic
[ ] Documentation updated if required
[ ] No breaking changes
155. Integration Checklist Reference

The complete integration validation shall be performed according to:

DOC-20 Integration Checklist
156. Testing Reference

Testing requirements are defined in:

DOC-18 Testing and QA Strategy

All critical modules created under this architecture must have corresponding tests.

157. Deployment Reference

Deployment configuration is defined in:

DOC-19 Deployment Architecture

Production-specific changes must not alter application architecture without documentation.

158. GitHub Task Reference

Implementation tasks shall be generated according to:

DOC-17 GitHub Task Breakdown

Each implementation task should reference:

Requirement ID
Module
Acceptance Criteria
Dependencies
Test Requirement
159. Traceability Reference

All implemented requirements must eventually map through:

Requirement
 ↓
GitHub Issue
 ↓
Code Module
 ↓
Test
 ↓
Acceptance Criteria

The authoritative matrix is:

DOC-23 Requirements Traceability Matrix
160. Architecture Decision Reference

Architecture changes must be recorded in:

DOC-22 Architecture Decision Records
161. Implementation Readiness

The codebase is considered ready for implementation when:

DOC-01 approved
DOC-02 approved
DOC-05 approved
DOC-06 reviewed
DOC-07 reviewed
DOC-08 reviewed
DOC-09 reviewed
DOC-10 reviewed
DOC-11 reviewed
DOC-12 reviewed
DOC-13 reviewed
162. Architecture Freeze

Before significant coding begins, the team should freeze:

Repository structure
Database entities
API naming
Authentication approach
Frontend architecture
Backend architecture
AI abstraction
Recommendation pipeline

Changes after freeze require documented change control.

163. AI-Assisted Development Safety

Because AI coding assistants may generate inconsistent implementations, every generated code contribution must be checked against:

DOC-07
DOC-08
DOC-09
DOC-10
DOC-11
DOC-12
DOC-13

AI-generated code must not be accepted solely because it compiles.

It must also satisfy architecture and contract requirements.

164. Generated Code Acceptance

Generated code must pass:

Syntax Check
 ↓
Unit Tests
 ↓
Integration Tests
 ↓
Contract Validation
 ↓
Security Review
 ↓
Manual Feature Test
165. Definition of Done for a Module

A module is complete only when:

[ ] Code implemented
[ ] Correct folder structure
[ ] Requirement mapped
[ ] API contract implemented if applicable
[ ] Database contract implemented if applicable
[ ] Validation implemented
[ ] Error handling implemented
[ ] Tests written
[ ] Integration tested
[ ] Documentation updated
[ ] GitHub issue updated
[ ] PR reviewed
[ ] Merged into main
166. Definition of Done for a Feature

A feature is complete only when:

Frontend
   +
Backend
   +
Database
   +
Tests
   +
Documentation
   +
Integration

have all been validated.

167. Definition of Done for MVP

The MVP is complete when the following journey works end-to-end:

Learner Registration
        ↓
Authentication
        ↓
Profile
        ↓
Goal
        ↓
Current Skills
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
        ↓
Dashboard
168. Final Target Repository

At implementation completion, the repository should approximately resemble:

HCL_PathFinder/
│
├── README.md
├── .gitignore
├── .env.example
├── docker-compose.yml
│
├── docs/
│
├── frontend/
│   ├── package.json
│   ├── package-lock.json
│   ├── public/
│   └── src/
│       ├── main.jsx
│       ├── App.jsx
│       ├── routes/
│       ├── pages/
│       ├── components/
│       ├── layouts/
│       ├── features/
│       ├── services/
│       ├── hooks/
│       ├── context/
│       ├── schemas/
│       ├── utils/
│       ├── constants/
│       ├── assets/
│       └── styles/
│
├── backend/
│   ├── requirements.txt
│   ├── app/
│   │   ├── main.py
│   │   ├── config/
│   │   ├── api/
│   │   ├── core/
│   │   ├── models/
│   │   ├── schemas/
│   │   ├── repositories/
│   │   ├── services/
│   │   ├── ai/
│   │   ├── recommendation/
│   │   ├── path_generator/
│   │   ├── integrations/
│   │   ├── middleware/
│   │   └── utils/
│   │
│   ├── tests/
│   │   └── ...
│   │
│   └── scripts/
│
├── data/
│   ├── raw/
│   ├── processed/
│   ├── seed/
│   └── README.md
│
├── ml/
│   ├── notebooks/
│   ├── experiments/
│   ├── models/
│   ├── embeddings/
│   └── evaluation/
│
├── tests/
│   └── ...
│
└── deployment/
    ├── docker/
    ├── nginx/
    └── README.md
169. Document Dependencies

DOC-13 depends on:

DOC-01 Project Charter
DOC-02 Software Requirements Specification
DOC-03 User Roles and Journeys
DOC-04 Use Cases and User Stories
DOC-05 MVP Scope
DOC-06 System Architecture
DOC-07 Database Design
DOC-08 API Contract
DOC-09 ML/AI Architecture
DOC-10 Frontend Architecture
DOC-11 Backend Architecture
DOC-12 Authentication and Security

DOC-13 provides the structural foundation for:

DOC-14 Technology Stack and Dependencies
DOC-15 Environment and Configuration
DOC-16 Git/GitHub Workflow
DOC-17 GitHub Task Breakdown
DOC-18 Testing and QA
DOC-19 Deployment
DOC-20 Integration
DOC-21 Master AI Implementation Specification
DOC-22 Architecture Decision Records
DOC-23 Requirements Traceability Matrix
170. Document Completion Criteria

DOC-13 is complete when:

[ ] Root repository structure defined
[ ] Frontend structure defined
[ ] Backend structure defined
[ ] AI structure defined
[ ] Recommendation structure defined
[ ] Learning-path structure defined
[ ] Database access boundaries defined
[ ] API boundaries defined
[ ] Authentication boundaries defined
[ ] Testing structure defined
[ ] Data structure defined
[ ] Deployment structure defined
[ ] Naming conventions defined
[ ] Dependency rules defined
[ ] Integration order defined
[ ] AI coding rules defined
[ ] Documentation mappings defined
[ ] Traceability mappings defined
171. Final Architecture Rule

The implementation team shall treat DOC-13 as the authoritative source for the physical code and folder structure.

No team member should create a new major directory or move a major module without first checking the architecture documents.

If a new requirement cannot be cleanly represented in the existing architecture, the team must:

Identify Gap
   ↓
Discuss Architecture
   ↓
Create/Update ADR
   ↓
Update Relevant Document
   ↓
Update DOC-13
   ↓
Implement
172. Status

Document: DOC-13
Status: DRAFT
Implementation Status: NOT STARTED
Architecture Status: CONTROLLED
Parent: DOC-01 through DOC-12
Next: DOC-14 Technology Stack and Dependencies

173. Approval
Role	Name	Status
Project Lead	Siddharth Ramnath Ghawate	Pending
Team Member	Gayatri Balasaheb Autade	Pending
Team Member	Shruti Yuvaraj Gawali	Pending
Team Member	Palak Atul Desai	Pending
Team Member	Vivek Dnyaneshwar Shinde	Pending
END OF DOC-13