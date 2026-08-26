DOC-17 — GITHUB TASK BREAKDOWN & IMPLEMENTATION PLAN

Document ID: DOC-17
Project: PathFinder AI
Competition: HCLTech Round 2 — PathFinder Prototype
Team: AlgoX
Development Model: Single Developer / Solo Implementation
Version: 1.0
Status: DRAFT
Parent Documents: DOC-01 through DOC-16
Next Document: DOC-18 Testing and QA Strategy

1. Purpose

This document converts the approved PathFinder AI architecture and requirements into an executable GitHub development plan.

The purpose of DOC-17 is to ensure that implementation is performed in a controlled sequence so that:

frontend and backend contracts remain synchronized
database models match API requirements
AI modules match the defined data structures
authentication is implemented before protected features
recommendation logic is implemented before learning-path generation
learning-path generation is implemented before progress tracking
testing occurs throughout development rather than only at the end
deployment is prepared without changing the core architecture
every implementation task can be traced to a requirement
AI-assisted coding tools can understand the intended implementation order
the entire project can be developed by one developer without creating conflicting parallel implementations

This document is therefore the primary implementation roadmap for the project.

2. Development Model
2.1 Development Approach

PathFinder AI will be developed as a single integrated application.

The implementation will not be divided into independent team-member branches.

The preferred development model is:

Specification
      ↓
Repository Setup
      ↓
Backend Foundation
      ↓
Database
      ↓
Authentication
      ↓
Core APIs
      ↓
AI / Recommendation Engine
      ↓
Learning Path Engine
      ↓
Frontend
      ↓
Integration
      ↓
Testing
      ↓
Deployment
      ↓
Demo Preparation
3. Single-Developer Rule

The project shall assume that one developer is responsible for:

repository management
backend implementation
frontend implementation
database implementation
AI/ML implementation
testing
deployment
documentation synchronization

This prevents architectural decisions from being duplicated across multiple independent implementations.

4. GitHub Project Structure

GitHub shall be used as the central implementation tracking system.

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
├── .github/
│   ├── ISSUE_TEMPLATE/
│   └── workflows/
│
├── .gitignore
├── .env.example
└── LICENSE

The exact source-code structure must remain consistent with:

DOC-06 System Architecture
DOC-10 Frontend Architecture
DOC-11 Backend Architecture
DOC-13 Code and Folder Architecture
DOC-14 Technology Stack and Dependencies
DOC-15 Environment and Configuration
5. GitHub Milestone Structure

The implementation shall be divided into the following milestones.

Milestone	Name	Purpose
M0	Project Foundation	Repository and development environment
M1	Backend Foundation	Backend application infrastructure
M2	Database	Database schema and persistence
M3	Authentication	User registration and authentication
M4	Learner Profile	Profile and learner-state management
M5	Goal Management	Learning-goal management
M6	Skill Intelligence	Skills and skill-gap analysis
M7	Resource Engine	Learning-resource management
M8	Recommendation Engine	Personalized recommendations
M9	Learning Path Engine	Roadmap generation
M10	AI Assistant	Conversational AI
M11	Frontend	User interface
M12	Progress & Feedback	Tracking and adaptation
M13	Integration	Full-system integration
M14	Testing	QA and validation
M15	Deployment	Production/demo deployment
M16	Demo & Submission	Competition deliverables
6. Implementation Dependency Rule

Tasks shall not be implemented out of dependency order unless the task is explicitly independent.

The fundamental dependency chain is:

Project Foundation
        ↓
Environment
        ↓
Backend Foundation
        ↓
Database
        ↓
Authentication
        ↓
Profile
        ↓
Goals
        ↓
Skills
        ↓
Resources
        ↓
Recommendation Engine
        ↓
Learning Path Engine
        ↓
AI Assistant
        ↓
Frontend Integration
        ↓
Progress
        ↓
Feedback
        ↓
Adaptation
        ↓
Testing
        ↓
Deployment
7. GitHub Issue Naming Convention

Every issue shall use the following format:

[TYPE] Short Task Description

Examples:

[SETUP] Initialize backend project
[DB] Implement learner schema
[AUTH] Implement registration endpoint
[API] Implement goal endpoints
[AI] Implement goal extraction
[ML] Implement recommendation ranking
[FE] Create learner dashboard
[TEST] Add authentication tests
[DEPLOY] Configure production environment
[DOCS] Update API documentation
8. GitHub Issue Labels

Recommended labels:

Label	Purpose
setup	Project setup
backend	Backend implementation
frontend	Frontend implementation
database	Database work
api	API work
auth	Authentication/security
ai	AI functionality
ml	Machine learning/recommendation
data	Data processing
testing	Testing
deployment	Deployment
documentation	Documentation
bug	Defect
integration	Cross-module integration
priority-p0	Mandatory MVP
priority-p1	Important
priority-p2	Enhancement
blocked	Blocked task
9. Priority Rules

