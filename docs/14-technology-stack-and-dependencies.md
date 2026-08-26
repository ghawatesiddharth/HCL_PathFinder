# PathFinder AI — Technology Stack and Dependencies

**Document ID:** DOC-14  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT / ARCHITECTURE BASELINE  
**Parent Documents:** DOC-01, DOC-02, DOC-06, DOC-07, DOC-08, DOC-09, DOC-10, DOC-11, DOC-12, DOC-13  
**Next Dependent Document:** DOC-15 Environment and Configuration  
**Related Documents:** DOC-16, DOC-18, DOC-19, DOC-20, DOC-21, DOC-22, DOC-23

---

# 1. Purpose

This document defines the approved technology stack, frameworks, libraries, runtime versions, development tools, AI/ML dependencies, database technologies, testing tools, deployment technologies, and supporting dependencies for PathFinder AI.

The purpose of this document is to prevent technology drift during implementation.

Every developer must use the technologies defined in this document unless a formal Architecture Decision Record is created and the change is approved.

This document acts as the technology baseline for the complete project.

---

# 2. Technology Selection Principles

The PathFinder AI technology stack shall follow the following principles:

1. Simplicity
2. Maintainability
3. Reproducibility
4. Rapid MVP development
5. AI/ML compatibility
6. API-first architecture
7. Strong typing where practical
8. Clear separation of concerns
9. Easy local development
10. Easy GitHub collaboration
11. Easy deployment
12. Minimal unnecessary dependencies
13. Testability
14. Security
15. Future scalability

The project is a prototype/MVP for the HCLTech competition.

Therefore, the architecture should not introduce unnecessary enterprise complexity.

---

# 3. Approved Technology Stack Summary

The baseline stack is:

| Layer | Technology |
|---|---|
| Frontend | React |
| Frontend Language | TypeScript |
| Frontend Build Tool | Vite |
| Frontend Styling | Tailwind CSS |
| Frontend Routing | React Router |
| Frontend Server State | TanStack Query |
| Frontend Forms | React Hook Form |
| Frontend Validation | Zod |
| Charts | Recharts |
| Backend | Python |
| Backend Framework | FastAPI |
| API Server | Uvicorn |
| API Validation | Pydantic |
| ORM | SQLAlchemy |
| Database Migration | Alembic |
| Database | PostgreSQL |
| Authentication | JWT |
| Password Hashing | bcrypt/passlib-compatible implementation |
| AI/ML Language | Python |
| ML Framework | scikit-learn |
| Semantic Embeddings | sentence-transformers |
| Vector Similarity | NumPy / cosine similarity |
| Data Processing | pandas |
| Scientific Computing | NumPy |
| Optional LLM | Provider abstraction |
| Testing Backend | pytest |
| Testing Frontend | Vitest |
| Frontend Component Testing | React Testing Library |
| API Testing | httpx |
| Code Formatting | Black |
| Python Linting | Ruff |
| Frontend Formatting | Prettier |
| Frontend Linting | ESLint |
| API Documentation | OpenAPI |
| Containerization | Docker |
| Version Control | Git |
| Repository | GitHub |
| Environment Variables | `.env` |
| Deployment | Container-compatible cloud platform |
| CI | GitHub Actions |

---

# 4. Technology Architecture

The application will follow the following high-level technology architecture.

