# PathFinder AI — Architecture Decision Records

**Document ID:** DOC-22  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** APPROVED FOR MVP IMPLEMENTATION  
**Parent Documents:** DOC-01 through DOC-21  
**Related Documents:** DOC-06, DOC-07, DOC-08, DOC-09, DOC-10, DOC-11, DOC-12, DOC-13, DOC-14, DOC-15, DOC-16, DOC-17, DOC-18, DOC-19, DOC-20, DOC-21  
**Next Document:** DOC-23 Requirements Traceability Matrix  

---

# 1. Purpose

This document records the major architectural and technical decisions made for PathFinder AI.

The purpose of an Architecture Decision Record (ADR) is to document:

- what decision was made
- why the decision was made
- what alternatives were considered
- what consequences the decision creates
- which documents and implementation components are affected

These decisions provide a common technical foundation for implementation.

The project shall use this document to avoid contradictory technical decisions across the frontend, backend, database, AI layer, authentication system, deployment configuration, and testing strategy.

---

# 2. Decision Status Convention

Each decision uses one of the following statuses.

| Status | Meaning |
|---|---|
| Proposed | Decision is being evaluated |
| Accepted | Decision has been approved |
| Rejected | Alternative was explicitly rejected |
| Superseded | Decision has been replaced |
| Deprecated | Decision should no longer be used |

For the MVP, decisions marked **Accepted** are considered implementation constraints unless a later ADR explicitly changes them.

---

# 3. Decision Principles

The following principles apply to all architecture decisions.

## ADR-PRINCIPLE-001 — Simplicity First

The MVP architecture shall prioritize simplicity and reliability over unnecessary infrastructure complexity.

The system should be sufficiently modular for future expansion without introducing distributed-system complexity prematurely.

---

## ADR-PRINCIPLE-002 — Clear Separation of Responsibilities

The application shall maintain clear boundaries between:

- frontend
- backend API
- authentication
- database
- recommendation engine
- AI services
- external resource integrations
- testing
- deployment

UI components shall not contain core business logic that belongs in backend services.

---

## ADR-PRINCIPLE-003 — Deterministic Core Logic

Critical application behavior shall not depend exclusively on an external LLM.

Deterministic application logic shall remain available for:

- authentication
- authorization
- database operations
- skill matching
- prerequisite validation
- progress calculation
- recommendation filtering
- ranking logic
- learning-path ordering

AI services may enhance the system but shall not become the sole source of truth.

---

## ADR-PRINCIPLE-004 — API-First Backend

Frontend and backend communication shall occur through documented APIs.

The frontend shall not directly access protected database resources.

---

## ADR-PRINCIPLE-005 — Security by Default

Security requirements shall be considered during architecture and implementation rather than added after development.

Sensitive information shall be protected through:

- password hashing
- authentication
- authorization
- environment variables
- input validation
- secure error handling
- secret management

---

## ADR-PRINCIPLE-006 — Traceability

Major implementation decisions shall remain traceable to:

- requirements
- architecture
- database design
- API contracts
- testing
- deployment

---

# 4. ADR-001 — Monolithic Application Architecture

**Status:** Accepted

## Context

PathFinder AI is an MVP/prototype intended to demonstrate a complete personalized learning workflow.

A distributed microservice architecture would introduce additional complexity in:

- service discovery
- deployment
- networking
- authentication propagation
- observability
- debugging
- local development

The project does not currently require independent scaling of multiple services.

## Decision

PathFinder AI shall use a modular monolithic application architecture for the MVP.

The architecture shall contain logically separated modules while remaining deployable as a small number of application services.

Logical modules include:

- authentication
- learner profile
- goals
- skills
- resources
- recommendations
- prerequisites
- learning paths
- progress
- feedback
- AI orchestration

## Consequences

### Positive

- simpler development
- easier debugging
- easier deployment
- lower infrastructure requirements
- easier local development
- faster MVP delivery

### Negative

- modules share deployment lifecycle
- future independent scaling may require architectural evolution
- strict module boundaries must be maintained manually

## Implementation Rule

Business logic shall be organized into independent backend modules even though the application is deployed as a modular monolith.

---

# 5. ADR-002 — Frontend Framework

**Status:** Accepted

## Context

The application requires:

- interactive dashboards
- forms
- authentication pages
- learning-path visualization
- conversational UI
- progress tracking
- reusable components

## Decision

The frontend shall use:

**React**

The project shall use a modern React-based development environment.

## Consequences

React provides:

- component-based development
- reusable UI components
- strong ecosystem support
- straightforward state management
- suitable support for dashboards and interactive workflows

---

# 6. ADR-003 — Frontend Build Tool

**Status:** Accepted

## Decision

The frontend shall use:

**Vite**

for local development and production frontend builds.

## Reasons

- fast development server
- simple configuration
- modern JavaScript tooling
- fast rebuilds
- suitable React integration

---

# 7. ADR-004 — Backend Framework

**Status:** Accepted

## Context

PathFinder AI requires APIs for:

- authentication
- learner profiles
- goals
- skills
- recommendations
- learning paths
- progress
- feedback
- AI orchestration

The project also requires strong integration with Python-based AI and ML libraries.

## Decision

The backend shall use:

**Python + FastAPI**

## Reasons

FastAPI provides:

- REST API support
- automatic OpenAPI documentation
- request validation
- asynchronous support
- Python ecosystem integration
- straightforward AI/ML integration

---

# 8. ADR-005 — Database

**Status:** Accepted