The following priority system shall be used.

P0 = Required for working MVP
P1 = Important feature
P2 = Enhancement
P3 = Future feature

A P2/P3 feature shall never block completion of the P0 MVP.

10. Milestone M0 — Project Foundation
TASK-001 — Repository Verification

Type: Setup
Priority: P0

Objective

Verify the GitHub repository and documentation structure.

Dependencies

None.

Requirements

Confirm:

README.md
docs/

exist.

Acceptance Criteria
Repository opens correctly.
Main branch exists.
Documentation is accessible.
Git history is working.
Working tree is clean.
11. TASK-002 — Git Configuration

Priority: P0

Configure:

.gitignore

The .gitignore must exclude:

.env
.env.*
__pycache__/
*.pyc
venv/
.venv/
node_modules/
dist/
build/
coverage/
.pytest_cache/
.idea/
.vscode/
.DS_Store
Acceptance Criteria

Secrets and generated dependencies cannot accidentally be committed.

12. TASK-003 — Environment Template

Priority: P0

Create:

.env.example

It shall document required configuration variables.

Example categories:

DATABASE_URL
JWT_SECRET
AI_API_KEY
AI_MODEL
FRONTEND_URL
BACKEND_URL
ENVIRONMENT
LOG_LEVEL

Actual secret values must never be committed.

Related Documents

DOC-12
DOC-15

13. TASK-004 — Project README

Priority: P0

README must contain:

project overview
architecture overview
technology stack
setup instructions
environment variables
backend startup
frontend startup
testing
API information
deployment information
demo instructions
14. Milestone M1 — Backend Foundation
TASK-005 — Initialize Backend

Priority: P0

Create backend application according to DOC-11 and DOC-13.

Acceptance Criteria

Backend starts successfully.

Example:

GET /health

returns a successful response.

15. TASK-006 — Backend Configuration

Implement centralized configuration.

Configuration must not be scattered throughout application code.

Required areas:

database
authentication
AI
CORS
environment
logging
16. TASK-007 — API Error Handling

Implement centralized error handling.

Errors must return predictable API structures.

Example:

{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request"
  }
}

Internal stack traces must not be exposed in production.

17. TASK-008 — Request Validation

Implement validation for all API inputs.

Validation shall occur before business logic.

18. TASK-009 — Logging

Implement structured application logging.

Logs must not contain:

passwords
JWT secrets
API keys
tokens
sensitive personal information
19. Milestone M2 — Database
TASK-010 — Database Connection

Implement database connectivity according to DOC-07 and DOC-14.

Acceptance Criteria

Backend can:

connect
query
insert
update
disconnect

without application crashes.

20. TASK-011 — Learner Data Model

Implement the learner model according to DOC-07.

Must support the profile requirements from DOC-02.

21. TASK-012 — Goal Data Model

Implement goal persistence.

Must support:

goal
description
status
created_at
updated_at
target information

Exact fields must follow DOC-07.

22. TASK-013 — Skill Data Model

Implement:

skills
learner skills
skill proficiency
skill relationships
23. TASK-014 — Resource Data Model

Implement learning-resource persistence.

Resources must support metadata required by the recommendation engine.

24. TASK-015 — Learning Path Data Model

Implement:

learning paths
path stages
path items
milestones
25. TASK-016 — Progress Data Model

Implement progress persistence.

26. TASK-017 — Feedback Data Model

Implement feedback persistence.

27. TASK-018 — Database Indexes

Add indexes required for:

user lookup
goal lookup
skill lookup
resource lookup
path lookup
progress lookup
feedback lookup

Indexes must be based on actual query patterns.

28. TASK-019 — Database Seed Data

Create development/demo seed data.

Seed data shall include:

skills
skill relationships
learning resources
resource skills
sample users if required

Seed data must be reproducible.

29. Milestone M3 — Authentication
TASK-020 — Password Hashing

Implement secure password hashing.

Passwords must never be stored directly.

30. TASK-021 — Registration API

Implement:

POST /auth/register

according to DOC-08.

31. TASK-022 — Login API

Implement:

POST /auth/login
32. TASK-023 — Authentication Middleware

Implement authentication protection for protected routes.

33. TASK-024 — Current User API

Implement authenticated-user retrieval.

Example:

GET /auth/me
34. TASK-025 — Logout

Implement logout behavior according to the authentication strategy defined in DOC-12.

35. TASK-026 — Authentication Security Tests

Test:

valid registration
duplicate email
invalid password
invalid login
unauthorized request
authorized request
36. Milestone M4 — Learner Profile
TASK-027 — Profile API