```text
                         USER
                           |
                           v
                  +----------------+
                  |    Browser     |
                  | React + TS     |
                  +----------------+
                           |
                           | HTTPS / JSON
                           v
                  +----------------+
                  |   FastAPI      |
                  | REST API       |
                  +----------------+
                           |
            +--------------+--------------+
            |              |              |
            v              v              v
      +-----------+  +-----------+  +-----------+
      | Auth      |  | Business  |  | AI/ML     |
      | Services  |  | Services  |  | Services  |
      +-----------+  +-----------+  +-----------+
            |              |              |
            |              |              |
            +--------------+--------------+
                           |
                           v
                    +-------------+
                    | SQLAlchemy  |
                    | Data Layer  |
                    +-------------+
                           |
                           v
                    +-------------+
                    | PostgreSQL  |
                    +-------------+

AI/ML Services
      |
      +--> Skill Processing
      |
      +--> Embeddings
      |
      +--> Semantic Matching
      |
      +--> Skill Gap Analysis
      |
      +--> Recommendation Ranking
      |
      +--> Path Generation
      |
      +--> Explanation Layer
      |
      +--> Optional LLM Provider
5. Frontend Technology Stack
5.1 Frontend Framework

The frontend shall use:

React

React is selected because:

it is widely adopted
it supports component-based architecture
it provides strong ecosystem support
it is suitable for dashboard interfaces
it works well with REST APIs
it is suitable for rapid MVP development
6. Frontend Programming Language

The frontend shall use:

TypeScript

TypeScript is mandatory for application code.

JavaScript-only application files should not be introduced unless explicitly justified.

TypeScript provides:

static typing
better IDE support
safer API integration
improved refactoring
better maintainability
reduced runtime errors
7. Frontend Build Tool

The frontend shall use:

Vite

Vite shall be responsible for:

development server
module bundling
production builds
environment variable handling
frontend development workflow

The standard command shall be:

npm run dev

Production build:

npm run build

Preview:

npm run preview
8. Frontend Package Manager

The project shall use:

npm

The frontend dependency lock file shall be committed.

Required file:

frontend/package-lock.json

Developers shall not mix package managers.

The project shall not simultaneously use:

npm
yarn
pnpm

as competing package management systems.

9. Frontend Routing

The frontend shall use:

react-router-dom

Routing responsibilities include:

/login
/register
/onboarding
/dashboard
/goals
/learning-path
/resources
/skills
/chat
/progress
/profile

The exact routes shall follow DOC-10.

10. Frontend Styling

The baseline styling framework shall be:

Tailwind CSS

Tailwind shall be used for:

layout
spacing
responsive design
typography
cards
buttons
forms
dashboards
navigation
status indicators

Custom CSS may be added when necessary.

However, styling should not become fragmented across many independent CSS systems.

11. Frontend State Management

Application state shall be divided into:

Local UI State
Server State
Authentication State
Form State

The project should avoid unnecessary global state.

Server state shall primarily use:

TanStack Query

Examples:

GET /goals
GET /learning-paths
GET /recommendations
GET /progress
GET /skills

TanStack Query shall handle:

API requests
caching
loading state
error state
refetching
invalidation
12. Frontend API Communication

The frontend shall communicate with the backend through HTTP/HTTPS REST APIs.

The baseline HTTP client shall be:

fetch

An additional HTTP abstraction layer may be implemented around fetch.

The frontend shall not directly communicate with PostgreSQL.

The frontend shall never contain database credentials.

13. Frontend Forms

The preferred form library shall be:

react-hook-form

Form responsibilities include:

registration
login
profile
goals
skills
feedback
learning preferences
14. Frontend Validation

The preferred schema validation library shall be:

zod

Validation shall occur at the frontend boundary for user experience.

Backend validation remains mandatory.

Frontend validation shall never replace backend validation.

15. Frontend Data Visualization

The preferred charting library shall be:

recharts

Charts may be used for:

skill development
learning progress
milestone completion
activity completion
recommendation statistics

The dashboard shall prioritize clarity over excessive visualization.

16. Frontend Testing

The frontend testing stack shall include:

Vitest
React Testing Library

Testing shall cover:

components
forms
validation
API states
navigation
user interactions
error states
17. Backend Technology Stack

The backend shall use:

Python
FastAPI
Uvicorn
Pydantic
SQLAlchemy
Alembic
PostgreSQL
18. Backend Programming Language

The backend shall use:

Python 3.11+

The preferred project runtime baseline is:

Python 3.11

All developers should use the same major/minor Python version during development.

Python version changes require a dependency compatibility review.

19. Backend Framework

The backend shall use:

FastAPI

FastAPI shall provide:

REST API
request validation
response validation
dependency injection
OpenAPI documentation
authentication integration
asynchronous request handling where required
20. API Server

The development and production application server shall use:

Uvicorn

Development command:

uvicorn app.main:app --reload

Production execution shall disable development reload.

21. Backend Validation

The backend shall use:

Pydantic

Pydantic models shall define:

request schemas
response schemas
configuration schemas
validation rules
structured AI outputs where applicable

Example conceptual structure:

UserCreate
UserResponse
LoginRequest
LoginResponse
GoalCreate
GoalResponse
SkillResponse
RecommendationResponse
LearningPathResponse
ProgressResponse
ChatRequest
ChatResponse
22. ORM

The database abstraction layer shall use:

SQLAlchemy

SQLAlchemy shall be responsible for:

database models
relationships
queries
transactions
persistence
database abstraction

Application business logic shall not contain raw SQL everywhere.

Raw SQL may be used when there is a justified performance or database-specific requirement.

23. Database Migration Tool

Database schema migrations shall use:

Alembic

Every production-relevant schema change shall have a migration.

Developers shall not manually modify the production database schema without recording the change through migration management.

Typical migration workflow:

alembic revision --autogenerate -m "description"

Then:

alembic upgrade head
24. Database

The primary relational database shall be:

PostgreSQL

PostgreSQL is selected because it supports:

relational data
transactions
constraints
indexes
JSON fields where required
strong integrity
production deployment
future scalability
25. Database Responsibility

PostgreSQL shall store persistent application data.

Examples:

Users
Profiles
Goals
Skills
User Skills
Resources
Resource Skills
Skill Dependencies
Learning Paths
Path Items
Progress
Feedback
Chat Sessions
Chat Messages

The exact schema is defined in:

DOC-07 Database Design
26. Database Access Rule

The following architecture is mandatory:

Frontend
   |
   v
FastAPI
   |
   v
Service Layer
   |
   v
Repository/Data Access Layer
   |
   v
SQLAlchemy
   |
   v
PostgreSQL

The frontend must never access PostgreSQL directly.

AI services must not directly manipulate database tables without passing through the backend data-access architecture.

27. Authentication Technology

Authentication shall be implemented using:

JWT

JSON Web Tokens shall be used for authenticated API access.

Authentication implementation shall follow:

DOC-12 Authentication and Security
28. Password Hashing

Passwords shall never be stored directly.

Password hashing shall use a secure password hashing implementation.

The selected baseline is:

bcrypt

or the corresponding supported password hashing implementation through the chosen authentication library.

Plaintext password storage is prohibited.

29. Token Handling

Authentication tokens shall:

have expiration
be signed using a secret
not contain sensitive information unnecessarily
be validated on protected endpoints
never be committed to Git

Token configuration shall be controlled through environment variables.

30. AI/ML Technology Stack

The AI/ML layer shall use Python.

Baseline libraries:

numpy
pandas
scikit-learn
sentence-transformers

Optional LLM provider libraries shall be isolated behind a provider abstraction.

31. NumPy

NumPy shall be used for:

numerical computation
vector operations
similarity calculations
numerical transformations
32. pandas

pandas shall be used for:

dataset loading
preprocessing
resource catalog preparation
analysis
feature engineering
evaluation

pandas should primarily be used in:

data/
ml/
scripts/
notebooks/

and not unnecessarily inside high-frequency API endpoints.

33. scikit-learn

scikit-learn shall be used for deterministic or classical recommendation components where appropriate.

Potential applications include:

cosine similarity
TF-IDF
nearest-neighbor retrieval
ranking features
evaluation metrics
preprocessing
34. Sentence Transformers

Semantic embeddings shall use:

sentence-transformers

Embeddings may be generated for:

learner goals
skills
resource descriptions
course descriptions
project descriptions
learning objectives

Semantic similarity can then be calculated between learner requirements and resources.

35. Semantic Similarity

The baseline semantic similarity method shall use:

Cosine Similarity

Conceptual flow:

Learner Goal
      |
      v
Embedding Model
      |
      v
Goal Vector
      |
      v
Compare Against
Resource Vectors
      |
      v
Cosine Similarity
      |
      v
Candidate Ranking
36. Recommendation Architecture

The recommendation engine shall not depend on a single AI model.

The preferred architecture is hybrid.

                 Learner Profile
                       |
                       v
                  Goal Analysis
                       |
                       v
                  Skill Gap
                       |
                       v
              Candidate Generation
                       |
             +---------+---------+
             |                   |
             v                   v
       Semantic Score      Rule-Based Score
             |                   |
             +---------+---------+
                       |
                       v
                 Ranking Layer
                       |
                       v
              Personalized Results
37. Recommendation Scoring

A recommendation score may combine:

semantic relevance
skill gap relevance
prerequisite validity
difficulty fit
learner preference
resource quality
progress state
feedback

Conceptual scoring:

Final Score =
    Semantic Relevance
    + Skill Gap Relevance
    + Goal Alignment
    + Difficulty Fit
    + Preference Fit
    + Resource Quality
    + Feedback Adjustment
    - Prerequisite Penalty

The exact formula shall be defined in DOC-09.

38. AI Provider Abstraction

If an external LLM is used, application code shall not directly depend on provider-specific APIs throughout the codebase.

Instead:

Application
    |
    v
AI Service Interface
    |
    +------> Provider A
    |
    +------> Provider B
    |
    +------> Local/Fallback

This prevents vendor lock-in.

39. LLM Responsibilities

An LLM may be used for:

natural-language goal interpretation
structured goal extraction
conversational responses
recommendation explanation
learning assistance
question answering

The LLM shall not be the sole authority for:

user authorization
database permissions
progress calculation
prerequisite validation
authentication
deterministic scoring
security decisions
40. Structured AI Output

AI-generated structured information shall be validated.

For example:

{
  "target_role": "Machine Learning Engineer",
  "domain": "Artificial Intelligence",
  "skills": [
    "Python",
    "NumPy",
    "Pandas",
    "Machine Learning"
  ],
  "experience_level": "Beginner"
}

The backend shall validate such outputs using Pydantic schemas.

41. AI Failure Strategy

AI services can fail.

Possible failures include:

timeout
rate limit
invalid response
provider unavailable
malformed output
network failure
API quota exhausted

The application shall handle these failures gracefully.

The system should fall back to deterministic logic wherever possible.

42. AI Dependency Isolation

AI-specific dependencies shall be isolated from general business logic.

Preferred architecture:

app/
    services/
        ai/
            base.py
            goal_parser.py
            explanation_service.py
            recommendation_ai.py

Business services should communicate with AI services through clear interfaces.

43. Vector Storage Strategy

For the MVP, the project shall avoid introducing a separate vector database unless required.

The initial implementation may use:

PostgreSQL
+
stored embeddings
+
application-level similarity

or an appropriate PostgreSQL-compatible vector extension if deployment supports it.

A dedicated vector database shall not be introduced without an ADR.

44. Caching

Caching is not mandatory for the first MVP.

If caching becomes necessary, the architecture may introduce:

Redis

However:

Redis is optional for MVP.

The application must not require Redis unless DOC-22 records the decision.

45. Background Processing

Long-running operations should not block normal API requests.

Potential future operations include:

embedding generation
resource ingestion
large recommendation calculations
dataset processing

For the MVP, synchronous execution is acceptable where execution time is small.

Background processing may later use:

Celery

or another task queue.

No task queue is mandatory for MVP.

46. External Resource Integration

Learning resources may originate from:

internal dataset
external APIs
curated URLs
public learning platforms

External integrations shall be isolated behind service modules.

Example:

app/
    integrations/
        learning_resources/
            base.py
            provider_a.py
            provider_b.py
47. HTTP Client

Backend external HTTP requests shall use:

httpx

The HTTP client shall support:

timeouts
error handling
connection management
response validation

External requests must not have unlimited timeouts.

48. Configuration Management

Configuration shall use environment variables.

Example:

DATABASE_URL
JWT_SECRET_KEY
JWT_ALGORITHM
ACCESS_TOKEN_EXPIRE_MINUTES
AI_API_KEY
AI_MODEL
CORS_ORIGINS
ENVIRONMENT

Secrets must not be hardcoded.

49. Environment File

Local development shall use:

.env

A safe example file shall be provided:

.env.example

The actual .env file must not be committed.

50. Git Ignore Requirements

The repository shall contain:

.gitignore

It must exclude at minimum:

.env
.env.*
__pycache__/
*.pyc
.venv/
venv/
node_modules/
dist/
build/
coverage/
.pytest_cache/
.mypy_cache/
.vscode/
.idea/

Exceptions may be made for:

.env.example
51. Python Dependency Management

Python dependencies shall be declared in:

backend/requirements.txt

A lock or pinned dependency strategy should be used for reproducibility.

Major dependencies should use compatible versions rather than unrestricted latest versions.

52. Frontend Dependency Management

Frontend dependencies shall be declared in:

frontend/package.json

The lock file shall be committed:

frontend/package-lock.json
53. Backend Core Dependencies

The backend baseline dependencies are:

fastapi
uvicorn
pydantic
pydantic-settings
sqlalchemy
alembic
psycopg
python-jose
passlib/bcrypt-compatible password hashing package
python-multipart
email-validator
httpx

Exact versions shall be frozen during environment setup.

54. AI/ML Dependencies

The baseline AI/ML dependencies are:

numpy
pandas
scikit-learn
sentence-transformers

Additional libraries may be introduced only when justified by the implementation.

55. Testing Dependencies

Backend:

pytest
pytest-asyncio
httpx

Frontend:

vitest
@testing-library/react
@testing-library/jest-dom
@testing-library/user-event
56. Code Quality Tools

Python:

ruff
black

Frontend:

eslint
prettier

These tools shall be integrated into development workflow where practical.

57. Python Formatting

The standard Python formatter shall be:

Black

Developers should not manually maintain competing formatting conventions.

58. Python Linting

The standard Python linting tool shall be:

Ruff

Ruff should be configured at repository level.

59. Frontend Formatting

The standard frontend formatter shall be:

Prettier
60. Frontend Linting

The standard frontend linter shall be:

ESLint
61. API Documentation

FastAPI shall automatically generate OpenAPI documentation.

Expected development URLs:

/docs

and:

/redoc

The API contract shall additionally be documented in:

DOC-08 API Contract
62. OpenAPI Rule

The generated OpenAPI schema must remain consistent with DOC-08.

If an endpoint changes:

DOC-08
backend implementation
frontend API client
tests
DOC-23 traceability

must be reviewed.

63. Containerization

The application shall support Docker-based execution.

The repository may contain:

Dockerfile
docker-compose.yml

or:

compose.yaml

The final choice shall be documented in DOC-19.

64. Docker Services

The development architecture may include:

frontend
backend
postgres

Optional services:

redis

Redis is not mandatory for MVP.

65. Development Environment

Minimum development tools:

Git
Node.js
npm
Python
pip
PostgreSQL
VS Code
Docker

Docker may be used to simplify PostgreSQL setup.

66. Recommended Runtime Versions

Baseline:

Python 3.11
Node.js 20 LTS
npm compatible with Node.js 20
PostgreSQL 16

These versions form the initial project baseline.

If a newer stable version becomes necessary, compatibility must be checked before changing the baseline.

67. Operating System Support

Development should support:

Windows
Linux
macOS

Windows development shall be explicitly supported because team members may use Windows.

Commands should avoid unnecessary OS-specific assumptions.

68. Windows Development

PowerShell commands may be documented for Windows.

Example:

python -m venv .venv

Activation:

.\.venv\Scripts\Activate.ps1

Linux/macOS:

python3 -m venv .venv
source .venv/bin/activate
69. Project Runtime Structure

The technology stack shall map to the following high-level structure:

HCL_PathFinder/
│
├── frontend/
│
├── backend/
│
├── data/
│
├── ml/
│
├── scripts/
│
├── tests/
│
├── docs/
│
├── .github/
│
├── .gitignore
├── .env.example
├── README.md
├── docker-compose.yml
└── LICENSE

The exact folder structure shall follow:

DOC-13 Code and Folder Architecture
70. Frontend Dependency Structure

Conceptual frontend dependencies:

frontend/
├── package.json
├── package-lock.json
├── tsconfig.json
├── vite.config.ts
├── eslint.config.js
├── index.html
└── src/
71. Frontend Core Packages

Expected packages:

react
react-dom
react-router-dom
@tanstack/react-query
react-hook-form
zod
recharts

Additional UI packages may be introduced only where necessary.

72. Frontend Development Packages

Expected development packages include:

typescript
vite
vitest
eslint
prettier
@testing-library/react
@testing-library/jest-dom
@testing-library/user-event

Exact versions shall be locked in package-lock.json.

73. Backend Structure

Conceptual backend dependency structure:

backend/
├── requirements.txt
├── alembic.ini
├── app/
│   ├── main.py
│   ├── core/
│   ├── api/
│   ├── models/
│   ├── schemas/
│   ├── services/
│   ├── repositories/
│   ├── db/
│   ├── integrations/
│   └── ml/
└── tests/
74. AI/ML Structure

The AI/ML layer should follow:

ml/
├── data/
├── preprocessing/
├── embeddings/
├── skill_gap/
├── recommendation/
├── ranking/
├── evaluation/
└── models/

The implementation should avoid putting all AI logic into a single file.

75. Data Directory

The project may contain:

data/
├── raw/
├── processed/
├── external/
└── README.md

Raw datasets should remain immutable whenever possible.

Processed datasets should be generated through documented scripts.

76. Dataset Policy

Datasets shall have:

source
license
version
collection date
preprocessing procedure
schema
limitations

Dataset documentation shall be maintained.

77. Model Artifact Policy

Generated ML artifacts shall not automatically be committed to Git.

Large files should use:

Git LFS

or external model storage if necessary.

The MVP should prefer lightweight models where possible.

78. Model Reproducibility

Every trained or generated model should have:

model name
model version
embedding model
dataset version
preprocessing version
configuration
evaluation result

recorded where applicable.

79. Embedding Model Policy

The embedding model shall be configurable.

Example:

EMBEDDING_MODEL=sentence-transformers-model-name

The model name must not be hardcoded throughout application logic.

80. LLM Model Configuration

If an LLM is used:

LLM_PROVIDER
LLM_MODEL
LLM_API_KEY
LLM_TEMPERATURE
LLM_MAX_TOKENS

shall be configurable through environment variables.

81. External API Keys

External API keys must:

exist only in environment configuration
never be committed
never be displayed in logs
never be sent to frontend clients
never be embedded in source code
82. CORS

The backend shall explicitly configure allowed frontend origins.

Example:

http://localhost:5173

Production origins shall be configured through environment variables.

Wildcard CORS should not be used in production unless explicitly justified.

83. Logging

The backend shall use Python logging.

Logs may include:

request ID
endpoint
status code
processing time
error category
service name

Logs must not include:

passwords
JWT secrets
API keys
database passwords
sensitive user information
84. Error Handling Stack

The backend shall use:

FastAPI exception handlers
Pydantic validation
structured error responses
logging

Frontend shall display user-friendly error messages.

85. API Error Format

The API should use a consistent error structure.

Example:

{
  "error": {
    "code": "GOAL_NOT_FOUND",
    "message": "The requested goal could not be found."
  }
}

The exact format must remain consistent with DOC-08.

86. Authentication Dependencies

Authentication-related implementation may use:

python-jose

or an approved JWT implementation.

Password hashing shall use:

bcrypt-compatible secure hashing

Authentication implementation shall remain consistent with DOC-12.

87. Database Driver

PostgreSQL access shall use:

psycopg

The SQLAlchemy connection layer shall use the supported PostgreSQL driver.

The database URL shall be configurable.

Example:

postgresql+psycopg://username:password@localhost:5432/pathfinder
88. Migration Policy

Every schema change must follow:

Modify SQLAlchemy model
        |
        v
Generate migration
        |
        v
Review migration
        |
        v
Test migration
        |
        v
Commit migration
        |
        v
Apply migration
89. Dependency Version Policy

Dependencies shall not be blindly upgraded during development.

A dependency upgrade must consider:

breaking changes
API changes
security fixes
compatibility
test results
deployment compatibility
90. Version Pinning

The project should use pinned or controlled dependency versions for reproducibility.

Example:

fastapi==<approved-version>

rather than:

fastapi

for final reproducible builds.

Exact versions shall be frozen during implementation.

91. Security Dependency Updates

Security-related dependency updates shall receive priority.

If a critical vulnerability is discovered:

identify affected dependency
check compatible version
update dependency
run tests
run security checks
document the change
commit the change
92. Dependency Addition Rule

Before adding a new library, the developer must determine:

Why is it required?
Can an existing dependency solve the problem?
Does it increase security risk?
Does it increase deployment complexity?
Is it maintained?
Is it compatible with the current Python/Node version?
Does it introduce licensing concerns?
93. Dependency Review

Every major dependency should be reviewed for:

maintenance
license
security
community adoption
documentation
compatibility
bundle size
performance
94. Avoided Technologies

The MVP shall avoid unnecessary introduction of:

Kubernetes
Kafka
RabbitMQ
microservice architecture
multiple databases
multiple frontend frameworks
multiple backend frameworks
multiple ORMs
multiple state-management libraries
dedicated vector database
complex distributed systems

unless a documented requirement requires them.

95. Monolith Decision

The MVP shall use a modular monolithic backend.

Architecture:

One Backend Application
        |
        +-- Authentication
        +-- Users
        +-- Goals
        +-- Skills
        +-- Recommendations
        +-- Learning Paths
        +-- Progress
        +-- Feedback
        +-- AI Services

This provides faster development and easier deployment.

96. Microservices Policy

Microservices are not part of the MVP architecture.

They may be considered in future versions if:

scale requires them
team structure requires them
deployment requirements require them

Any transition must be documented in DOC-22.

97. API Architecture

The backend shall expose REST APIs.

Primary communication:

JSON over HTTP/HTTPS

The API shall be versioned where required.

Potential prefix:

/api/v1

The final path convention shall follow DOC-08.

98. Frontend-Backend Contract

The frontend shall consume only documented API responses.

The frontend shall not make assumptions about undocumented backend fields.

If a response changes:

backend schema
API contract
frontend types
tests
documentation

must be reviewed.

99. Shared Type Strategy

Frontend TypeScript types should correspond to backend Pydantic schemas.

Example:

Backend:
GoalResponse

Frontend:
Goal

The project may later introduce generated API types from OpenAPI.

For MVP, manually maintained TypeScript interfaces are acceptable if kept synchronized.

100. API Type Generation

Automatic OpenAPI type generation may be introduced later.

Possible tools include:

openapi-typescript

This is optional for MVP.

101. Environment Separation

The application shall support at least:

development
testing
production

Environment-specific configuration must not be hardcoded.

102. Development Environment

Development should use:

React + Vite
FastAPI + Uvicorn
PostgreSQL
Local AI/ML models or configured external provider
103. Testing Environment

Testing should use isolated configuration.

Testing must not unintentionally use production database credentials.

Where possible:

test database

shall be used.

104. Production Environment

Production should use:

React production build
FastAPI
Uvicorn
PostgreSQL
HTTPS
environment-based secrets
105. Deployment Strategy

The architecture shall prefer a simple deployment model suitable for a competition prototype.

Preferred conceptual deployment:

Internet
   |
   v
Frontend Hosting
   |
   HTTPS
   |
   v
Backend Hosting
   |
   v
Managed PostgreSQL

Exact deployment providers shall be finalized in DOC-19.

106. Docker Development

Docker should provide reproducible backend/database environments.

Example:

docker compose up

should eventually start required services.

The exact compose configuration shall be defined during implementation.

107. CI/CD

GitHub Actions shall be used for automated checks.

Minimum CI pipeline:

Push
 |
 v
Install dependencies
 |
 v
Lint
 |
 v
Unit Tests
 |
 v
Build
 |
 v
Report
108. Backend CI

Backend CI shall perform:

Python setup
dependency installation
Ruff
Black check
pytest
109. Frontend CI

Frontend CI shall perform:

Node setup
npm ci
ESLint
Prettier check
Vitest
npm run build
110. Build Reproducibility

The repository shall contain enough information to allow a new developer to reproduce the environment.

Required:

README.md
requirements.txt
package.json
package-lock.json
.env.example
Docker configuration
setup instructions
111. Local Development Command Baseline

Frontend:

cd frontend
npm install
npm run dev

Backend:

cd backend
python -m venv .venv

Windows:

.\.venv\Scripts\Activate.ps1

Then:

pip install -r requirements.txt

Run:

uvicorn app.main:app --reload
112. Database Setup Baseline

The local database shall be PostgreSQL.

Example conceptual configuration:

Database:
pathfinder

User:
pathfinder_user

Host:
localhost

Port:
5432

Actual credentials shall exist only in .env.

113. Database Migration Commands

Baseline commands:

alembic upgrade head

Create migration:

alembic revision --autogenerate -m "migration description"

Downgrade where appropriate:

alembic downgrade -1
114. Testing Commands

Backend:

pytest

Frontend:

npm run test

Build:

npm run build

Lint:

npm run lint
115. Development Branch Strategy

Developers should work on feature branches.

Example:

main
 |
 +-- feature/authentication
 +-- feature/profile
 +-- feature/recommendation-engine
 +-- feature/learning-path
 +-- feature/dashboard

Direct development on main should be avoided for feature work.

116. Git Commit Technology Rule

Commit messages should follow a consistent format.

Examples:

feat: add goal creation API
feat: implement skill gap analysis
fix: correct recommendation ranking
docs: update API contract
test: add recommendation tests
refactor: separate recommendation service
chore: update dependencies
117. Documentation Dependency

The following documents depend directly on this technology baseline:

DOC-15 Environment and Configuration
DOC-16 Git/GitHub Development Workflow
DOC-18 Testing and QA Strategy
DOC-19 Deployment Architecture
DOC-20 Integration Checklist
DOC-21 Master AI Implementation Specification
118. Cross-Document Dependency Matrix
Technology Decision	Related Document
React	DOC-10
TypeScript	DOC-10
Vite	DOC-10
Tailwind	DOC-10
React Router	DOC-10
TanStack Query	DOC-10
FastAPI	DOC-11
Python	DOC-11
SQLAlchemy	DOC-11
PostgreSQL	DOC-07
Alembic	DOC-07 / DOC-11
JWT	DOC-12
bcrypt	DOC-12
sentence-transformers	DOC-09
scikit-learn	DOC-09
NumPy	DOC-09
pandas	DOC-09
REST API	DOC-08
Docker	DOC-19
GitHub Actions	DOC-16 / DOC-18
Environment variables	DOC-15
Testing stack	DOC-18
119. Technology-to-Requirement Mapping
Requirement	Technology
FR-001 Registration	FastAPI + PostgreSQL + React
FR-002 Login	FastAPI + JWT + React
FR-005 Profile	React + FastAPI + PostgreSQL
FR-012 Goals	React + FastAPI + PostgreSQL
FR-016 Chat	React + FastAPI + AI service
FR-020 Skills	PostgreSQL + SQLAlchemy
FR-023 Skill Gap	Python + recommendation engine
FR-027 Resource Search	PostgreSQL + semantic search
FR-029 Recommendations	AI/ML + FastAPI
FR-036 Dependencies	PostgreSQL + rule engine
FR-040 Learning Path	backend service + AI/ML
FR-047 Explanations	AI service
FR-051 Progress	PostgreSQL + FastAPI
FR-056 Feedback	PostgreSQL + FastAPI
FR-059 Adaptation	recommendation service
FR-063 Dashboard	React + Recharts
120. Technology-to-Architecture Mapping
DOC-06 System Architecture
        |
        +-- React
        +-- FastAPI
        +-- PostgreSQL
        +-- AI/ML services
        +-- REST APIs
        +-- Docker
121. Technology-to-Database Mapping

The database technology must support:

users
profiles
goals
skills
user_skills
resources
resource_skills
skill_dependencies
learning_paths
learning_path_items
progress
feedback
chat_sessions
chat_messages

PostgreSQL is the authoritative persistence layer.

122. Technology-to-AI Mapping
Learner Goal
      |
      v
Python
      |
      +--> preprocessing
      |
      +--> skill extraction
      |
      +--> sentence-transformers
      |
      +--> embeddings
      |
      +--> cosine similarity
      |
      +--> scikit-learn
      |
      +--> ranking
      |
      +--> path generation
      |
      +--> optional LLM
123. Technology-to-Frontend Mapping
React
 |
 +-- TypeScript
 |
 +-- React Router
 |
 +-- TanStack Query
 |
 +-- React Hook Form
 |
 +-- Zod
 |
 +-- Recharts
 |
 +-- Tailwind CSS
 |
 +-- Vite
124. Technology-to-Backend Mapping
Python
 |
 +-- FastAPI
 |
 +-- Pydantic
 |
 +-- SQLAlchemy
 |
 +-- Alembic
 |
 +-- PostgreSQL Driver
 |
 +-- JWT
 |
 +-- AI Services
 |
 +-- httpx
125. Technology-to-Testing Mapping
Backend
 |
 +-- pytest
 +-- pytest-asyncio
 +-- httpx

Frontend
 |
 +-- Vitest
 +-- React Testing Library
 +-- User Event

Quality
 |
 +-- Ruff
 +-- Black
 +-- ESLint
 +-- Prettier
126. Technology-to-Deployment Mapping
GitHub
   |
   v
GitHub Actions
   |
   +--> Build Frontend
   |
   +--> Test Backend
   |
   +--> Test Frontend
   |
   +--> Build Docker
   |
   v
Deployment Platform
   |
   +--> Frontend
   |
   +--> Backend
   |
   +--> PostgreSQL
127. Technology Governance

Technology choices shall be governed by the following hierarchy:

Project Charter
      |
      v
SRS
      |
      v
System Architecture
      |
      v
Technology Stack
      |
      v
Implementation

Implementation must not silently contradict the approved architecture.

128. Technology Change Process

If a developer wants to change a major technology:

Identify the current technology.
Identify the proposed technology.
Explain the reason.
Identify affected documents.
Identify affected code.
Identify affected dependencies.
Identify migration effort.
Evaluate risk.
Record the decision in DOC-22.
Update all affected documentation.
Update implementation.
Update tests.
129. Prohibited Silent Changes

The following changes must not happen silently:

React -> another frontend framework
FastAPI -> another backend framework
PostgreSQL -> another primary database
SQLAlchemy -> another ORM
JWT -> another authentication strategy
sentence-transformers -> another embedding strategy
npm -> another package manager
Python version change
Node version change
major AI provider change
deployment architecture change
130. Dependency Ownership
Area	Primary Responsibility
Frontend	Frontend Developer
Backend	Backend Developer
Database	Backend/Database Developer
AI/ML	AI/ML Developer
Testing	All Developers
DevOps	Assigned Developer
Documentation	Entire Team
131. Team Development Rule

All team members shall use the same baseline:

Python version
Node version
database version
dependency versions
environment variable names
API contract
database schema
Git workflow

This reduces integration failures.

132. Integration Rule

Before merging code, the developer must verify:

Does the implementation match DOC-08?
Does the implementation match DOC-07?
Does the implementation match DOC-09?
Does the implementation match DOC-10?
Does the implementation match DOC-11?
Does the implementation match DOC-12?
Does the implementation match DOC-13?
133. Frontend Integration Rule

Frontend developers shall not invent backend endpoints.

The endpoint must exist in:

DOC-08 API Contract

before frontend integration.

134. Backend Integration Rule

Backend developers shall not invent database fields without updating:

DOC-07 Database Design

and the affected schemas.

135. AI Integration Rule

AI developers shall not directly change database structures.

AI output must pass through:

validation
service layer
business rules
persistence layer

where applicable.

136. Dependency Installation Rule

Dependencies must be installed from project manifests.

Backend:

pip install -r requirements.txt

Frontend:

npm ci

CI should prefer:

npm ci

instead of:

npm install

for reproducible builds.

137. Virtual Environment Rule

Python development must use an isolated environment.

Example:

backend/.venv

The virtual environment must not be committed to Git.

138. Node Modules Rule

The following directory must never be committed:

frontend/node_modules/

It must be listed in .gitignore.

139. Build Artifact Rule

Build artifacts must not be committed unless specifically required.

Examples:

frontend/dist/
backend/__pycache__/
coverage/
140. Secrets Rule

The following must never be committed:

API keys
JWT secrets
database passwords
cloud credentials
private tokens
LLM keys
deployment credentials
141. Local Setup Principle

A new developer should be able to follow:

Clone repository
      |
      v
Install prerequisites
      |
      v
Create environment
      |
      v
Configure .env
      |
      v
Install dependencies
      |
      v
Start PostgreSQL
      |
      v
Run migrations
      |
      v
Start backend
      |
      v
Start frontend
      |
      v
Open application
142. Reproducibility Requirement

A clean machine must be able to reproduce the project using:

README.md
DOC-15
requirements.txt
package.json
package-lock.json
.env.example
Docker configuration
143. Prototype Optimization

Because this project is being developed for the HCLTech Round 2 prototype, technology decisions shall optimize for:

working functionality
demonstrability
reliability
development speed
AI capability
clear architecture
low integration risk

rather than unnecessary enterprise complexity.

144. Production-readiness Boundary

The MVP is not required to implement:

Kubernetes
multi-region deployment
complex event streaming
microservices
advanced distributed caching
large-scale orchestration

These are future considerations.

145. AI/ML MVP Boundary

The MVP shall prioritize:

goal understanding
skill extraction
skill-gap analysis
semantic resource matching
recommendation ranking
prerequisite-aware ordering
learning-path generation
recommendation explanations

The system does not require training a large custom foundation model.

146. Recommendation MVP Strategy

The recommended MVP approach is:

Structured Resource Dataset
        +
Skill Taxonomy
        +
Prerequisite Graph
        +
Sentence Embeddings
        +
Similarity
        +
Rule-Based Ranking
        +
Learner Context

This provides explainability and predictable behavior.

147. AI Explainability Strategy

Recommendation explanations should be generated from structured signals.

Example:

Recommended because:

1. It teaches Python.
2. Python is missing from your current skills.
3. Python is a prerequisite for the next ML module.
4. The resource matches your beginner experience level.
5. Estimated duration fits your weekly learning preference.

The system should not depend exclusively on free-form LLM explanations.

148. Deterministic Core

The following must remain deterministic where possible:

authentication
authorization
database validation
progress calculation
prerequisite validation
skill-level calculations
resource availability checks
basic recommendation scoring
149. AI-Augmented Layer

AI may enhance:

goal interpretation
natural-language interaction
explanations
semantic matching
personalized suggestions
150. Technology Risk Management

Major technology risks include:

Risk	Mitigation
AI API unavailable	fallback logic
Dependency conflict	version control
Database mismatch	Alembic migrations
Frontend/API mismatch	DOC-08 contract
AI output malformed	Pydantic validation
Secret leakage	.env + .gitignore
Model too slow	lightweight embedding model
External API failure	timeout + fallback
Team environment mismatch	documented versions
Package update breaking code	controlled upgrades
151. Performance Considerations

The MVP should prioritize acceptable interactive performance.

Potential optimizations:

database indexing
query optimization
embedding reuse
resource caching
pagination
lazy loading
frontend caching

Optimization should be evidence-based.

152. Pagination

Large resource collections shall use pagination.

The API should not return thousands of resources in a single request.

Example conceptual API:

GET /resources?page=1&page_size=20
153. Search Strategy

Resource search may combine:

keyword filtering
structured filtering
semantic similarity
skill matching
154. Resource Ranking

Resource ranking may consider:

semantic similarity
goal relevance
skill gap
difficulty
prerequisites
duration
quality
learner preference
feedback
155. Database Indexing

PostgreSQL indexes should be added for frequently queried fields.

Examples:

user.email
goal.user_id
goal.status
resource.type
resource.difficulty
resource.provider
progress.user_id
feedback.user_id

Exact indexes remain governed by DOC-07.

156. API Performance

API endpoints should avoid:

unnecessary database queries
N+1 queries
large uncontrolled responses
repeated embedding generation
unbounded external API calls
157. AI Performance

Embeddings should be generated once and reused where possible.

The system should avoid recomputing embeddings for unchanged resources.

158. Resource Embedding Strategy

For a resource:

Title
+
Description
+
Skills
+
Learning Objectives

may be combined into a canonical text representation.

The embedding is generated from that representation.

159. Learner Embedding Strategy

Learner representation may combine:

Goal
+
Current Skills
+
Interests
+
Experience
+
Learning Objective

The exact implementation shall follow DOC-09.

160. Model Storage

Model configuration shall be separated from application code.

Example:

ML_MODEL_DIR
EMBEDDING_MODEL
MODEL_VERSION
161. Data Processing

Data processing scripts should be repeatable.

Example:

python scripts/process_resources.py

Generated datasets should have documented provenance.

162. Notebook Policy

Jupyter notebooks may be used for:

EDA
experimentation
model comparison
evaluation
visualization

Production application logic must not depend directly on notebooks.

Reusable logic shall be moved into Python modules.

163. ML Evaluation

The AI/ML system should be evaluated using suitable metrics.

Potential metrics:

Precision@K
Recall@K
Hit Rate@K
MRR
NDCG

The final evaluation methodology shall be defined in DOC-18 and DOC-09.

164. Recommendation Quality

Recommendation quality should be evaluated using:

offline evaluation
rule validation
sample learner scenarios
manual expert review
demo workflow validation
165. AI Hallucination Control

The AI assistant shall be constrained where possible.

The assistant should rely on:

known learner data
known resources
known skills
known path data

rather than inventing resources.

166. Resource Truth Policy

If the system does not have a resource in its dataset or verified external source, it should not present the resource as a confirmed available resource.

167. AI Explanation Policy

AI-generated explanations must not contradict deterministic recommendation signals.

If an explanation conflicts with the actual scoring signals, the deterministic system shall be considered authoritative.

168. Logging AI Requests

AI request logging shall not store sensitive information unnecessarily.

API keys and secrets must never appear in logs.

169. Privacy Considerations

Learner information shall be treated as private application data.

The application should minimize storage of unnecessary personal information.

170. Dependency Security

Dependencies should be periodically checked for known vulnerabilities.

The team should use available package security tools where practical.

171. Frontend Security

Frontend code shall not contain:

database credentials
private API keys
JWT signing secrets
server-side secrets

Only public configuration may be exposed.

172. Backend Security

Backend shall enforce:

authentication
authorization
input validation
rate-aware external requests
secure headers where applicable
safe error handling
173. Database Security

Database access shall use:

least privilege
separate credentials
environment variables
parameterized queries through SQLAlchemy
174. Production Secrets

Production secrets shall be stored through the deployment platform's secret-management mechanism.

They must not be committed to GitHub.

175. Documentation Synchronization

If the technology stack changes, the following documents must be checked:

DOC-06
DOC-07
DOC-08
DOC-09
DOC-10
DOC-11
DOC-12
DOC-13
DOC-14
DOC-15
DOC-16
DOC-18
DOC-19
DOC-20
DOC-21
DOC-22
DOC-23
176. Technology Baseline Checklist

Before implementation starts:

 Python version selected
 Node version selected
 PostgreSQL version selected
 React selected
 TypeScript selected
 Vite selected
 FastAPI selected
 SQLAlchemy selected
 Alembic selected
 PostgreSQL driver selected
 JWT strategy selected
 Password hashing selected
 AI/ML stack selected
 Testing stack selected
 Formatting tools selected
 Linting tools selected
 Docker strategy selected
 GitHub Actions strategy selected
177. Developer Installation Checklist

Each developer should verify:

python --version
node --version
npm --version
git --version
docker --version

PostgreSQL should also be available locally or through Docker.

178. Expected Version Baseline

Initial baseline:

Python: 3.11
Node.js: 20 LTS
PostgreSQL: 16
React: project-approved current compatible version
FastAPI: project-approved current compatible version
SQLAlchemy: project-approved current compatible version

Exact package versions shall be frozen when the implementation repository is initialized.

179. Dependency Lock Completion

DOC-14 establishes technology families.

During implementation, exact versions must be recorded in:

backend/requirements.txt
frontend/package.json
frontend/package-lock.json

and documented in the implementation setup instructions.

180. No "Latest" Dependency Rule

Production documentation shall not instruct developers to install arbitrary latest versions.

Avoid:

pip install package
npm install package

without a controlled project manifest.

Prefer:

pip install -r requirements.txt
npm ci
181. New Developer Onboarding

A new developer must not need to guess:

which Python version
which Node version
which database
which commands
which environment variables
which API
which branch
which dependencies

All required information shall be documented.

182. Team Integration Safety

The selected stack minimizes integration risk by maintaining:

single frontend framework
single backend framework
single relational database
single ORM
single package manager
single Python environment strategy
documented REST API
documented database schema
documented AI architecture
183. Implementation Dependency Order

Technology implementation shall follow:

1. Repository setup
        |
2. Environment setup
        |
3. PostgreSQL
        |
4. Backend skeleton
        |
5. Database models
        |
6. Alembic migrations
        |
7. Authentication
        |
8. API modules
        |
9. AI/ML modules
        |
10. Frontend skeleton
        |
11. API integration
        |
12. Learning path UI
        |
13. Dashboard
        |
14. Testing
        |
15. Docker
        |
16. Deployment

This sequence minimizes cross-layer integration failures.

184. Technology Freeze

After DOC-14 approval, the following should be considered frozen for MVP:

Frontend framework
Backend framework
Database
ORM
API style
Python baseline
Node baseline
AI/ML baseline
Testing baseline
Package managers
185. Exceptions

An exception is allowed only when:

A technology creates a blocker.
The current technology cannot satisfy a documented requirement.
A security issue requires replacement.
A deployment limitation requires replacement.

The exception must be documented in DOC-22.

186. Architecture Decision Record Requirement

Any major stack change must create an ADR.

Example:

ADR-001: Select PostgreSQL
ADR-002: Select FastAPI
ADR-003: Select React + TypeScript
ADR-004: Select sentence-transformers

The exact ADR list shall be maintained in DOC-22.

187. Relationship With DOC-15

DOC-15 shall define:

environment variables
.env.example
local setup
database configuration
development commands
environment separation
secret handling

DOC-15 must not introduce technologies that contradict DOC-14.

188. Relationship With DOC-16

DOC-16 shall define:

Git workflow
branch strategy
commit conventions
pull requests
code review
GitHub issues
GitHub project board
release process

The technology stack defined here shall be used in those workflows.

189. Relationship With DOC-18

DOC-18 shall define how the selected technologies are tested.

Expected coverage:

frontend
backend
database
API
AI/ML
integration
security
end-to-end
190. Relationship With DOC-19

DOC-19 shall define actual deployment infrastructure.

DOC-14 establishes the technologies.

DOC-19 establishes where and how those technologies are deployed.

191. Relationship With DOC-20

DOC-20 shall provide the final integration checklist.

Every technology boundary must be verified.

Examples:

React -> FastAPI
FastAPI -> PostgreSQL
FastAPI -> AI service
AI -> resource data
Frontend -> authentication
Frontend -> progress
192. Relationship With DOC-21

DOC-21 shall convert this stack into implementation instructions.

DOC-21 must not introduce undocumented technologies unless explicitly marked and approved.

193. Relationship With DOC-23

DOC-23 shall trace requirements to:

architecture
technology
API
database
implementation
tests
GitHub issues

Technology decisions shall therefore remain traceable.

194. Final Approved MVP Stack

The initial approved MVP stack is:

FRONTEND
--------
React
TypeScript
Vite
Tailwind CSS
React Router
TanStack Query
React Hook Form
Zod
Recharts

BACKEND
-------
Python 3.11
FastAPI
Uvicorn
Pydantic
SQLAlchemy
Alembic
httpx

DATABASE
--------
PostgreSQL 16
psycopg

AUTHENTICATION
--------------
JWT
bcrypt-compatible password hashing

AI / ML
-------
NumPy
pandas
scikit-learn
sentence-transformers
cosine similarity
optional LLM provider abstraction

TESTING
-------
pytest
pytest-asyncio
httpx
Vitest
React Testing Library

QUALITY
-------
Ruff
Black
ESLint
Prettier

DEVOPS
------
Docker
Docker Compose
GitHub Actions
Git

REPOSITORY
----------
GitHub
195. Technology Stack Non-Goals

This document does not require:

microservices
Kubernetes
Kafka
RabbitMQ
Redis
dedicated vector database
custom foundation model training
multiple frontend frameworks
multiple backend frameworks
multiple relational databases

These may be considered only if future requirements justify them.

196. MVP Technology Philosophy

The PathFinder AI prototype shall use:

simple architecture
+
strong data model
+
deterministic recommendation logic
+
AI augmentation
+
clear APIs
+
testable modules
+
reproducible environments

rather than:

maximum number of technologies
197. Final Technology Decision

For the HCLTech Round 2 prototype, the technology baseline is:

React + TypeScript
        +
FastAPI + Python
        +
PostgreSQL
        +
SQLAlchemy + Alembic
        +
JWT Authentication
        +
Sentence Transformers
        +
scikit-learn
        +
Optional LLM
        +
Docker
        +
GitHub Actions

This stack is approved as the implementation baseline unless superseded through DOC-22.

198. Approval Criteria

DOC-14 is ready for approval when:

 frontend stack is defined
 backend stack is defined
 database technology is defined
 authentication technology is defined
 AI/ML stack is defined
 testing stack is defined
 dependency management is defined
 runtime versions are defined
 environment strategy is defined
 deployment technology direction is defined
 GitHub/CI technology is defined
 cross-document dependencies are documented
 major technology decisions are frozen
199. Implementation Readiness

After DOC-14 approval, the project has a technology baseline sufficient to begin environment design.

However, implementation shall not begin until:

DOC-15 Environment and Configuration

is completed.

The next implementation-related documents will further define:

environment
Git workflow
testing
deployment
integration
master implementation
ADRs
traceability
200. Document Status
Document ID: DOC-14

Document Name:
Technology Stack and Dependencies

Status:
DRAFT / ARCHITECTURE BASELINE

Implementation Status:
NOT STARTED

Technology Status:
BASELINE DEFINED

Architecture Status:
PARTIALLY FROZEN

Next Document:
DOC-15 Environment and Configuration
201. Change Control

Any modification to this document must identify:

technology changed
old technology
new technology
reason
benefit
risk
affected documents
affected dependencies
affected code
affected tests
affected deployment
migration strategy

Changes must be recorded in:

DOC-22 Architecture Decision Records

and reflected in:

DOC-23 Requirements Traceability Matrix
202. Final Cross-Document Consistency Rule

The following rule is mandatory for the entire project:

NO CODE
   |
   v
WITHOUT ARCHITECTURE
   |
   v
NO ARCHITECTURE
   |
   v
WITHOUT DOCUMENTED REQUIREMENT
   |
   v
NO NEW DEPENDENCY
   |
   v
WITHOUT TECHNOLOGY JUSTIFICATION
   |
   v
NO API IMPLEMENTATION
   |
   v
WITHOUT DOC-08
   |
   v
NO DATABASE CHANGE
   |
   v
WITHOUT DOC-07
   |
   v
NO MAJOR TECHNOLOGY CHANGE
   |
   v
WITHOUT DOC-22

This rule exists specifically to prevent integration problems when multiple team members and AI coding tools work on the repository.

203. Master Technology Baseline

The following baseline shall be treated as the authoritative technology reference for PathFinder AI MVP:

Category	Selected Technology	Status
Frontend Framework	React	APPROVED
Frontend Language	TypeScript	APPROVED
Build Tool	Vite	APPROVED
Styling	Tailwind CSS	APPROVED
Routing	React Router	APPROVED
Server State	TanStack Query	APPROVED
Forms	React Hook Form	APPROVED
Validation	Zod	APPROVED
Charts	Recharts	APPROVED
Backend Language	Python 3.11	APPROVED
Backend Framework	FastAPI	APPROVED
API Server	Uvicorn	APPROVED
Validation	Pydantic	APPROVED
ORM	SQLAlchemy	APPROVED
Migration	Alembic	APPROVED
Database	PostgreSQL 16	APPROVED
Database Driver	psycopg	APPROVED
Authentication	JWT	APPROVED
Password Hashing	bcrypt-compatible implementation	APPROVED
Numerical Computing	NumPy	APPROVED
Data Processing	pandas	APPROVED
ML	scikit-learn	APPROVED
Embeddings	sentence-transformers	APPROVED
Similarity	Cosine Similarity	APPROVED
External HTTP	httpx	APPROVED
Backend Testing	pytest	APPROVED
Frontend Testing	Vitest	APPROVED
Component Testing	React Testing Library	APPROVED
Python Linting	Ruff	APPROVED
Python Formatting	Black	APPROVED
Frontend Linting	ESLint	APPROVED
Frontend Formatting	Prettier	APPROVED
Containers	Docker	APPROVED
Container Orchestration	Docker Compose	APPROVED FOR LOCAL MVP
CI	GitHub Actions	APPROVED
Repository	GitHub	APPROVED
Package Manager	npm	APPROVED
Python Dependency File	requirements.txt	APPROVED
204. Final Statement

PathFinder AI shall be implemented using a modular monolithic architecture based on:

React
TypeScript
Vite
        |
        | REST / JSON
        v
FastAPI
Python
        |
        +-----------------------+
        |                       |
        v                       v
PostgreSQL              AI/ML Services
SQLAlchemy              sentence-transformers
Alembic                 scikit-learn
                        NumPy
                        pandas
                        Optional LLM

The stack is intentionally designed to provide:

fast development
+
clear ownership
+
strong integration boundaries
+
AI capability
+
reproducibility
+
testability
+
deployment simplicity
+
future extensibility

This technology baseline shall be used as the source of truth for subsequent implementation documents and code generation.

END OF DOC-14