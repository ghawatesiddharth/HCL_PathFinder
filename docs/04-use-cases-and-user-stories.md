# PathFinder AI — Use Cases and User Stories

**Document ID:** DOC-04  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT  
**Parent Documents:** DOC-01, DOC-02, DOC-03  
**Next Document:** DOC-05 MVP Scope and Acceptance Criteria  

---

# 1. Purpose

This document converts the requirements and user journeys defined in DOC-02 and DOC-03 into detailed use cases and user stories.

The purpose is to establish a direct connection between:

```text
Business Requirement
        ↓
User Journey
        ↓
Use Case
        ↓
User Story
        ↓
Acceptance Criteria
        ↓
GitHub Issue
        ↓
Implementation
        ↓
Test Case

This document will therefore act as one of the primary foundations for project task planning.

2. Use Case Identification

Use cases use the following format:

UC-XXX

User stories use:

US-XXX

Acceptance criteria use:

AC-XXX

Example:

UC-001
US-001
AC-001
3. Primary Actors
Actor ID	Actor	Responsibility
ACT-001	Learner	Uses PathFinder for personalized learning
ACT-002	Administrator	Manages controlled system information
ACT-003	AI/LLM Service	Provides AI-related intelligence
ACT-004	Resource Provider	Provides learning-resource information
4. Use Case Overview

The primary MVP use cases are:

ID	Use Case	Priority
UC-001	Register Account	P0
UC-002	Login	P0
UC-003	Manage Learner Profile	P0
UC-004	Create Learning Goal	P0
UC-005	Manage Current Skills	P0
UC-006	Analyze Skill Gap	P0
UC-007	Generate Recommendations	P0
UC-008	Generate Learning Path	P0
UC-009	View Recommendation Explanation	P0
UC-010	Track Learning Progress	P0
UC-011	View Dashboard	P0
UC-012	Interact With AI Assistant	P0
UC-013	Provide Recommendation Feedback	P1
UC-014	Adapt Recommendations	P1
UC-015	Manage Learning Resources	P2
UC-016	Manage Skills and Prerequisites	P2
5. UC-001 — Register Account
Objective

Allow a new learner to create a PathFinder account.

Primary Actor

ACT-001 — Learner

Preconditions
learner is not authenticated
registration service is available
Main Flow
1. Learner opens application.
2. Learner selects Register.
3. System displays registration form.
4. Learner enters name.
5. Learner enters email.
6. Learner enters password.
7. Learner submits form.
8. System validates input.
9. System checks whether email already exists.
10. System securely hashes password.
11. System creates learner account.
12. System returns successful registration response.
13. Learner is redirected to login or onboarding.
Alternative Flows
A1 — Invalid Email
System detects invalid email.
        ↓
Validation error returned.
        ↓
Learner corrects email.
A2 — Duplicate Email
Email already exists.
        ↓
System rejects registration.
        ↓
System asks learner to log in.
A3 — Weak Password
Password fails security rules.
        ↓
System displays validation message.
Postconditions

A valid learner account exists.

Related Requirements
FR-001
SEC-001
SEC-005
FR-068
6. UC-002 — Login
Objective

Authenticate an existing learner.

Primary Actor

ACT-001 — Learner

Preconditions
learner account exists
Main Flow
1. Learner opens login page.
2. Learner enters email.
3. Learner enters password.
4. Learner submits login form.
5. System validates credentials.
6. System creates authenticated session/token.
7. System returns authentication result.
8. Learner enters dashboard.
Alternative Flows
A1 — Invalid Credentials
Invalid credentials.
        ↓
Authentication rejected.
        ↓
Safe error message displayed.
A2 — Account Not Found
Account does not exist.
        ↓
Login rejected.
Related Requirements
FR-002
FR-003
FR-004
SEC-002
SEC-003
7. UC-003 — Manage Learner Profile
Objective

Allow the learner to create and update their personalized profile.

Primary Actor

ACT-001 — Learner

Main Flow
1. Learner opens profile.
2. System retrieves existing profile.
3. Learner enters or updates profile information.
4. System validates data.
5. System saves profile.
6. System confirms successful update.
Profile Information

The profile may contain:

name
experience level
interests
current skills
learning preferences
weekly learning time
preferred resource type
Related Requirements
FR-005
FR-006
FR-007
FR-008
FR-009
FR-011
8. UC-004 — Create Learning Goal
Objective

Allow the learner to define a learning or career goal.

Primary Actor

ACT-001 — Learner

Supporting Actor

ACT-003 — AI/LLM Service

Main Flow
1. Learner opens goal creation.
2. Learner enters goal in natural language.
3. System sends goal to goal-understanding service.
4. AI service extracts structured information.
5. System validates extracted information.
6. System presents interpreted goal.
7. Learner confirms or edits interpretation.
8. System saves goal.
9. Goal becomes active.
Example

Input:

I want to become a Machine Learning Engineer.

Possible interpretation:

Target Role:
Machine Learning Engineer

Domain:
Machine Learning

Relevant Skills:
Python
Statistics
Machine Learning
Deep Learning
Deployment
Alternative Flow
A1 — AI Service Unavailable
AI service unavailable.
        ↓
System falls back to manual goal entry.
        ↓
Learner can continue.
Related Requirements
FR-012
FR-013
FR-014
FR-015
AI-001
AI-002
AI-009
9. UC-005 — Manage Current Skills
Objective

Allow the learner to specify existing skills and proficiency levels.

Main Flow
1. Learner opens skill section.
2. System displays available skills.
3. Learner selects or enters skills.
4. Learner specifies proficiency.
5. System validates skills.
6. System saves learner skill state.
Example
Python       → Intermediate
SQL          → Basic
Statistics   → Beginner
Machine Learning → Not Known
Related Requirements
FR-008
FR-020
FR-021
10. UC-006 — Analyze Skill Gap
Objective

Determine which skills the learner needs to develop to achieve the selected goal.

Primary Actor

ACT-001 — Learner

Supporting Component

Recommendation / Skill Intelligence Engine

Preconditions
learner profile exists
active goal exists
learner skills are available
Main Flow
1. System retrieves learner profile.
2. System retrieves active goal.
3. System determines required skills.
4. System retrieves learner's current skills.
5. System normalizes skill representations.
6. System compares current and required skills.
7. System calculates skill gaps.
8. System assigns gap priorities.
9. System stores or returns analysis.
10. System displays results.
Example
Required:
Python
NumPy
Pandas
Statistics
Machine Learning

Current:
Python
Basic SQL

Gap:
NumPy
Pandas
Statistics
Machine Learning
Related Requirements
FR-022
FR-023
FR-024
AI-004
AI-005
11. UC-007 — Generate Recommendations
Objective

Generate personalized learning-resource recommendations.

Primary Actor

ACT-001 — Learner

Supporting Components
Recommendation Engine
Skill Engine
Resource Dataset
AI/ML Layer
Preconditions
learner profile exists
active goal exists
skill-gap analysis is available
Main Flow
1. System retrieves learner state.
2. System retrieves active goal.
3. System retrieves skill gaps.
4. System retrieves learning preferences.
5. System generates candidate resources.
6. System filters incompatible resources.
7. System calculates recommendation scores.
8. System ranks candidates.
9. System selects top resources.
10. System generates recommendation reasons.
11. System returns recommendations.
Recommendation Factors
Goal relevance
Skill-gap relevance
Difficulty
Prerequisites
Learner experience
Duration
Learning preference
Learning history
Feedback
Related Requirements
FR-029
FR-030
FR-031
FR-032
FR-033
FR-034
FR-035
AI-003
AI-006
AI-007
12. UC-008 — Generate Learning Path
Objective

Convert recommendations and prerequisites into an ordered personalized roadmap.

Preconditions
goal exists
skill-gap analysis exists
candidate resources exist
Main Flow
1. System retrieves required skills.
2. System retrieves current skills.
3. System retrieves skill gaps.
4. System retrieves resource candidates.
5. System retrieves prerequisite relationships.
6. System builds dependency structure.
7. System determines learning sequence.
8. System assigns resources to stages.
9. System creates milestones.
10. System calculates estimated effort.
11. System identifies next action.
12. System saves learning path.
13. System displays roadmap.
Example Structure
Goal
 ↓
Foundation
 ↓
Core Skills
 ↓
Applied Skills
 ↓
Projects
 ↓
Advanced Topics
 ↓
Assessment
Related Requirements
FR-036
FR-037
FR-038
FR-039
FR-040
FR-041
FR-042
FR-043
FR-044
FR-045
FR-046
13. UC-009 — View Recommendation Explanation
Objective

Explain why a particular recommendation was selected.

Main Flow
1. Learner opens recommendation.
2. Learner selects "Why this?"
3. System retrieves recommendation metadata.
4. System identifies related skill gap.
5. System identifies prerequisite relationship.
6. System generates explanation.
7. System displays explanation.
Example
Why this resource?

You currently have beginner-level
statistics knowledge.

Statistics is required for your
Machine Learning goal.

This resource strengthens the
statistics foundation needed before
the next Machine Learning stage.
Related Requirements
FR-047
FR-048
FR-049
FR-050
AI-007
14. UC-010 — Track Learning Progress
Objective

Allow learners to track progress through the roadmap.

Main Flow
1. Learner opens learning path.
2. System displays activities.
3. Learner starts an activity.
4. System changes status to In Progress.
5. Learner completes activity.
6. Learner marks activity Completed.
7. System updates milestone progress.
8. System updates overall progress.
9. System updates relevant skill state.
10. System determines next action.
Activity States
NOT_STARTED
IN_PROGRESS
COMPLETED
SKIPPED
Related Requirements
FR-051
FR-052
FR-053
FR-054
FR-055
15. UC-011 — View Dashboard
Objective

Provide the learner with a consolidated overview of their learning state.

Dashboard Information

The dashboard should display:

active goal
overall progress
current milestone
skill progress
completed activities
pending activities
recommended next action
recent feedback
learning statistics
Main Flow
1. Learner opens dashboard.
2. System authenticates learner.
3. System retrieves learner state.
4. System calculates current metrics.
5. System retrieves next recommended action.
6. System returns dashboard data.
7. Frontend displays dashboard.
Related Requirements
FR-063
FR-064
FR-065
FR-066
FR-067
16. UC-012 — Interact With AI Assistant
Objective

Allow learners to ask questions about their learning journey.

Primary Actor

ACT-001 — Learner

Supporting Actor

ACT-003 — AI/LLM Service

Example Questions
What should I learn next?
Why was this course recommended?
Can I skip this topic?
Why do I need statistics?
How long will my roadmap take?
Main Flow
1. Learner opens AI assistant.
2. Learner enters question.
3. Frontend sends request to backend.
4. Backend authenticates learner.
5. Backend retrieves relevant learner context.
6. Backend prepares structured AI request.
7. AI service processes request.
8. Backend validates response.
9. Backend returns response.
10. UI displays answer.
Context

The assistant may use:

profile
active goal
current skills
skill gaps
learning path
progress
completed activities
feedback
Related Requirements
FR-016
FR-017
FR-018
FR-019
AI-001
AI-007
17. UC-013 — Provide Recommendation Feedback
Objective

Allow the learner to provide feedback about recommendations.

Feedback Types
USEFUL
NOT_USEFUL
TOO_EASY
TOO_DIFFICULT
ALREADY_KNOWN
NOT_RELEVANT
Main Flow
1. Learner views recommendation.
2. Learner selects feedback.
3. System validates feedback.
4. System stores feedback.
5. System associates feedback with recommendation.
6. System updates learner state.
7. Future recommendation scoring may use feedback.
Related Requirements
FR-056
FR-057
FR-058
DATA-008
18. UC-014 — Adapt Recommendations
Objective

Update recommendations when learner state changes.

Trigger Conditions

Adaptation may occur after:

course completion
skill improvement
negative recommendation feedback
positive recommendation feedback
goal change
preference change
activity skip
Main Flow
Learner State Changes
        ↓
System Updates State
        ↓
Skill Gap Recalculation
        ↓
Candidate Re-generation
        ↓
Recommendation Re-ranking
        ↓
Updated Next Action
Related Requirements
FR-059
FR-060
FR-061
FR-062
AI-008
19. UC-015 — Manage Learning Resources
Actor

ACT-002 — Administrator

Priority

P2

Objective

Allow controlled management of learning resources.

Possible Operations
Create Resource
Update Resource
Delete Resource
View Resource
Search Resource
Assign Skills
Assign Prerequisites
Resource Metadata
Title
Description
URL
Provider
Type
Difficulty
Duration
Skills
Prerequisites
Rating
Language
MVP Note

A full admin UI is not mandatory.

Resource management may initially use:

seed scripts
database imports
controlled backend APIs
20. UC-016 — Manage Skills and Prerequisites
Actor

ACT-002 — Administrator

Priority

P2

Objective

Maintain the structured skill graph used by PathFinder.

Possible Operations
Create Skill
Update Skill
Delete Skill
Create Relationship
Update Relationship
Delete Relationship
Example
Python
  ↓
NumPy
  ↓
Pandas
  ↓
Machine Learning
  ↓
Deep Learning
21. User Story Format

Every user story follows:

As a [user],
I want [capability],
so that [benefit].

Each story should eventually contain:

acceptance criteria
dependencies
priority
implementation owner
test mapping
GitHub issue
22. Authentication User Stories
US-001 — Registration

As a learner,

I want to create an account,

so that I can use PathFinder and save my learning journey.

Acceptance Criteria
AC-001
User can enter required registration information.

AC-002
Invalid input is rejected.

AC-003
Duplicate email is rejected.

AC-004
Password is never stored in plaintext.

AC-005
Successful registration creates a learner account.
US-002 — Login

As a learner,

I want to securely log in,

so that I can access my personalized learning data.

Acceptance Criteria
AC-006
Valid credentials authenticate successfully.

AC-007
Invalid credentials are rejected.

AC-008
Protected resources cannot be accessed without authentication.

AC-009
Authentication state is maintained securely.
23. Profile User Stories
US-003 — Create Profile

As a learner,

I want to create my learner profile,

so that PathFinder can personalize my experience.

Acceptance Criteria
AC-010
Learner can enter experience level.

AC-011
Learner can enter interests.

AC-012
Learner can enter current skills.

AC-013
Learner can specify learning preferences.

AC-014
Profile is saved successfully.
US-004 — Update Profile

As a learner,

I want to update my profile,

so that my recommendations remain relevant as my situation changes.

Acceptance Criteria
AC-015
Learner can edit profile information.

AC-016
Updated information is persisted.

AC-017
Future recommendations can use updated information.
24. Goal User Stories
US-005 — Define Goal

As a learner,

I want to describe my learning goal in natural language,

so that PathFinder can understand what I want to achieve.

Acceptance Criteria
AC-018
Learner can enter a natural-language goal.

AC-019
System accepts valid goal input.

AC-020
System extracts or determines relevant goal information.

AC-021
Learner can review interpreted goal information.

AC-022
Learner can confirm or modify the goal.
US-006 — Manage Goals

As a learner,

I want to edit or change my active goal,

so that my learning path reflects my current objectives.

Acceptance Criteria
AC-023
Learner can edit a goal.

AC-024
Learner can change goal status.

AC-025
Changing the active goal can trigger path regeneration.
25. Skill User Stories
US-007 — Add Current Skills

As a learner,

I want to specify the skills I already know,

so that PathFinder does not recommend unnecessary beginner content.

Acceptance Criteria
AC-026
Learner can select or enter skills.

AC-027
Learner can specify proficiency.

AC-028
Skills are stored against the learner profile.
US-008 — View Skill Gaps

As a learner,

I want to see which skills I am missing,

so that I understand what I need to learn.

Acceptance Criteria
AC-029
System identifies skills required by the goal.

AC-030
System compares required skills with learner skills.

AC-031
System displays missing or weak skills.

AC-032
System assigns meaningful priority to important gaps.
26. Recommendation User Stories
US-009 — Receive Recommendations

As a learner,

I want personalized learning-resource recommendations,

so that I can focus on resources relevant to my goal.

Acceptance Criteria
AC-033
Recommendations consider active goal.

AC-034
Recommendations consider skill gaps.

AC-035
Recommendations consider learner experience.

AC-036
Recommendations consider relevant preferences.

AC-037
Recommendations are ranked.

AC-038
Recommendations contain sufficient metadata.
US-010 — Understand Recommendation

As a learner,

I want to know why a resource was recommended,

so that I can trust and evaluate the recommendation.

Acceptance Criteria
AC-039
Each recommendation can expose an explanation.

AC-040
Explanation identifies relevant skill or gap.

AC-041
Explanation is understandable to the learner.
27. Learning Path User Stories
US-011 — Generate Learning Path

As a learner,

I want an ordered learning roadmap,

so that I know what to learn and in what sequence.

Acceptance Criteria
AC-042
A path is generated for the active goal.

AC-043
Path contains ordered learning stages.

AC-044
Prerequisites are respected.

AC-045
Path contains learning resources.

AC-046
Path contains milestones.

AC-047
Path identifies a next action.
US-012 — Regenerate Path

As a learner,

I want my learning path to update when my situation changes,

so that the roadmap remains personalized.

Acceptance Criteria
AC-048
Path can be regenerated after significant learner-state changes.

AC-049
Completed activities are considered.

AC-050
Updated skill information is considered.

AC-051
Relevant feedback can influence the new path.
28. Progress User Stories
US-013 — Track Activity

As a learner,

I want to mark learning activities as completed,

so that PathFinder can track my progress.

Acceptance Criteria
AC-052
Learner can start an activity.

AC-053
Learner can mark an activity completed.

AC-054
Activity status is persisted.

AC-055
Overall progress is updated.
US-014 — View Progress

As a learner,

I want to see my learning progress,

so that I know how far I have progressed toward my goal.

Acceptance Criteria
AC-056
Overall progress is displayed.

AC-057
Milestone progress is displayed.

AC-058
Relevant skill development is displayed.

AC-059
Next recommended action is displayed.
29. AI Assistant User Stories
US-015 — Ask Learning Question

As a learner,

I want to ask questions using natural language,

so that I can receive guidance about my learning journey.

Acceptance Criteria
AC-060
Learner can enter a natural-language question.

AC-061
System processes the request.

AC-062
Response considers relevant learner context.

AC-063
Response is displayed in conversational form.
US-016 — Ask Why

As a learner,

I want to ask why something was recommended,

so that I can understand the reasoning behind the recommendation.

Acceptance Criteria
AC-064
System identifies the referenced recommendation.

AC-065
System explains the relevant skill gap.

AC-066
System explains the recommendation's relationship to the goal.
30. Feedback User Stories
US-017 — Provide Feedback

As a learner,

I want to provide feedback on recommendations,

so that future recommendations can become more relevant.

Acceptance Criteria
AC-067
Learner can provide supported feedback.

AC-068
Feedback is stored.

AC-069
Feedback is associated with the relevant recommendation.

AC-070
Future recommendation logic can consume feedback.
31. Adaptive Recommendation User Stories
US-018 — Adapt Recommendations

As a learner,

I want recommendations to change as I learn,

so that PathFinder remains useful throughout my journey.

Acceptance Criteria
AC-071
Completed activities can influence future recommendations.

AC-072
Skill updates can influence recommendations.

AC-073
Relevant feedback can influence recommendation ranking.

AC-074
The system can identify a new next action.
32. Dashboard User Stories
US-019 — View Dashboard

As a learner,

I want a dashboard showing my current learning state,

so that I can understand my progress at a glance.

Acceptance Criteria
AC-075
Dashboard displays active goal.

AC-076
Dashboard displays overall progress.

AC-077
Dashboard displays milestone status.

AC-078
Dashboard displays skill progress.

AC-079
Dashboard displays next recommended action.
33. Error Handling User Stories
US-020 — Handle AI Failure

As a learner,

I want the application to remain usable when AI services fail,

so that one external dependency does not break my entire learning experience.

Acceptance Criteria
AC-080
AI failures are detected.

AC-081
User receives a controlled response.

AC-082
Secrets are not exposed.

AC-083
Deterministic functionality continues where possible.
US-021 — Handle Empty Recommendations

As a learner,

I want a useful response when no suitable resource is found,

so that I am not left with a broken or empty screen.

Acceptance Criteria
AC-084
System detects empty recommendation results.

AC-085
System provides a meaningful fallback.

AC-086
Learner can continue using the application.
34. User Story Priority
User Story	Priority
US-001 Registration	P0
US-002 Login	P0
US-003 Profile	P0
US-004 Update Profile	P0
US-005 Define Goal	P0
US-006 Manage Goal	P0
US-007 Current Skills	P0
US-008 Skill Gaps	P0
US-009 Recommendations	P0
US-010 Recommendation Explanation	P0
US-011 Learning Path	P0
US-012 Regenerate Path	P1
US-013 Track Activity	P0
US-014 View Progress	P0
US-015 AI Question	P0
US-016 Recommendation Why	P0
US-017 Feedback	P1
US-018 Adaptive Recommendations	P1
US-019 Dashboard	P0
US-020 AI Failure Handling	P0
US-021 Empty Recommendations	P0
35. End-to-End User Story Chain

The complete MVP can be represented as:

US-001
Registration
    ↓
US-002
Login
    ↓
US-003
Profile
    ↓
US-005
Goal
    ↓
US-007
Current Skills
    ↓
US-008
Skill Gap
    ↓
US-009
Recommendations
    ↓
US-010
Explanation
    ↓
US-011
Learning Path
    ↓
US-013
Progress
    ↓
US-014
Dashboard
    ↓
US-015
AI Assistant
    ↓
US-017
Feedback
    ↓
US-018
Adaptation
36. GitHub Issue Mapping Foundation

Each implementation-ready user story will eventually become one or more GitHub Issues.

Example:

US-001
Registration
      ↓
GitHub Issue
#XX — Implement user registration API
      ↓
Backend Code
      ↓
Database
      ↓
Tests

Another example:

US-009
Recommendations
      ↓
GitHub Issues
      ├── Resource dataset
      ├── Candidate generation
      ├── Recommendation scoring
      ├── Ranking API
      ├── Recommendation UI
      └── Recommendation tests

The complete task breakdown will be defined in DOC-17.

37. Traceability Mapping
User Story	Requirement	Journey
US-001	FR-001	J-001
US-002	FR-002–FR-004	J-002
US-003	FR-005–FR-011	J-003
US-004	FR-006	J-003
US-005	FR-012–FR-013	J-004
US-006	FR-014–FR-015	J-004
US-007	FR-008, FR-020–FR-021	J-005
US-008	FR-022–FR-024	J-006
US-009	FR-029–FR-035	J-007
US-010	FR-047–FR-050	J-009
US-011	FR-040–FR-045	J-008
US-012	FR-046	J-013
US-013	FR-051–FR-052	J-010
US-014	FR-053–FR-055	J-011
US-015	FR-016–FR-018	J-014
US-016	FR-019	J-009
US-017	FR-056–FR-058	J-012
US-018	FR-059–FR-062	J-013
US-019	FR-063–FR-067	J-015
US-020	FR-070	AI Failure
US-021	FR-069	Error Handling
38. MVP Critical Path

The most important implementation chain is:

Authentication
      ↓
Learner Profile
      ↓
Goal
      ↓
Skills
      ↓
Skill Gap
      ↓
Resources
      ↓
Recommendation Engine
      ↓
Learning Path
      ↓
Progress
      ↓
Dashboard
      ↓
AI Assistant

If time becomes limited, this chain takes priority over advanced functionality.

39. Definition of Ready

A user story is ready for implementation only when:

 requirement is identified
 user journey is identified
 expected behavior is clear
 acceptance criteria are defined
 dependencies are known
 affected database entities are known
 affected API capability is known
 affected UI capability is known
 test expectations are understood
 priority is assigned
40. Definition of Done

A user story is considered complete only when:

 implementation is complete
 unit tests are implemented where applicable
 integration tests are implemented where applicable
 API contract is respected
 database changes are complete
 frontend integration is complete
 error handling is implemented
 security requirements are satisfied
 manual verification is complete
 documentation is updated
 GitHub issue is updated
 code is reviewed
 changes are merged according to Git workflow
41. Competition Demo Critical Stories

For the HCLTech Round 2 demonstration, the following stories are mandatory:

US-002
Login

US-003
Profile

US-005
Goal

US-007
Current Skills

US-008
Skill Gap

US-009
Recommendations

US-010
Recommendation Explanation

US-011
Learning Path

US-013
Progress

US-014
Dashboard

US-015
AI Assistant

These stories should form the primary 3–5 minute demo flow.

42. Future Stories

Potential future stories include:

Advanced assessments
Resume-based skill extraction
Job-description-based learning paths
Live course aggregation
Multi-provider search
Peer learning
Learning streaks
Notifications
Calendar integration
Advanced analytics
Collaborative learning
Enterprise learning paths

These are outside the initial MVP unless explicitly promoted later.

43. Document Completion Criteria

DOC-04 is considered complete when:

 all major MVP use cases are defined
 all major actors are mapped
 primary flows are defined
 alternative flows are defined
 MVP user stories are defined
 acceptance criteria are defined
 priorities are assigned
 requirements are mapped
 journeys are mapped
 GitHub issue mapping foundation is established
 Definition of Ready is established
 Definition of Done is established
 critical demo stories are identified
44. Document Status

Status: DRAFT

Parent Documents:

DOC-01 Project Charter
DOC-02 Software Requirements Specification
DOC-03 User Roles and Journeys

Next Document:

DOC-05 MVP Scope and Acceptance Criteria

Implementation Status: NOT STARTED

Architecture Status: NOT FROZEN

45. Change Control

Any change to a user story or use case must identify:

changed use case
changed user story
reason for change
affected requirement
affected journey
affected UI
affected API
affected database
affected AI workflow
affected tests
affected GitHub issue

All approved changes must be reflected in DOC-23 Requirements Traceability Matrix.