Implement profile creation and retrieval.

37. TASK-028 — Profile Update

Allow users to update:

experience
interests
learning preferences
skills
learning history

according to the approved schema.

38. TASK-029 — Profile Validation

Validate profile information.

39. TASK-030 — Learner State Service

Create a centralized service that retrieves the learner state required by AI/recommendation components.

Conceptually:

Learner State
    ├── profile
    ├── goals
    ├── skills
    ├── history
    ├── preferences
    ├── progress
    └── feedback

This service becomes a critical integration boundary.

40. Milestone M5 — Goal Management
TASK-031 — Goal Creation API

Implement goal creation.

41. TASK-032 — Goal Retrieval API

Implement active and historical goal retrieval.

42. TASK-033 — Goal Update API

Implement goal modification.

43. TASK-034 — Goal Status Management

Support:

ACTIVE
PAUSED
COMPLETED
ARCHIVED

according to DOC-07.

44. TASK-035 — Natural Language Goal Input

Allow the learner to enter:

"I want to become a machine learning engineer."

The input shall be sent to the goal-understanding service.

45. Milestone M6 — Skill Intelligence
TASK-036 — Skill Catalog

Create structured skill data.

Example:

Python
NumPy
Pandas
Statistics
Machine Learning
Deep Learning
SQL
46. TASK-037 — Skill Relationship Graph

Implement relationships such as:

Python
   ↓
NumPy
   ↓
Machine Learning

Relationships must follow DOC-07 and DOC-09.

47. TASK-038 — Learner Skill Management

Implement APIs for learner skills.

48. TASK-039 — Skill Proficiency

Implement proficiency representation.

49. TASK-040 — Goal Skill Requirements

Map target goals to required skills.

50. TASK-041 — Skill Gap Analysis

Implement:

required skills
      -
learner skills
      =
skill gaps

The actual scoring method must follow DOC-09.

51. TASK-042 — Skill Gap Explanation

Return explanations such as:

Python: sufficient
Statistics: basic
Machine Learning: missing
Deep Learning: missing
52. Milestone M7 — Resource Engine
TASK-043 — Resource Repository

Implement learning-resource retrieval.

53. TASK-044 — Resource Metadata

Ensure every resource has structured metadata.

54. TASK-045 — Resource-Skill Mapping

Connect resources to skills.

55. TASK-046 — Resource Prerequisites

Implement prerequisite relationships.

56. TASK-047 — Resource Filtering

Support filtering by:

skill
difficulty
type
duration
provider
57. TASK-048 — Resource Search

Implement keyword and/or semantic resource retrieval according to DOC-09.

58. Milestone M8 — Recommendation Engine
TASK-049 — Recommendation Input Model

Create a standardized recommendation request.

Conceptually:

RecommendationRequest
    ├── learner
    ├── goal
    ├── skills
    ├── skill_gaps
    ├── history
    ├── preferences
    └── progress

This model must be shared by recommendation services.

59. TASK-050 — Candidate Generation

Generate candidate resources.

Candidates must address learner needs.

60. TASK-051 — Recommendation Scoring

Implement recommendation scoring.

The scoring method must follow DOC-09.

Possible signals include:

goal relevance
skill-gap relevance
difficulty fit
prerequisite compatibility
learner preference
learning history
resource quality
61. TASK-052 — Recommendation Ranking

Rank candidates.

62. TASK-053 — Recommendation Reason

Generate structured reasons.

Example:

{
  "reason_codes": [
    "SKILL_GAP",
    "GOAL_ALIGNMENT",
    "PREREQUISITE_MATCH"
  ]
}
63. TASK-054 — Recommendation API

Expose recommendations through the API defined in DOC-08.

64. TASK-055 — Recommendation Evaluation

Create test cases for different learner profiles.

Example:

Beginner learner
Intermediate learner
Advanced learner

The recommendation results should differ appropriately.

65. Milestone M9 — Learning Path Engine
TASK-056 — Path Generation Input

Define the standardized path-generation input.

66. TASK-057 — Prerequisite Ordering

Ensure resources/skills appear in logically valid order.

67. TASK-058 — Path Stage Generation

Generate stages such as:

Foundation
Core Skills
Applied Learning
Projects
Assessment

Actual stages must remain consistent with DOC-06 and DOC-09.

68. TASK-059 — Milestone Generation

Generate measurable milestones.

69. TASK-060 — Learning Activity Assignment

Assign:

courses
videos
articles
projects
assessments

where appropriate.

70. TASK-061 — Effort Estimation

Estimate learning effort using available resource metadata.

71. TASK-062 — Next Action

Determine the learner's immediate next action.

72. TASK-063 — Path Persistence

