# PathFinder AI — User Roles and Journeys

**Document ID:** DOC-03  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT  
**Parent Documents:** DOC-01 Project Charter, DOC-02 Software Requirements Specification  
**Next Document:** DOC-04 Use Cases and User Stories  

---

# 1. Purpose

This document defines the users, actors, responsibilities, permissions, goals, workflows, and end-to-end user journeys of PathFinder AI.

The purpose of this document is to translate the requirements defined in DOC-02 into concrete user experiences.

This document establishes how different users interact with the system.

The workflows defined here will later be used to derive:

- use cases
- user stories
- UI screens
- API requirements
- database entities
- AI workflows
- test scenarios
- GitHub development tasks

---

# 2. User Model

PathFinder AI is primarily designed for learners.

The MVP contains three major actor categories:

```text
                    PathFinder AI
                         |
        +----------------+----------------+
        |                |                |
     Learner        Administrator     External Services
        |                |                |
        |                |         +------+------+
        |                |         |             |
       User          System AI/LLM Provider   Resource Provider

The learner is the primary actor.

Administrator functionality is intentionally limited in the MVP.

External services are supporting actors and must not control the application's core business logic.

3. Actor Definitions
ACT-001 — Learner
Description

The learner is the primary user of PathFinder AI.

The learner uses the system to identify a learning goal, understand skill gaps, receive recommendations, follow a personalized roadmap, and track progress.

Primary objectives

The learner wants to:

understand what they need to learn
identify missing skills
receive relevant resources
follow a structured learning sequence
understand why resources were recommended
track learning progress
adapt the learning path over time
Main capabilities

The learner can:

register
log in
create a profile
update profile information
define learning goals
describe goals using natural language
specify existing skills
specify interests
specify learning preferences
generate a learning path
view recommendations
interact with the AI assistant
view skill gaps
track progress
mark resources complete
provide feedback
receive updated recommendations
4. ACT-002 — System Administrator
Description

The administrator manages selected system-level information.

The administrator is not the primary MVP user.

Administrator capabilities will initially be restricted to controlled internal operations.

Possible responsibilities

The administrator may manage:

learning resources
skill definitions
skill relationships
prerequisite relationships
resource metadata
system configuration
dataset information
MVP limitation

The MVP should not require a complex administration portal unless development time permits.

Administrative operations may initially be performed through:

database seed scripts
controlled APIs
backend administration utilities
development tools

A full administrator dashboard is considered a future enhancement.

5. ACT-003 — AI/LLM Service
Description

An external AI/LLM service may provide intelligence-related functionality.

Potential responsibilities include:

natural-language understanding
goal interpretation
skill extraction
conversational responses
recommendation explanations
structured information extraction
Important architectural rule

The external AI service must not become the sole source of truth.

Deterministic application logic such as:

authentication
authorization
progress calculation
prerequisite validation
database operations
access control

must remain under application control.

6. ACT-004 — Learning Resource Provider
Description

A learning-resource provider supplies information about learning resources.

Resources may include:

courses
videos
articles
documentation
projects
assessments

The MVP may use a controlled internal dataset instead of live external integrations.

This approach improves:

reliability
reproducibility
demo stability
testing
development speed

External integrations can be added later.

7. Learner Persona
Persona P-001 — Career-Oriented Learner
Example

A student wants to become a Machine Learning Engineer.

Current situation

The learner may know:

basic Python
basic programming
introductory mathematics

but may not know:

NumPy
Pandas
statistics
machine learning
model evaluation
deployment
Goal

The learner wants a structured roadmap instead of randomly selecting courses.

Pain points
too many available resources
uncertainty about what to learn first
unclear prerequisites
difficulty identifying skill gaps
lack of personalized recommendations
uncertainty about whether they are progressing correctly
PathFinder solution

PathFinder:

Learner Goal
     ↓
Profile Analysis
     ↓
Skill Extraction
     ↓
Required Skill Identification
     ↓
Skill Gap Analysis
     ↓
Resource Recommendation
     ↓
Prerequisite Ordering
     ↓
Personalized Learning Path
     ↓
Progress Tracking
     ↓
Adaptive Recommendations
8. Core Learner Journey

The primary PathFinder journey is:

Register
   ↓
Login
   ↓
Onboarding
   ↓
Create Profile
   ↓
Define Learning Goal
   ↓
Enter Current Skills
   ↓
Enter Learning Preferences
   ↓
Analyze Learner State
   ↓
Identify Required Skills
   ↓
Calculate Skill Gaps
   ↓
Generate Candidate Resources
   ↓
Rank Resources
   ↓
Generate Learning Path
   ↓
Explain Recommendations
   ↓
Start Learning
   ↓
Track Progress
   ↓
Provide Feedback
   ↓
Update Learner State
   ↓
Re-rank Recommendations
   ↓
Continue Learning

This is the primary end-to-end workflow that must be demonstrated in the MVP.

9. Journey J-001 — Registration
Objective

Allow a new learner to create an account.

Starting condition

The learner does not have an authenticated account.

Flow
Open Application
      ↓
Select Register
      ↓
Enter Name
      ↓
Enter Email
      ↓
Enter Password
      ↓
Submit Registration
      ↓
Validate Input
      ↓
Create Account
      ↓
Redirect to Login / Dashboard
Success condition

A learner account is created successfully.

Failure conditions

Possible failures include:

invalid email
weak password
duplicate email
missing required field
database failure
Related requirements
FR-001
SEC-001
SEC-005
FR-068
10. Journey J-002 — Login
Objective

Allow an existing learner to securely access the application.

Flow
Open Application
      ↓
Enter Email
      ↓
Enter Password
      ↓
Submit
      ↓
Validate Credentials
      ↓
Create Authenticated Session
      ↓
Open Dashboard
Failure conditions
invalid credentials
missing credentials
account unavailable
authentication service failure
Related requirements
FR-002
FR-003
SEC-002
SEC-006
11. Journey J-003 — Learner Onboarding
Objective

Collect enough learner information to generate useful personalization.

Flow
New Learner
     ↓
Welcome Screen
     ↓
Enter Experience Level
     ↓
Select / Enter Current Skills
     ↓
Enter Interests
     ↓
Enter Weekly Learning Time
     ↓
Select Learning Preferences
     ↓
Define Career / Learning Goal
     ↓
Save Profile
Required MVP information

The onboarding process should capture:

experience level
current skills
interests
primary goal
preferred learning format
approximate weekly learning time
Design principle

The onboarding process should not become unnecessarily long.

The system should prioritize information that materially improves recommendations.

Related requirements
FR-005
FR-007
FR-008
FR-009
FR-011
UX-001
12. Journey J-004 — Goal Creation
Objective

Allow a learner to define what they want to achieve.

Example
"I want to become a Machine Learning Engineer."
Flow
Learner
   ↓
Enter Goal
   ↓
AI Goal Understanding
   ↓
Extract Target Role
   ↓
Extract Domain
   ↓
Identify Relevant Skills
   ↓
Confirm Interpretation
   ↓
Save Goal
Example AI output

Input:

"I want to become a Machine Learning Engineer."

Possible structured representation:

{
  "target_role": "Machine Learning Engineer",
  "domain": "Machine Learning",
  "experience_level": "beginner",
  "skills": [
    "Python",
    "Statistics",
    "Machine Learning",
    "Deep Learning",
    "Model Deployment"
  ]
}

The exact schema will be defined later in DOC-07 and DOC-09.

Related requirements
FR-012
FR-013
AI-001
AI-002
13. Journey J-005 — Skill Assessment
Objective

Understand what the learner already knows.

Flow
Learner Profile
      ↓
Current Skills
      ↓
Skill Levels
      ↓
Normalize Skills
      ↓
Compare With Goal Requirements
      ↓
Identify Missing Skills
      ↓
Identify Weak Skills
      ↓
Calculate Skill Gap
Example

Goal:

Machine Learning Engineer

Learner skills:

Python       → Intermediate
NumPy        → Beginner
Statistics   → Beginner
ML           → Not Known
Deployment   → Not Known

Required skills:

Python
NumPy
Pandas
Statistics
Machine Learning
Deep Learning
Deployment

Result:

Strong:
Python

Needs Improvement:
NumPy
Statistics

Missing:
Pandas
Machine Learning
Deep Learning
Deployment
14. Journey J-006 — Skill Gap Analysis
Objective

Identify the difference between the learner's current capabilities and the target requirements.

Conceptual model
Required Skills
      -
Current Skills
      =
Skill Gap

The system should consider:

skill existence
skill proficiency
skill importance
prerequisites
target goal
Output

The system should generate:

Skill
Current Level
Required Level
Gap
Priority

Example:

Skill	Current	Required	Gap	Priority
Python	4	4	0	Low
NumPy	2	3	1	Medium
Statistics	1	4	3	High
Machine Learning	0	4	4	Critical
Deployment	0	3	3	High

The final scoring algorithm will be defined in DOC-09.

15. Journey J-007 — Recommendation Generation
Objective

Find learning resources that address the learner's skill gaps.

Flow
Learner Profile
      +
Goal
      +
Skill Gaps
      +
Preferences
      +
Learning History
      ↓
Candidate Generation
      ↓
Filtering
      ↓
Scoring
      ↓
Ranking
      ↓
Top Recommendations
Recommendation factors

The system may consider:

goal relevance
skill-gap relevance
difficulty
prerequisite compatibility
learner experience
estimated duration
preferred format
previous learning history
learner feedback
16. Journey J-008 — Personalized Path Generation
Objective

Transform individual recommendations into an ordered learning roadmap.

Example
Goal:
Machine Learning Engineer

          ↓

Stage 1
Python Foundations

          ↓

Stage 2
NumPy + Pandas

          ↓

Stage 3
Statistics

          ↓

Stage 4
Machine Learning Fundamentals

          ↓

Stage 5
ML Projects

          ↓

Stage 6
Deep Learning

          ↓

Stage 7
Deployment

          ↓

Final Project

The path should respect prerequisite relationships.

17. Journey J-009 — Recommendation Explanation
Objective

Help the learner understand why a recommendation was selected.

For each recommended resource the system should ideally explain:

Why recommended?
        ↓
Which skill does it improve?
        ↓
Which gap does it address?
        ↓
Why is it placed here?
        ↓
What should be learned afterward?

Example:

Recommended because:

You have intermediate Python skills,
but your target role requires stronger
data-processing knowledge.

This resource develops Pandas,
which is a prerequisite for several
upcoming Machine Learning topics.
18. Journey J-010 — Learning Activity
Objective

Allow the learner to follow the generated roadmap.

Each learning activity may contain:

title
description
resource URL
type
skills
prerequisites
estimated duration
difficulty
status
Activity lifecycle
Not Started
     ↓
In Progress
     ↓
Completed

Alternative:

Not Started
     ↓
Skipped
19. Journey J-011 — Progress Tracking
Objective

Track learner advancement through the learning path.

Progress levels

The system may track:

Activity progress
Not Started
In Progress
Completed
Skipped
Milestone progress
0% → 100%
Overall path progress
Completed Activities
        /
Total Required Activities
Example
Overall Progress: 42%

Foundation        ██████████ 100%
Core Skills       ███████░░░  70%
Machine Learning  ████░░░░░░  40%
Projects          ░░░░░░░░░░   0%
Deployment        ░░░░░░░░░░   0%
20. Journey J-012 — Feedback
Objective

Capture learner feedback to improve future recommendations.

Possible feedback
Useful
Not Useful
Too Easy
Too Difficult
Already Known
Not Relevant
Flow
Recommendation
      ↓
Learner Feedback
      ↓
Store Feedback
      ↓
Update Learner State
      ↓
Adjust Recommendation Score
      ↓
Generate Improved Recommendations
21. Journey J-013 — Adaptive Learning
Objective

Allow the system to adapt to changes in learner state.

Changes may occur because of:

completed courses
improved skill levels
skipped resources
negative feedback
positive feedback
changed goals
changed availability
Flow
Learner State Changes
        ↓
Update Profile / Progress
        ↓
Recalculate Relevant State
        ↓
Recalculate Skill Gaps
        ↓
Re-rank Resources
        ↓
Update Next Action
22. Journey J-014 — AI Assistant
Objective

Provide a conversational interface for learning-related questions.

Example questions

The learner may ask:

Why should I learn statistics before machine learning?
What should I study next?
Why was this course recommended?
I already know Python. Can I skip this section?
How much time will this roadmap take?
Context available to assistant

The assistant may use:

learner profile
active goal
current skills
skill gaps
learning path
completed activities
progress
feedback
23. Journey J-015 — Dashboard
Objective

Provide a single view of the learner's current state.

The dashboard should show:

+--------------------------------------+
|          PathFinder Dashboard        |
+--------------------------------------+
| Active Goal                          |
| Machine Learning Engineer            |
+--------------------------------------+
| Overall Progress                     |
| 42%                                  |
+--------------------------------------+
| Current Milestone                    |
| Machine Learning Fundamentals        |
+--------------------------------------+
| Skill Progress                       |
| Python        ██████████             |
| Statistics    ████░░░░░░             |
| ML            ███░░░░░░░              |
+--------------------------------------+
| Recommended Next Action              |
| Complete ML Fundamentals Course      |
+--------------------------------------+

The exact UI design will be defined in DOC-10.

24. Administrator Journey

The administrator workflow is intentionally limited.

Admin Login
     ↓
Admin Authentication
     ↓
Select Management Area
     ↓
Manage Resources / Skills
     ↓
Validate Changes
     ↓
Save

Potential management areas:

resources
skills
prerequisites
system configuration

A full admin dashboard is not mandatory for the MVP.

25. External AI Service Journey

The AI service interaction should follow:

User Request
     ↓
Frontend
     ↓
Backend API
     ↓
AI Service Abstraction
     ↓
Prompt / Structured Input
     ↓
External AI Provider
     ↓
Response Validation
     ↓
Backend Processing
     ↓
Frontend Response

The frontend must not directly expose AI provider credentials.

26. AI Failure Journey

If the AI service becomes unavailable:

User Request
     ↓
Backend
     ↓
AI Service
     ↓
Failure
     ↓
Fallback Logic
     ↓
Controlled Response

The application should avoid crashing.

Where possible, deterministic features should continue operating.

27. External Resource Failure Journey

If an external resource provider fails:

Resource Request
      ↓
External Provider
      ↓
Failure
      ↓
Timeout / Error Detection
      ↓
Fallback Resource / Cached Data
      ↓
Controlled User Response

The MVP should preferably use an internal resource dataset to minimize dependency on external availability.

28. Database Failure Journey

If the database is unavailable:

Application Request
      ↓
Backend
      ↓
Database
      ↓
Failure
      ↓
Exception Handling
      ↓
Logging
      ↓
Safe API Response

The system must not expose database credentials or internal stack traces to users.

29. Unauthorized Access Journey
Request Protected Resource
        ↓
Authentication Check
        ↓
Authenticated?
     /       \
   YES        NO
    ↓          ↓
Continue     Reject
              ↓
          401 Response

Authorization must additionally verify that the authenticated user has permission to access the requested resource.

30. Learner State Model

The learner's state can be represented conceptually as:

Learner
   |
   +-- Profile
   |
   +-- Goals
   |
   +-- Skills
   |
   +-- Interests
   |
   +-- Learning History
   |
   +-- Preferences
   |
   +-- Active Learning Path
   |
   +-- Progress
   |
   +-- Feedback

This model will later map to the database entities defined in DOC-07.

31. Learner State Transition

The learner's personalization state evolves over time.

Initial State
     ↓
Profile Created
     ↓
Goal Defined
     ↓
Skill State Established
     ↓
Learning Path Generated
     ↓
Learning Started
     ↓
Progress Updated
     ↓
Feedback Received
     ↓
Learner State Updated
     ↓
Recommendations Re-ranked
     ↓
Path Updated
32. Primary MVP Journey

The minimum demonstrable journey is:

1. Register
      ↓
2. Login
      ↓
3. Create Profile
      ↓
4. Enter Goal
      ↓
5. Enter Current Skills
      ↓
6. Analyze Skill Gap
      ↓
7. Generate Personalized Recommendations
      ↓
8. Generate Learning Path
      ↓
9. View Explanation
      ↓
10. Track Progress
      ↓
11. Ask AI Assistant
      ↓
12. Provide Feedback

This journey represents the core competition demonstration.

33. Golden Demo Scenario

The team should use one consistent learner scenario during development and demonstration.

Example learner
Name:
Siddharth

Experience:
Beginner / Intermediate

Current Skills:
Python
Basic SQL
Basic Mathematics

Goal:
Become a Machine Learning Engineer

Weekly Learning Time:
10 hours

Preferred Format:
Courses + Videos + Projects
Expected system behavior

The system should:

understand the goal
identify required skills
compare current skills with required skills
identify gaps
prioritize gaps
retrieve candidate resources
rank resources
build a learning path
explain recommendations
show progress
provide the next recommended action
adapt after learner feedback
34. Journey-to-Requirement Mapping
Journey	Requirements
Registration	FR-001, SEC-001, SEC-005
Login	FR-002, FR-003, SEC-002
Profile	FR-005–FR-011
Goal	FR-012–FR-015, AI-001, AI-002
Skill Assessment	FR-020–FR-024
Skill Gap	FR-023, FR-024, AI-005
Recommendations	FR-029–FR-035, AI-003, AI-006
Prerequisites	FR-036–FR-039
Learning Path	FR-040–FR-046
Explainability	FR-047–FR-050, AI-007
Progress	FR-051–FR-055
Feedback	FR-056–FR-058
Adaptation	FR-059–FR-062, AI-008
Dashboard	FR-063–FR-067
AI Assistant	FR-016–FR-019, AI-001, AI-007
Error Handling	FR-068–FR-072
35. Journey-to-UI Mapping

The following mapping will guide frontend development.

Journey	Expected UI
Registration	Register Screen
Login	Login Screen
Onboarding	Profile Setup
Goal Creation	Goal Input
Skill Assessment	Skills Input
Skill Gap	Skill Gap View
Recommendations	Recommendation View
Learning Path	Roadmap View
Explanation	Recommendation Explanation
Progress	Progress Dashboard
Feedback	Feedback Controls
AI Assistant	Chat Interface
Dashboard	Learner Dashboard

Detailed UI architecture will be defined in DOC-10.

36. Journey-to-API Mapping Foundation

The following API capabilities will eventually be required:

Authentication
    ↓
Profile Management
    ↓
Goal Management
    ↓
Skill Management
    ↓
Skill Gap Analysis
    ↓
Recommendation Generation
    ↓
Learning Path Generation
    ↓
Progress Management
    ↓
Feedback Management
    ↓
AI Assistant
    ↓
Dashboard

Exact endpoint names, request schemas, response schemas, authentication mechanisms, status codes, and validation rules will be defined in DOC-08.

37. Journey-to-Database Mapping Foundation

The journeys imply the following logical data domains:

Users
Profiles
Goals
Skills
Learner Skills
Skill Relationships
Resources
Resource Skills
Learning Paths
Path Stages
Learning Activities
Progress
Feedback
Chat Sessions
Chat Messages

The final database schema will be defined in DOC-07.

38. Journey-to-AI Mapping
Journey	AI Capability
Goal Creation	Natural-language understanding
Goal Interpretation	Information extraction
Skill Identification	Skill extraction
Skill Gap	Gap reasoning
Recommendation	Semantic matching
Ranking	Relevance scoring
Explanation	Explainable generation
Chat	Conversational AI
Adaptation	Personalization

The exact algorithms and AI architecture will be defined in DOC-09.

39. User Experience Principles

PathFinder AI should follow these principles.

UX-P001 — Minimize Cognitive Load

The system should avoid overwhelming learners with too many choices.

UX-P002 — Show the Next Step

The learner should always understand what to do next.

UX-P003 — Explain Recommendations

The learner should understand why the system recommended something.

UX-P004 — Keep Progress Visible

The learner should be able to see improvement over time.

UX-P005 — Avoid Unnecessary Complexity

The MVP should focus on the core learning journey.

UX-P006 — Maintain Trust

AI-generated information should be presented in a transparent and understandable way.

40. Accessibility Principles

The interface should aim to provide:

readable typography
sufficient contrast
keyboard-friendly interactions
clear labels
understandable error messages
responsive layouts
accessible interactive controls

Detailed accessibility implementation will be handled during frontend development.

41. MVP User Journey Boundary

The MVP must prioritize the learner.

Included
learner registration
login
onboarding
profile
goal creation
current skills
skill-gap analysis
recommendations
personalized learning path
explanations
progress tracking
dashboard
AI assistant
Limited
administrator functionality
external resource integrations
advanced adaptive learning
Future
advanced analytics
multi-provider resource aggregation
advanced assessment engine
collaborative learning
social learning
enterprise integrations
42. Journey Completion Criteria

DOC-03 is considered complete when:

 all primary actors are defined
 learner persona is defined
 core learner journey is defined
 registration journey is defined
 onboarding journey is defined
 goal journey is defined
 skill-gap journey is defined
 recommendation journey is defined
 learning-path journey is defined
 progress journey is defined
 feedback journey is defined
 adaptive journey is defined
 AI assistant journey is defined
 dashboard journey is defined
 failure journeys are defined
 journey-to-requirement mapping is established
 journey-to-UI mapping is established
 journey-to-API mapping foundation is established
 journey-to-database mapping foundation is established
 journey-to-AI mapping is established
43. Document Status

Status: DRAFT

Parent Documents:

DOC-01 Project Charter
DOC-02 Software Requirements Specification

Next Document:

DOC-04 Use Cases and User Stories

Implementation Status: NOT STARTED

Architecture Status: NOT FROZEN

44. Change Control

Any change to a defined user journey must identify:

affected actor
affected journey
affected requirement
affected UI
affected API
affected database
affected AI workflow
affected tests
affected GitHub issues

Changes must be reflected in DOC-23 Requirements Traceability Matrix.