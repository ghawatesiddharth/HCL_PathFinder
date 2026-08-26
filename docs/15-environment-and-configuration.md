# PathFinder AI — Environment and Configuration Specification

**Document ID:** DOC-15  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT  
**Parent Documents:** DOC-01, DOC-02, DOC-06, DOC-07, DOC-08, DOC-09, DOC-10, DOC-11, DOC-12, DOC-13, DOC-14  
**Next Related Documents:** DOC-16 Git & GitHub Development Workflow, DOC-17 GitHub Task Breakdown, DOC-18 Testing & QA Strategy, DOC-19 Deployment Architecture  
**Implementation Status:** NOT STARTED  
**Architecture Status:** CONFIGURATION BASELINE DEFINED  

---

# 1. Purpose

This document defines the complete environment, configuration, secret-management, runtime, and environment-variable strategy for PathFinder AI.

The objective is to ensure that every developer, development machine, testing environment, CI/CD environment, and deployment environment uses a consistent configuration model.

This document is intended to prevent configuration-related integration failures such as:

- frontend connecting to the wrong backend
- backend connecting to the wrong database
- incorrect API URLs
- missing environment variables
- incorrect CORS configuration
- AI API configuration failures
- authentication secret mismatches
- development and production configuration conflicts
- hardcoded secrets
- inconsistent ports
- incorrect database names
- inconsistent environment variable names
- missing configuration validation
- deployment configuration mismatches

All implementation code must follow the configuration conventions defined in this document unless a formally approved Architecture Decision Record changes them.

---

# 2. Configuration Principles

PathFinder AI shall follow the following configuration principles.

## 2.1 No Hardcoded Secrets

Secrets shall never be hardcoded into source code.

Examples of prohibited hardcoding:

```text
JWT_SECRET = "my-secret"
OPENAI_API_KEY = "sk-..."
DATABASE_PASSWORD = "password123"

Secrets must be provided through environment configuration or a secure deployment secret manager.

2.2 One Configuration Naming Convention

Environment variables shall use uppercase snake case.

Example:

DATABASE_URL
JWT_SECRET
AI_API_KEY
FRONTEND_URL
BACKEND_URL

Do not use inconsistent alternatives such as:

databaseUrl
database_url
Database_URL
DBURL
2.3 Environment Separation

The project shall support at least:

development
testing
production

Each environment may have different:

database
API URLs
secrets
logging levels
CORS origins
AI configuration
deployment URLs
2.4 Configuration Must Be Explicit

Application startup should validate required configuration.

The backend must fail fast when mandatory configuration is missing.

Example:

DATABASE_URL missing
JWT_SECRET missing

The application should provide a clear configuration error rather than failing later during a database query or authentication request.

2.5 Frontend and Backend Configuration Separation

Frontend configuration and backend configuration must not be mixed.

Frontend configuration may contain values required by the browser.

Backend configuration may contain secrets and server-only values.

Secrets must never be exposed through frontend environment variables.

3. Environment Model

PathFinder AI shall use three logical environments.

Environment	Purpose
Development	Local developer implementation
Testing	Automated and integration testing
Production	Deployed prototype
4. Development Environment

The development environment is used by team members during implementation.

Typical development architecture:

Developer Machine
        |
        +-------------------+
        |                   |
        v                   v
Frontend              Backend API
localhost:5173        localhost:8000
                            |
                            v
                       Database

The exact ports are defined later in this document.

5. Testing Environment

The testing environment is used for:

unit tests
integration tests
API tests
authentication tests
recommendation tests
database tests
end-to-end tests

Testing should not use the production database.

Recommended structure:

Test Frontend
      |
      v
Test Backend
      |
      v
Test Database

AI services should preferably be mocked or controlled during automated testing.

6. Production Environment

The production environment represents the deployed PathFinder AI prototype.

Logical structure:

User Browser
      |
      v
Production Frontend
      |
      v
Production Backend API
      |
      +----------------------+
      |                      |
      v                      v
Production Database      AI Service

Production configuration must never depend on local development files.

7. Technology Configuration Baseline

The configuration model is based on the technology decisions documented in DOC-14.

The implementation must use the exact versions finalized in the project dependency files.

The environment configuration must therefore be compatible with:

frontend framework
backend framework
database
AI/ML libraries
authentication libraries
API libraries
testing libraries
deployment environment

DOC-14 remains the source of truth for dependency versions.

DOC-15 is the source of truth for runtime configuration.

8. Repository Configuration Files

The following files are expected.

HCL_PathFinder/
│
├── .gitignore
├── .env.example
├── README.md
│
├── frontend/
│   ├── .env.example
│   └── ...
│
├── backend/
│   ├── .env.example
│   └── ...
│
└── docs/
    └── ...

Actual file placement must remain consistent with DOC-13.

9. Root Environment Files

The repository may contain a root-level .env.example.

Example:

.env.example

This file contains variable names and safe placeholder values only.

It must never contain real credentials.

Example:

NODE_ENV=development
APP_ENV=development

FRONTEND_URL=http://localhost:5173
BACKEND_URL=http://localhost:8000

DATABASE_URL=postgresql://USER:PASSWORD@HOST:PORT/DATABASE

JWT_SECRET=replace_with_secure_secret

AI_PROVIDER=mock
AI_API_KEY=
AI_MODEL=

LOG_LEVEL=INFO

The exact variables must match the final implementation.

10. Actual Environment Files

Local development may use:

.env

Actual .env files containing secrets must not be committed.

Example:

.env

must be included in .gitignore.

11. Environment File Rules

The following rules are mandatory.

Rule 1

.env.example may be committed.

Rule 2

.env must not be committed.

Rule 3

Production secrets must not be stored in Git.

Rule 4

API keys must not be stored in source code.

Rule 5

Passwords must not be stored in source code.

Rule 6

JWT secrets must not be stored in source code.

Rule 7

Database credentials must not be stored in source code.

12. Required Environment Variable Categories

Configuration variables are grouped into:

Application
Frontend
Backend
Database
Authentication
AI
CORS
Logging
External Services
Testing
Deployment
13. Application Configuration
APP_ENV

Defines the active application environment.

Allowed values:

development
testing
production

Example:

APP_ENV=development
NODE_ENV

Used by JavaScript-based tooling where applicable.

Development:

NODE_ENV=development

Production:

NODE_ENV=production

If the backend is Python-based, this variable may be used only where required by supporting tooling.

14. Application Name

The application name should be defined consistently.

Recommended:

APP_NAME=PathFinder AI

This may be used for:

logging
monitoring
diagnostics
API metadata
15. Application Version

The application version may be represented using:

APP_VERSION=1.0.0

The version should be updated according to the project's release strategy.

16. Frontend Configuration

The frontend requires configuration for communication with the backend.

Primary variable:

VITE_API_BASE_URL=http://localhost:8000

If the frontend framework changes, the environment-variable prefix must follow the selected framework's convention.

For the current architecture, the expected frontend variable naming convention is:

VITE_*
17. Frontend API URL

Development:

VITE_API_BASE_URL=http://localhost:8000

Production example:

VITE_API_BASE_URL=https://api.example.com

The actual production URL will be defined in DOC-19.

18. Frontend Application URL

Backend configuration should know the frontend origin.

Development:

FRONTEND_URL=http://localhost:5173

Production:

FRONTEND_URL=https://example.com

The actual deployed URL will be finalized during deployment.

19. Backend Configuration

The backend shall expose its configured service address.

Development:

BACKEND_HOST=127.0.0.1
BACKEND_PORT=8000

The application should use these values consistently.

20. Frontend Development Server

The frontend development server is expected to use:

localhost:5173

Therefore:

VITE_API_BASE_URL=http://localhost:8000

The frontend and backend port relationship must not be changed independently.

If ports change, the following must be updated:

frontend environment
backend CORS
README
testing configuration
deployment configuration
integration checklist
21. Backend API Base Path

The backend API should use a consistent API prefix.

Recommended:

/api/v1

Therefore endpoints should follow:

/api/v1/auth/register
/api/v1/auth/login
/api/v1/profile
/api/v1/goals
/api/v1/skills
/api/v1/resources
/api/v1/recommendations
/api/v1/learning-paths
/api/v1/progress
/api/v1/feedback
/api/v1/chat

The exact endpoint definitions remain governed by DOC-08.

22. Database Configuration

The database connection shall be configured through:

DATABASE_URL=

Example development format:

DATABASE_URL=postgresql://pathfinder_user:password@localhost:5432/pathfinder_db

The actual credentials are environment-specific.

23. Database Host Configuration

If individual database variables are used instead of a single URL, they must follow:

DB_HOST=localhost
DB_PORT=5432
DB_NAME=pathfinder_db
DB_USER=pathfinder_user
DB_PASSWORD=

The implementation should preferably use one canonical database configuration method.

The final implementation must not simultaneously maintain conflicting database connection methods unless explicitly required.

24. Database Name

Recommended development database:

pathfinder_db

Testing database:

pathfinder_test_db

Production database name must be environment-specific.

25. Database Isolation

Development and testing databases must be separate.

Example:

pathfinder_db
pathfinder_test_db

Never run destructive automated tests against:

production database
26. Database Migration Configuration

Database schema changes must be handled through the migration mechanism defined by the backend technology stack.

Developers must not manually modify production tables without migration tracking.

Every schema change must have:

migration
documentation
testing
rollback consideration
27. Authentication Configuration

Authentication requires server-side configuration.

Required variables include:

JWT_SECRET=
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

The exact authentication implementation must remain consistent with DOC-12.

28. JWT Secret

Example:

JWT_SECRET=replace_with_long_random_secret

The real value must be generated securely.

It must not be:

PathFinder
password
123456
secret
HCL
AlgoX
29. JWT Algorithm

The algorithm must match the implementation defined in DOC-12.

Recommended prototype configuration:

JWT_ALGORITHM=HS256

Changing the algorithm after implementation may require coordinated changes to:

authentication middleware
token generation
token validation
tests
deployment secrets
30. Token Expiration

Recommended:

ACCESS_TOKEN_EXPIRE_MINUTES=60

The exact duration may be adjusted during security review.

31. Password Hashing Configuration

Password hashing must be performed using the library selected in DOC-12 and DOC-14.

No plaintext password storage is permitted.

Password hashing parameters should remain configuration-driven only if the selected library supports safe configuration.

32. AI Configuration

The AI system shall use configuration variables rather than hardcoded provider settings.

Recommended variables:

AI_PROVIDER=mock
AI_API_KEY=
AI_MODEL=
AI_TEMPERATURE=0.2
AI_MAX_TOKENS=1000
AI_TIMEOUT_SECONDS=30
33. AI Provider

Example:

AI_PROVIDER=mock

Possible implementation values may include:

mock
openai
local

The exact supported providers will be defined by DOC-09.

34. AI API Key

Example:

AI_API_KEY=

The real API key must never be committed.

35. AI Model

Example:

AI_MODEL=

The exact model must be selected according to:

availability
competition requirements
cost
latency
capability
reliability

The selected model must be recorded in DOC-09 or an Architecture Decision Record.

36. AI Temperature

Recommended prototype default:

AI_TEMPERATURE=0.2

Lower values are preferred for structured recommendation tasks.

37. AI Timeout

Recommended:

AI_TIMEOUT_SECONDS=30

AI calls must not block the entire application indefinitely.

38. AI Fallback

The system shall support a fallback mode.

Example:

AI_FALLBACK_ENABLED=true

If the external AI provider fails, deterministic recommendation logic should remain available where possible.

39. Mock AI Mode

Development and automated testing may use:

AI_PROVIDER=mock

This allows:

deterministic testing
offline development
reduced API cost
faster test execution
predictable outputs
40. AI Service Abstraction

Frontend components must never directly call external AI APIs.

Correct architecture:

Frontend
   |
   v
Backend API
   |
   v
AI Service Abstraction
   |
   +--------> External AI Provider
   |
   +--------> Mock Provider

This requirement originates from DOC-08, DOC-09, and DOC-11.

41. CORS Configuration

The backend shall explicitly configure allowed frontend origins.

Development:

CORS_ORIGINS=http://localhost:5173

If multiple origins are supported:

CORS_ORIGINS=http://localhost:5173,http://127.0.0.1:5173

Production must use the actual deployed frontend URL.

42. CORS Security

The following must not be used in production unless explicitly justified:

allow_origins=["*"]

Production CORS should be restricted to known application origins.

43. Logging Configuration

Recommended variables:

LOG_LEVEL=INFO
LOG_FORMAT=json

Development may use:

LOG_LEVEL=DEBUG

Production should normally use:

LOG_LEVEL=INFO

or a stricter level depending on deployment requirements.

44. Logging Security

Logs must never contain:

passwords
JWT secrets
API keys
database passwords
authentication tokens
sensitive personal information

Example of prohibited logging:

JWT_SECRET=abc123
45. External Resource Configuration

If external learning-resource APIs are integrated, configuration should use variables such as:

RESOURCE_PROVIDER=
RESOURCE_API_KEY=
RESOURCE_API_BASE_URL=
RESOURCE_API_TIMEOUT_SECONDS=15

The actual provider-specific variables will be defined after the resource integration decision.

46. External Resource Failure Handling

External resource calls must use:

timeout
error handling
retry policy where appropriate
fallback behavior

The backend must not hang indefinitely because an external resource provider is unavailable.

47. HTTP Configuration

Recommended:

HTTP_TIMEOUT_SECONDS=30

All external HTTP requests must have explicit timeouts.

48. Request Size Limits

The backend should define reasonable request-size limits.

This protects against accidental or malicious oversized requests.

The exact limits will be finalized during security and implementation testing.

49. File Upload Configuration

If profile images or documents are introduced later, configuration should define:

MAX_UPLOAD_SIZE_MB=
ALLOWED_UPLOAD_TYPES=
UPLOAD_DIRECTORY=

File uploads are not mandatory for the core MVP unless explicitly approved.

50. Email Configuration

Email functionality is not mandatory for the initial MVP.

If introduced later:

EMAIL_PROVIDER=
EMAIL_HOST=
EMAIL_PORT=
EMAIL_USERNAME=
EMAIL_PASSWORD=
EMAIL_FROM=

These variables must not be implemented unless the feature is included in the approved scope.

51. Testing Configuration

Testing should use explicit environment configuration.

Example:

APP_ENV=testing
DATABASE_URL=postgresql://pathfinder_user:password@localhost:5432/pathfinder_test_db
AI_PROVIDER=mock
AI_FALLBACK_ENABLED=true
LOG_LEVEL=WARNING
52. Test Isolation

Tests must not accidentally use:

development database
production database
production AI credentials
production API URLs

The test configuration should make accidental cross-environment access difficult.

53. Test Data

Testing should use synthetic data.

Example:

test@example.com
Test Learner
Test Goal
Test Skill

Real user data should not be used in automated tests.

54. Development Configuration Example

A development .env may resemble:

APP_NAME=PathFinder AI
APP_VERSION=1.0.0
APP_ENV=development
NODE_ENV=development

FRONTEND_URL=http://localhost:5173
BACKEND_URL=http://localhost:8000
VITE_API_BASE_URL=http://localhost:8000

BACKEND_HOST=127.0.0.1
BACKEND_PORT=8000

DATABASE_URL=postgresql://pathfinder_user:password@localhost:5432/pathfinder_db

JWT_SECRET=CHANGE_ME
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

AI_PROVIDER=mock
AI_API_KEY=
AI_MODEL=
AI_TEMPERATURE=0.2
AI_MAX_TOKENS=1000
AI_TIMEOUT_SECONDS=30
AI_FALLBACK_ENABLED=true

CORS_ORIGINS=http://localhost:5173

LOG_LEVEL=DEBUG
LOG_FORMAT=text

This is a template only.

Actual credentials must be supplied locally.

55. Testing Configuration Example
APP_NAME=PathFinder AI
APP_VERSION=1.0.0
APP_ENV=testing
NODE_ENV=test

FRONTEND_URL=http://localhost:5173
BACKEND_URL=http://localhost:8000
VITE_API_BASE_URL=http://localhost:8000

BACKEND_HOST=127.0.0.1
BACKEND_PORT=8000

DATABASE_URL=postgresql://pathfinder_user:password@localhost:5432/pathfinder_test_db

JWT_SECRET=TEST_ONLY_SECRET
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

AI_PROVIDER=mock
AI_API_KEY=
AI_MODEL=
AI_TEMPERATURE=0.0
AI_MAX_TOKENS=500
AI_TIMEOUT_SECONDS=10
AI_FALLBACK_ENABLED=true

CORS_ORIGINS=http://localhost:5173

LOG_LEVEL=WARNING
LOG_FORMAT=text

Testing secrets must never be reused for production.

56. Production Configuration Example

Production values must be supplied by the deployment platform.

Conceptual example:

APP_NAME=PathFinder AI
APP_VERSION=1.0.0
APP_ENV=production

FRONTEND_URL=https://production-frontend.example
BACKEND_URL=https://production-api.example

DATABASE_URL=<SECURE_DATABASE_URL>

JWT_SECRET=<SECURE_RANDOM_SECRET>
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

AI_PROVIDER=<SELECTED_PROVIDER>
AI_API_KEY=<SECURE_API_KEY>
AI_MODEL=<SELECTED_MODEL>
AI_TEMPERATURE=0.2
AI_MAX_TOKENS=1000
AI_TIMEOUT_SECONDS=30
AI_FALLBACK_ENABLED=true

CORS_ORIGINS=https://production-frontend.example

LOG_LEVEL=INFO
LOG_FORMAT=json

Actual production values must never be stored in this repository.

57. Frontend .env.example

The frontend should contain:

frontend/.env.example

Example:

VITE_API_BASE_URL=http://localhost:8000

Only frontend-safe variables may be included.

58. Backend .env.example

The backend should contain:

backend/.env.example

Example:

APP_NAME=PathFinder AI
APP_VERSION=1.0.0
APP_ENV=development

BACKEND_HOST=127.0.0.1
BACKEND_PORT=8000

DATABASE_URL=postgresql://pathfinder_user:password@localhost:5432/pathfinder_db

JWT_SECRET=CHANGE_ME
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

AI_PROVIDER=mock
AI_API_KEY=
AI_MODEL=
AI_TEMPERATURE=0.2
AI_MAX_TOKENS=1000
AI_TIMEOUT_SECONDS=30
AI_FALLBACK_ENABLED=true

CORS_ORIGINS=http://localhost:5173

LOG_LEVEL=DEBUG
LOG_FORMAT=text
59. Environment Variable Validation

The backend must validate required variables during startup.

Required development variables should include:

APP_ENV
DATABASE_URL
JWT_SECRET
JWT_ALGORITHM
ACCESS_TOKEN_EXPIRE_MINUTES
AI_PROVIDER
CORS_ORIGINS

Additional variables may become mandatory depending on the selected AI provider.

60. Configuration Validation Behavior

If a required variable is missing:

Application Startup
       |
       v
Load Configuration
       |
       v
Validate Configuration
       |
       +---- Invalid ----> Clear Error + Stop
       |
       +---- Valid ------> Start Application

The application must not silently continue with unsafe defaults for security-critical configuration.

61. Safe Defaults

Safe defaults may be used for non-sensitive configuration.

Examples:

LOG_LEVEL=INFO
AI_TIMEOUT_SECONDS=30
ACCESS_TOKEN_EXPIRE_MINUTES=60

Unsafe defaults must not be used for:

JWT_SECRET
DATABASE_PASSWORD
AI_API_KEY
62. Configuration Object

The backend should centralize configuration into a dedicated configuration module.

Conceptual structure:

backend/
└── app/
    └── core/
        └── config.py

The exact implementation must remain consistent with DOC-13.

Application modules should obtain configuration from the central configuration system rather than reading environment variables independently.

63. Configuration Access Rule

Preferred:

Environment
     |
     v
Configuration Module
     |
     v
Application Services

Avoid:

Service A -> os.getenv()
Service B -> os.getenv()
Service C -> os.getenv()
Service D -> os.getenv()

Centralization prevents configuration inconsistency.

64. Configuration Dependency Direction

The configuration module must not depend on:

API routes
database repositories
frontend
AI providers
business logic

It should be a low-level infrastructure component.

65. Configuration Loading Order

Recommended startup sequence:

1. Process starts
2. Environment variables loaded
3. Configuration object initialized
4. Configuration validated
5. Logging initialized
6. Database connection initialized
7. Application services initialized
8. API server starts
66. Frontend Configuration Loading

Frontend configuration should be loaded through the framework-supported environment mechanism.

Components should not contain hardcoded backend URLs.

Incorrect:

fetch("http://localhost:8000/api/v1/goals")

Preferred:

Configured API Base URL
        +
API route
67. Frontend API Configuration

The frontend should have a centralized API client.

Conceptual structure:

frontend/
└── src/
    └── services/
        └── api/
            └── client

The API client reads:

VITE_API_BASE_URL
68. Backend URL Construction

Backend URLs must not be duplicated throughout frontend components.

Instead:

API Client
    |
    +-- baseURL
    |
    +-- authentication
    |
    +-- error handling
    |
    +-- request handling
69. CORS and Frontend URL Consistency

The following must always agree:

Frontend actual URL
        =
CORS allowed origin

Example:

Frontend:
http://localhost:5173

CORS:
http://localhost:5173

Mismatch will result in browser request failures.

70. Localhost Consistency

The project should use a consistent local host convention.

Recommended:

Frontend:
localhost:5173

Backend:
127.0.0.1:8000

However, CORS should account for the actual browser origin.

If the frontend is accessed using:

http://localhost:5173

the backend must allow:

http://localhost:5173
71. Local Development Commands

The final README must provide commands similar to:

Frontend:

cd frontend
npm install
npm run dev

Backend:

cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000

The exact commands must match DOC-14 and DOC-13 after implementation.

72. Windows Environment Considerations

The primary development environment for the team may include Windows systems.

PowerShell activation:

.\.venv\Scripts\Activate.ps1

If PowerShell execution policy prevents activation, the team must follow the documented local Python environment setup procedure.

73. Python Virtual Environment

The backend must use an isolated Python virtual environment.

Recommended:

backend/.venv/

The directory must not be committed to Git.

74. Node Dependencies

Frontend dependencies must be installed through:

package.json
package-lock.json

or the package manager selected in DOC-14.

node_modules must not be committed.

75. Dependency Reproducibility

A fresh developer machine must be able to reproduce the environment from:

package.json
package-lock.json
requirements.txt

or the final dependency-management files selected by DOC-14.

76. .gitignore Configuration

The root .gitignore must include appropriate environment and dependency exclusions.

Minimum expected entries:

# Environment
.env
.env.*
!.env.example

# Python
__pycache__/
*.py[cod]
.venv/
venv/
.pytest_cache/
.mypy_cache/

# Node
node_modules/
dist/
build/

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db

# Logs
*.log

# Test coverage
.coverage
htmlcov/

# Temporary files
*.tmp
*.temp

The team may modify this based on the actual stack.

77. Environment File Exception

The following should remain committed:

.env.example

The following should not:

.env
78. Secret Rotation

Secrets must be replaceable without source-code changes.

Examples:

JWT_SECRET
AI_API_KEY
DATABASE_PASSWORD

Secret rotation should involve:

Generate new secret
        |
        v
Update environment/deployment secret
        |
        v
Restart/redeploy service
        |
        v
Verify functionality
79. Production Secret Storage

Production secrets should be stored in the hosting platform's secret/environment-variable mechanism.

Do not store production secrets in:

GitHub repository
README
documentation
source code
frontend bundle
screenshots
demo video
80. GitHub Secret Handling

If CI/CD requires secrets, they should be configured using:

GitHub Actions Secrets

or the equivalent secure CI/CD secret mechanism.

Never place secrets directly inside workflow YAML files.

81. Configuration and GitHub Issues

Configuration changes must be tracked through GitHub issues when they affect:

ports
API contracts
database configuration
AI provider
authentication
deployment
environment variables
82. Configuration Change Procedure

A configuration change should follow:

Identify change
      |
      v
Check affected documents
      |
      v
Create/update GitHub issue
      |
      v
Update documentation
      |
      v
Update .env.example
      |
      v
Update implementation
      |
      v
Update tests
      |
      v
Run integration checks
      |
      v
Commit
      |
      v
Push
83. Configuration Change Impact Matrix
Change	Potentially Affected Documents
Port	DOC-08, DOC-10, DOC-11, DOC-15, DOC-19
Database	DOC-07, DOC-11, DOC-14, DOC-15, DOC-19
JWT	DOC-08, DOC-12, DOC-15, DOC-18
AI Provider	DOC-09, DOC-14, DOC-15, DOC-19
Frontend URL	DOC-08, DOC-10, DOC-15, DOC-19
Backend URL	DOC-08, DOC-10, DOC-11, DOC-15, DOC-19
CORS	DOC-12, DOC-15, DOC-19
External API	DOC-08, DOC-09, DOC-11, DOC-15
Logging	DOC-11, DOC-15, DOC-18, DOC-19
84. Environment Compatibility Matrix
Component	Development	Testing	Production
Frontend	Local	Test	Deployed
Backend	Local	Test	Deployed
Database	Dev DB	Test DB	Production DB
AI	Mock/Provider	Mock	Configured Provider
CORS	Local origin	Test origin	Production origin
Logs	DEBUG	WARNING	INFO
Secrets	Local .env	Test secrets	Secure secret store
85. Configuration Ownership

Configuration responsibilities:

Area	Responsibility
Frontend variables	Frontend developer
Backend variables	Backend developer
Database variables	Backend/database owner
AI variables	AI/ML owner
Authentication variables	Backend/security owner
Deployment variables	Deployment owner
Documentation	Team lead / documentation owner

All final changes require team-level awareness when they affect integration.

86. Team Coordination Rule

No team member should independently rename an environment variable without checking:

frontend
backend
tests
deployment
documentation

For example, changing:

VITE_API_BASE_URL

to:

VITE_BACKEND_URL

requires coordinated changes wherever the variable is referenced.

87. Configuration Naming Registry

The following initial registry is authoritative.

Variable	Owner	Scope
APP_NAME	Backend	Application
APP_VERSION	Backend	Application
APP_ENV	Backend	Application
NODE_ENV	Tooling	Runtime
FRONTEND_URL	Backend	Integration
BACKEND_URL	Backend	Integration
VITE_API_BASE_URL	Frontend	Frontend
BACKEND_HOST	Backend	Server
BACKEND_PORT	Backend	Server
DATABASE_URL	Backend	Database
JWT_SECRET	Backend	Security
JWT_ALGORITHM	Backend	Security
ACCESS_TOKEN_EXPIRE_MINUTES	Backend	Security
AI_PROVIDER	AI	AI
AI_API_KEY	AI	AI
AI_MODEL	AI	AI
AI_TEMPERATURE	AI	AI
AI_MAX_TOKENS	AI	AI
AI_TIMEOUT_SECONDS	AI	AI
AI_FALLBACK_ENABLED	AI	AI
CORS_ORIGINS	Backend	Security
LOG_LEVEL	Backend	Operations
LOG_FORMAT	Backend	Operations
88. Variables That Must Never Reach Frontend

The following are server-only:

DATABASE_URL
DATABASE_PASSWORD
JWT_SECRET
AI_API_KEY
RESOURCE_API_KEY
EMAIL_PASSWORD

These must never be exposed through frontend environment variables.

89. Frontend Public Configuration

Only configuration required by the browser may be exposed.

Examples:

VITE_API_BASE_URL

Public configuration is not equivalent to secret configuration.

90. Configuration Security Classification
Configuration	Classification
API base URL	Public
Frontend URL	Public
Backend port	Internal
Database URL	Secret
JWT secret	Highly sensitive
AI API key	Highly sensitive
Database password	Highly sensitive
Log level	Internal
AI model name	Internal/Public
91. Local Secret Generation

Secrets should be generated using a secure random generator.

For example, a Python-based generation approach may be used locally.

The generated value should then be stored in:

.env

and never committed.

92. Configuration Health Check

The backend should provide a health endpoint as defined in DOC-08.

Example:

GET /api/v1/health

A health check should confirm application availability without exposing secrets.

93. Health Check Security

Health endpoints must not return:

DATABASE_URL
JWT_SECRET
AI_API_KEY
environment variables
passwords
internal credentials

A safe response may resemble:

{
  "status": "healthy"
}
94. Configuration Diagnostics

Development diagnostics may expose non-sensitive configuration status.

Example:

Database configured: true
AI provider configured: true
JWT configured: true

They must not expose actual secret values.

95. AI Provider Configuration Validation

If:

AI_PROVIDER=openai

then:

AI_API_KEY
AI_MODEL

must be validated.

If:

AI_PROVIDER=mock

then an external API key should not be required.

96. Database Configuration Validation

If the database is required, startup must verify:

DATABASE_URL exists
DATABASE_URL is syntactically valid
database connection can be established

The application should provide a clear error if the connection cannot be established.

97. Authentication Configuration Validation

The application should validate:

JWT_SECRET exists
JWT_SECRET meets minimum security requirements
JWT_ALGORITHM is supported
ACCESS_TOKEN_EXPIRE_MINUTES is valid
98. CORS Configuration Validation

The application should validate:

CORS_ORIGINS exists
origins are syntactically valid
production does not unintentionally use wildcard origins
99. Configuration Error Format

Configuration errors should be developer-readable.

Example:

Configuration Error:
DATABASE_URL is not configured.

Set DATABASE_URL in the backend environment file.
See:
docs/15-environment-and-configuration.md
100. Configuration and README

README.md must provide:

prerequisites
environment setup
.env creation
backend setup
frontend setup
database setup
startup commands
testing commands
troubleshooting
deployment reference

The README must reference this document for detailed configuration.

101. First-Time Developer Setup

A new developer should follow:

Clone repository
      |
      v
Install prerequisites
      |
      v
Install dependencies
      |
      v
Create .env files
      |
      v
Configure database
      |
      v
Configure AI provider
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
102. First-Time Environment Checklist

Developer must verify:

[ ] Repository cloned
[ ] Correct branch checked out
[ ] Python installed
[ ] Node.js installed
[ ] Backend virtual environment created
[ ] Backend dependencies installed
[ ] Frontend dependencies installed
[ ] .env created
[ ] Database configured
[ ] Database reachable
[ ] JWT secret configured
[ ] AI provider configured
[ ] CORS configured
[ ] Backend starts
[ ] Frontend starts
[ ] Health endpoint works
[ ] Login works
103. Local Development Ports

Initial standard:

Service	Port
Frontend	5173
Backend	8000
PostgreSQL	5432

These ports should remain stable throughout MVP development unless an ADR approves a change.

104. Port Conflict Procedure

If a developer has a local port conflict:

identify conflicting process
stop conflicting process where appropriate
prefer restoring standard project port
if changing project port is necessary, update all affected configuration

Do not silently use random ports that cause integration inconsistencies.

105. Localhost URL Registry

Initial development registry:

Frontend:
http://localhost:5173

Backend:
http://localhost:8000

API:
http://localhost:8000/api/v1

Health:
http://localhost:8000/api/v1/health

Database:
localhost:5432
106. API URL Construction

If:

VITE_API_BASE_URL=http://localhost:8000

and API prefix is:

/api/v1

then frontend requests should resolve to:

http://localhost:8000/api/v1/...

The API prefix must not be duplicated.

Incorrect:

http://localhost:8000/api/v1/api/v1/goals

Correct:

http://localhost:8000/api/v1/goals
107. Configuration and API Contract

DOC-08 defines:

API routes
methods
request structures
response structures
authentication requirements
error formats

DOC-15 defines the environment required to operate those APIs.

The two documents must remain synchronized.

108. Configuration and Database Design

DOC-07 defines:

entities
relationships
fields
constraints
indexes

DOC-15 defines:

connection configuration
database environment separation
database credentials
runtime database configuration

Changing database architecture requires reviewing both documents.

109. Configuration and AI Architecture

DOC-09 defines:

AI pipeline
model responsibilities
recommendation logic
embeddings
ranking
fallback architecture

DOC-15 defines:

provider configuration
API credentials
model configuration
timeout
fallback switches
110. Configuration and Frontend Architecture

DOC-10 defines:

frontend structure
pages
components
state
API integration

DOC-15 defines:

VITE_API_BASE_URL

and other frontend-safe configuration.

111. Configuration and Backend Architecture

DOC-11 defines:

backend modules
services
repositories
API layers
middleware

DOC-15 defines centralized runtime configuration used by those modules.

112. Configuration and Security

DOC-12 defines security architecture.

DOC-15 implements the configuration side of security by protecting:

JWT_SECRET
DATABASE_URL
AI_API_KEY
CORS_ORIGINS
113. Configuration and Folder Architecture

DOC-13 defines where configuration files and modules belong.

Expected conceptual structure:

HCL_PathFinder/
│
├── .gitignore
├── .env.example
│
├── frontend/
│   ├── .env.example
│   └── ...
│
├── backend/
│   ├── .env.example
│   └── app/
│       └── core/
│           └── config.py
│
└── docs/
114. Configuration and Dependency Management

DOC-14 defines dependencies.

DOC-15 must not introduce a new library merely for configuration without updating DOC-14.

If a configuration library is required, the dependency must be:

documented
installed
versioned
tested
115. Configuration and Testing

DOC-18 will define testing procedures.

Tests must verify:

configuration loading
missing variables
invalid variables
database configuration
authentication configuration
AI configuration
CORS configuration
environment separation
116. Configuration and Deployment

DOC-19 will define deployment architecture.

Production deployment must provide the variables defined here through secure environment configuration.

Deployment must not rely on:

developer .env
local database
localhost URLs
local secrets
117. Configuration and Integration

DOC-20 will contain integration checks.

Minimum configuration integration checks:

Frontend URL -> Backend
Backend -> Database
Backend -> AI Provider
Backend -> CORS
Backend -> Authentication
Testing -> Test Database
Production -> Production Services
118. Configuration and Master AI Specification

DOC-21 will provide the implementation specification used by AI coding assistants.

DOC-21 must reference DOC-15 for:

environment variables
configuration module
startup configuration
AI provider configuration
API URL configuration
database configuration

AI-generated code must not invent new environment variable names without updating this document.

119. Configuration Change Control

Any new environment variable requires:

Variable name
Purpose
Owner
Environment
Required/Optional
Default
Secret classification
Consumer
Documentation update
120. Environment Variable Change Template

When adding a variable:

Variable:
Purpose:
Environment:
Required:
Default:
Secret:
Used By:
Related Document:

Example:

Variable:
AI_TIMEOUT_SECONDS

Purpose:
Maximum external AI request duration.

Environment:
Development, Testing, Production

Required:
No

Default:
30

Secret:
No

Used By:
AI Service

Related Document:
DOC-09, DOC-11, DOC-15
121. Prohibited Configuration Practices

The following are prohibited:

hardcoded passwords
hardcoded API keys
hardcoded JWT secrets
hardcoded production URLs
hardcoded database credentials
committing .env
committing node_modules
committing .venv
using production database for tests
using wildcard production CORS without justification
reading environment variables randomly throughout business logic
122. Configuration Review Checklist

Before merging configuration-related code:

[ ] Variable name follows naming convention
[ ] Variable documented
[ ] .env.example updated
[ ] No secret committed
[ ] Configuration validation added
[ ] README updated if necessary
[ ] Tests updated
[ ] Related documents checked
[ ] Frontend/backend compatibility checked
[ ] Deployment impact checked
123. Developer Machine Validation

Each developer should be able to execute:

backend starts successfully
frontend starts successfully
database connects successfully
health endpoint succeeds
authentication works
profile works
goal creation works
recommendation endpoint works
learning path endpoint works
124. Configuration Smoke Test

A basic smoke test should validate:

Frontend
   |
   v
Backend
   |
   v
Database
   |
   v
AI/Recommendation Layer
125. Environment Failure Matrix
Failure	Expected Behavior
Missing DATABASE_URL	Backend startup failure
Invalid database URL	Clear database configuration error
Missing JWT_SECRET	Backend startup failure
Missing AI key	Provider-specific validation/fallback
AI unavailable	Fallback recommendation
Wrong CORS origin	Browser request blocked
Wrong API URL	Frontend API error
Test DB unavailable	Tests fail clearly
Production secret missing	Deployment/startup failure
126. Configuration Troubleshooting
Problem: Frontend cannot connect to backend

Check:

VITE_API_BASE_URL
backend running
backend port
API prefix
browser console
CORS
Problem: CORS error

Check:

frontend actual origin
CORS_ORIGINS
protocol
port

Example:

http://localhost:5173

must match the configured allowed origin.

Problem: Database connection failure

Check:

DATABASE_URL
database running
database port
database name
username
password
Problem: Authentication failure

Check:

JWT_SECRET
JWT_ALGORITHM
token expiration
Authorization header
frontend API client
Problem: AI recommendation failure

Check:

AI_PROVIDER
AI_API_KEY
AI_MODEL
AI_TIMEOUT_SECONDS
AI_FALLBACK_ENABLED
127. Configuration Testing Scenarios

The following tests should exist.

CFG-TEST-001

Application starts with valid configuration.

CFG-TEST-002

Application rejects missing database configuration.

CFG-TEST-003

Application rejects missing JWT secret.

CFG-TEST-004

Application handles mock AI provider.

CFG-TEST-005

Application handles unavailable AI provider.

CFG-TEST-006

Frontend uses configured backend URL.

CFG-TEST-007

Backend accepts configured frontend origin.

CFG-TEST-008

Testing environment uses test database.

CFG-TEST-009

Production configuration does not use localhost.

CFG-TEST-010

No secret is exposed in API responses or logs.

128. Configuration Acceptance Criteria

DOC-15 is accepted when:

[ ] Environment model is defined
[ ] Development configuration is defined
[ ] Testing configuration is defined
[ ] Production configuration is defined
[ ] Environment variable naming is standardized
[ ] Database configuration is defined
[ ] Authentication configuration is defined
[ ] AI configuration is defined
[ ] CORS configuration is defined
[ ] Logging configuration is defined
[ ] Secret-management rules are defined
[ ] Frontend configuration is defined
[ ] Backend configuration is defined
[ ] Configuration validation is defined
[ ] Configuration troubleshooting is documented
[ ] Cross-document dependencies are documented
129. Final Environment Variable Registry

The initial registry is:

APP_NAME=
APP_VERSION=
APP_ENV=
NODE_ENV=

FRONTEND_URL=
BACKEND_URL=
VITE_API_BASE_URL=

BACKEND_HOST=
BACKEND_PORT=

DATABASE_URL=

JWT_SECRET=
JWT_ALGORITHM=
ACCESS_TOKEN_EXPIRE_MINUTES=

AI_PROVIDER=
AI_API_KEY=
AI_MODEL=
AI_TEMPERATURE=
AI_MAX_TOKENS=
AI_TIMEOUT_SECONDS=
AI_FALLBACK_ENABLED=

CORS_ORIGINS=

LOG_LEVEL=
LOG_FORMAT=

No additional variable should be introduced casually.

130. Source of Truth Hierarchy

When configuration conflicts occur, use the following order:

1. Approved Architecture Decision Record
2. DOC-15 Environment & Configuration
3. DOC-14 Technology Stack & Dependencies
4. DOC-11 Backend Architecture
5. DOC-10 Frontend Architecture
6. DOC-09 ML/AI Architecture
7. DOC-08 API Contract
8. Implementation

Conflicts must be resolved before continuing implementation.

131. Configuration Freeze Rule

Once implementation begins, the following configuration should be treated as frozen for the MVP unless necessary:

Frontend port
Backend port
API prefix
Database technology
Environment variable naming
Authentication mechanism
AI abstraction boundary

Changes require review because they may affect multiple modules.

132. MVP Configuration Baseline

The MVP baseline is:

Frontend:
http://localhost:5173

Backend:
http://localhost:8000

API Prefix:
/api/v1

Database:
PostgreSQL

Authentication:
JWT-based

AI:
Provider abstraction with mock fallback

Configuration:
Environment variables

Frontend API variable:
VITE_API_BASE_URL

Backend database variable:
DATABASE_URL

Authentication secret:
JWT_SECRET

AI configuration:
AI_PROVIDER
AI_API_KEY
AI_MODEL

CORS:
CORS_ORIGINS
133. Configuration Dependency Map
                    DOC-15
                      |
       +--------------+--------------+
       |              |              |
       v              v              v
   Frontend        Backend        Database
       |              |              |
       |              v              |
       |        Authentication       |
       |              |              |
       |              v              |
       |         AI Service           |
       |              |              |
       +------------->|               |
                      v
                 API Contract
134. Cross-Document Consistency Matrix
Configuration Area	Primary Source	Supporting Documents
Environment	DOC-15	DOC-14
API URL	DOC-15	DOC-08, DOC-10, DOC-11
Database	DOC-15	DOC-07, DOC-11
JWT	DOC-15	DOC-12
AI	DOC-15	DOC-09, DOC-11
CORS	DOC-15	DOC-12
Frontend	DOC-15	DOC-10
Backend	DOC-15	DOC-11
Dependencies	DOC-14	DOC-15
Deployment	DOC-19	DOC-15
Testing	DOC-18	DOC-15
135. Implementation Rule for AI Coding Assistants

Any AI coding assistant generating project code must:

read DOC-01 through DOC-15 where relevant
use existing configuration names
not invent new ports
not invent new API base URLs
not invent new database technologies
not hardcode secrets
use the centralized configuration module
update .env.example when a new configuration value is approved
update documentation when configuration changes
preserve cross-document consistency
136. Example AI Coding Instruction

When generating backend code, the implementation instruction should effectively be:

Use the environment variables and configuration architecture defined in DOC-15.

Do not hardcode:
- database URLs
- JWT secrets
- AI API keys
- frontend URLs
- backend URLs

Use the centralized backend configuration module.

Do not create a second configuration mechanism.
137. Configuration Readiness for Implementation

Before writing production application code, the team must have:

[ ] DOC-14 approved
[ ] DOC-15 approved
[ ] Backend configuration module planned
[ ] Frontend environment file planned
[ ] Backend .env.example planned
[ ] Frontend .env.example planned
[ ] .gitignore configured
[ ] Database configuration decided
[ ] Authentication configuration decided
[ ] AI provider configuration decided
[ ] Local ports decided
[ ] API base path decided
138. Implementation Deliverables

The implementation phase must eventually produce:

.env.example
frontend/.env.example
backend/.env.example
backend/app/core/config.py
.gitignore
configuration validation
environment documentation
startup validation
configuration tests

Actual filenames must remain consistent with DOC-13.

139. Documentation Requirements

Any future configuration change must update:

DOC-15
.env.example
README.md
DOC-19 if deployment-related
DOC-18 if testing-related
DOC-20 if integration-related
DOC-23 traceability if requirement-related
140. Final Configuration Principle

PathFinder AI configuration must follow one simple rule:

Code defines behavior.
Environment defines deployment-specific values.
Documentation defines the contract.
Git tracks the implementation.
Secrets remain outside Git.
141. Document Completion Status

Document ID: DOC-15

Title: Environment and Configuration Specification

Status: DRAFT

Implementation Status: NOT STARTED

Configuration Baseline: DEFINED

Dependencies: DOC-01 through DOC-14

Next Document: DOC-16 Git & GitHub Development Workflow

Related Documents:

DOC-07 Database Design
DOC-08 API Contract
DOC-09 ML/AI Architecture
DOC-10 Frontend Architecture
DOC-11 Backend Architecture
DOC-12 Authentication and Security
DOC-13 Code and Folder Architecture
DOC-14 Technology Stack and Dependencies
DOC-16 Git & GitHub Development Workflow
DOC-18 Testing & QA Strategy
DOC-19 Deployment Architecture
DOC-20 Integration Checklist
DOC-21 Master AI Implementation Specification
DOC-23 Requirements Traceability Matrix
142. Approval Checklist

Before marking DOC-15 approved:

[ ] Team reviewed environment variables
[ ] Team reviewed frontend/backend ports
[ ] Team reviewed database configuration
[ ] Team reviewed JWT configuration
[ ] Team reviewed AI configuration
[ ] Team reviewed CORS
[ ] Team reviewed secret management
[ ] Team reviewed development environment
[ ] Team reviewed testing environment
[ ] Team reviewed production environment
[ ] Team confirmed compatibility with DOC-14
[ ] Team confirmed compatibility with DOC-13
[ ] Team confirmed compatibility with DOC-12
[ ] Team confirmed compatibility with DOC-11
[ ] Team confirmed compatibility with DOC-10
[ ] Team confirmed compatibility with DOC-09
[ ] Team confirmed compatibility with DOC-08
[ ] Team confirmed compatibility with DOC-07
143. Change History
Version	Date	Author	Change
1.0	2026-08-26	AlgoX	Initial environment and configuration specification
END OF DOC-15