## Decision

The primary application database shall be:

**PostgreSQL**

## Reasons

PathFinder AI requires structured relational relationships between:

- learners
- goals
- skills
- skill dependencies
- resources
- paths
- path items
- progress
- feedback

PostgreSQL provides strong support for relational integrity and structured queries.

---

# 9. ADR-006 — ORM/Data Access

**Status:** Accepted

## Decision

The backend shall use a Python ORM/data-access layer compatible with PostgreSQL.

The implementation shall keep database access separate from:

- API routes
- AI orchestration
- frontend logic

Database models and repository/service logic shall be organized according to DOC-07 and DOC-11.

---

# 10. ADR-007 — Authentication Strategy

**Status:** Accepted

## Decision

PathFinder AI shall use token-based authentication for API access.

The authentication design shall support:

- registration
- login
- password hashing
- authenticated requests
- authorization
- logout/session invalidation strategy where applicable

The implementation shall follow DOC-12.

---

# 11. ADR-008 — Password Storage

**Status:** Accepted

## Decision

Passwords shall never be stored in plaintext.

Passwords shall be securely hashed using an approved password hashing mechanism.

The database shall store only the password hash and related authentication metadata required by the implementation.

---

# 12. ADR-009 — Secret Management

**Status:** Accepted

## Decision

Secrets shall be provided through environment configuration.

Secrets may include:

- database credentials
- JWT/authentication secrets
- AI API keys
- external provider credentials
- deployment credentials

Secrets shall not be committed to Git.

The repository shall contain safe example configuration files without real credentials.

---

# 13. ADR-010 — REST API

**Status:** Accepted

## Decision

The primary frontend/backend communication mechanism shall be REST APIs.

API endpoints shall follow the contract defined in DOC-08.

The API shall use standard HTTP methods where appropriate:

- GET
- POST
- PUT/PATCH
- DELETE

---

# 14. ADR-011 — API Versioning

**Status:** Accepted

## Decision

The API shall support a versioned route structure.

The initial implementation shall use:

```text
/api/v1/

Example:

/api/v1/auth/login
/api/v1/goals
/api/v1/skills
/api/v1/recommendations
/api/v1/learning-paths
Reason

Versioning allows future API evolution without immediately breaking existing clients.

15. ADR-012 — API Validation

Status: Accepted

Decision

All externally supplied API data shall be validated before processing.

Validation shall occur for:

registration
login
profile updates
goal creation
skill updates
feedback
recommendation requests
path operations

Invalid data shall return controlled validation errors.

16. ADR-013 — API Documentation

Status: Accepted

Decision

The backend shall expose automatically generated API documentation through the FastAPI/OpenAPI ecosystem.

The documented API shall remain aligned with DOC-08.

17. ADR-014 — AI Architecture

Status: Accepted

Context

PathFinder AI requires AI capabilities for:

natural-language goal understanding
skill extraction
semantic matching
explanations
conversational assistance

However, deterministic application logic must remain reliable.

Decision

The AI layer shall use an orchestration architecture.

Conceptually:

User Request
     |
     v
AI Orchestrator
     |
     +---- Goal Understanding
     |
     +---- Skill Extraction
     |
     +---- Retrieval
     |
     +---- Recommendation Engine
     |
     +---- Explanation Generation
     |
     +---- Response Formatting

The orchestration layer shall remain independent of frontend components.

18. ADR-015 — External LLM Dependency

Status: Accepted

Decision

External LLM services may be used for:

natural-language understanding
conversational responses
explanation generation
structured extraction

However, external LLM output shall not directly modify critical database state without validation.

AI-generated structured output shall be validated before use.

19. ADR-016 — AI Provider Abstraction

Status: Accepted

Decision

The application shall use an abstraction layer for AI providers.

Conceptually:

AIService
   |
   +---- Provider A
   |
   +---- Provider B
   |
   +---- Local/Mock Provider

The rest of the backend shall communicate with the abstraction rather than directly depending on one provider.

Reason

This allows:

provider replacement
testing without external API calls
fallback behavior
reduced vendor coupling
20. ADR-017 — AI Fallback

Status: Accepted

Decision

The application shall provide graceful fallback behavior when an external AI provider is unavailable.

Fallback mechanisms may include:

deterministic recommendation rules
database-based matching
predefined responses
cached information
controlled error messages

Critical application functionality shall not completely fail because of an AI provider outage.

21. ADR-018 — Recommendation Architecture

Status: Accepted

Decision

Recommendations shall use a multi-stage process.

Learner Context
      |
      v
Candidate Generation
      |
      v
Filtering
      |
      v
Scoring
      |
      v
Ranking
      |
      v
Explanation
      |
      v
Final Recommendations

The recommendation pipeline shall consider:

active goal
current skills
skill gaps
experience
learning history
preferences
progress
prerequisites
22. ADR-019 — Hybrid Recommendation Strategy

Status: Accepted

Decision

The MVP shall use a hybrid recommendation approach combining deterministic logic with AI/semantic capabilities.

The architecture shall allow:

Rule-Based Signals
        +
Skill Matching
        +
Prerequisite Logic
        +
Semantic Similarity
        +
Learner Context
        =
Recommendation Score
Reason

A hybrid approach provides:

explainability
deterministic constraints
semantic relevance
future extensibility
23. ADR-020 — Recommendation Explainability

Status: Accepted

Decision

Each recommendation shall maintain machine-readable reasoning metadata where practical.

Possible signals include:

goal_match
skill_gap_match
difficulty_match
prerequisite_match
interest_match
progress_match
semantic_similarity

The frontend shall display a human-readable explanation derived from these signals.

24. ADR-021 — Skill Representation

Status: Accepted

Decision

Skills shall be represented as structured entities.

A skill may contain:

unique identifier
name
description
category/domain
proficiency information
relationships
metadata

Learner proficiency shall be represented separately from the canonical skill definition.

25. ADR-022 — Skill Dependency Graph

Status: Accepted

Decision

Skill prerequisites shall be represented as explicit relationships.

Example:

Python
   |
   v
NumPy
   |
   v
Machine Learning
   |
   v
Deep Learning

The graph shall support prerequisite-aware path generation.

26. ADR-023 — Learning Resource Representation

Status: Accepted

Decision

Learning resources shall be represented as structured database entities.

Resources may contain:

title
description
URL
provider
type
difficulty
duration
language
skills
prerequisites
rating
metadata

The resource model shall follow DOC-07.

27. ADR-024 — Resource Integration

Status: Accepted

Decision

External resource integrations shall be abstracted from recommendation logic.

Conceptually:

Resource Provider
      |
      v
Resource Adapter
      |
      v
Normalized Resource
      |
      v
PathFinder Resource Store

This prevents recommendation logic from becoming tightly coupled to one external provider.

28. ADR-025 — Learning Path Generation

Status: Accepted

Decision

Learning paths shall be generated using:

learner goal
current skills
skill gaps
prerequisite relationships
recommended resources
learner preferences
progress

The generated path shall preserve prerequisite ordering where applicable.

29. ADR-026 — Path Persistence

Status: Accepted

Decision

Generated learning paths shall be persisted.

The system shall distinguish between:

active path
historical path
regenerated path
completed path

Path regeneration shall not silently destroy historical information.

30. ADR-027 — Progress Tracking

Status: Accepted

Decision

Progress shall be tracked at activity and milestone levels.

Activity statuses shall support:

NOT_STARTED
IN_PROGRESS
COMPLETED
SKIPPED

Progress calculations shall be performed deterministically.

31. ADR-028 — Feedback Storage

Status: Accepted

Decision

Learner feedback shall be stored as structured data.

Feedback may include:

useful
not useful
too easy
too difficult
already known
not relevant

Feedback shall be linked to the relevant recommendation/resource/path where applicable.

32. ADR-029 — Adaptive Recommendations

Status: Accepted

Decision

Recommendation behavior may change according to:

completed activities
skipped activities
feedback
learner proficiency
goal changes
learning history

The adaptation mechanism shall initially remain rule-based and deterministic where practical.

More advanced personalization can be added later.

33. ADR-030 — Frontend State Management

Status: Accepted

Decision

Frontend state shall be divided into:

Local UI State

Examples:

modal visibility
form state
temporary filters
UI controls
Server State

Examples:

learner profile
goals
recommendations
learning paths
progress

Server data shall not be duplicated unnecessarily across unrelated components.

34. ADR-031 — Frontend API Layer

Status: Accepted

Decision

The frontend shall communicate with backend APIs through a dedicated API/client layer.

Components should not repeatedly implement raw HTTP request logic.

Conceptually:

UI Component
     |
     v
Feature Hook / Service
     |
     v
API Client
     |
     v
FastAPI
35. ADR-032 — Frontend Component Architecture

Status: Accepted

Decision

Frontend components shall be organized around reusable responsibilities.

Major areas include:

Authentication
Dashboard
Profile
Goals
Skills
Recommendations
Learning Path
Progress
Chat
Feedback
Common UI

Feature-specific components should remain close to their feature modules.

36. ADR-033 — Database Integrity

Status: Accepted

Decision

Database integrity shall be enforced wherever practical through:

primary keys
foreign keys
unique constraints
non-null constraints
controlled status values
indexes

Application-level validation shall complement database constraints.

37. ADR-034 — Database Migration

Status: Accepted

Decision

Database schema changes shall use a migration system.

Developers shall not rely on manually editing production database tables.

Every schema change shall be reproducible.

38. ADR-035 — Transaction Management

Status: Accepted

Decision

Operations involving multiple related database changes shall use transactions where required.

Examples:

creating a goal with related state
generating and persisting a learning path
updating progress and milestone state
recording related feedback

Partial database updates shall be avoided.

39. ADR-036 — Logging

Status: Accepted

Decision

Backend services shall produce structured and useful logs.

Logs should help diagnose:

authentication failures
API errors
database errors
AI failures
external integration failures
recommendation failures

Secrets and sensitive credentials shall never be logged.

40. ADR-037 — Error Handling

Status: Accepted

Decision

The backend shall use standardized error responses.

Errors shall provide:

HTTP status
machine-readable error identifier where applicable
safe human-readable message

Internal stack traces shall not be returned to users in production.

41. ADR-038 — HTTP Status Codes

Status: Accepted

The API shall use appropriate HTTP status codes.

Examples:

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
502 Bad Gateway
503 Service Unavailable

Only applicable statuses shall be used for each endpoint.

42. ADR-039 — CORS

Status: Accepted

Decision

The backend shall explicitly configure allowed frontend origins.

Development and production origins shall be configurable through environment variables.

Wildcard origins shall not be used unnecessarily for protected production APIs.

43. ADR-040 — Environment Separation

Status: Accepted

The project shall support separate configuration for:

development
testing
production

Configuration differences shall not require source-code modification.

44. ADR-041 — Configuration Management

Status: Accepted

Application configuration shall be centralized.

Configuration shall include:

application environment
database URL
authentication settings
AI provider configuration
external provider settings
CORS origins
logging configuration

Configuration shall be loaded from environment variables or environment-specific configuration.

45. ADR-042 — Local Development

Status: Accepted

The project shall support local development on a developer workstation.

The local environment shall provide:

frontend development server
backend development server
local/test database
environment configuration
dependency installation instructions

The setup process shall be documented in DOC-15.

46. ADR-043 — Dependency Management

Status: Accepted

Backend dependencies shall be explicitly versioned.

Frontend dependencies shall be recorded in the frontend package manifest and lockfile.

The project shall avoid undocumented runtime dependencies.

47. ADR-044 — Testing Strategy

Status: Accepted

The project shall use multiple testing levels.

Unit Tests
    |
    v
Integration Tests
    |
    v
API Tests
    |
    v
Frontend Tests
    |
    v
End-to-End Validation

Critical business logic shall have automated tests.

Testing requirements shall follow DOC-18.

48. ADR-045 — Test Database

Status: Accepted

Automated backend tests shall not depend on a developer's personal database.

Tests shall use:

isolated test database
temporary database
controlled test schema
appropriate fixtures

The exact implementation shall follow DOC-18.

49. ADR-046 — Mocking External AI

Status: Accepted

Tests shall be able to run without requiring a live external AI provider.

AI provider interfaces shall support mocks/fakes for testing.

This reduces:

cost
test instability
network dependency
provider rate-limit problems
50. ADR-047 — Git Workflow

Status: Accepted

Development shall use Git for version control.

The repository shall maintain:

meaningful commits
documented branches where required
pull-request discipline when collaborating
issue/task references where practical
clean project history

The workflow shall follow DOC-16.

51. ADR-048 — Task Management

Status: Accepted

Implementation tasks shall be derived from:

DOC-05 MVP scope
DOC-17 GitHub task breakdown
DOC-23 traceability matrix

Tasks shall identify:

feature
requirement
owner
dependencies
acceptance criteria
status
52. ADR-049 — Documentation as Source of Truth

Status: Accepted

The project documentation shall serve as the technical reference for implementation.

The following documents shall be considered authoritative for their respective areas:

DOC-01  Project Charter
DOC-02  Software Requirements
DOC-03  User Roles and Journeys
DOC-04  Use Cases and User Stories
DOC-05  MVP Scope
DOC-06  System Architecture
DOC-07  Database Design
DOC-08  API Contract
DOC-09  ML/AI Architecture
DOC-10  Frontend Architecture
DOC-11  Backend Architecture
DOC-12  Authentication and Security
DOC-13  Code and Folder Architecture
DOC-14  Technology Stack
DOC-15  Environment Configuration
DOC-16  Git/GitHub Workflow
DOC-17  GitHub Task Breakdown
DOC-18  Testing and QA
DOC-19  Deployment Architecture
DOC-20  Integration Checklist
DOC-21  Master AI Implementation Specification
DOC-22  Architecture Decision Records
DOC-23  Requirements Traceability Matrix
53. ADR-050 — Single Developer Implementation Model

Status: Accepted

Context

Although the competition documentation may describe team-oriented responsibilities, the actual implementation may be performed by a single developer.

The architecture must therefore avoid dependencies on simultaneous contributions from multiple developers.

Decision

The repository shall be structured so that one developer can independently:

install the project
run the frontend
run the backend
run the database
execute tests
modify AI logic
modify API endpoints
modify frontend features
deploy the application
Consequences

The architecture shall prioritize:

clear modules
simple setup
centralized documentation
reproducible environments
meaningful commits
minimal unnecessary infrastructure

No feature shall require multiple people to be online simultaneously.

54. ADR-051 — MVP-First Implementation

Status: Accepted

Decision

Implementation shall follow the MVP priority defined in DOC-05.

The project shall first implement the complete core learner journey.

Register
   ↓
Login
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
Progress
   ↓
Feedback
   ↓
Updated Recommendations

Advanced features shall not block the core workflow.

55. ADR-052 — Vertical Slice Development

Status: Accepted

Decision

Development shall prefer vertical slices that produce working functionality across the required layers.

Example:

Frontend
   ↓
API
   ↓
Service
   ↓
Database
   ↓
Response

Instead of implementing every frontend screen before backend integration, features should be connected progressively.

56. ADR-053 — Core Feature Development Order

Status: Accepted

The recommended implementation order is:

1. Repository setup
2. Backend environment
3. Frontend environment
4. Database connection
5. Database migrations
6. Authentication
7. Learner profile
8. Goals
9. Skills
10. Skill-gap analysis
11. Resources
12. Recommendation engine
13. Learning path generation
14. Progress tracking
15. Feedback
16. AI assistant
17. Dashboard
18. Adaptive recommendations
19. Testing
20. Deployment

This order may be adjusted when implementation dependencies require it.

57. ADR-054 — No Premature Microservices

Status: Accepted

The MVP shall not introduce independent microservices for:

authentication
recommendations
AI
resources
progress

unless a concrete technical requirement demonstrates the need.

Future extraction into services may occur if justified by a later ADR.

58. ADR-055 — No Direct Frontend Database Access

Status: Accepted

The frontend shall never directly access PostgreSQL.

All database operations shall pass through the backend API.

Frontend
   |
   v
Backend API
   |
   v
Service Layer
   |
   v
Database
59. ADR-056 — No Business Logic in UI Components

Status: Accepted

React components shall focus on:

rendering
user interaction
local UI state

Core logic such as:

recommendation scoring
prerequisite evaluation
skill-gap calculation
progress calculation

shall remain outside UI components.

60. ADR-057 — Recommendation Determinism

Status: Accepted

For identical learner state and identical resource data, deterministic recommendation components should produce reproducible results.

Randomized behavior shall not be introduced into the core recommendation pipeline without explicit justification.

61. ADR-058 — AI Output Validation

Status: Accepted

AI-generated structured output shall be validated before being used by backend services.

The system shall not assume that an LLM always returns:

valid JSON
valid skill IDs
valid resource IDs
valid enum values
valid database references

Invalid AI output shall be rejected or safely transformed.

62. ADR-059 — AI Does Not Own Application State

Status: Accepted

The AI service shall not be considered the source of truth for:

learner profile
goal status
skill records
progress
resource records
learning path records
feedback

The database and deterministic application services remain authoritative.

63. ADR-060 — Resource URL Safety

Status: Accepted

External learning-resource URLs shall be validated before being presented as trusted resource links.

The application shall avoid accepting arbitrary unsafe URLs without validation.

64. ADR-061 — Database as Source of Truth

Status: Accepted

Persistent application state shall be stored in PostgreSQL.

Temporary AI responses, frontend state, or cached values shall not replace persistent database state.

65. ADR-062 — Cache Strategy

Status: Accepted

Caching shall not be required for the initial MVP.

Caching may be introduced later for:

expensive AI operations
resource retrieval
frequently accessed reference data
recommendation results

Any caching layer must define invalidation behavior.

66. ADR-063 — Background Jobs

Status: Accepted

The MVP shall avoid introducing a complex background-job infrastructure unless required.

Long-running asynchronous tasks may later be moved to background workers.

Examples:

large resource ingestion
bulk embedding generation
expensive recommendation computation
67. ADR-064 — Search Strategy

Status: Accepted

The MVP shall support structured filtering and semantic matching without requiring a dedicated search cluster.

A dedicated search engine may be introduced later if resource volume requires it.

68. ADR-065 — Embedding Strategy

Status: Accepted

Semantic matching may use embeddings for:

learner goal representation
resource representation
skill representation

The embedding implementation shall remain abstracted so that the embedding provider can be changed.

69. ADR-066 — Vector Storage

Status: Accepted

The initial implementation shall avoid introducing a separate vector database unless required by actual scale.

If vector search is required, vector storage may initially be integrated with PostgreSQL-compatible capabilities where technically appropriate.

A separate vector database requires a new ADR.

70. ADR-067 — Resource Ingestion

Status: Accepted

External learning resources shall be normalized before entering the recommendation pipeline.

Normalization shall establish consistent fields for:

title
URL
provider
type
difficulty
skills
prerequisites
duration
71. ADR-068 — Data Ownership

Status: Accepted

The application shall distinguish between:

System-Owned Data

Examples:

skills
prerequisite relationships
resource metadata
system configuration
Learner-Owned Data

Examples:

profile
goals
progress
feedback
preferences

Authorization rules shall respect ownership boundaries.

72. ADR-069 — User Data Isolation

Status: Accepted

Authenticated learners shall only access their own private learner data unless a resource is explicitly public.

Backend queries shall apply user ownership checks where required.

73. ADR-070 — Administrative Access

Status: Accepted

Administrative functionality shall remain limited for the MVP.

Potential administrative capabilities include:

resource management
skill management
prerequisite management
dataset management

Administrative APIs shall be protected by role-based authorization.

74. ADR-071 — Role-Based Authorization

Status: Accepted

The application shall support authorization roles where necessary.

Initial conceptual roles:

LEARNER
ADMIN

The role model may be extended later.

75. ADR-072 — Data Privacy

Status: Accepted

The application shall collect only data required for the intended functionality.

Sensitive information shall not be collected unnecessarily.

Learner data shall be protected through authentication and authorization controls.

76. ADR-073 — Production Error Exposure

Status: Accepted

Production responses shall not expose:

stack traces
SQL statements
secret values
internal filesystem paths
provider credentials
internal implementation details

Detailed diagnostics shall remain in controlled server logs.

77. ADR-074 — Deployment Simplicity

Status: Accepted

The deployment architecture shall prioritize a simple reproducible deployment.

The deployed system shall contain:

Frontend
   |
   v
Backend API
   |
   v
PostgreSQL
   |
   +---- AI Provider
   |
   +---- External Resource Providers

Deployment details shall follow DOC-19.

78. ADR-075 — Production Configuration

Status: Accepted

Production configuration shall be provided through deployment environment variables or secure configuration management.

Development secrets shall never be reused as production secrets.

79. ADR-076 — Database Backups

Status: Accepted

Production database backups shall be enabled according to the selected deployment provider's capabilities.

The project shall document:

backup frequency
retention
restoration procedure

The exact provider configuration belongs to deployment documentation.

80. ADR-077 — Health Checks

Status: Accepted

The backend shall expose an appropriate health-check mechanism.

The health check should allow deployment infrastructure to determine whether the application is operational.

A health check shall not expose sensitive internal information.

81. ADR-078 — API Health Dependencies

Status: Accepted

The project shall distinguish between:

Application Health

Whether the backend application is running.

Dependency Health

Whether critical dependencies such as PostgreSQL are available.

A dependency failure shall produce a controlled status rather than an uncontrolled application crash.

82. ADR-079 — Frontend Environment Configuration

Status: Accepted

The frontend shall obtain environment-specific API configuration through its supported build-time environment mechanism.

The API base URL shall not be hardcoded throughout components.

83. ADR-080 — Backend Environment Configuration

Status: Accepted

Backend configuration shall be centralized and environment-driven.

Application code shall not contain hardcoded:

database passwords
API keys
authentication secrets
production URLs
84. ADR-081 — Repository Structure

Status: Accepted

The repository shall use a clear separation between:

frontend/
backend/
docs/
tests/
scripts/

Additional directories may be introduced where required by implementation.

The structure shall remain aligned with DOC-13.

85. ADR-082 — Documentation Directory

Status: Accepted

Project specification documents shall remain under:

docs/

Document numbering shall be preserved.

New architecture decisions shall be added to DOC-22 or recorded as a new ADR within the established documentation structure.

86. ADR-083 — Naming Conventions

Status: Accepted

The project shall use consistent naming conventions.

Examples:

Backend
snake_case
Frontend
camelCase
PascalCase for React components
Database

Consistent relational naming shall follow DOC-07.

API

REST resource naming shall follow DOC-08.

87. ADR-084 — API Response Consistency

Status: Accepted

API responses should use consistent structures.

Successful responses should contain predictable data.

Error responses should follow a standardized structure.

This simplifies frontend API integration.

88. ADR-085 — Pagination

Status: Accepted

List endpoints that may eventually contain large datasets shall support pagination where appropriate.

Potential endpoints include:

resources
recommendations
learning activities
feedback
administrative records

Pagination shall be introduced according to actual endpoint requirements.

89. ADR-086 — Filtering

Status: Accepted

Resource and recommendation endpoints may support query filtering.

Filtering shall be performed server-side where appropriate.

Frontend filtering shall not be relied upon as the only mechanism for large datasets.

90. ADR-087 — Sorting

Status: Accepted

Server-side sorting may be supported for list endpoints where useful.

Recommendation ranking shall remain part of recommendation business logic rather than generic API sorting.

91. ADR-088 — Rate Limiting

Status: Accepted

Rate limiting shall be considered for public or expensive endpoints.

Particular attention shall be given to:

login
registration
AI endpoints
recommendation generation

The exact production implementation shall be finalized during deployment hardening.

92. ADR-089 — AI Cost Control

Status: Accepted

AI calls shall be controlled to avoid unnecessary cost.

The application should:

avoid duplicate requests
limit prompt size
validate inputs
cache reusable outputs where appropriate
use deterministic logic where AI is unnecessary
93. ADR-090 — AI Prompt Management

Status: Accepted

AI prompts shall not be scattered throughout frontend components.

Prompts shall be maintained within the backend AI layer.

Prompt changes shall be version-controlled.

94. ADR-091 — AI Context Management

Status: Accepted

The AI assistant shall receive only relevant learner context.

Context may include:

active goal
current skills
skill gaps
progress
current path
relevant resources

Unnecessary private information shall not be included.

95. ADR-092 — Conversational Memory

Status: Accepted

Conversation history shall be controlled by the application.

The AI provider shall not be assumed to permanently retain learner conversation state.

If conversation persistence is required, it shall be stored according to the database design.

96. ADR-093 — Recommendation Regeneration

Status: Accepted

Recommendation regeneration shall be triggered when meaningful learner state changes.

Examples:

goal changed
skill updated
major activity completed
significant feedback submitted

Minor UI state changes shall not trigger expensive recommendation regeneration.

97. ADR-094 — Path Regeneration

Status: Accepted

Learning paths may be regenerated when:

learner goal changes
prerequisite state changes
major skills change
significant progress occurs
recommendation state changes materially

Existing path history shall remain available where required.

98. ADR-095 — Explainability Data

Status: Accepted

Recommendation explanations should originate from actual recommendation signals wherever possible.

The system shall avoid presenting fabricated reasoning.

Example:

Recommended because:
- You are targeting Machine Learning Engineer.
- Python is already present in your profile.
- NumPy is a prerequisite for the next skill.
- This resource covers the identified gap.
99. ADR-096 — Deterministic Progress

Status: Accepted

Progress percentages shall be calculated from stored activity/milestone state.

The AI layer shall not determine official completion percentages.

100. ADR-097 — Completion Criteria

Status: Accepted

A learning activity shall be considered completed only when the application records the appropriate completion state.

The frontend shall not independently determine persistent completion.

101. ADR-098 — Goal Lifecycle

Status: Accepted

Goals shall support a controlled lifecycle.

Conceptual states:

ACTIVE
PAUSED
COMPLETED
ARCHIVED

Only valid transitions shall be permitted.

102. ADR-099 — Resource Status

Status: Accepted

Resources shall support lifecycle information where required.

Possible states:

ACTIVE
INACTIVE
DEPRECATED

Inactive or invalid resources should not normally be recommended.

103. ADR-100 — Architecture Change Process

Status: Accepted

Any significant change to an accepted architectural decision shall require:

identification of the affected ADR
reason for change
alternatives considered
impact analysis
affected documents
affected code
affected tests
updated ADR status

A new decision shall not silently contradict an existing accepted decision.

104. ADR-101 — Documentation Consistency Rule

Status: Accepted

When an architectural decision changes, all affected documents shall be reviewed.

Potentially affected documents include:

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
DOC-18
DOC-19
DOC-20
DOC-21
DOC-23
105. ADR-102 — Implementation Source of Truth

Status: Accepted

During implementation, conflicts shall be resolved using the following priority:

Approved Architecture Decision
        ↓
Approved Requirement
        ↓
Detailed Architecture Document
        ↓
Implementation
        ↓
Developer Convenience

Developer convenience shall not override an approved security, data, or architectural requirement.

106. ADR-103 — Prototype vs Production Distinction

Status: Accepted

The project shall distinguish between:

MVP Prototype Requirements

Required to demonstrate the HCLTech competition workflow.

Production Hardening

Additional requirements for a production-grade system.

Production hardening may include:

advanced observability
stronger rate limiting
advanced caching
high availability
automated scaling
comprehensive backup strategy
advanced monitoring
advanced security controls

Prototype implementation shall not be unnecessarily delayed by production-only complexity.

107. ADR-104 — MVP Definition

Status: Accepted

The MVP is considered technically complete when the following workflow works end-to-end:

Register
   ↓
Login
   ↓
Create Profile
   ↓
Enter Learning Goal
   ↓
Add Current Skills
   ↓
Analyze Skill Gap
   ↓
Generate Recommendations
   ↓
Generate Learning Path
   ↓
View Explanations
   ↓
Start Learning Activity
   ↓
Update Progress
   ↓
Submit Feedback
   ↓
Re-rank Recommendations
108. ADR-105 — Implementation Quality Gate

Status: Accepted

A feature shall not be considered complete merely because the UI exists.

A feature should satisfy:

UI
+
API
+
Business Logic
+
Database
+
Validation
+
Error Handling
+
Testing
+
Documentation

where applicable.

109. ADR-106 — Definition of Done

A major feature is considered complete when:

requirements are identified
API contract is implemented
database changes are implemented
frontend integration is complete
backend logic is implemented
validation exists
error handling exists
tests pass
documentation is updated
Git changes are committed
repository remains reproducible
110. ADR-107 — Single-Developer Workflow

Status: Accepted

Because the project may be implemented by one developer, implementation shall follow a controlled sequence.

The developer shall work feature-by-feature rather than attempting to build all layers simultaneously.

Recommended workflow:

Read Requirement
      ↓
Read Related ADR
      ↓
Read Architecture
      ↓
Implement Database
      ↓
Implement Backend
      ↓
Test API
      ↓
Implement Frontend
      ↓
Integrate
      ↓
Test End-to-End
      ↓
Document
      ↓
Commit
111. ADR-108 — Avoiding Architecture Drift

Status: Accepted

Before implementing a feature, the developer shall check:

SRS requirement
user journey
use case
MVP scope
system architecture
database design
API contract
frontend architecture
backend architecture
AI architecture
security specification
technology stack
testing strategy
deployment architecture
master AI implementation specification

This prevents incompatible implementation decisions.

112. ADR-109 — Cross-Document Dependency Rule

Status: Accepted

Each implementation feature shall reference its relevant documentation.

Example:

Authentication
    |
    +-- DOC-02
    +-- DOC-08
    +-- DOC-12
    +-- DOC-14
    +-- DOC-15
    +-- DOC-18
    +-- DOC-20
    +-- DOC-23

The developer shall review all directly related documents before implementing a major feature.

113. ADR-110 — Technology Stack Freeze for MVP

Status: Accepted

The following core technology decisions are frozen for the MVP unless an ADR changes them:

Frontend:
React
Vite

Backend:
Python
FastAPI

Database:
PostgreSQL

API:
REST
/api/v1/

Authentication:
Token-based authentication
Secure password hashing

AI:
Provider abstraction
LLM-assisted capabilities
Deterministic fallback

Architecture:
Modular monolith

Version Control:
Git
GitHub
114. ADR-111 — No Unapproved Technology Introduction

Status: Accepted

A new infrastructure technology shall not be introduced into the MVP merely for convenience.

Examples requiring architectural review include:

Redis
Kafka
RabbitMQ
Kubernetes
Elasticsearch
separate vector databases
microservices
complex workflow engines

A technology may be introduced when a documented requirement justifies it.

115. ADR-112 — Dependency Minimization

Status: Accepted

The project shall avoid unnecessary third-party packages.

Every important dependency should have a clear purpose.

Dependencies shall be reviewed for:

maintenance
security
compatibility
license
project necessity
116. ADR-113 — Reproducible Setup

Status: Accepted

A clean developer environment shall be capable of reproducing the project using:

repository clone
documented runtime versions
dependency installation
environment configuration
database setup
migration execution
frontend startup
backend startup

The setup shall follow DOC-15.

117. ADR-114 — Clean Git State

Status: Accepted

Before major implementation milestones, the repository should have:

working tree clean

Changes should be committed in logical units.

Generated files and secrets shall not be committed unless explicitly required.

118. ADR-115 — Branch Strategy

Status: Accepted

The project may use:

main

as the stable branch.

Feature development may use temporary feature branches where useful.

For a single-developer workflow, direct development on main may be acceptable for small controlled changes, provided commits remain clean and recoverable.

119. ADR-116 — Commit Convention

Status: Accepted

Commit messages shall describe the change clearly.

Preferred categories include:

feat:
fix:
docs:
test:
refactor:
chore:

Examples:

feat: implement goal creation API
feat: add learning path generation
fix: validate recommendation request
docs: update API contract
test: add recommendation service tests
120. ADR-117 — Documentation Before Implementation

Status: Accepted

For major architecture-sensitive functionality, the relevant documentation should be established before implementation.

This prevents code-first decisions from silently redefining system requirements.

121. ADR-118 — Implementation After Specification

Status: Accepted

The project shall follow:

Specification
      ↓
Architecture
      ↓
Design
      ↓
Implementation
      ↓
Testing
      ↓
Deployment

The project shall not intentionally reverse this sequence for core architecture decisions.

122. ADR-119 — Final Architecture Consistency Check

Before the first complete implementation milestone, the following must agree:

DOC-02 Requirements
        |
        v
DOC-05 MVP
        |
        v
DOC-06 System Architecture
        |
        v
DOC-07 Database
        |
        v
DOC-08 API
        |
        v
DOC-09 AI
        |
        v
DOC-10 Frontend
        |
        v
DOC-11 Backend
        |
        v
DOC-12 Security
        |
        v
DOC-13 Code Structure
        |
        v
DOC-14 Technology
        |
        v
DOC-15 Environment
        |
        v
DOC-18 Testing
        |
        v
DOC-19 Deployment
        |
        v
DOC-20 Integration
        |
        v
DOC-21 AI Implementation
        |
        v
DOC-22 ADR
        |
        v
DOC-23 Traceability

Any contradiction shall be resolved before the affected feature is implemented.

123. Decision Summary

The major accepted MVP architecture decisions are summarized below.

Decision	Choice	Status
Application Architecture	Modular Monolith	Accepted
Frontend	React	Accepted
Frontend Build Tool	Vite	Accepted
Backend	Python + FastAPI	Accepted
Database	PostgreSQL	Accepted
API Style	REST	Accepted
API Version	/api/v1	Accepted
Authentication	Token-Based	Accepted
Password Storage	Secure Hashing	Accepted
Secrets	Environment Configuration	Accepted
AI	Provider Abstraction	Accepted
Recommendation	Hybrid	Accepted
Skill Model	Structured Graph	Accepted
Progress	Deterministic	Accepted
Frontend API Access	Dedicated API Layer	Accepted
Database Access	Backend Only	Accepted
Testing	Unit + Integration + API + E2E	Accepted
Deployment	Simple Reproducible Architecture	Accepted
Version Control	Git/GitHub	Accepted
Development Model	Single Developer Compatible	Accepted
MVP Strategy	Vertical Slices	Accepted
Microservices	Not Required	Rejected for MVP
Dedicated Search Cluster	Not Required Initially	Deferred
Dedicated Vector DB	Not Required Initially	Deferred
Complex Background Workers	Not Required Initially	Deferred
Kubernetes	Not Required for MVP	Deferred
124. Deferred Architecture Decisions

The following decisions may be revisited after MVP validation.

ADR-DEF-001

Advanced caching infrastructure.

ADR-DEF-002

Dedicated vector database.

ADR-DEF-003

Dedicated search engine.

ADR-DEF-004

Background job infrastructure.

ADR-DEF-005

Microservice decomposition.

ADR-DEF-006

Advanced recommendation models.

ADR-DEF-007

Real-time collaboration.

ADR-DEF-008

Advanced analytics infrastructure.

ADR-DEF-009

Multi-region deployment.

ADR-DEF-010

High-availability architecture.

125. ADR Change Template

Future architectural decisions shall use the following structure.

# ADR-XXX — Decision Title

Status:
Proposed / Accepted / Rejected / Superseded / Deprecated

Context:
Describe the problem.

Decision:
Describe the selected solution.

Alternatives:
List alternatives considered.

Reasons:
Explain why the decision was selected.

Consequences:
Describe positive and negative effects.

Affected Documents:
List affected documentation.

Affected Components:
List affected implementation areas.

Affected Tests:
List affected tests.

Migration:
Describe migration requirements if applicable.
126. Approval Criteria

DOC-22 is considered approved when:

major architectural decisions are documented
technology decisions are consistent with DOC-14
database decisions are consistent with DOC-07
API decisions are consistent with DOC-08
frontend decisions are consistent with DOC-10
backend decisions are consistent with DOC-11
security decisions are consistent with DOC-12
deployment decisions are consistent with DOC-19
AI decisions are consistent with DOC-09 and DOC-21
single-developer implementation is supported
MVP scope remains achievable
deferred decisions are clearly identified
127. Final Architecture Rule

The following rule applies to the entire project:

No implementation decision should silently contradict an approved requirement, architecture decision, security rule, database design, API contract, or technology-stack decision.

If a contradiction is discovered, the developer shall:

Identify Conflict
      ↓
Determine Affected Documents
      ↓
Evaluate Alternatives
      ↓
Create/Update ADR
      ↓
Update Documentation
      ↓
Update Implementation
      ↓
Update Tests
      ↓
Commit Changes

This ensures that PathFinder AI remains internally consistent throughout development.

128. Document Status

Document ID: DOC-22
Status: APPROVED FOR MVP IMPLEMENTATION
Architecture Status: MVP ARCHITECTURE FROZEN
Implementation Status: READY TO BEGIN
Parent: DOC-01 Project Charter
Previous: DOC-21 Master AI Implementation Specification
Next: DOC-23 Requirements Traceability Matrix

129. Change Control

Any future change to an accepted architecture decision must identify:

ADR affected
original decision
proposed change
reason
alternatives
impact on requirements
impact on database
impact on API
impact on frontend
impact on backend
impact on AI
impact on security
impact on testing
impact on deployment
migration requirements

All approved changes shall be reflected in the relevant project documents and DOC-23 Requirements Traceability Matrix.