Persist generated learning paths.

73. TASK-064 — Path Retrieval API

Implement path retrieval according to DOC-08.

74. TASK-065 — Path Regeneration

Allow path regeneration when learner state changes significantly.

75. Milestone M10 — AI Assistant
TASK-066 — AI Service Abstraction

Create an abstraction between application logic and external AI providers.

The rest of the application must not directly depend on a specific AI SDK.

Conceptually:

Application
     ↓
AI Service Interface
     ↓
Provider Adapter
     ↓
External LLM
76. TASK-067 — Goal Understanding

Implement natural-language goal understanding.

Input:

"I want to become a data scientist."

Potential structured output:

target_role
domain
skills
experience
objective
77. TASK-068 — Skill Extraction

Extract relevant skills from learner input.

78. TASK-069 — AI Recommendation Explanation

Generate natural-language explanations from structured recommendation reasons.

79. TASK-070 — AI Chat Context

Provide relevant learner context to the conversational assistant.

The context must be limited to information required for the request.

80. TASK-071 — Chat API

Implement the conversational API defined in DOC-08.

81. TASK-072 — AI Safety and Validation

AI output must not directly modify critical database state without application validation.

The application shall validate structured AI output before persistence.

82. TASK-073 — AI Fallback

Implement fallback behavior if:

AI provider unavailable
API timeout
invalid AI output
rate limit
service failure
83. Milestone M11 — Frontend
TASK-074 — Frontend Initialization

Initialize frontend according to DOC-10 and DOC-14.

84. TASK-075 — Frontend Routing

Implement required application routes.

Expected logical areas include:

/login
/register
/onboarding
/dashboard
/goals
/learning-path
/chat
/profile

Exact routes must follow DOC-10 and DOC-08.

85. TASK-076 — Authentication UI

Implement:

registration
login
logout
authenticated state
86. TASK-077 — Onboarding UI

Create onboarding flow.

The learner should be able to provide:

experience
interests
skills
goal
preferences
87. TASK-078 — Goal UI

Allow natural-language goal creation.

88. TASK-079 — Skill Gap UI

Display:

current skills
required skills
skill gaps
89. TASK-080 — Recommendation UI

Display recommended resources.

Each recommendation should show:

resource
type
difficulty
estimated effort
skills
reason
90. TASK-081 — Learning Path UI

Display the generated roadmap.

Example:

Goal
 ↓
Foundation
 ↓
Core Skills
 ↓
Project
 ↓
Assessment
91. TASK-082 — Progress Dashboard

Display:

overall progress
completed items
current milestone
skills
next action
92. TASK-083 — AI Chat UI

Implement conversational interface.

93. TASK-084 — Responsive UI

Validate supported screen sizes.

94. Milestone M12 — Progress and Feedback
TASK-085 — Activity Completion API

Allow learners to mark learning activities complete.

95. TASK-086 — Progress Calculation

Calculate:

activity progress
milestone progress
overall path progress
96. TASK-087 — Skill Progress

Reflect relevant completed learning activities in skill progress.

97. TASK-088 — Recommendation Feedback

Allow:

Useful
Not Useful
Too Easy
Too Difficult
Already Known
Not Relevant
98. TASK-089 — Feedback Persistence

Store feedback.

99. TASK-090 — Feedback Integration

Feed relevant feedback into recommendation ranking.

100. TASK-091 — Progress-Based Adaptation

Use learner progress to influence future recommendations.

101. TASK-092 — Difficulty Adaptation

Adjust difficulty when sufficient learner evidence exists.

102. Milestone M13 — Full Integration
TASK-093 — Authentication Integration

Verify:

Frontend
 ↓
Auth API
 ↓
Database

works end-to-end.

103. TASK-094 — Profile Integration

Verify:

Frontend
 ↓
Profile API
 ↓
Database
104. TASK-095 — Goal Integration

Verify:

Goal UI
 ↓
Goal API
 ↓
Goal Service
 ↓
Database
105. TASK-096 — AI Goal Integration

Verify:

Natural Language Goal
        ↓
AI Goal Understanding
        ↓
Structured Goal
        ↓
Database
106. TASK-097 — Skill Gap Integration

Verify:

Learner Profile
       +
Goal
       ↓
Required Skills
       ↓
Skill Gap Engine
       ↓
Skill Gaps
107. TASK-098 — Recommendation Integration

Verify:

Learner State
      ↓
Skill Gaps
      ↓
Candidate Generation
      ↓
Ranking
      ↓
Recommendations
108. TASK-099 — Path Integration

Verify:

Recommendations
      +
Prerequisites
      +
Goal
      +
Skill Gaps
      ↓
Learning Path
109. TASK-100 — Progress Integration

Verify:

Learning Path
      ↓
