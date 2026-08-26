# PathFinder AI — Deployment Architecture

**Document ID:** DOC-19  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT  
**Parent Documents:** DOC-06 System Architecture, DOC-11 Backend Architecture, DOC-12 Authentication and Security, DOC-14 Technology Stack and Dependencies, DOC-15 Environment and Configuration, DOC-18 Testing and QA Strategy  
**Related Documents:** DOC-08 API Contract, DOC-09 ML/AI Architecture, DOC-10 Frontend Architecture, DOC-13 Code and Folder Architecture, DOC-16 Git/GitHub Development Workflow, DOC-17 GitHub Task Breakdown  
**Implementation Owner:** Siddharth Ghawate  
**Deployment Target:** Prototype / Competition Demonstration

---

# 1. Purpose

This document defines the deployment architecture for PathFinder AI.

The purpose of this document is to establish how the PathFinder AI application will be:

- built
- configured
- packaged
- deployed
- started
- monitored
- tested after deployment
- updated
- recovered after failure

The deployment architecture must remain consistent with the system architecture, frontend architecture, backend architecture, database design, authentication model, AI architecture, and environment configuration defined in the other project documents.

The deployment design is intentionally suitable for an individual developer building and maintaining the complete prototype.

The architecture must avoid unnecessary operational complexity.

---

# 2. Deployment Objectives

The deployment architecture shall satisfy the following objectives.

## DEP-OBJ-001 — Application Accessibility

The final prototype should be accessible through a browser using a publicly reachable application URL where deployment infrastructure permits.

## DEP-OBJ-002 — Reproducibility

A developer must be able to reproduce the application environment from the GitHub repository.

## DEP-OBJ-003 — Environment Separation

Development, testing, and production/demo environments should be logically separated.

## DEP-OBJ-004 — Secure Configuration

Secrets must not be stored directly in source code.

## DEP-OBJ-005 — Reliable Startup

The application must start using documented commands and configuration.

## DEP-OBJ-006 — API Availability

The frontend must be able to communicate with the deployed backend using the API contract defined in DOC-08.

## DEP-OBJ-007 — Database Availability

The backend must be able to securely connect to the configured database.

## DEP-OBJ-008 — AI Availability

The AI/recommendation layer must be able to operate using the configured AI provider or deterministic fallback mechanisms defined in DOC-09.

## DEP-OBJ-009 — Demonstration Readiness

The deployment must support the complete competition demonstration workflow.

## DEP-OBJ-010 — Maintainability

The deployment process must be simple enough for one developer to maintain.

---

# 3. Deployment Scope

This document covers deployment of the following major components:

1. Frontend application
2. Backend API
3. Database
4. AI/ML services
5. Authentication infrastructure
6. Configuration and secrets
7. Static assets
8. Logging
9. Health checks
10. Monitoring
11. CI/CD
12. Backup and recovery
13. Deployment validation
14. Demo environment

The following are outside the initial deployment scope:

- large-scale enterprise orchestration
- Kubernetes
- multi-region deployment
- complex service mesh infrastructure
- advanced distributed tracing
- high-volume production infrastructure
- multi-database sharding

These may be considered in future versions.

---

# 4. Deployment Philosophy

PathFinder AI is a competition prototype.

Therefore, deployment architecture should prioritize:

- reliability
- simplicity
- reproducibility
- security
- demonstrability
- low operational overhead

The project should not introduce infrastructure complexity that does not directly improve the prototype.

The preferred deployment model is:

```text
Browser
   |
   v
Frontend Application
   |
   v
Backend API
   |
   +--------------------+
   |                    |
   v                    v
Database             AI Service
   |
   v
Learning Data

External learning-resource providers may additionally be accessed by the backend.

5. High-Level Deployment Architecture

The deployed PathFinder AI system shall follow the logical architecture below.

                        INTERNET
                           |
                           v
                 +-------------------+
                 |       USER        |
                 |     Browser       |
                 +---------+---------+
                           |
                           | HTTPS
                           v
                 +-------------------+
                 |     FRONTEND      |
                 | React Application  |
                 +---------+---------+
                           |
                           | HTTPS / REST
                           v
                 +-------------------+
                 |    BACKEND API    |
                 |   FastAPI App     |
                 +----+---------+----+
                      |         |
                      |         |
                      v         v
             +------------+  +----------------+
             | PostgreSQL |  | AI/ML Service  |
             | Database   |  | LLM / Models   |
             +------------+  +----------------+
                      |
                      v
             +----------------+
             | Learning Data  |
             | Skills/Paths   |
             | Progress/etc.  |
             +----------------+

                 Optional External Providers
                           |
                           v
                +-----------------------+
                | Courses / Resources   |
                | Documentation / URLs  |
                +-----------------------+
6. Deployment Components
6.1 Frontend

The frontend is the user-facing application.

The frontend is responsible for:

authentication screens
onboarding
learner profile
goal creation
skill input
dashboard
learning roadmap
recommendation display
recommendation explanations
progress tracking
feedback
AI chat interface

The frontend must not contain secrets such as:

database credentials
backend private keys
JWT signing secrets
private AI API keys

Only public configuration values may be exposed to browser-side code.

7. Frontend Deployment Model

The frontend should be deployed as a web application.

The build process should generally follow:

Source Code
    |
    v
Install Dependencies
    |
    v
Run Tests
    |
    v
Build Frontend
    |
    v
Generate Production Assets
    |
    v
Deploy Static/Web Application

The deployed frontend communicates with the backend using the configured API base URL.

Example:

FRONTEND_URL
https://pathfinder.example.com

API_BASE_URL
https://api.pathfinder.example.com

Actual production URLs will be determined by the selected hosting providers.

8. Backend Deployment

The backend is the central application service.

The backend shall expose the APIs defined in DOC-08.

The backend is responsible for:

authentication
user management
learner profile
goals
skills
skill-gap analysis
recommendations
learning paths
progress
feedback
AI orchestration
database operations
external resource access
validation
authorization

The backend should be deployed as an API service.

9. Backend Startup

The backend must have a documented production startup command.

For the FastAPI-based architecture, the deployment process should support an ASGI server.

Conceptually:

FastAPI Application
        |
        v
ASGI Server
        |
        v
HTTP Requests

A typical deployment command may be:

uvicorn app.main:app --host 0.0.0.0 --port $PORT

If the selected hosting platform requires a different startup format, the command must be documented in the project README.

10. Database Deployment

The primary application database is PostgreSQL as established by the project technology architecture.

The database stores:

users
learner profiles
goals
skills
skill relationships
learning resources
learning paths
path items
progress
feedback
conversations where applicable
recommendation records where applicable

The database should preferably be hosted using a managed PostgreSQL service for the competition deployment.

11. Database Connectivity

The backend shall connect to PostgreSQL using an environment-based database URL.

Example:

DATABASE_URL=postgresql://username:password@host:5432/pathfinder

The actual credential must never be committed to Git.

The database URL must be supplied through environment configuration.

12. Database Security

The deployment database shall use:

authenticated access
encrypted connections where supported
restricted credentials
environment-based secrets
least-privilege access where practical

Database credentials must not appear in:

source code
README
screenshots
frontend JavaScript
GitHub commits
public documentation
demo videos
13. Database Migration Strategy

Database schema changes must be handled through a migration mechanism.

The preferred approach is to use the project's selected ORM/migration tooling.

Conceptually:

Model Change
     |
     v
Migration Generation
     |
     v
Migration Review
     |
     v
Migration Execution
     |
     v
Updated Database

Migrations must be committed to Git.

Manual database modifications should be avoided except for controlled maintenance.

14. Database Initialization

A new environment should be capable of initializing the database using documented commands.

The initialization sequence should be:

Create Database
       |
       v
Configure DATABASE_URL
       |
       v
Run Migrations
       |
       v
Load Required Seed Data
       |
       v
Run Application
15. Seed Data

The prototype may require seed data for:

skills
skill relationships
learning resources
resource metadata
example goals
development accounts

Seed data must be clearly separated from production learner data.

The repository should contain a reproducible seed mechanism.

Example:

backend/
    seed/
        skills
        resources
        relationships

The exact folder structure must remain consistent with DOC-13.

16. AI Service Deployment

The AI layer is defined in DOC-09.

The deployment must support an AI abstraction layer.

The backend should not directly embed provider-specific logic throughout business services.

Instead:

Backend
   |
   v
AI Service Interface
   |
   +--------------------+
   |                    |
   v                    v
External LLM        Local/Rule
Provider             Fallback

This allows the application to continue functioning when the external AI service is unavailable.

17. AI API Key Management

AI provider credentials must be stored as environment variables.

Example:

AI_API_KEY=...

The key must never be:

committed to Git
exposed to frontend code
included in documentation
embedded in source files
displayed in logs
18. AI Fallback

The deployment must support graceful AI failure.

If the external AI service becomes unavailable:

AI Request
    |
    v
External AI Service
    |
    +---- Success ---> AI Response
    |
    +---- Failure ---> Fallback Logic
                            |
                            v
                    Controlled Response

Fallback functionality may include:

deterministic skill-gap analysis
database-based recommendations
template explanations
predefined assistant responses
rule-based next-action selection

The fallback must not expose internal errors to the learner.

19. External Learning Resources

Learning resources may originate from:

curated internal datasets
external course providers
documentation websites
public learning platforms
manually entered resources

The backend should store resource metadata rather than unnecessarily copying external content.

Stored information may include:

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
rating
language
20. External Resource Availability

External resource URLs may become unavailable.

Therefore, the system should:

validate URLs where practical
avoid assuming permanent availability
gracefully handle broken links
display provider information
avoid crashing when a resource is unavailable
21. Domain Architecture

The final application may use separate domains or paths.

Recommended logical model:

Frontend:
https://pathfinder.<domain>

Backend:
https://api.pathfinder.<domain>

For a simple prototype deployment, the frontend and backend may also use provider-generated domains.

The exact domain names are deployment-specific.

22. HTTPS Requirement

All public application traffic should use HTTPS.

HTTPS must protect:

login credentials
authentication tokens
learner profile information
goal information
AI conversations
feedback
progress information

HTTP should not be used for public production/demo access except where automatically redirected to HTTPS.

23. CORS Configuration

The backend must explicitly configure allowed frontend origins.

Development example:

http://localhost:5173

Production example:

https://pathfinder.example.com

The backend must not use unrestricted CORS such as:

*

for authenticated production APIs unless there is a documented security reason.

24. Environment Architecture

PathFinder AI should support at least three logical environments.

Development
    |
    v
Testing
    |
    v
Demo / Production

These environments should not share secrets unnecessarily.

25. Development Environment

The development environment is used for local implementation.

Typical components:

Developer Computer
    |
    +-- Frontend
    |
    +-- Backend
    |
    +-- PostgreSQL / Development DB
    |
    +-- AI Provider

The developer should be able to run the complete application locally.

26. Testing Environment

The testing environment is used for:

automated tests
API validation
integration tests
frontend tests
database tests
deployment validation

Testing should avoid modifying production/demo learner data.

27. Demo/Production Environment

The demo environment is the environment used for:

competition judges
demonstration video
final prototype presentation
application URL submission

The demo environment must be stable.

Changes should not be made directly during an active judging session unless necessary.

28. Environment Variables

Environment configuration must follow DOC-15.

Typical backend variables include:

APP_ENV
APP_NAME
APP_DEBUG
DATABASE_URL
JWT_SECRET_KEY
JWT_ALGORITHM
ACCESS_TOKEN_EXPIRE_MINUTES
AI_API_KEY
AI_MODEL
CORS_ORIGINS
LOG_LEVEL

Actual variable names must remain consistent across the project.

29. Frontend Environment Variables

Frontend variables may include:

VITE_API_BASE_URL
VITE_APP_NAME
VITE_APP_ENV

Only non-secret values may be exposed to the frontend.

A frontend variable must never contain:

DATABASE_PASSWORD
JWT_SECRET
PRIVATE_API_KEY
AI_SECRET
30. Secret Management

Secrets must be managed outside the Git repository.

Development:

.env

Production:

Hosting Provider Environment Variables

The repository must include an example configuration file.

Example:

.env.example

The example file must contain placeholders only.

31. Example Environment File

Example:

APP_ENV=development
APP_DEBUG=true

DATABASE_URL=postgresql://username:password@localhost:5432/pathfinder

JWT_SECRET_KEY=replace_with_secure_secret
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

AI_API_KEY=replace_with_ai_key
AI_MODEL=replace_with_model

CORS_ORIGINS=http://localhost:5173

LOG_LEVEL=INFO

No real credentials should be placed in this example.

32. Docker Strategy

Docker may be used to improve reproducibility.

Docker is not mandatory if the selected deployment platform provides a reliable native Python/Node deployment environment.

If Docker is used, the project should contain:

Dockerfile

and optionally:

docker-compose.yml

for local multi-service development.

33. Backend Docker Architecture

Conceptually:

Docker Image
     |
     v
Python Runtime
     |
     v
Dependencies
     |
     v
PathFinder Backend
     |
     v
ASGI Server

The image should not contain production secrets.

34. Frontend Docker Architecture

If containerized, the frontend may follow:

Source
   |
   v
Node Build Environment
   |
   v
Production Build
   |
   v
Web Server

Alternatively, the frontend may be deployed directly through a managed frontend platform.

35. Container Image Requirements

Container images should:

use an appropriate stable base image
minimize unnecessary packages
avoid secrets
use deterministic dependency versions
expose only required ports
run using a non-root user where practical
include health-check support where practical
36. Dependency Installation

Dependencies must be defined in project manifests.

Backend dependencies should be defined in the backend dependency file.

Frontend dependencies should be defined in the frontend package manifest.

Dependency versions should be controlled sufficiently to ensure reproducibility.

37. Build Pipeline

The recommended build pipeline is:

Git Push
   |
   v
Repository Checkout
   |
   v
Install Dependencies
   |
   v
Lint
   |
   v
Unit Tests
   |
   v
Integration Tests
   |
   v
Build
   |
   v
Deployment
   |
   v
Smoke Tests

The exact CI provider is determined by the GitHub workflow architecture.

38. CI/CD Strategy

GitHub Actions may be used for CI/CD.

The project should maintain workflows under:

.github/
    workflows/

Possible workflows:

ci.yml
frontend.yml
backend.yml
deploy.yml

The actual number of workflows may remain smaller if simplicity is preferable.

39. Continuous Integration

Every important push should trigger automated validation.

CI should check:

dependency installation
formatting
linting
unit tests
backend imports
frontend build
API tests where practical

A failed CI pipeline should prevent deployment where deployment protection is configured.

40. Continuous Deployment

Continuous deployment may be enabled after the project reaches a stable state.

Recommended flow:

main branch
     |
     v
CI Validation
     |
     v
Build
     |
     v
Deploy
     |
     v
Smoke Test

For competition stability, automatic deployment may be disabled shortly before final submission.

41. Branch Deployment Strategy

The recommended branch model is:

main
 |
 +---- development work
 |
 +---- stable deployment

If feature branches are used:

feature/*
     |
     v
Pull Request
     |
     v
main
     |
     v
Deployment

The branch workflow must remain consistent with DOC-16.

42. Release Strategy

A release should be created when a stable milestone is reached.

Example:

v0.1.0

for initial prototype.

Later:

v0.2.0
v1.0.0

The final competition version should be tagged.

Example:

v1.0.0-hcl-round2
43. Deployment Versioning

The application should expose a version identifier where practical.

Example:

PathFinder AI
Version 1.0.0
Environment: demo

This helps diagnose which version is deployed.

44. Health Check

The backend should expose a lightweight health endpoint.

Example:

GET /health

Expected response:

{
  "status": "ok"
}

The endpoint should not expose secrets or internal infrastructure information.

45. Readiness Check

A readiness endpoint may be used.

Example:

GET /health/ready

The readiness check may verify:

application initialization
database connectivity
required configuration
critical service availability

The AI provider should not necessarily make the application unhealthy if deterministic fallback functionality is available.

46. Liveness Check

A liveness check should verify that the application process is running.

Example:

GET /health/live

Expected response:

{
  "status": "alive"
}
47. Database Health

Database connectivity should be checked separately from application liveness.

Conceptually:

Application Alive
        |
        +---- Database Ready
        |
        +---- AI Optional

This prevents temporary AI outages from being interpreted as complete application failure.

48. Startup Validation

At application startup, the backend should validate critical configuration.

Examples:

required environment variables
database configuration
JWT configuration
application environment
allowed CORS origins

Invalid critical configuration should fail fast with a useful server-side error.

Secrets must not appear in the error.

49. Deployment Sequence

The recommended deployment sequence is:

1. Push stable code
2. Run CI
3. Build frontend
4. Build backend
5. Verify environment variables
6. Run database migrations
7. Deploy backend
8. Verify backend health
9. Deploy frontend
10. Verify frontend
11. Verify API connectivity
12. Run smoke tests
13. Test login
14. Test onboarding
15. Test goal creation
16. Test path generation
17. Test progress
18. Test AI assistant
19. Verify logs
20. Mark deployment stable
50. Database Migration During Deployment

Database migrations must be executed carefully.

Recommended sequence:

Backup / Verify
      |
      v
Migration
      |
      v
Application Deployment
      |
      v
Smoke Test

For backward-compatible migrations:

Database Migration
       |
       v
Backend Deployment

Migration strategy must be documented whenever schema changes are introduced.

51. Deployment Rollback

If a deployment fails, the system should support rollback where the hosting platform permits.

Rollback process:

Detect Failure
      |
      v
Stop Further Deployment
      |
      v
Identify Previous Stable Version
      |
      v
Rollback Application
      |
      v
Verify Health
      |
      v
Verify Critical Workflow

Database rollback should not be performed blindly.

Destructive schema changes must be avoided or carefully planned.

52. Rollback Decision Rules

Rollback should be considered when:

application cannot start
frontend cannot load
API returns persistent 5xx errors
authentication is broken
database connectivity fails
learning-path generation is broken
critical user workflow fails

Minor UI issues may be fixed forward if rollback would create greater risk.

53. Logging Architecture

The backend must produce useful application logs.

Important events include:

startup
shutdown
authentication failures
API errors
database failures
AI failures
recommendation generation
path generation
unexpected exceptions

Logs must not contain:

passwords
JWT secrets
API keys
complete authentication tokens
sensitive personal information unnecessarily
54. Log Levels

Recommended levels:

DEBUG
INFO
WARNING
ERROR
CRITICAL

Development may use:

DEBUG

Demo/production should generally use:

INFO

with appropriate error logging.

55. Error Monitoring

The deployed application should make failures observable.

At minimum, developers should be able to inspect:

application logs
deployment logs
frontend build logs
database errors
API responses
AI provider errors

A third-party error monitoring service may be added later.

56. Performance Monitoring

The prototype should monitor basic performance indicators.

Examples:

API response time
Database query duration
Recommendation generation time
AI response time
Frontend load time

The project does not require enterprise-level performance monitoring for the MVP.

57. API Performance

The backend should remain responsive for normal prototype usage.

Potential slow operations include:

AI generation
recommendation ranking
path generation
external resource retrieval

These operations should be handled without blocking unrelated requests unnecessarily.

58. AI Timeout

External AI requests must use a timeout.

Conceptually:

AI Request
   |
   v
Timeout
   |
   +---- Response ---> Continue
   |
   +---- Timeout ---> Fallback

The exact timeout is implementation-specific.

59. External API Timeout

External learning-resource APIs must also use controlled timeouts.

A failed external service must not cause the entire backend to hang indefinitely.

60. Rate Limiting

Rate limiting should be considered for publicly exposed APIs.

Priority endpoints include:

/auth/*
/chat/*
/recommendations/*
/learning-path/*

The exact rate limits should be defined during implementation if required.

61. Authentication Deployment

Authentication must follow DOC-12.

The deployment must ensure:

passwords are hashed
tokens are signed securely
secrets are environment-based
protected routes require authentication
unauthorized requests are rejected
CORS is controlled
HTTPS is enabled
62. JWT Secret

The JWT secret must be:

generated securely
stored outside Git
different between environments
sufficiently long
rotated if compromised

Example:

JWT_SECRET_KEY=<secret>

The literal secret must never appear in documentation.

63. Authentication Token Handling

The frontend must handle authentication tokens according to the security architecture defined in DOC-12.

The deployment environment must not alter the security model simply to simplify hosting.

64. Frontend-to-Backend Configuration

The frontend must know the deployed backend URL.

Example:

VITE_API_BASE_URL=https://api.pathfinder.example.com

This value should be configured during frontend deployment.

65. API URL Validation

Before final deployment, verify:

Frontend
    |
    v
Correct API URL
    |
    v
Backend
    |
    v
Database

Incorrect API base URLs are a common deployment failure and must be checked before demo submission.

66. Production CORS

Production CORS must include the actual deployed frontend origin.

Example:

https://pathfinder.example.com

It should not accidentally remain configured only for:

http://localhost:5173
67. Frontend Routing

If the frontend uses client-side routing, the hosting platform must support fallback routing.

For example:

/login
/dashboard
/path
/profile
/chat

Direct navigation to these routes must not return a server-side 404 when the application is correctly deployed.

68. Static Asset Deployment

Frontend static assets may include:

JavaScript bundles
CSS
images
icons
fonts
application metadata

The deployment platform should serve these efficiently.

69. Frontend Build Validation

Before deployment, run:

npm run build

The build must complete successfully.

A failed production build must block deployment.

70. Backend Build Validation

The backend must pass:

import validation
dependency installation
linting where configured
tests
application startup
health endpoint validation
71. API Smoke Test

After backend deployment, test:

GET /health

Then test:

POST /auth/register
POST /auth/login
GET /users/me

The exact endpoint paths must follow DOC-08.

72. Learning Workflow Smoke Test

The complete learning workflow must be tested after deployment.

Register
   ↓
Login
   ↓
Profile
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
Updated Recommendation
73. Demo Workflow

The competition demo should use a stable learner account.

The recommended demo flow is:

Open Application
       |
       v
Login / Register
       |
       v
Enter Learning Goal
       |
       v
Set Experience + Skills
       |
       v
Generate Personalized Path
       |
       v
View Skill Gaps
       |
       v
View Roadmap
       |
       v
Open Recommendation
       |
       v
View Explanation
       |
       v
Mark Activity Complete
       |
       v
View Progress
       |
       v
Ask AI Assistant
       |
       v
Submit Feedback
       |
       v
Regenerate / Re-rank
74. Demo Account

A dedicated demo account may be created.

The account should contain non-sensitive demonstration information.

Do not use real personal information unnecessarily.

Example:

Name:
Demo Learner

Email:
demo@pathfinder.local

If the deployment platform requires a real email, use a controlled project account.

75. Demo Data

Demo data should demonstrate the strongest project capabilities.

Recommended demo scenario:

Goal:
Become a Machine Learning Engineer

Experience:
Beginner / Intermediate

Current Skills:
Python
Basic SQL
Basic Mathematics

Skill Gaps:
NumPy
Pandas
Statistics
Machine Learning
Model Evaluation
Deployment

The exact demo profile should remain consistent with the recommendation dataset.

76. Demo Stability

Before the competition submission:

freeze major architecture changes
freeze database schema
verify deployment
verify authentication
verify AI
verify recommendations
verify learning path
verify dashboard
verify progress
verify links
77. Deployment Freeze

A deployment freeze should be considered before the final competition submission.

During the freeze:

Allowed:

critical bug fixes
security fixes
broken deployment fixes
small UI fixes

Avoid:

major architecture changes
database redesign
replacing the AI architecture
replacing the authentication mechanism
large dependency upgrades
78. Production Configuration Checklist

Before deployment, verify:

[ ] APP_ENV configured
[ ] APP_DEBUG disabled
[ ] DATABASE_URL configured
[ ] JWT secret configured
[ ] AI API key configured
[ ] AI model configured
[ ] CORS configured
[ ] API URL configured
[ ] HTTPS enabled
[ ] Database migrations applied
[ ] Seed data available
[ ] Health endpoint working
[ ] Authentication working
79. Frontend Deployment Checklist
[ ] Dependencies install successfully
[ ] Build succeeds
[ ] API base URL is correct
[ ] Production environment variables configured
[ ] Routes work
[ ] Login works
[ ] Dashboard works
[ ] Learning path works
[ ] AI chat works
[ ] Progress works
[ ] No localhost API URL remains
80. Backend Deployment Checklist
[ ] Dependencies install
[ ] Application starts
[ ] Health endpoint works
[ ] Database connection works
[ ] Migrations complete
[ ] Authentication works
[ ] CORS works
[ ] AI service works
[ ] Fallback works
[ ] Recommendation service works
[ ] Learning path generation works
[ ] Logs are readable
81. Database Deployment Checklist
[ ] Database exists
[ ] Credentials configured
[ ] Connection works
[ ] Migrations applied
[ ] Seed data loaded
[ ] Tables exist
[ ] Relationships work
[ ] Indexes exist where required
[ ] Backup strategy understood
82. AI Deployment Checklist
[ ] AI API key configured
[ ] Model configured
[ ] AI request works
[ ] Prompt construction works
[ ] Structured output works
[ ] Timeout configured
[ ] Error handling works
[ ] Fallback works
[ ] No API key exposed to frontend
83. Security Deployment Checklist
[ ] HTTPS enabled
[ ] Passwords hashed
[ ] JWT secret secured
[ ] Database credentials secured
[ ] AI key secured
[ ] CORS restricted
[ ] Debug mode disabled
[ ] Sensitive errors hidden
[ ] Secrets absent from repository
[ ] .env excluded from Git
84. Git Security

Before pushing:

git status

Check for:

.env
.env.local
credentials
keys
tokens
password files

These must not be committed.

85. Git Ignore Requirements

The repository should include appropriate .gitignore entries.

Example:

.env
.env.*
!.env.example

__pycache__/
*.pyc

node_modules/

.venv/
venv/

dist/
build/

*.log

coverage/
.pytest_cache/

.DS_Store

The exact entries must match the technology stack.

86. Deployment Secrets Rotation

If a secret is accidentally committed:

1. Revoke secret
2. Generate new secret
3. Update deployment environment
4. Remove secret from repository history where required
5. Verify application

Simply deleting the secret from the latest commit is not sufficient if it has already been exposed.

87. Backup Strategy

The database should use the backup capabilities provided by the selected database provider where available.

For the prototype:

maintain provider-managed backups
avoid destructive schema changes
keep seed data reproducible
keep migration files in Git
88. Application Backup

Application source code is backed up through:

Git
+
GitHub Repository

The repository is the source of truth for application code.

89. Recovery Strategy

If the deployment is lost:

Clone Repository
       |
       v
Configure Environment
       |
       v
Create Database
       |
       v
Run Migrations
       |
       v
Load Seed Data
       |
       v
Deploy Backend
       |
       v
Deploy Frontend
       |
       v
Run Smoke Tests

The application should be recoverable without manually reconstructing source code.

90. Disaster Recovery Scope

The prototype does not require enterprise disaster recovery.

The minimum recovery goal is:

Source Code Recoverable
Database Recoverable
Configuration Reproducible
Application Redeployable
91. Deployment Dependency Graph

The deployment dependencies are:

Frontend
   |
   v
Backend
   |
   +----> Database
   |
   +----> AI Service
   |
   +----> External Resources

The frontend depends on the backend API.

The backend depends on:

configuration
database
authentication configuration
AI service where applicable

The AI service should not be a hard dependency for deterministic fallback functionality.

92. Startup Dependency Order

Recommended order:

1. Database
2. Backend
3. AI configuration validation
4. Backend health verification
5. Frontend
6. End-to-end validation
93. Shutdown Behavior

The backend should shut down gracefully.

Graceful shutdown should allow:

active requests to complete where practical
database connections to close
resources to be released
94. Resource Limits

The deployment should respect hosting-provider resource limits.

Potential limits include:

CPU
memory
request duration
storage
database connections
API rate limits

The application should avoid unnecessary background processes.

95. Database Connection Pool

The backend should use a controlled database connection pool.

The pool size must be appropriate for the selected hosting environment.

Excessive connections can cause database failures on low-resource hosting plans.

96. AI Cost Management

AI usage should be controlled.

The application should avoid unnecessary AI calls.

AI should be used for tasks where it provides clear value:

goal interpretation
semantic reasoning
explanations
conversational assistance

Deterministic logic should be preferred for:

authentication
progress calculation
status management
basic prerequisite validation
database operations
97. AI Request Caching

Caching may be considered for repeated AI operations.

For example:

Same goal
+
Same learner state
+
Same context

may not always require a new AI request.

Caching is optional for the MVP.

98. Recommendation Computation

Recommendation generation may combine:

Database Filtering
        +
Skill Matching
        +
Semantic Matching
        +
Ranking
        +
Prerequisite Logic
        +
AI Explanation

Only the required components should execute for a particular request.

99. Learning Path Generation

The deployment must support the complete learning path generation pipeline:

Learner Goal
     |
     v
Goal Understanding
     |
     v
Required Skills
     |
     v
Current Skills
     |
     v
Skill Gap
     |
     v
Candidate Resources
     |
     v
Prerequisites
     |
     v
Ranking
     |
     v
Ordered Learning Path
100. Deployment and Recommendation Consistency

The deployed recommendation engine must use the same:

skill IDs
resource IDs
prerequisite relationships
scoring rules
database schema
model configuration

defined in the development environment.

Development and demo environments should not silently use incompatible datasets.

101. Data Versioning

If recommendation data changes significantly, the project should record the data version.

Example:

RESOURCE_DATA_VERSION=1.0
SKILL_DATA_VERSION=1.0

This helps reproduce demonstration results.

102. Model Versioning

If an AI model or embedding model is used, record the configured model name.

Example:

AI_MODEL=<configured-model>
EMBEDDING_MODEL=<configured-model>

The exact model names must be defined by the implementation.

103. AI Prompt Versioning

Important prompts should be treated as application logic.

Where practical, prompt versions should be identifiable.

Example:

GOAL_ANALYSIS_PROMPT_VERSION=1
RECOMMENDATION_EXPLANATION_PROMPT_VERSION=1
CHAT_PROMPT_VERSION=1

This is useful for reproducibility.

104. Deployment Metadata

The deployed backend may expose safe metadata through a non-sensitive endpoint.

Example:

{
  "application": "PathFinder AI",
  "version": "1.0.0",
  "environment": "demo"
}

Sensitive infrastructure details must not be exposed.

105. Monitoring Dashboard

A basic deployment monitoring view should track:

Application Status
API Status
Database Status
AI Status
Recent Errors
Deployment Version

A third-party monitoring dashboard is optional.

106. Uptime Expectations

The competition demo environment should remain available during expected judging periods.

No formal enterprise SLA is required.

The priority is:

Stable
Accessible
Predictable
Recoverable
107. Deployment Testing

Every deployment must undergo smoke testing.

Minimum smoke tests:

Application loads
Login works
API works
Database works
Goal creation works
Recommendation generation works
Learning path works
Dashboard works
AI assistant works
Progress update works
108. End-to-End Deployment Test

The following end-to-end scenario must be tested:

Open Application
       |
       v
Register
       |
       v
Login
       |
       v
Complete Profile
       |
       v
Enter Goal
       |
       v
Enter Current Skills
       |
       v
Generate Skill Gap
       |
       v
Generate Recommendations
       |
       v
Generate Learning Path
       |
       v
Open Resource
       |
       v
Mark Activity In Progress
       |
       v
Mark Activity Completed
       |
       v
Verify Progress
       |
       v
Ask AI Assistant
       |
       v
Submit Feedback
       |
       v
Generate Updated Recommendation
109. Deployment Acceptance Criteria

Deployment is accepted only if:

AC-DEP-001
Frontend is accessible.

AC-DEP-002
Backend health endpoint responds successfully.

AC-DEP-003
Backend connects to database.

AC-DEP-004
Authentication works.

AC-DEP-005
Learner profile can be created.

AC-DEP-006
Goal can be created.

AC-DEP-007
Skill-gap analysis works.

AC-DEP-008
Recommendations are generated.

AC-DEP-009
Learning path is generated.

AC-DEP-010
Recommendation explanation works.

AC-DEP-011
Progress can be updated.

AC-DEP-012
Dashboard displays updated state.

AC-DEP-013
AI assistant responds or fallback activates.

AC-DEP-014
Feedback can be submitted.

AC-DEP-015
Updated recommendation can be generated.

AC-DEP-016
No production secrets are exposed.

AC-DEP-017
Application can be redeployed from the repository.
110. Deployment Failure Categories

Deployment failures shall be categorized as:

BUILD_FAILURE
CONFIGURATION_FAILURE
DATABASE_FAILURE
AUTHENTICATION_FAILURE
API_FAILURE
FRONTEND_FAILURE
AI_FAILURE
EXTERNAL_SERVICE_FAILURE
SECURITY_FAILURE
PERFORMANCE_FAILURE
111. Build Failure

Symptoms:

dependency installation fails
frontend build fails
backend import fails
tests fail

Action:

Review CI logs
     |
     v
Identify dependency/code issue
     |
     v
Fix
     |
     v
Run locally
     |
     v
Push
112. Configuration Failure

Symptoms:

missing environment variable
invalid database URL
invalid AI key
incorrect CORS origin

Action:

Check deployment environment variables
       |
       v
Compare with .env.example
       |
       v
Correct configuration
       |
       v
Restart deployment
113. Database Failure

Symptoms:

connection refused
migration failure
query errors

Action:

Check database availability
       |
       v
Check DATABASE_URL
       |
       v
Check credentials
       |
       v
Check migrations
       |
       v
Run health check
114. Authentication Failure

Symptoms:

registration fails
login fails
token rejected
protected API inaccessible

Action:

Check JWT configuration
       |
       v
Check backend secret
       |
       v
Check frontend API URL
       |
       v
Check CORS
       |
       v
Check authentication middleware
115. AI Failure

Symptoms:

AI response timeout
provider error
invalid API key
model unavailable

Action:

Check AI configuration
       |
       v
Check provider status
       |
       v
Check timeout
       |
       v
Activate fallback
       |
       v
Verify core application remains operational
116. Frontend Failure

Symptoms:

blank screen
failed API calls
route 404
build failure

Action:

Check browser console
       |
       v
Check API URL
       |
       v
Check build logs
       |
       v
Check routing configuration
       |
       v
Redeploy
117. Production Debugging Rules

Production debugging must not involve:

printing secrets
exposing database credentials
disabling authentication
enabling unrestricted CORS permanently
exposing stack traces to users

Debugging should be performed through server-side logs and controlled diagnostics.

118. Local-to-Cloud Consistency

The application should use the same logical architecture locally and remotely.

Local:

Frontend
Backend
Database
AI

Cloud:

Frontend
Backend
Managed Database
AI Provider

Differences should primarily be infrastructure configuration rather than application logic.

119. Development-to-Demo Promotion

The promotion process should be:

Local Development
       |
       v
Automated Tests
       |
       v
Stable Git Commit
       |
       v
Demo Deployment
       |
       v
Smoke Test
       |
       v
Demo Ready
120. Demo Deployment Branch

The primary stable branch is:

main

The final competition deployment should be based on a known stable commit on main.

121. Git Commit Reference

Before final deployment, record:

Git Commit:
<commit-hash>

Version:
<version>

Deployment Date:
<date>

This allows the deployed system to be traced back to source code.

122. Final Competition Deployment

Before submission, the project owner must verify:

Repository
    |
    v
Stable Commit
    |
    v
Deployment
    |
    v
Public URL
    |
    v
Complete Demo
123. Competition Application URL

The final submission should contain:

Application URL:
<deployed-url>

If no public deployment is available, the submission must include clear local execution instructions.

124. Local Execution Fallback

The project must remain runnable locally.

The README should document:

Backend Setup
Frontend Setup
Database Setup
Environment Setup
Migration
Seed Data
Application Startup

The deployment architecture must not make local development dependent on production infrastructure.

125. Local Backend Startup

Example:

cd backend
.\venv\Scripts\activate
uvicorn app.main:app --reload --port 8000

The exact command must match the final implementation.

126. Local Frontend Startup

Example:

cd frontend
npm install
npm run dev

The final command must match the actual frontend implementation.

127. Local Database

The local environment may use:

Local PostgreSQL

or another development database mechanism only if approved by the architecture.

The production database must remain PostgreSQL if that is the final selected technology.

128. Environment Configuration Validation

Before starting locally:

[ ] .env exists
[ ] DATABASE_URL exists
[ ] JWT secret exists
[ ] AI key exists if required
[ ] CORS configured
[ ] frontend API URL configured
129. Deployment Documentation

The repository README must contain:

Project Overview
Prerequisites
Installation
Environment Variables
Database Setup
Backend Setup
Frontend Setup
Local Execution
Testing
Production Deployment
Troubleshooting
130. Deployment Documentation Consistency

README instructions must remain consistent with:

DOC-14
DOC-15
DOC-16
DOC-18
DOC-19

If the technology stack changes, deployment documentation must be updated.

131. Infrastructure Documentation

The deployment provider should be recorded in the final project documentation.

Example:

Frontend Hosting:
<provider>

Backend Hosting:
<provider>

Database:
<provider>

AI Provider:
<provider>

The exact providers remain an implementation/deployment decision.

132. Recommended Prototype Hosting Model

A simple deployment model is preferred:

                 Browser
                    |
                    v
          Managed Frontend Host
                    |
                    | HTTPS
                    v
           Managed Backend Host
                 /       \
                /         \
               v           v
       Managed PostgreSQL  AI Provider

This model minimizes infrastructure management.

133. Alternative Single-Server Model

If necessary, the entire application may be hosted on one server.

Example:

Single Server
   |
   +-- Frontend
   |
   +-- Backend
   |
   +-- Reverse Proxy

Database and AI services may remain external.

This option is simpler in some environments but may be less convenient than managed hosting.

134. Reverse Proxy

If frontend and backend are hosted on the same infrastructure, a reverse proxy may route requests.

Example:

Internet
   |
   v
Reverse Proxy
   |
   +---- / ---> Frontend
   |
   +---- /api ---> Backend

HTTPS termination may occur at the reverse proxy.

135. CDN

A CDN may be used for frontend static assets.

CDN usage is optional for the MVP.

The primary requirement is reliable frontend delivery.

136. Static Caching

Static assets may be cached.

API responses containing personalized learner information should not be cached publicly.

137. Personalized Data Security

The following data must be treated as user-specific:

profile
skills
goals
learning history
progress
feedback
AI conversations
recommendations

Public caching must not expose another learner's information.

138. Database Isolation

Learner data must be logically isolated by authenticated user identity.

Backend queries must apply authorization checks.

Deployment infrastructure must not bypass application-level authorization.

139. Multi-User Considerations

Although the project is a prototype, the deployed application may be accessed by multiple users.

The backend must therefore avoid:

global mutable learner state
hardcoded current user
shared recommendation state
shared progress variables
demo-only user assumptions in production APIs
140. Session Isolation

One user's:

goals
skills
progress
recommendations

must not appear in another user's session.

141. Demo Data Isolation

If a demo account exists, it should remain separate from other users.

Demo data must not be used as a global fallback for authenticated users.

142. Deployment and API Contract

The deployed API must follow DOC-08.

The frontend must not rely on undocumented API behavior.

Any endpoint change must update:

DOC-08
Backend
Frontend
Tests
Deployment Configuration
143. Deployment and Database Contract

The deployed backend must use the database schema defined in DOC-07.

Any schema change must update:

DOC-07
Migrations
Backend Models
Services
Tests
Seed Data
144. Deployment and AI Contract

The deployed AI system must follow DOC-09.

Changes to:

model
prompts
embeddings
ranking
fallback
AI provider

must be reflected in the implementation documentation.

145. Deployment and Frontend Contract

The deployed frontend must follow DOC-10.

The deployment must ensure:

correct API URL
correct routing
correct environment configuration
compatible API version
146. Deployment and Backend Contract

The deployed backend must follow DOC-11.

The deployment must preserve:

modular services
API routers
authentication middleware
database layer
AI service layer
recommendation service
path generation service
147. Deployment and Security Contract

The deployment must follow DOC-12.

No deployment optimization may weaken:

authentication
authorization
password security
secret management
CORS
input validation
148. Deployment and Folder Architecture

The deployment process must respect DOC-13.

Deployment files should be located predictably.

Example:

HCL_PathFinder/
│
├── frontend/
├── backend/
├── docs/
├── tests/
├── .github/
├── .gitignore
├── README.md
└── deployment configuration files

The exact final structure must match DOC-13.

149. Deployment and Technology Stack

The deployment must use the technology stack approved in DOC-14.

No undocumented framework replacement should occur during deployment.

If a hosting limitation requires a change, the relevant architecture documents must be updated.

150. Deployment and Environment Configuration

All deployment configuration must follow DOC-15.

The environment variable names should not be randomly changed between environments.

151. Deployment and Git Workflow

Deployment must follow DOC-16.

The stable deployment should originate from a controlled Git commit.

152. Deployment and Task Breakdown

Deployment-related implementation tasks should be tracked according to DOC-17.

Examples:

DEP-001 Configure frontend hosting
DEP-002 Configure backend hosting
DEP-003 Configure database
DEP-004 Configure environment variables
DEP-005 Configure CORS
DEP-006 Configure health checks
DEP-007 Configure CI
DEP-008 Configure deployment
DEP-009 Run smoke tests
DEP-010 Finalize demo URL
153. Deployment and Testing

Deployment validation must follow DOC-18.

Testing must occur:

Before Deployment
+
After Deployment
154. Deployment Test Levels

The deployment test strategy includes:

Unit Tests
Integration Tests
API Tests
Frontend Tests
End-to-End Tests
Smoke Tests
Production Validation
155. Smoke Test Definition

A smoke test verifies that the deployed application is fundamentally usable.

Minimum:

Open frontend
Login
Create/read profile
Create goal
Generate recommendation
Generate path
Update progress
Use AI
Logout
156. Post-Deployment Validation

After every meaningful deployment:

1. Open frontend
2. Check browser console
3. Test backend health
4. Test login
5. Test profile
6. Test goal
7. Test recommendation
8. Test path
9. Test dashboard
10. Test AI
11. Check logs
157. Deployment Acceptance Gate

A deployment is considered stable only after:

CI = PASS
Build = PASS
Database = PASS
Backend Health = PASS
Frontend = PASS
Authentication = PASS
Core Workflow = PASS
AI/Fallback = PASS
Security Checks = PASS
158. Final Submission Validation

Before submitting the HCLTech Round 2 prototype:

[ ] Source code ZIP ready
[ ] GitHub repository accessible
[ ] Documentation PDF/PPT ready
[ ] Demo video ready
[ ] Application URL ready
[ ] README complete
[ ] Local setup tested
[ ] Public deployment tested
[ ] Demo account tested
[ ] Core workflow tested
159. Demo Video Deployment Validation

The recorded demo must show the actual deployed or final application.

The video should demonstrate:

1. Application opening
2. Learner onboarding
3. Goal input
4. Skill profile
5. Skill-gap analysis
6. Recommendations
7. Personalized roadmap
8. Explanation
9. Progress
10. AI assistant
11. Feedback/adaptation

The demonstrated behavior must match the submitted source code.

160. Pitch-Day Reliability

Before Pitch Day:

Freeze Stable Version
       |
       v
Verify URL
       |
       v
Verify Database
       |
       v
Verify AI
       |
       v
Verify Demo Account
       |
       v
Verify Complete Workflow
       |
       v
Prepare Backup Local Environment
161. Backup Demonstration Strategy

A local backup environment should be available in case the public deployment fails.

The backup should include:

source code
local environment
database seed data
AI configuration
demo account
local startup commands
162. Internet Failure Strategy

If the competition environment has unreliable internet access, the team should maintain a local fallback demonstration.

The local version should be able to demonstrate as much of the workflow as technically possible.

AI-dependent functionality should have a fallback path where practical.

163. External AI Outage Strategy

If the AI provider fails during demonstration:

AI Service Failure
       |
       v
Fallback
       |
       v
Deterministic Recommendation
       |
       v
Template Explanation
       |
       v
Continue Demo

The application should still demonstrate the core personalization concept.

164. External Resource Outage Strategy

If an external course URL fails:

Broken External Resource
       |
       v
Display Resource Metadata
       |
       v
Show Alternative Resource

where an alternative is available.

165. Database Failure Strategy

If the demo database becomes unavailable:

Verify provider status.
Verify connection string.
Verify database availability.
Verify credentials.
Restore from backup if required.
Run migrations if necessary.
Run smoke tests.
166. Deployment Incident Procedure

For a serious issue:

Detect
  |
  v
Assess
  |
  v
Contain
  |
  v
Recover
  |
  v
Validate
  |
  v
Document
167. Incident Severity

Suggested severity levels:

SEV-1
Complete application unavailable.

SEV-2
Critical workflow unavailable.

SEV-3
Major feature degraded.

SEV-4
Minor issue.
168. SEV-1 Response

If the entire application is unavailable:

1. Check hosting provider
2. Check deployment status
3. Check backend logs
4. Check frontend
5. Check database
6. Roll back if required
7. Validate health
169. SEV-2 Response

If a critical feature fails:

Examples:

login
path generation
recommendation engine

Prioritize restoring the core workflow.

170. SEV-3 Response

If a non-critical feature fails:

Examples:

minor visualization
optional filtering
non-essential UI behavior

The application may remain deployed while the issue is fixed.

171. SEV-4 Response

Minor UI or documentation problems can be fixed during normal development.

172. Deployment Security Review

Before final submission, review:

Authentication
Authorization
Secrets
CORS
HTTPS
Input Validation
Database Access
AI Keys
Error Responses
Logs
173. Public Repository Review

The public GitHub repository must be checked for accidental secrets.

Search for:

API_KEY
SECRET
PASSWORD
TOKEN
DATABASE_URL
JWT
PRIVATE_KEY

No real secret values should be present.

174. Repository Hygiene

The repository should not contain:

node_modules/
.venv/
__pycache__/
.env
large binaries
temporary files
IDE-specific temporary files
production secrets
175. Deployment Artifact Hygiene

Deployment artifacts should not include:

unnecessary development dependencies
secret files
local databases
temporary logs
cache directories
176. Application Configuration Matrix
Configuration	Development	Testing	Demo
APP_ENV	development	testing	production/demo
Debug	Enabled if needed	Controlled	Disabled
Database	Local/Test	Test DB	Managed DB
AI	Configured	Mock/Configured	Configured
CORS	Local origin	Test origin	Public frontend
Logging	Debug/Info	Info	Info/Error
HTTPS	Optional local	Required where public	Required
Secrets	Local env	CI secrets	Hosting secrets
177. Service Dependency Matrix
Component	Frontend	Backend	Database	AI
Frontend	—	Required	No direct access	No direct access
Backend	No	—	Required	Optional/Configured
Database	No	Required	—	No
AI Service	No	Required where AI features used	No	—
178. Deployment Responsibility

Although the original competition team contains multiple members, the implementation and deployment workflow defined in this document assumes:

Primary Implementation Owner:
Siddharth Ghawate

All project documentation should therefore remain executable by a single developer.

No deployment step should depend on another team member's local machine.

179. Single-Developer Deployment Requirement

The project must be maintainable by one developer.

The developer must be able to:

clone repository
install dependencies
configure environment
initialize database
run tests
deploy backend
deploy frontend
verify application
update deployment

without requiring undocumented manual actions.

180. Knowledge Transfer Requirement

Even though the implementation owner is a single developer, documentation should remain sufficiently clear for another developer to understand:

architecture
deployment
configuration
database
API
AI layer
troubleshooting
181. Deployment Runbook

The final deployment runbook should contain:

Prerequisites
Environment Variables
Database Setup
Migration
Backend Deployment
Frontend Deployment
Health Check
Smoke Test
Rollback
Troubleshooting
182. Deployment Runbook — Quick Version
1. Pull latest main
2. Install dependencies
3. Run tests
4. Build frontend
5. Verify backend
6. Configure environment
7. Apply database migrations
8. Deploy backend
9. Deploy frontend
10. Verify /health
11. Test login
12. Test goal
13. Test recommendation
14. Test learning path
15. Test progress
16. Test AI
17. Verify logs
18. Record deployed commit
183. Final Deployment Architecture

The final logical architecture is:

                         USER
                          |
                          v
                    HTTPS Browser
                          |
                          v
                 +------------------+
                 |    FRONTEND      |
                 | React Application |
                 +--------+---------+
                          |
                          | REST / HTTPS
                          v
                 +------------------+
                 |    BACKEND API   |
                 |     FastAPI      |
                 +--------+---------+
                          |
          +---------------+----------------+
          |               |                |
          v               v                v
   +-------------+  +------------+  +--------------+
   | PostgreSQL  |  | AI Service |  | External     |
   | Database    |  | LLM/Models |  | Resources    |
   +-------------+  +------------+  +--------------+
          |
          v
   Learner / Skill /
   Resource / Path /
   Progress Data
184. Core Deployment Principle

The most important deployment principle is:

Simple
+
Secure
+
Reproducible
+
Tested
+
Recoverable

The system should not introduce infrastructure complexity unless it directly supports a project requirement.

185. Deployment Traceability

Deployment requirements must trace to:

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
DOC-14 Technology Stack
DOC-15 Environment and Configuration
DOC-16 Git/GitHub Workflow
DOC-17 GitHub Task Breakdown
DOC-18 Testing and QA
DOC-19 Deployment Architecture
186. Deployment Requirement IDs

Deployment-specific requirements use the prefix:

DEP

Examples:

DEP-001
Frontend must be deployable.

DEP-002
Backend must be deployable independently.

DEP-003
Database configuration must be environment-based.

DEP-004
Secrets must not be committed.

DEP-005
Health checks must be available.

DEP-006
Deployment must support smoke testing.

DEP-007
Deployment must support recovery.

DEP-008
Demo environment must support the complete learner journey.
187. Deployment Traceability Matrix
Deployment Requirement	Related Document
DEP-001	DOC-10
DEP-002	DOC-11
DEP-003	DOC-15
DEP-004	DOC-12
DEP-005	DOC-18
DEP-006	DOC-18
DEP-007	DOC-16
DEP-008	DOC-02
DEP-009	DOC-06
DEP-010	DOC-14
188. Deployment Completion Criteria

DOC-19 is considered complete when:

[ ] Deployment architecture is defined
[ ] Frontend deployment is defined
[ ] Backend deployment is defined
[ ] Database deployment is defined
[ ] AI deployment is defined
[ ] Environment strategy is defined
[ ] Secret management is defined
[ ] CI/CD strategy is defined
[ ] Health checks are defined
[ ] Monitoring is defined
[ ] Backup strategy is defined
[ ] Recovery strategy is defined
[ ] Rollback strategy is defined
[ ] Demo deployment is defined
[ ] Competition submission validation is defined
[ ] Single-developer deployment is supported
[ ] Deployment traceability is established
189. Implementation Notes

The actual hosting providers may be selected during implementation.

The architecture must remain provider-neutral until the final deployment decision.

Once hosting providers are selected, the following must be updated:

DOC-14 Technology Stack
DOC-15 Environment Configuration
DOC-19 Deployment Architecture
README.md
GitHub Actions
Deployment configuration
190. Future Deployment Improvements

Future versions may introduce:

Docker-based production deployment
automated CI/CD
advanced monitoring
error tracking
CDN
Redis caching
background workers
scheduled recommendation refresh
scalable AI inference
managed vector database
horizontal backend scaling
infrastructure-as-code

These are not mandatory for the competition MVP.

191. Final Architecture Decision

For the competition prototype, the preferred deployment architecture is:

Managed Frontend
        |
        v
Managed Backend API
        |
        +------> Managed PostgreSQL
        |
        +------> External AI Provider
        |
        +------> External Learning Resources

This architecture provides a practical balance between:

functionality
reliability
security
simplicity
development speed
demonstration readiness
192. Final Deployment Statement

PathFinder AI shall be deployed as a secure web application consisting of a frontend client, backend API, PostgreSQL database, and AI/recommendation layer.

The frontend shall communicate with the backend through the API contract defined in DOC-08.

The backend shall manage authentication, authorization, learner data, recommendations, learning paths, progress, feedback, and AI orchestration.

The database shall provide persistent storage for learner and learning-path data.

The AI layer shall provide natural-language understanding, semantic reasoning, explanations, and conversational functionality while maintaining deterministic fallback behavior where possible.

All environment-specific values shall be externalized through configuration.

All sensitive credentials shall remain outside the Git repository.

The deployed application shall be validated through automated testing and post-deployment smoke testing.

The final competition environment shall support the complete personalized learning journey from learner onboarding through goal definition, skill-gap analysis, recommendation, learning-path generation, progress tracking, feedback, and adaptive recommendation.

The deployment must remain reproducible by a single developer from the GitHub repository.

193. Document Status

Document ID: DOC-19

Document Name: Deployment Architecture

Project: PathFinder AI

Status: DRAFT

Implementation Owner: Siddharth Ghawate

Parent Documents:

DOC-06 System Architecture
DOC-11 Backend Architecture
DOC-12 Authentication and Security
DOC-14 Technology Stack and Dependencies
DOC-15 Environment and Configuration
DOC-18 Testing and QA Strategy

Next Document: DOC-20 Integration Checklist

Implementation Status: NOT STARTED

Deployment Status: NOT DEPLOYED

Architecture Status: DEFINED FOR PROTOTYPE

194. Change Control

Any deployment architecture change must identify:

Changed component
Reason for change
Affected environment
Affected infrastructure
Affected API
Affected database
Affected security controls
Affected tests
Affected documentation

Changes must be reflected in the relevant architecture documents.

195. Approval

The deployment architecture shall be considered approved for implementation when:

[ ] Technology stack is finalized
[ ] Hosting strategy is selected
[ ] Environment variables are finalized
[ ] Database deployment is finalized
[ ] AI provider is finalized
[ ] Security configuration is reviewed
[ ] Testing strategy is confirmed
[ ] Local deployment is verified
[ ] Demo deployment is verified
END OF DOC-19

### After pasting DOC-19

Run these commands:

```powershell
git status
git add docs/19-deployment-architecture.md
git commit -m "docs: add deployment architecture"
git push origin main
git status

The final command should show:

nothing to commit, working tree clean

Next is DOC-20 — Integration Checklist. It will connect the frontend ↔ backend ↔ database ↔ authentication ↔ AI ↔ recommendation engine ↔ learning path ↔ progress flow, so this one is particularly important before we move into actual implementation.