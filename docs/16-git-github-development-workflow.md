Document ID: DOC-16
Project: PathFinder AI
Competition: HCLTech Round 2 — PathFinder Prototype
Team: AlgoX
Version: 1.0
Status: DRAFT
Parent Documents: DOC-01 through DOC-15
Next Document: DOC-17 GitHub Task Breakdown
Repository: HCL_PathFinder
Primary Branch: main

1. Purpose

This document defines the Git and GitHub development workflow for the PathFinder AI project.

The purpose is to ensure that all five team members can work simultaneously without overwriting one another's work, introducing integration conflicts, or creating an untraceable codebase.

The workflow defined in this document applies to:

source-code development
frontend development
backend development
AI/ML development
database development
testing
documentation
configuration
deployment preparation
bug fixing
feature development
integration

The workflow is designed specifically for the PathFinder AI prototype and must remain consistent with the architecture, API, database, security, testing, and deployment specifications defined in the other project documents.

2. Repository Information
2.1 Repository Name
HCL_PathFinder
2.2 Repository URL
https://github.com/ghawatesiddharth/HCL_PathFinder
2.3 Default Branch
main

The main branch represents the stable integrated state of the project.

The main branch must always contain code that is intended to remain runnable.

3. Team

The project team consists of five members.

Member	Responsibility
Siddharth Ramnath Ghawate	Team Lead / Full-Stack / Integration
Gayatri Balasaheb Autade	Backend / API
Gawali Shruti Yuvaraj	Frontend / UI
Palak Atul Desai	AI/ML / Recommendation Engine
Vivek Dnyaneshwar Shinde	Database / Testing / DevOps

Responsibilities may be adjusted during implementation, but ownership of every GitHub issue must remain explicit.

4. Development Philosophy

PathFinder AI shall follow these principles:

Never directly develop large features on main.
Every meaningful feature must have a GitHub issue.
Every issue must have an owner.
Every feature must be implemented on a dedicated branch.
Pull Requests must be used to merge feature work.
Code must be reviewed before merging where practical.
A branch must remain synchronized with main.
Commits must be small and meaningful.
Secrets must never be committed.
Documentation must be updated when architecture or behavior changes.
Tests must accompany important business logic.
Integration must occur continuously rather than only at the end.
5. Source-of-Truth Hierarchy

The project uses multiple documents.

To prevent contradictory implementations, the following hierarchy applies.

DOC-01 Project Charter
        ↓
DOC-02 Software Requirements Specification
        ↓
DOC-03 User Roles & Journeys
        ↓
DOC-04 Use Cases & User Stories
        ↓
DOC-05 MVP Scope & Acceptance Criteria
        ↓
DOC-06 System Architecture
        ↓
DOC-07 Database Design
        ↓
DOC-08 API Contract
        ↓
DOC-09 ML/AI Architecture
        ↓
DOC-10 Frontend Architecture
        ↓
DOC-11 Backend Architecture
        ↓
DOC-12 Authentication & Security
        ↓
DOC-13 Code & Folder Architecture
        ↓
DOC-14 Technology Stack & Dependencies
        ↓
DOC-15 Environment & Configuration
        ↓
DOC-16 Git & GitHub Workflow
        ↓
DOC-17 GitHub Task Breakdown
        ↓
Implementation

When implementation conflicts with documentation, the team must not silently modify code.

The discrepancy must first be identified.

The relevant document must either:

be confirmed as still correct, or
be updated through change control.
6. Git Branching Strategy

The project shall use a lightweight feature-branch workflow.

Primary branches:

main

Temporary branches:

feature/*
fix/*
refactor/*
docs/*
test/*
chore/*
7. Main Branch

The main branch represents the stable project version.

Rules:

Do not use main for experimental development.
Do not push unfinished features directly to main.
Do not commit secrets.
Do not commit broken code intentionally.
Every merge must represent a meaningful change.
The repository should remain runnable after every major merge.

Example:

main
8. Feature Branches

Feature branches must be created for new functionality.

Naming convention:

feature/<issue-number>-<short-description>

Examples:

feature/101-user-registration
feature/102-login-api
feature/103-profile-management
feature/104-goal-parser
feature/105-skill-gap-engine
feature/106-learning-path-generator

The GitHub issue number must be included whenever possible.

9. Bug-Fix Branches

Bug fixes must use:

fix/<issue-number>-<short-description>

Examples:

fix/201-login-token-error
fix/202-profile-validation
fix/203-path-generation-order
10. Refactoring Branches

Refactoring work must use:

refactor/<issue-number>-<short-description>

Examples:

refactor/301-api-service-layer
refactor/302-recommendation-module

Refactoring must not silently change product behavior unless explicitly documented.

11. Documentation Branches

Documentation-only changes may use:

docs/<issue-number>-<short-description>

Examples:

docs/401-api-documentation
docs/402-deployment-guide
12. Testing Branches

Testing-specific work may use:

test/<issue-number>-<short-description>

Examples:

test/501-authentication-tests
test/502-recommendation-tests
13. Chore Branches

Repository maintenance may use:

chore/<issue-number>-<short-description>

Examples:

chore/601-dependency-update
chore/602-github-actions
14. Branch Creation

Before creating a branch, the developer must synchronize with main.

Command:

git checkout main
git pull origin main

Then create the branch:

git checkout -b feature/101-user-registration

Or:

git switch main
git pull origin main
git switch -c feature/101-user-registration
15. Branch Ownership

Each active branch must have one primary owner.

Example:

feature/101-user-registration
Owner: Gayatri

Other developers should not modify the same branch unless explicitly coordinated.

16. Avoiding Parallel File Conflicts

Two team members should avoid simultaneously modifying the same files.

For example:

backend/app/api/auth.py

should not be modified independently by two people for unrelated tasks.

If two issues require changes to the same file, the team must coordinate before implementation.

17. GitHub Issues

Every meaningful implementation task must have a GitHub Issue.

An issue should contain:

Issue ID
Title
Description
Objective
Requirements
Dependencies
Owner
Priority
Acceptance Criteria
Affected Components
Testing Requirements
Related Documents
18. Issue Naming Convention

Issue titles should be concise and action-oriented.

Examples:

[Backend] Implement user registration API
[Frontend] Build learner onboarding form
[AI] Implement goal extraction
[Database] Create learner skill schema
[Testing] Add authentication integration tests
[DevOps] Configure production environment
19. GitHub Issue Labels

Recommended labels:

frontend
backend
ai-ml
database
authentication
testing
devops
documentation
integration
bug
enhancement
priority-p0
priority-p1
priority-p2
blocked
review-needed
20. Priority Labels

Priority definitions:

priority-p0

Mandatory for MVP.

priority-p1

Important but not blocking the initial demo.

priority-p2

Enhancement.

21. Issue Status

GitHub Projects should use:

Backlog
Ready
In Progress
Blocked
Review
Testing
Done

Workflow:

Backlog
   ↓
Ready
   ↓
In Progress
   ↓
Review
   ↓
Testing
   ↓
Done

If blocked:

In Progress
      ↓
   Blocked
      ↓
 In Progress
22. Issue Assignment

Every issue must have an owner.

Example:

Issue:
[Backend] Implement registration API

Assignee:
Gayatri

Priority:
P0

Dependencies:
DOC-07
DOC-08
DOC-11
DOC-12

An issue without an owner should not normally move to In Progress.

23. Dependency Tracking

Issues must explicitly identify dependencies.

Example:

Feature:
Learning Path Generator

Depends on:
Skill Gap Engine
Resource Repository
Prerequisite Graph
Recommendation Engine

This prevents developers from implementing features before required components exist.

24. Commit Convention

Commits should use a consistent format.

Recommended format:

<type>: <description>

Examples:

feat: add learner registration API
feat: implement skill gap calculation
fix: handle invalid goal input
docs: update API contract
test: add authentication integration tests
refactor: simplify recommendation service
chore: update backend dependencies
25. Commit Types

Allowed commit types:

feat
fix
docs
test
refactor
chore
style
perf
build
ci
26. Commit Guidelines

A commit should:

contain one logical change
have a meaningful message
avoid unrelated modifications
avoid generated files
avoid secrets
avoid large unnecessary binary files

Bad:

update

Good:

feat: add learner profile creation endpoint
27. Recommended Commit Size

Developers should prefer several logical commits over one massive commit.

Example:

feat: create learner model
feat: add learner repository
feat: add learner service
feat: add learner profile endpoint
test: add learner profile tests
28. Pull Request Workflow

A Pull Request should be opened when a feature is ready for integration.

Workflow:

Create Issue
     ↓
Create Branch
     ↓
Implement
     ↓
Test
     ↓
Push Branch
     ↓
Open Pull Request
     ↓
Review
     ↓
Fix Review Comments
     ↓
Testing
     ↓
Merge
     ↓
Delete Branch
29. Pull Request Naming

PR titles should follow:

[TYPE] Short description

Examples:

[FEATURE] Add learner profile API
[FEATURE] Implement personalized roadmap
[FIX] Resolve JWT refresh issue
[DOCS] Update deployment documentation
30. Pull Request Description

Every PR should contain:

## Summary

Brief description of the change.

## Related Issue

Closes #123

## Changes

- Change 1
- Change 2
- Change 3

## Testing

- Unit tests
- Integration tests
- Manual testing

## Documentation

Updated:
- DOC-08
- DOC-11

## Risks

Any known risks.

## Screenshots

If UI changes are included.
31. Pull Request Review Checklist

Before merge, reviewers should verify:

[ ] Requirement is implemented
[ ] Issue is correctly linked
[ ] Code follows project architecture
[ ] Database changes match DOC-07
[ ] API changes match DOC-08
[ ] Security follows DOC-12
[ ] Folder structure follows DOC-13
[ ] Dependencies follow DOC-14
[ ] Environment variables follow DOC-15
[ ] Tests are included
[ ] No secrets are committed
[ ] No unnecessary files are committed
[ ] Error handling exists
[ ] Documentation is updated where necessary
32. Merge Policy

Preferred merge strategy:

Pull Request
    ↓
Review
    ↓
Tests
    ↓
Squash and Merge

Squash merging keeps the main branch history clean.

However, merge commits may be used when preserving branch history is useful.

33. Pull Before Push

Before pushing major changes:

git fetch origin
git pull origin main

If working on a feature branch:

git fetch origin
git rebase origin/main

or, where the team prefers merge-based synchronization:

git merge origin/main

The team must use one approach consistently within a shared branch.

34. Recommended Team Strategy

For PathFinder AI, the recommended strategy is:

main
  ↓
feature branch
  ↓
implementation
  ↓
PR
  ↓
review
  ↓
squash merge

Feature branches should be short-lived.

35. Conflict Resolution

If a merge conflict occurs:

git status

Identify conflicted files.

Open the affected files and resolve conflicts.

Then:

git add .

If using merge:

git commit

If using rebase:

git rebase --continue

Then push:

git push origin <branch-name>

If the branch was rebased:

git push --force-with-lease

Never use:

git push --force

unless explicitly authorized and understood by the team.

36. Conflict Resolution Rules

When resolving conflicts:

Do not blindly choose "ours".
Do not blindly choose "theirs".
Understand both changes.
Compare against the architecture documents.
Preserve required functionality from both sides where possible.
Run tests after resolution.
Inform the affected developer when necessary.
37. Protected Main Branch

GitHub branch protection should eventually be enabled for main.

Recommended rules:

Require Pull Request
Require status checks
Require branch to be up to date
Restrict direct pushes
Require conversation resolution

For the prototype, at minimum:

No direct feature development on main.
38. Repository Structure

The repository must maintain the structure defined by DOC-13.

Expected high-level structure:

HCL_PathFinder/
│
├── docs/
│
├── frontend/
│
├── backend/
│
├── ml/
│
├── data/
│
├── tests/
│
├── scripts/
│
├── .github/
│
├── .gitignore
├── README.md
└── docker-compose.yml

The exact structure must remain consistent with DOC-13.

39. Documentation Workflow

Documentation is treated as part of the product.

If implementation changes:

API behavior
database schema
AI architecture
authentication
environment variables
folder structure
deployment

the corresponding document must be reviewed.

Examples:

API change
→ DOC-08

Database change
→ DOC-07

AI change
→ DOC-09

Frontend architecture change
→ DOC-10

Backend architecture change
→ DOC-11

Security change
→ DOC-12

Folder change
→ DOC-13

Dependency change
→ DOC-14

Environment change
→ DOC-15
40. Documentation Change Control

When a document changes:

Identify reason
     ↓
Identify affected documents
     ↓
Update documents
     ↓
Update implementation
     ↓
Update tests
     ↓
Update traceability matrix
41. Git Ignore Policy

The repository must contain a .gitignore.

It must exclude:

.env
.env.*
venv/
.venv/
__pycache__/
*.pyc
node_modules/
dist/
build/
coverage/
.pytest_cache/
.vscode/
.idea/
*.log
*.tmp
*.sqlite
*.db

Additional exclusions may be added based on actual tools.

42. Secret Management

The following must never be committed:

API keys
JWT secrets
Database passwords
Cloud credentials
LLM API keys
OAuth secrets
Private keys
Production credentials

Secrets belong in environment configuration.

See:

DOC-12 Authentication & Security
DOC-15 Environment & Configuration
43. .env.example

The repository should contain:

.env.example

This file may contain variable names but must not contain real secrets.

Example:

APP_ENV=development
DATABASE_URL=
JWT_SECRET=
AI_PROVIDER=
AI_API_KEY=
CORS_ORIGINS=

Actual values remain local.

44. Environment Files

Development:

.env

Testing:

.env.test

Production:

.env.production

Production secrets must be supplied by the deployment platform or secure secret-management mechanism.

45. Dependency Management

Dependencies must be explicitly declared.

Backend:

requirements.txt

or the project's selected Python dependency manager.

Frontend:

package.json
package-lock.json

or equivalent.

Dependencies must not be installed manually without recording them.

46. Dependency Change Workflow

When adding a dependency:

Determine why it is required.
Verify compatibility.
Add it to the dependency file.
Install it.
Test the project.
Commit the dependency file.
Update DOC-14 if the technology stack materially changes.
47. Local Development Workflow

Each developer should follow:

git clone <repository>
cd HCL_PathFinder

Then:

git checkout main
git pull origin main

Create a branch:

git checkout -b feature/<issue-number>-<name>

Implement.

Test.

Commit.

Push:

git push -u origin <branch-name>

Create PR.

48. Daily Developer Workflow

Recommended daily sequence:

1. Pull latest main
2. Check assigned GitHub issues
3. Move selected issue to In Progress
4. Create/update feature branch
5. Implement small change
6. Run tests
7. Commit
8. Push
9. Keep branch synchronized
10. Open/update PR
11. Address review
12. Merge
13. Pull latest main
49. Issue Completion Workflow

An issue is considered complete only when:

Code implemented
       +
Tests passed
       +
Review completed
       +
Documentation updated if required
       +
PR merged
       +
Issue acceptance criteria satisfied

Then the issue can be marked:

Done
50. Definition of Ready

A task is ready for development when:

[ ] Requirement is understood
[ ] Acceptance criteria exist
[ ] Dependencies identified
[ ] Owner assigned
[ ] Relevant documents identified
[ ] Required API/data contracts known
[ ] No major unresolved architecture decision blocks implementation
51. Definition of Done

A task is done when:

[ ] Implementation complete
[ ] Unit tests complete
[ ] Integration tests where applicable
[ ] Manual verification complete
[ ] Error handling implemented
[ ] Security reviewed
[ ] Documentation updated
[ ] PR reviewed
[ ] PR merged
[ ] GitHub issue closed
52. Integration-First Development

Because PathFinder AI contains multiple interconnected components, development must not occur as five isolated projects.

The system must be integrated continuously.

Major integration boundaries are:

Frontend
   ↓
Backend API
   ↓
Application Services
   ↓
Database
   ↓
AI/Recommendation Services
   ↓
Learning Resource Data

Each boundary must be validated before moving to the next major feature.

53. Frontend/Backend Coordination

Frontend developers must not invent API structures independently.

The frontend must follow:

DOC-08 API Contract

If an endpoint changes, DOC-08 must be updated before implementation is finalized.

Example:

Frontend expects:

POST /api/v1/goals

Backend must implement the documented request and response structure.

54. Backend/Database Coordination

Backend implementation must follow:

DOC-07 Database Design

Database changes must be coordinated with backend changes.

Example:

If learner_skills changes:

Database schema
    ↓
ORM/model
    ↓
Repository
    ↓
Service
    ↓
API
    ↓
Tests

All affected layers must be considered.

55. Backend/AI Coordination

The backend must communicate with the AI layer through defined service interfaces.

The frontend must not directly call AI providers.

Recommended architecture:

Frontend
   ↓
Backend API
   ↓
AI Service Interface
   ↓
AI Provider

This follows DOC-09, DOC-11 and DOC-12.

56. AI Provider Independence

The application must avoid hard-coding one external AI provider throughout the system.

Recommended abstraction:

AIService
   ↓
GoalUnderstandingService
   ↓
RecommendationExplanationService

The actual provider implementation may be replaced later.

57. Integration Contracts

The following contracts must remain synchronized:

Frontend ↔ API
API ↔ Database
API ↔ AI Service
Recommendation Engine ↔ Resource Data
Path Generator ↔ Skill Graph
Authentication ↔ Protected APIs
Configuration ↔ Deployment
58. API Contract Changes

Any API contract modification must include:

Endpoint
HTTP method
Request schema
Response schema
Authentication
Validation
Error responses
Frontend impact
Tests

Relevant document:

DOC-08
59. Database Changes

Database changes must include:

Table/model change
Column change
Relationship change
Index change
Migration
Backend impact
Testing impact

Relevant document:

DOC-07
60. AI Model Changes

AI changes must include:

Model/provider
Prompt changes
Input schema
Output schema
Fallback behavior
Evaluation impact
Latency impact
Cost impact
Security impact

Relevant document:

DOC-09
61. Frontend Changes

Frontend changes should identify:

Page
Component
State
API dependency
User flow
Validation
Loading state
Error state
Responsive behavior

Relevant document:

DOC-10
62. Backend Changes

Backend changes should identify:

Router
Controller
Service
Repository
Schema
Model
Validation
Authentication
Error handling
Tests

Relevant document:

DOC-11
63. Security Review

Any feature involving:

authentication
authorization
user data
AI APIs
external APIs
file uploads
passwords
tokens
configuration

must be reviewed against:

DOC-12
64. Testing Before Merge

Minimum testing depends on feature type.

Backend
Unit test
API test
Validation test
Error test
Frontend
Component test where appropriate
User-flow test
API integration test
Responsive/manual verification
AI
Input test
Output-schema test
Edge-case test
Fallback test
Database
Schema validation
CRUD test
Relationship test
Migration test
65. Continuous Integration

The project may use GitHub Actions.

Recommended workflow:

Pull Request
      ↓
Install dependencies
      ↓
Lint
      ↓
Run unit tests
      ↓
Run integration tests
      ↓
Build frontend
      ↓
Validate backend
      ↓
PR status
66. Suggested GitHub Actions Checks

Potential checks:

frontend-lint
frontend-test
frontend-build
backend-lint
backend-test
backend-import-check
integration-test

Exact CI implementation will be finalized during DOC-18 and DOC-19 implementation.

67. Build Validation

Before merging a major feature:

Frontend:

npm install
npm run build

Backend:

pip install -r requirements.txt
pytest

Exact commands must match DOC-14 and DOC-15.

68. Local Integration Check

Developers should verify:

Frontend starts
Backend starts
Database connects
Authentication works
API responds
AI service responds or falls back
Dashboard loads
69. Pull Request Integration Order

Major PathFinder features should preferably be merged in dependency order.

Recommended order:

1. Project setup
2. Database
3. Authentication
4. Learner profile
5. Goal management
6. Skill management
7. Resource data
8. Skill-gap engine
9. Recommendation engine
10. Learning path generator
11. Progress tracking
12. Feedback
13. AI assistant
14. Dashboard
15. Deployment
70. Why Dependency Order Matters

For example, the learning-path generator depends on:

Goal
+
Learner Profile
+
Skills
+
Skill Gaps
+
Resources
+
Prerequisites

Therefore it should not be developed as an isolated UI feature before these contracts exist.

71. GitHub Project Board

A GitHub Project should be created for:

PathFinder AI Development

Suggested fields:

Status
Priority
Assignee
Component
Sprint
Requirement ID
Document ID
72. Component Values

Recommended component values:

Frontend
Backend
AI/ML
Database
Authentication
Testing
DevOps
Documentation
Integration
73. Requirement ID Tracking

Every major GitHub issue should reference one or more requirements.

Example:

Requirement:
FR-001
FR-002
FR-003

Issue:

[Backend] Implement authentication APIs

This creates traceability from:

Requirement
    ↓
GitHub Issue
    ↓
Code
    ↓
Test
74. Document ID Tracking

Issues should also identify affected documents.

Example:

Documents:
DOC-07
DOC-08
DOC-11
DOC-12

This prevents implementation from drifting away from the architecture.

75. Example GitHub Issue
# [Backend] Implement Learner Profile API

## Requirement IDs

FR-005
FR-006
FR-007
FR-008
FR-009
FR-010
FR-011

## Documents

DOC-07
DOC-08
DOC-11
DOC-12

## Objective

Implement backend APIs required to create and update learner profiles.

## Scope

- Create learner profile endpoint
- Update learner profile endpoint
- Validation
- Authentication
- Database persistence

## Acceptance Criteria

- Authenticated learner can create profile
- Authenticated learner can update profile
- Invalid input returns validation error
- User cannot modify another learner's profile
- Data is persisted correctly
- Tests pass

## Priority

P0

## Assignee

Gayatri
76. Example Branch
git checkout main
git pull origin main

git checkout -b feature/103-learner-profile-api
77. Example Commit Sequence
git add .
git commit -m "feat: add learner profile model"

git add .
git commit -m "feat: add learner profile service"

git add .
git commit -m "feat: add learner profile API"

git add .
git commit -m "test: add learner profile API tests"
78. Example Push
git push -u origin feature/103-learner-profile-api
79. Example PR
feature/103-learner-profile-api
              ↓
            main

PR title:

[FEATURE] Implement learner profile API
80. Release Tags

Major prototype milestones may be tagged.

Example:

v0.1.0

Initial backend.

v0.2.0

Authentication and profile.

v0.3.0

Recommendation engine.

v0.4.0

Learning path.

v1.0.0

Competition-ready prototype.

81. Milestone Definitions

Suggested milestones:

M0 — Documentation Complete

All core architecture documents approved.

M1 — Foundation

Repository, environment, database and basic application setup.

M2 — Authentication

Registration and login.

M3 — Learner Profiling

Profile, skills and preferences.

M4 — Goal Intelligence

Goal parsing and skill requirements.

M5 — Recommendation Engine

Candidate generation and ranking.

M6 — Learning Path

Prerequisite-aware roadmap.

M7 — AI Assistant

Conversational interface and explanations.

M8 — Dashboard

Progress and next actions.

M9 — Integration

End-to-end workflow.

M10 — Deployment

Competition-ready build.

82. Release Candidate

Before competition submission, create a release candidate.

Example:

v1.0.0-rc1

Perform:

Full test
Full integration
Security check
Deployment check
Demo check
Documentation check
83. Competition Freeze

Before final submission:

CODE FREEZE

After code freeze:

only critical fixes
no unnecessary features
no major architecture changes
no experimental dependencies
84. Final Submission Branch

If required, create:

release/hcl-round2

This branch can represent the final submission state.

However, main must remain the canonical source of truth.

85. Source Code ZIP Preparation

Before creating the final ZIP:

Include:

source code
configuration examples
README
documentation
scripts
tests

Exclude:

.git
node_modules
venv
.venv
.env
build artifacts
cache
large generated files
private credentials
86. README Requirements

The final README must contain:

Project Overview
Features
Architecture
Repository Structure
Prerequisites
Installation
Environment Variables
Database Setup
Backend Setup
Frontend Setup
AI Setup
Running Tests
Running Application
API Documentation
Deployment
Demo Instructions
Team
87. Commit History Requirement

Because the competition explicitly requires a GitHub repository, commit history should demonstrate genuine development.

The team should avoid creating one final giant commit.

Preferred history:

docs: initialize project specification
docs: add project charter
docs: add software requirements specification
docs: add system architecture
feat: initialize backend
feat: initialize frontend
feat: add authentication
feat: add learner profile
feat: implement skill gap analysis
feat: implement recommendations
feat: generate learning path
test: add integration tests
fix: resolve recommendation ordering
docs: update deployment instructions
88. No Fake Commit History

Do not fabricate development history merely for appearance.

The commit history should represent actual work.

89. Code Review Responsibility

Reviewers should focus on:

Correctness
Architecture
Security
Maintainability
Testing
Performance
Integration

Reviewers should not focus only on formatting.

90. Review Severity

Review comments may be classified as:

BLOCKER
MAJOR
MINOR
SUGGESTION
QUESTION

A BLOCKER must be resolved before merge.

91. Blocked Issues

If an issue cannot continue:

Add:

blocked

and document:

Blocked by:
#123

Example:

Learning path generator

Blocked by:
#114 Skill Graph
#118 Resource Dataset
92. Avoiding Duplicate Work

Before starting a feature:

Search GitHub Issues
Search GitHub PRs
Search current branches

If a similar issue exists, reuse or extend it.

93. Communication Rules

When a developer changes a shared contract, they must notify the relevant owners.

Examples:

API change:

Backend → Frontend

Database change:

Database → Backend

AI output change:

AI → Backend → Frontend

Authentication change:

Security → Backend → Frontend
94. Shared Contract Principle

The following should never be changed independently:

API request/response schemas
Database models
AI structured outputs
Authentication token format
Environment variable names

Any change requires coordination.

95. Breaking Changes

A breaking change is any change that can cause an existing component to stop working.

Examples:

Rename API field
Remove API endpoint
Change response format
Rename database column
Change authentication mechanism
Change AI output schema

Breaking changes require:

Issue
Documentation update
Affected-component identification
Migration plan
Tests
96. Backward Compatibility

Where practical, APIs should remain backward-compatible during development.

If compatibility is impossible, coordinate the change across all affected branches.

97. Database Migration Rule

Never manually modify the shared database schema without documenting the change.

Use the project's migration mechanism.

Every schema change must be reproducible.

98. AI Prompt Versioning

Important AI prompts should be version-controlled.

Example:

prompts/
├── goal_extraction_v1.txt
├── recommendation_explanation_v1.txt
└── learning_path_v1.txt

Prompt changes should be reviewed like code.

99. AI Output Schema Stability

AI output consumed by backend code must have a defined structure.

Example:

{
  "goal": "Machine Learning Engineer",
  "skills": [
    "Python",
    "NumPy",
    "Pandas",
    "Machine Learning"
  ]
}

Changes to this schema must be coordinated with backend parsing logic.

100. Test Data

Test data must not contain real sensitive learner information.

Use synthetic data.

Example:

learner_test_001
learner_test_002
101. Production Data

Production data must never be copied into development environments unless explicitly authorized and appropriately sanitized.

102. Logging

Logs must not contain:

Passwords
JWT secrets
API keys
Database passwords
Sensitive personal data
103. Error Reproduction

Bug issues should contain:

Environment
Steps to reproduce
Expected behavior
Actual behavior
Error message
Screenshots/logs
Relevant branch
Relevant commit
104. Bug Issue Example
# [BUG] Learning path contains prerequisite violation

## Environment

Development

## Steps

1. Create learner
2. Select Machine Learning goal
3. Set Python proficiency to 0
4. Generate path

## Expected

Python appears before Machine Learning.

## Actual

Machine Learning appears first.

## Requirement

FR-038
FR-039

## Documents

DOC-07
DOC-09
DOC-11
105. Emergency Fix

Critical competition bugs may use:

fix/<issue-number>-critical-description

The fix must still be tested before merging.

106. Integration Branch

For large integration periods, the team may temporarily use:

integration

However, this branch is optional.

Recommended default:

feature → PR → main

If an integration branch is introduced, its purpose and lifetime must be documented.

107. Recommended Team Parallelization

The team can work approximately as follows:

Siddharth
Integration / Full Stack

Gayatri
Backend / API

Shruti
Frontend

Palak
AI/ML

Vivek
Database / Testing / DevOps
108. Dependency-Aware Parallel Work

The team does not need to wait for the entire backend before starting frontend development.

Instead, frontend can use documented API contracts and mocked responses.

Example:

DOC-08 API Contract
        ↓
Frontend Mock API
        ↓
Frontend Development

Meanwhile:

Backend API
        ↓
Actual Implementation

Later:

Frontend
   ↓
Actual Backend API
109. Mocking Strategy

Mock responses may be used during parallel development.

However:

Mock contract
=
DOC-08 contract

The frontend must not create arbitrary response structures.

110. Integration Validation

When frontend and backend are connected:

Mock API
   ↓
Real API
   ↓
Integration Test

All discrepancies must be resolved.

111. Feature Flags

Experimental functionality may use feature flags where appropriate.

Example:

ENABLE_ADAPTIVE_RECOMMENDATIONS=false

This allows incomplete functionality to remain isolated.

112. Experimental AI Features

Experimental AI functionality should not replace deterministic MVP functionality until validated.

Example:

AI recommendation
        ↓
Candidate generation
        ↓
Deterministic validation
        ↓
Ranking

This protects system reliability.

113. Documentation Branch Synchronization

Documentation changes should be merged before dependent implementation when possible.

Example:

DOC-08 API Contract
       ↓
Backend implementation
       ↓
Frontend integration
114. Architecture Freeze

Before full implementation, the following must be reviewed:

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

Once approved, architecture should be considered:

FROZEN FOR MVP

Only controlled changes should be made afterward.

115. Architecture Change Workflow

If architecture must change:

Identify problem
      ↓
Create GitHub issue
      ↓
Review affected documents
      ↓
Create/update ADR
      ↓
Update architecture documents
      ↓
Update implementation plan
      ↓
Implement
      ↓
Test
116. Architecture Decision Records

Major decisions should be recorded in:

DOC-22 Architecture Decision Records

Examples:

ADR-001 Database selection
ADR-002 Authentication strategy
ADR-003 AI provider selection
ADR-004 Recommendation strategy
ADR-005 Deployment platform
117. Traceability

Every implementation feature should eventually be traceable:

Requirement
   ↓
Use Case
   ↓
GitHub Issue
   ↓
Branch
   ↓
Commit
   ↓
Pull Request
   ↓
Code
   ↓
Test

This is one of the most important project-control mechanisms.

118. Requirements Traceability Matrix

DOC-23 will maintain the final mapping.

Example:

FR-029
   ↓
UC-REC-001
   ↓
Issue #120
   ↓
feature/120-recommendation-engine
   ↓
PR #135
   ↓
RecommendationService
   ↓
test_recommendation_service.py
119. GitHub Issue to Code Mapping

Whenever possible, commit messages should reference issue numbers.

Example:

feat: implement recommendation ranking (#120)
120. GitHub Issue Closure

Use:

Closes #120

in the PR description.

GitHub will automatically close the issue after merge.

121. Branch Cleanup

After successful merge:

Delete feature branch

Example:

feature/120-recommendation-engine

should be deleted after merging.

122. Local Branch Cleanup

After merge:

git checkout main
git pull origin main
git branch -d feature/120-recommendation-engine
123. Remote Branch Cleanup

GitHub PR merge usually provides an option to delete the remote branch.

Use it.

124. Repository Health

Periodically check:

git status
git branch
git remote -v
git log --oneline --decorate --graph --all
125. Useful Git Commands

Clone:

git clone https://github.com/ghawatesiddharth/HCL_PathFinder.git

Check status:

git status

Pull:

git pull origin main

Create branch:

git checkout -b feature/123-example

Stage:

git add .

Commit:

git commit -m "feat: add example feature"

Push:

git push -u origin feature/123-example

View history:

git log --oneline --decorate --graph --all
126. Safe Git Commands

Before destructive operations:

git status

Developers should understand the effect of commands such as:

git reset
git rebase
git clean
git push --force
127. Force Push Policy

Never use:

git push --force

on:

main

For a personal feature branch after a rebase:

git push --force-with-lease

may be used.

128. Backup Before Risky Operations

Before complicated rebases or conflict resolution:

git branch backup/<name>

Example:

git branch backup/before-rebase
129. Recovery Strategy

If a local branch is accidentally corrupted:

git reflog

can be used to identify previous commits.

Do not panic and immediately delete the repository.

130. Code Ownership

For critical modules, ownership should be documented.

Example:

Module	Primary Owner
Authentication	Gayatri
Learner Profile	Gayatri
Frontend	Shruti
AI/ML	Palak
Database	Vivek
Integration	Siddharth

Ownership does not prevent others from contributing.

131. Two-Person Review for Critical Changes

Where possible, critical changes should receive review from another team member.

Especially:

Authentication
Database migrations
AI recommendation logic
Production configuration
Deployment
132. Security-Critical PRs

Security-related PRs should explicitly state:

Security impact:
None / Low / Medium / High

If applicable, reference:

DOC-12
133. Database-Critical PRs

Database PRs should explicitly state:

Schema changed:
Yes / No

Migration required:
Yes / No
134. API-Critical PRs

API PRs should explicitly state:

API changed:
Yes / No

DOC-08 updated:
Yes / No
135. AI-Critical PRs

AI PRs should state:

AI behavior changed:
Yes / No

Model/provider changed:
Yes / No

Prompt changed:
Yes / No

Output schema changed:
Yes / No
136. Frontend PRs

Frontend PRs should include screenshots for significant UI changes.

Checklist:

Desktop
Mobile
Loading
Error
Empty state
Success state

where applicable.

137. Integration PR

An integration PR should verify:

Frontend
+
Backend
+
Database
+
AI

The complete user journey should be tested.

138. End-to-End GitHub Milestone

The most important milestone is:

E2E-001 Personalized Learning Journey

The implementation should support:

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
Explanation
   ↓
Progress
   ↓
Feedback
   ↓
Updated Recommendation

This maps directly to DOC-02.

139. Demo Readiness

Before demo recording, create a dedicated issue:

[Release] Prepare HCL Round 2 Demo

Checklist:

[ ] Clean database
[ ] Seed demo data
[ ] Test login
[ ] Test onboarding
[ ] Test goal creation
[ ] Test skill analysis
[ ] Test recommendation
[ ] Test learning path
[ ] Test AI assistant
[ ] Test progress
[ ] Test feedback
[ ] Test deployment
140. Final Git Status Check

Before submission:

git status

Expected:

nothing to commit, working tree clean
141. Final Branch Check

Verify:

git branch

Expected primary branch:

* main
142. Final Remote Check
git remote -v

Expected repository:

https://github.com/ghawatesiddharth/HCL_PathFinder.git
143. Final History Check
git log --oneline --decorate --graph --all -20

The history should demonstrate meaningful development.

144. Final Repository Validation

Before competition submission:

[ ] Repository accessible
[ ] Main branch updated
[ ] No secrets
[ ] README complete
[ ] Documentation complete
[ ] Source code complete
[ ] Tests present
[ ] Environment example present
[ ] Setup instructions verified
[ ] Git history meaningful
[ ] Deployment instructions verified
145. Submission Integrity

The GitHub repository must contain the same logical version of the application submitted as the source-code ZIP.

The ZIP and GitHub repository must not contain substantially different implementations.

146. Final Submission Tag

Recommended:

git tag -a v1.0.0 -m "HCLTech Round 2 submission"
git push origin v1.0.0

This creates a permanent reference to the submission version.

147. Versioning Strategy

Use semantic-style versioning:

MAJOR.MINOR.PATCH

Example:

1.0.0

Major:

Breaking architecture/product change

Minor:

New feature

Patch:

Bug fix
148. Development Phases

GitHub work should follow these phases.

PHASE 0
Documentation & Architecture

PHASE 1
Repository & Environment Setup

PHASE 2
Database & Backend Foundation

PHASE 3
Authentication

PHASE 4
Learner Profiling

PHASE 5
Goal & Skill Intelligence

PHASE 6
Recommendation Engine

PHASE 7
Learning Path Generator

PHASE 8
AI Assistant

PHASE 9
Frontend Dashboard

PHASE 10
Integration & Testing

PHASE 11
Deployment

PHASE 12
Competition Submission
149. Phase Transition Rules

A phase should not be considered complete merely because code exists.

Each phase requires:

Implementation
+
Testing
+
Integration
+
Documentation
+
Acceptance criteria
150. Documentation-to-Code Rule

No major feature should be implemented solely from a conversational instruction.

The implementation must reference:

Requirement
Architecture
API
Database
Security
Folder Structure
Environment

This is particularly important because multiple team members and AI coding assistants may work on the repository.

151. AI Coding Assistant Workflow

The repository may be provided to AI coding assistants such as ChatGPT or Claude.

Before asking an AI assistant to generate code, provide or reference:

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
DOC-14
DOC-15

The assistant must be instructed not to invent architecture outside these documents.

152. AI-Assisted Coding Rules

AI-generated code must:

Follow existing folder structure
Follow API contract
Follow database schema
Follow security rules
Follow dependency versions
Follow environment configuration
Include tests
Avoid introducing unnecessary libraries

AI-generated code must be reviewed by a human team member.

153. AI Code Verification

Before accepting AI-generated code:

Read
   ↓
Understand
   ↓
Run
   ↓
Test
   ↓
Review
   ↓
Integrate

Never merge AI-generated code blindly.

154. Preventing AI-Generated Architecture Drift

If an AI coding assistant proposes:

different database
different authentication
different API structure
different frontend framework
different AI architecture

the developer must stop and compare the proposal with the project documents.

The assistant must follow the approved architecture unless the architecture is intentionally changed through the documented change process.

155. Multi-Chat AI Development

If different team members use different AI assistants, every assistant must receive the same repository documentation.

The repository documents are the shared source of truth.

This prevents:

ChatGPT implementation
      ≠
Claude implementation
      ≠
Team implementation
156. Recommended AI Prompt Context

When generating implementation code, developers should tell the AI assistant:

You are implementing PathFinder AI.

Follow the repository documentation as the source of truth.

Do not change architecture without approval.

Relevant documents:
DOC-07
DOC-08
DOC-09
DOC-11
DOC-12
DOC-13
DOC-14
DOC-15

Implement only the requested GitHub issue.

Do not introduce unnecessary dependencies.

Preserve existing interfaces.

Include tests.

Explain any required changes to existing files.
157. No Unrequested Rewrites

A developer or AI assistant must not rewrite the entire project when implementing one issue unless explicitly required.

Example:

Issue:

Add learner profile endpoint

should not result in:

Rewrite authentication
Rewrite database
Rewrite frontend
Change framework
158. Minimal Change Principle

Implementation should modify the smallest reasonable set of files.

This reduces:

merge conflicts
regressions
integration problems
review complexity
159. Shared Repository Rule

All team members must work from the same GitHub repository.

Do not create separate permanent project copies.

Temporary local clones are acceptable.

160. Final Workflow Summary

The complete workflow is:

READ DOCUMENTS
      ↓
SELECT REQUIREMENT
      ↓
CREATE GITHUB ISSUE
      ↓
ASSIGN OWNER
      ↓
IDENTIFY DEPENDENCIES
      ↓
CREATE BRANCH
      ↓
IMPLEMENT
      ↓
WRITE TESTS
      ↓
RUN LOCAL VALIDATION
      ↓
UPDATE DOCUMENTATION
      ↓
COMMIT
      ↓
PUSH
      ↓
OPEN PR
      ↓
CODE REVIEW
      ↓
INTEGRATION TEST
      ↓
MERGE TO MAIN
      ↓
CLOSE ISSUE
      ↓
UPDATE TRACEABILITY
161. Master Development Rule

The most important rule for the PathFinder AI project is:

NO REQUIREMENT
        ↓
NO ISSUE

NO ISSUE
        ↓
NO FEATURE BRANCH

NO FEATURE BRANCH
        ↓
NO FEATURE IMPLEMENTATION

NO TEST
        ↓
NO MERGE

NO DOCUMENTATION UPDATE
        ↓
NO ARCHITECTURE CHANGE

NO TRACEABILITY
        ↓
NO FINAL ACCEPTANCE
162. GitHub Development Governance

The team shall use GitHub as the central coordination platform for:

Source Code
Issues
Pull Requests
Project Board
Milestones
Releases
Documentation
Code Review
Task Assignment
Development History
163. Final Acceptance Criteria

DOC-16 is considered complete when:

[ ] Branch strategy defined
[ ] Naming conventions defined
[ ] Commit conventions defined
[ ] Issue workflow defined
[ ] PR workflow defined
[ ] Review process defined
[ ] Merge policy defined
[ ] Conflict resolution defined
[ ] Secret management defined
[ ] Documentation workflow defined
[ ] Integration workflow defined
[ ] AI-assisted coding workflow defined
[ ] Release workflow defined
[ ] Competition submission workflow defined
[ ] Requirement traceability defined
[ ] Team ownership defined
164. Document Dependencies

This document depends on:

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
DOC-11 Backend Architecture
DOC-12 Authentication and Security
DOC-13 Code and Folder Architecture
DOC-14 Technology Stack and Dependencies
DOC-15 Environment and Configuration
165. Documents Affected by DOC-16

This document directly supports:

DOC-17 GitHub Task Breakdown
DOC-18 Testing and QA Strategy
DOC-19 Deployment Architecture
DOC-20 Integration Checklist
DOC-21 Master AI Implementation Specification
DOC-22 Architecture Decision Records
DOC-23 Requirements Traceability Matrix
166. Change Control

Any modification to the Git/GitHub workflow must identify:

Change
Reason
Affected branches
Affected team members
Affected documents
Affected issues
Affected CI/CD workflow
Migration requirements
167. Document Status
Document ID: DOC-16
Document Name: Git & GitHub Development Workflow
Version: 1.0
Status: DRAFT
Implementation Status: NOT STARTED
Architecture Status: CONSISTENT WITH CURRENT MVP ARCHITECTURE
168. Approval

The document should be reviewed by all five team members before implementation begins.

Approval:

Siddharth: __________________

Gayatri: ____________________

Shruti: _____________________

Palak: ______________________

Vivek: ______________________
69. Next Document

After DOC-16 is committed, the next document is:

DOC-17 — GitHub Task Breakdown

DOC-17 will convert the requirements and architecture into actual GitHub Issues, with:

Issue ID
Title
Description
Owner
Priority
Dependencies
Requirement IDs
Document IDs
Acceptance Criteria
Branch Name
Implementation Order
Testing Requirements

That is where we will turn this documentation set into the actual 5-member development plan, so don't create random GitHub issues yet. We should make DOC-17 first and then create the issues from it.