Activity
      ↓
Completion
      ↓
Progress
      ↓
Updated Recommendations
110. TASK-101 — Chat Integration

Verify:

Learner
 ↓
Chat UI
 ↓
Chat API
 ↓
AI Service
 ↓
Learner Context
 ↓
Response
111. TASK-102 — Full E2E Workflow

Run the complete workflow:

Register
 ↓
Login
 ↓
Onboarding
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
AI Explanation
 ↓
Start Learning
 ↓
Complete Activity
 ↓
Progress Update
 ↓
Feedback
 ↓
Recommendation Update

This is the most important integration task in the project.

112. Milestone M14 — Testing
TASK-103 — Unit Testing Foundation

Configure unit-testing infrastructure.

113. TASK-104 — Backend Unit Tests

Test:

services
utilities
validators
business logic
recommendation scoring
skill-gap calculations
path generation
114. TASK-105 — API Tests

Test every major API endpoint.

115. TASK-106 — Database Tests

Test:

create
read
update
delete
relationships
constraints
116. TASK-107 — Authentication Tests

Test authentication security.

117. TASK-108 — AI Tests

Test:

valid AI output
invalid AI output
AI timeout
AI unavailable
malformed response
118. TASK-109 — Recommendation Tests

Create deterministic test cases.

119. TASK-110 — Learning Path Tests

Test:

ordering
prerequisites
milestones
next action
120. TASK-111 — Frontend Tests

Test major UI components and workflows.

121. TASK-112 — End-to-End Tests

Test the complete learner journey.

122. TASK-113 — Error Scenario Tests

Test:

invalid input
database failure
AI failure
resource failure
unauthorized access
expired authentication
empty recommendations
123. TASK-114 — Performance Validation

Measure key operations.

At minimum:

login
profile retrieval
goal processing
skill-gap analysis
recommendations
path generation
chat
124. TASK-115 — Security Review

Verify:

secrets
authentication
authorization
input validation
CORS
error responses
logging
125. Milestone M15 — Deployment
TASK-116 — Production Configuration

Create production environment configuration.

126. TASK-117 — Production Database

Configure production database.

127. TASK-118 — Backend Deployment

Deploy backend.

128. TASK-119 — Frontend Deployment

Deploy frontend.

129. TASK-120 — CORS Configuration

Configure:

Frontend URL
Backend URL
130. TASK-121 — Production AI Configuration

Configure AI provider securely.

131. TASK-122 — Deployment Health Check

Verify:

frontend accessible
backend accessible
database connected
AI available
authentication working
132. TASK-123 — Production Smoke Test

Run the complete learner journey against the deployed application.

133. Milestone M16 — Demo and Submission
TASK-124 — Demo Dataset

Prepare stable demo data.

134. TASK-125 — Demo User

Create a clean demo learner account.

135. TASK-126 — Demo Scenario

Prepare one strong end-to-end scenario.

Recommended scenario:

Goal:
Become a Machine Learning Engineer

Current Skills:
Python
Basic SQL

Experience:
Beginner/Intermediate

System:
Identifies skill gaps
        ↓
Recommends resources
        ↓
Generates roadmap
        ↓
Explains recommendations
        ↓
Tracks progress
136. TASK-127 — Demo Video

Record a 3–5 minute demonstration.

137. TASK-128 — Solution Documentation

Prepare competition documentation covering:

problem
solution
architecture
AI/ML
workflow
features
innovation
technology
results
challenges
future scope
138. TASK-129 — Source ZIP

Create submission ZIP excluding:

node_modules
venv
.venv
build
dist
cache
.env
139. TASK-130 — GitHub Repository Finalization

Verify:

README
source code
documentation
commit history
setup instructions
environment example
tests
140. TASK-131 — Final Submission Validation

Verify all five competition deliverables:

1. Source Code ZIP
2. GitHub URL
3. Solution Documentation
4. Demo Video URL
5. Deployed Application URL / Local Setup
141. GitHub Issue Dependency Graph

The high-level dependency graph shall be:

TASK-001
   ↓
TASK-002
   ↓
TASK-003
   ↓
TASK-004
   ↓
TASK-005
   ↓
TASK-006
   ↓
TASK-010
   ↓
TASK-011 → TASK-012 → TASK-013 → TASK-014
                         ↓
                    TASK-015
                         ↓
                    TASK-016
                         ↓
                    TASK-017
                         ↓
                    TASK-020
                         ↓
                    TASK-021
                         ↓
                    TASK-022
                         ↓
                    TASK-023
                         ↓
                    TASK-027
                         ↓
                    TASK-031
                         ↓
                    TASK-036
                         ↓
                    TASK-041
                         ↓
                    TASK-043
                         ↓
                    TASK-049
                         ↓
                    TASK-050
                         ↓
                    TASK-051
                         ↓
                    TASK-052
                         ↓
                    TASK-056
                         ↓
                    TASK-057
                         ↓
                    TASK-058
                         ↓
                    TASK-063
                         ↓
                    TASK-066
                         ↓
                    TASK-067
                         ↓
                    TASK-071
                         ↓
                    TASK-074
                         ↓
                    TASK-077
                         ↓
                    TASK-081
                         ↓
                    TASK-085
                         ↓
                    TASK-088
                         ↓
                    TASK-090
                         ↓
                    TASK-102
                         ↓
                    TASK-103
                         ↓
                    TASK-112
                         ↓
                    TASK-116
                         ↓
                    TASK-123
                         ↓
                    TASK-131

This is the primary implementation path.

142. Critical Integration Boundaries

The following boundaries must never be changed casually.

Boundary 1 — Frontend ↔ Backend

Defined by:

DOC-08 API Contract
DOC-10 Frontend Architecture
DOC-11 Backend Architecture

Frontend must consume documented API contracts.

Boundary 2 — Backend ↔ Database

Defined by:

DOC-07 Database Design
DOC-11 Backend Architecture

Database field names must not be invented independently inside services.

Boundary 3 — Backend ↔ AI

Defined by:

DOC-09 ML/AI Architecture
DOC-11 Backend Architecture

AI services must use defined request/response structures.

Boundary 4 — Recommendation ↔ Learning Path

Recommendation output must contain enough structured information for path generation.

Conceptually:

Recommendation
    ↓
resource_id
skill_ids
reason_codes
score
prerequisites
difficulty
estimated_effort
Boundary 5 — Learning Path ↔ Progress

Every trackable path activity must have a stable identifier.

Boundary 6 — Progress ↔ Recommendation

Progress changes must be available to the recommendation engine.

143. Definition of Ready

A GitHub issue is Ready only when:

objective is defined
requirement IDs are identified
dependencies are identified
affected files/modules are known
API impact is known
database impact is known
acceptance criteria are defined
testing requirement is defined
related documentation is identified
144. Definition of Done

A GitHub issue is Done only when:

implementation completed
code follows architecture
validation implemented
tests added where applicable
error handling implemented
API contract updated if required
documentation updated if required
local verification completed
no known regression introduced
Git commit created
GitHub issue linked to commit
145. Commit Convention

Commits shall use:

<type>: <description>

Examples:

feat: implement learner profile API
feat: add recommendation ranking
fix: handle invalid goal input
test: add recommendation engine tests
docs: update API contract
refactor: isolate AI provider adapter
chore: configure backend environment
146. Branch Strategy

For normal implementation:

main

must remain stable.

Feature work should use:

feature/<short-name>

Examples:

feature/authentication
feature/recommendation-engine
feature/learning-path
feature/dashboard

Bug fixes:

fix/<short-name>
147. Pull Request Strategy

Even for a single developer, meaningful features should use pull requests when practical.

Example:

main
  ↑
feature/recommendation-engine

PR checklist:

[ ] Requirements identified
[ ] Tests added
[ ] API unchanged or documented
[ ] Database unchanged or documented
[ ] Security reviewed
[ ] Documentation updated
[ ] Local tests passed
148. AI-Assisted Development Rule

AI coding assistants may be used during implementation.

However, AI-generated code must follow this order:

Read relevant documentation
        ↓
Identify requirement
        ↓
Identify architecture
        ↓
Identify database/API contracts
        ↓
Implement
        ↓
Run tests
        ↓
Verify integration
        ↓
Commit

AI assistants must not independently redesign the architecture unless a documented architecture change is intentionally approved.

149. AI Coding Context Rule

When generating code for any feature, provide the AI assistant with the relevant documents.

For example, for authentication:

DOC-02
DOC-07
DOC-08
DOC-11
DOC-12
DOC-13
DOC-14
DOC-15

For recommendation engine:

DOC-02
DOC-05
DOC-06
DOC-07
DOC-08
DOC-09
DOC-11
DOC-13
DOC-14

For frontend dashboard:

DOC-02
DOC-05
DOC-08
DOC-10
DOC-13
DOC-14
150. Architecture Change Rule

No implementation task may silently change:

database schema
API contract
authentication strategy
AI architecture
frontend state model
backend service boundaries
folder architecture
deployment architecture

If a change becomes necessary:

Identify change
      ↓
Update ADR
      ↓
Update affected documentation
      ↓
Update API/database/architecture
      ↓
Update implementation
      ↓
Update tests

Relevant documents must remain synchronized.

151. Requirement Traceability

Every major GitHub issue shall reference at least one requirement.

Example:

TASK-041
Skill Gap Analysis

Related Requirements:
FR-023
FR-024
AI-005
DATA-003
152. Issue Example

Every major issue should follow this template:

## Objective

Implement skill-gap analysis.

## Related Requirements

FR-023
FR-024
AI-005

## Related Documents

DOC-02
DOC-07
DOC-09
DOC-11

## Dependencies

TASK-039
TASK-040

## Implementation

Compare learner proficiency against required goal skills.

## Acceptance Criteria

- Required skills are retrieved.
- Learner skills are retrieved.
- Skill gaps are calculated.
- Gap severity is calculated.
- Output follows the defined service schema.
- Tests pass.

## Testing

Unit tests:
- missing skill
- partial skill
- sufficient skill
- multiple gaps

## Integration

Output must be consumable by the recommendation engine.

## Definition of Done

Implementation + tests + documentation + integration verification.
153. Recommended GitHub Milestone Order

Create milestones in exactly this order:

M0 — Project Foundation
M1 — Backend Foundation
M2 — Database
M3 — Authentication
M4 — Learner Profile
M5 — Goal Management
M6 — Skill Intelligence
M7 — Resource Engine
M8 — Recommendation Engine
M9 — Learning Path Engine
M10 — AI Assistant
M11 — Frontend
M12 — Progress & Feedback
M13 — Full Integration
M14 — Testing & QA
M15 — Deployment
M16 — Demo & Submission
154. MVP Cut Line

If development time becomes limited, the MVP must prioritize:

Authentication
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
Explanation
        ↓
Dashboard
        ↓
Progress

The following may be reduced if necessary:

advanced adaptation
advanced external integrations
administrator portal
complex ML models
advanced analytics

The core learner journey must remain intact.

155. MVP Demonstration Requirement

The final prototype must demonstrate at least one complete learner journey.

Required flow:

User Login
    ↓
Learner Profile
    ↓
Learning Goal
    ↓
Current Skills
    ↓
AI Goal Understanding
    ↓
Skill Gap Analysis
    ↓
Personalized Recommendations
    ↓
Structured Learning Path
    ↓
Recommendation Explanation
    ↓
Progress Tracking
    ↓
Feedback
156. No-Duplicate-Logic Rule

The same business logic must not be independently implemented in multiple layers.

For example:

Skill-gap calculation must exist in:

Backend skill-gap service

and not separately in:

Frontend
AI prompt
Database trigger

The frontend displays the result.

The backend owns the business logic.

157. API Contract Rule

The frontend shall never assume an API response structure.

The implementation must follow DOC-08.

If an API changes:

DOC-08
      ↓
Backend
      ↓
Frontend
      ↓
Tests
      ↓
DOC-23

must be updated together.

158. Database Contract Rule

If a database field changes:

DOC-07
      ↓
Database model
      ↓
Backend service
      ↓
API schema
      ↓
Frontend
      ↓
Tests

must be reviewed.

159. AI Output Contract Rule

AI output must be treated as untrusted input.

The flow must be:

AI
 ↓
Parse
 ↓
Validate
 ↓
Normalize
 ↓
Business Logic
 ↓
Database

Never:

AI
 ↓
Database

directly.

160. Integration Freeze

Before final integration, the following must be frozen:

Database schema
API contracts
authentication mechanism
frontend routes
core recommendation input/output
learning-path data structure
environment variables

After freeze, changes require an architecture decision.

161. Final GitHub Board

The GitHub project board should contain:

BACKLOG
   ↓
READY
   ↓
IN PROGRESS
   ↓
CODE REVIEW
   ↓
TESTING
   ↓
INTEGRATION
   ↓
DONE

Blocked tasks should be marked:

BLOCKED
162. Recommended Development Sequence

The actual coding sequence should be:

Phase 1
Repository
Environment
Backend skeleton
Frontend skeleton
Database connection
Phase 2
Database models
Authentication
Profile
Goals
Phase 3
Skills
Skill relationships
Skill-gap engine
Resources
Phase 4
Recommendation engine
Ranking
Explainability
Phase 5
Learning-path generation
Prerequisite ordering
Milestones
Next action
Phase 6
AI service
Goal understanding
Skill extraction
Chat
Phase 7
Frontend
Dashboard
Learning path
Recommendations
Chat
Phase 8
Progress
Feedback
Adaptation
Phase 9
Integration
Testing
Bug fixing
Phase 10
Deployment
Demo
Documentation
Submission
163. Implementation Freeze Checklist

Before considering the MVP complete:

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
[ ] DOC-12 approved
[ ] DOC-13 approved
[ ] DOC-14 approved
[ ] DOC-15 approved
[ ] DOC-16 approved
[ ] DOC-17 approved
164. Relationship With Future Documents

DOC-17 is directly connected to:

DOC-18 → Testing and QA
DOC-19 → Deployment
DOC-20 → Integration Checklist
DOC-21 → Master AI Implementation Specification
DOC-22 → Architecture Decision Records
DOC-23 → Requirements Traceability Matrix

These documents must not contradict DOC-17.

165. Implementation Status Tracking

The following statuses shall be used:

NOT STARTED
READY
IN PROGRESS
BLOCKED
IMPLEMENTED
TESTING
INTEGRATED
DONE
166. GitHub Issue Number Mapping

GitHub issue numbers shall be assigned when issues are actually created.

Do not hard-code GitHub issue numbers into architecture documents before GitHub creates them.

The mapping should therefore be:

TASK ID → GitHub Issue ID

Example:

TASK-021 → #17
TASK-041 → #31
TASK-051 → #44

The TASK ID remains permanent even if the GitHub issue number changes.

167. Documentation Synchronization

Whenever implementation changes architecture, update documentation before closing the issue.

Minimum synchronization rules:

Database change
→ DOC-07

API change
→ DOC-08

AI change
→ DOC-09

Frontend change
→ DOC-10

Backend change
→ DOC-11

Security change
→ DOC-12

Folder/code structure change
→ DOC-13

Dependency change
→ DOC-14

Environment change
→ DOC-15

Git workflow change
→ DOC-16

Task change
→ DOC-17

Testing change
→ DOC-18

Deployment change
→ DOC-19

Integration change
→ DOC-20

AI implementation change
→ DOC-21

Architecture decision
→ DOC-22

Requirement mapping
→ DOC-23
168. Final Project Control Model

The entire project shall follow this control loop:

REQUIREMENT
    ↓
ARCHITECTURE
    ↓
DATABASE / API CONTRACT
    ↓
GITHUB ISSUE
    ↓
IMPLEMENTATION
    ↓
UNIT TEST
    ↓
INTEGRATION TEST
    ↓
DOCUMENTATION UPDATE
    ↓
COMMIT
    ↓
GITHUB
    ↓
NEXT TASK

No major feature should bypass this flow.

169. Single Source of Truth

The following hierarchy shall be used:

DOC-01
Project Vision
      ↓
DOC-02
Requirements
      ↓
DOC-05
MVP Scope
      ↓
DOC-06
System Architecture
      ↓
DOC-07
Database
      ↓
DOC-08
API
      ↓
DOC-09
AI/ML
      ↓
DOC-10
Frontend
      ↓
DOC-11
Backend
      ↓
DOC-12
Security
      ↓
DOC-13
Code Structure
      ↓
DOC-14
Dependencies
      ↓
DOC-15
Environment
      ↓
DOC-16
Git Workflow
      ↓
DOC-17
Implementation Tasks
      ↓
DOC-18
Testing
      ↓
DOC-19
Deployment
      ↓
DOC-20
Integration
      ↓
DOC-21
AI Implementation
      ↓
DOC-22
Architecture Decisions
      ↓
DOC-23
Traceability

This hierarchy is the primary mechanism for preventing implementation drift.

170. DOC-17 Completion Criteria

DOC-17 is considered complete when:

implementation milestones are defined
implementation tasks are defined
dependencies are defined
GitHub issue conventions are defined
GitHub labels are defined
priority rules are defined
development sequence is defined
acceptance criteria are defined
Definition of Ready is defined
Definition of Done is defined
AI-assisted development rules are defined
integration boundaries are defined
MVP cut line is defined
competition submission tasks are defined
documentation synchronization rules are defined
single-developer workflow is defined
171. Document Status

Document ID: DOC-17
Document Name: GitHub Task Breakdown & Implementation Plan
Project: PathFinder AI
Version: 1.0
Status: DRAFT
Development Model: Single Developer
Implementation Status: NOT STARTED
Architecture Status: Based on DOC-01 through DOC-16
Next Document: DOC-18 Testing and QA Strategy

172. Change Control

Any change to this document must identify:

Changed task
Reason for change
Affected requirement
Affected architecture
Affected API
Affected database
Affected frontend
Affected backend
Affected AI/ML
Affected tests
Affected deployment
Affected GitHub issues

Changes must be reflected in DOC-20 and DOC-23 where applicable.

173. Final Implementation Principle

PathFinder AI shall be built as one coherent system, not as independent features.

The critical principle is:

DESIGN ONCE
      ↓
DEFINE CONTRACTS
      ↓
IMPLEMENT ONCE
      ↓
TEST
      ↓
INTEGRATE
      ↓
DOCUMENT

No feature is considered complete merely because its individual code works.

A feature is complete only when it works correctly with the rest of PathFinder AI.

End of DOC-17