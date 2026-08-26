# PathFinder AI — MVP Scope and Acceptance Criteria

**Document ID:** DOC-05  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT  
**Parent Documents:** DOC-01 Project Charter, DOC-02 Software Requirements Specification, DOC-03 User Roles and Journeys, DOC-04 Use Cases and User Stories  

---

# 1. Purpose

This document defines the exact scope of the PathFinder AI Minimum Viable Product (MVP).

The purpose of this document is to establish:

- what will be implemented
- what will not be implemented
- which features are mandatory
- which features are optional
- what constitutes a completed feature
- how the MVP will be evaluated
- how the final prototype will be demonstrated
- how MVP requirements map to system components
- how MVP completion will be verified before final submission

This document acts as the primary boundary between the planned PathFinder AI product and the features that will actually be implemented for the HCLTech Round 2 prototype.

The team must not begin implementation of major features that are outside this document unless the scope is formally changed through the project's change-control process.

---

# 2. MVP Objective

The primary objective of the PathFinder AI MVP is to demonstrate an intelligent personalized learning assistant that can transform a learner's goal and current skill state into an understandable, personalized and actionable learning roadmap.

The MVP must demonstrate the following complete journey:

```text
Learner
   |
   v
Registration / Login
   |
   v
Profile Creation
   |
   v
Learning Goal
   |
   v
Current Skills
   |
   v
Goal Understanding
   |
   v
Skill Gap Analysis
   |
   v
Learning Resource Retrieval
   |
   v
Personalized Recommendation
   |
   v
Prerequisite Ordering
   |
   v
Learning Path Generation
   |
   v
Recommendation Explanation
   |
   v
Progress Tracking
   |
   v
Feedback
   |
   v
Updated Recommendation

The MVP is considered successful only when this end-to-end workflow can be demonstrated reliably.

3. MVP Design Principles

The MVP shall follow the following principles.

3.1 End-to-End First

The team shall prioritize a complete working learner journey over implementing a large number of disconnected features.

A smaller number of fully integrated features is preferred over many incomplete features.

3.2 AI With Deterministic Controls

AI may be used for:

natural-language goal understanding
skill extraction
semantic matching
recommendation explanation
conversational interaction

However, critical application logic shall not depend entirely on unrestricted LLM output.

The system shall use deterministic application logic for:

authentication
database operations
progress calculations
prerequisite validation
authorization
API contracts
data validation
recommendation constraints
3.3 Explainability

Every important recommendation should have an understandable reason.

Example:

Recommended: Python for Data Analysis

Why:
You selected Data Scientist as your target role.
Python is a prerequisite skill for several skills in your target roadmap.
Your current profile indicates beginner-level Python knowledge.

The system should avoid presenting recommendations as unexplained AI decisions.

3.4 Reproducibility

The complete project must be reproducible from the GitHub repository.

A new developer should be able to:

clone the repository
install dependencies
configure environment variables
initialize the database
start backend services
start frontend services
access the application
execute the main learner workflow

without requiring undocumented manual steps.

3.5 Prototype Reliability

The MVP is intended for a competition demonstration.

Therefore:

critical workflows must work reliably
error states must be handled
loading states must be visible
invalid input must not crash the application
unavailable AI services must have controlled fallback behavior
API contracts must remain stable
4. MVP Scope Summary

The following capabilities are included in the MVP.

Capability	MVP Status	Priority
User Registration	Included	P0
User Login	Included	P0
User Logout	Included	P0
Learner Profile	Included	P0
Experience Level	Included	P0
Current Skills	Included	P0
Learning Interests	Included	P0
Learning Preferences	Included	P1
Goal Creation	Included	P0
Natural-Language Goal Input	Included	P0
Goal Editing	Included	P1
Skill Gap Analysis	Included	P0
Skill Dependency Graph	Included	P0
Learning Resource Dataset	Included	P0
Resource Search	Included	P0
Resource Filtering	Included	P1
Recommendation Engine	Included	P0
Personalized Ranking	Included	P0
Recommendation Explanation	Included	P0
Learning Path Generation	Included	P0
Milestones	Included	P0
Progress Tracking	Included	P0
Dashboard	Included	P0
AI Chat Assistant	Included	P0
Feedback	Included	P1
Adaptive Re-ranking	Included in limited form	P1
External Learning Platform APIs	Optional	P2
Administrator Portal	Not required	P2
Multi-language Support	Not required	P3
Production-scale Recommendation Infrastructure	Not required	P3
Advanced Predictive Analytics	Not required	P3
5. P0 — Mandatory MVP Features

P0 features are mandatory.

The project shall not be considered MVP-complete if a critical P0 feature is missing or non-functional.

5.1 Authentication
Feature

Learners shall be able to create accounts and authenticate securely.

Required functionality
registration
login
logout
authenticated API access
password hashing
protected resources
MVP acceptance criteria

The feature is accepted when:

a new learner can register
duplicate email registration is rejected
invalid credentials are rejected
valid credentials create an authenticated session
protected endpoints reject unauthenticated requests
logout invalidates the authenticated session
passwords are never stored in plaintext
5.2 Learner Profile
Feature

The learner shall maintain a structured learning profile.

Required profile information

At minimum:

Name
Email
Experience Level
Current Skills
Interests

Optional:

Preferred Learning Format
Weekly Learning Time
Difficulty Preference
MVP acceptance criteria

The feature is accepted when:

learner can create a profile
learner can view the profile
learner can update the profile
current skills can be added
experience level can be selected
interests can be recorded
profile data persists after logout/login
5.3 Goal Management
Feature

Learners shall define a target learning objective.

Required functionality

The learner must be able to enter a natural-language goal.

Example:

I want to become a Machine Learning Engineer.

Other examples:

I want to become a Data Analyst.

I want to learn full-stack web development.

I want to prepare for a Python developer role.
MVP acceptance criteria

The feature is accepted when:

learner can enter a goal
goal is stored
goal is associated with the correct learner
goal can be retrieved
goal can be used by the recommendation system
natural-language goal can be processed by the AI layer or deterministic fallback
5.4 Goal Understanding
Feature

The system shall convert the learner's natural-language goal into structured information.

Example:

Input:

I want to become a Machine Learning Engineer.

Possible structured output:

Target Role:
Machine Learning Engineer

Domain:
Artificial Intelligence / Machine Learning

Potential Skills:
Python
NumPy
Pandas
Statistics
Machine Learning
Scikit-learn
Deep Learning
Model Deployment
MVP acceptance criteria

The feature is accepted when:

the system processes natural-language goals
a target role/domain can be identified
relevant skills can be extracted or mapped
structured information is available to downstream recommendation logic
malformed AI output does not crash the application
5.5 Skill Management
Feature

The system shall maintain structured learner skills.

Each skill shall have a proficiency level.

Recommended MVP scale:

0 = Not Known
1 = Beginner
2 = Basic
3 = Intermediate
4 = Advanced
5 = Expert
Example
Python        -> 3
SQL           -> 2
Statistics    -> 1
Machine Learning -> 0
MVP acceptance criteria

The feature is accepted when:

learner can add skills
learner can assign proficiency
learner can update proficiency
skills are stored in structured form
skills can be used by the skill-gap engine
5.6 Skill Gap Analysis
Feature

The system shall compare the learner's current skills against the skills required for the selected goal.

Conceptually:

Required Skills
       -
Current Skills
       =
Skill Gap
Example

Target:

Machine Learning Engineer

Current skills:

Python = Intermediate
SQL = Basic

Required skills:

Python
Statistics
NumPy
Pandas
Machine Learning
Scikit-learn
Deep Learning
Deployment

Skill gaps:

Statistics
NumPy
Pandas
Machine Learning
Scikit-learn
Deep Learning
Deployment
MVP acceptance criteria

The feature is accepted when:

required goal skills can be identified
current learner skills can be retrieved
current and required skills are compared
missing/insufficient skills are identified
skill-gap results are stored or returned in a structured response
skill-gap results influence recommendation generation
5.7 Skill Dependency System
Feature

The system shall support prerequisite relationships between skills.

Example:

Python
   |
   +----> NumPy
   |
   +----> Pandas
             |
             v
       Machine Learning
             |
             v
       Deep Learning
MVP acceptance criteria

The feature is accepted when:

skills can have prerequisite relationships
prerequisite relationships can be queried
learning paths respect important prerequisite relationships
a learner is not incorrectly placed into advanced content when a mandatory prerequisite is missing
5.8 Learning Resource Dataset
Feature

The MVP shall contain a structured learning-resource dataset.

The resource dataset may initially be curated rather than obtained through live external integrations.

This is intentional.

The MVP should prioritize recommendation quality and system reliability over complex external API integrations.

Required metadata

Each resource should contain:

Resource ID
Title
Description
URL
Provider
Resource Type
Difficulty
Duration
Skills
Prerequisites
Rating
Language
Example
Resource ID:
RES-001

Title:
Python for Data Analysis

Provider:
Example Provider

Type:
Course

Difficulty:
Beginner

Skills:
Python
Pandas
Data Analysis

Prerequisites:
Basic Programming
MVP acceptance criteria

The feature is accepted when:

resources exist in the database
resources contain structured metadata
resources are linked to skills
resources can be retrieved through backend APIs
resources can be used by the recommendation engine
5.9 Recommendation Engine
Feature

The system shall generate personalized learning-resource recommendations.

Recommendation input:

Learner Profile
+
Goal
+
Current Skills
+
Skill Gaps
+
Experience
+
Learning History
+
Preferences

Recommendation output:

Ranked Learning Resources
MVP recommendation pipeline
Learner Context
       |
       v
Goal Understanding
       |
       v
Skill Gap Analysis
       |
       v
Candidate Resource Retrieval
       |
       v
Candidate Filtering
       |
       v
Candidate Scoring
       |
       v
Ranking
       |
       v
Top Recommendations
MVP acceptance criteria

The feature is accepted when:

candidate resources are generated
irrelevant resources are filtered where possible
candidates are scored
candidates are ranked
top recommendations are returned
recommendations are influenced by learner context
different learner profiles can produce different results
5.10 Personalized Recommendation Scoring

The MVP should use a transparent scoring mechanism.

A recommended initial scoring model is:

Recommendation Score =
    Goal Alignment Score
    +
    Skill Gap Score
    +
    Prerequisite Fit Score
    +
    Difficulty Fit Score
    +
    Preference Score
    +
    Progress/History Score

The exact weights will be finalized in DOC-09.

Example:

Goal Alignment       = 0.30
Skill Gap Alignment  = 0.30
Prerequisite Fit     = 0.15
Difficulty Fit       = 0.10
Preference Fit       = 0.05
History/Progress     = 0.10

Total:

1.00

These values are initial prototype values and may be changed during experimentation.

Any final changes must be documented.

5.11 Learning Path Generation
Feature

The system shall convert ranked resources and prerequisite relationships into a structured learning roadmap.

Example:

Goal:
Machine Learning Engineer

Stage 1 — Foundation
    |
    +-- Python Fundamentals
    +-- Programming Basics

Stage 2 — Data Foundations
    |
    +-- NumPy
    +-- Pandas
    +-- Data Visualization

Stage 3 — Mathematics
    |
    +-- Statistics
    +-- Probability
    +-- Linear Algebra

Stage 4 — Machine Learning
    |
    +-- Supervised Learning
    +-- Unsupervised Learning
    +-- Model Evaluation

Stage 5 — Applied Projects
    |
    +-- ML Project
    +-- End-to-End Project

Stage 6 — Deployment
    |
    +-- Model Serving
    +-- API
    +-- Deployment
MVP acceptance criteria

The feature is accepted when:

a learning path can be generated
path items are ordered
prerequisites influence ordering
milestones are visible
resources are associated with path items
learner can identify the next recommended action
5.12 Milestones
Feature

The learning path shall contain milestones.

Example:

Milestone 1:
Python Foundation

Milestone 2:
Data Processing

Milestone 3:
Machine Learning Fundamentals

Milestone 4:
Applied ML Project

Milestone 5:
Deployment
MVP acceptance criteria
milestones can be created
milestones contain learning activities
milestone completion can be calculated
milestone status is visible on the dashboard
5.13 AI Recommendation Explanation
Feature

The system shall explain recommendations.

Example:

Why was this recommended?

Python for Data Analysis was recommended because:

1. Python is part of your target skill roadmap.
2. Your current Python proficiency is beginner level.
3. Pandas is required for the next stage of your roadmap.
4. This resource matches your selected difficulty preference.
MVP acceptance criteria
each major recommendation has an explanation
explanation references relevant learner context
explanation is understandable
AI-generated explanations are validated before display
explanation generation failure does not crash recommendation delivery
5.14 Conversational AI Assistant
Feature

The application shall provide an AI-powered conversational interface.

The learner should be able to ask:

Why do I need Python?

What should I learn next?

Why was this course recommended?

Can I skip this topic?

What skills am I missing?

How long will this roadmap take?

What should I learn after SQL?
MVP acceptance criteria
learner can submit natural-language questions
system returns an understandable response
relevant learner context is included
the assistant can reference the learner's current path
the assistant does not expose private data from another learner
AI failure has a controlled fallback
5.15 Progress Tracking
Feature

Learners shall be able to track learning activity progress.

Activity states:

NOT_STARTED
IN_PROGRESS
COMPLETED
SKIPPED
MVP acceptance criteria
learner can mark activities as started/completed
completion is persisted
milestone progress updates
overall path progress updates
dashboard reflects the updated progress
5.16 Dashboard
Feature

The dashboard shall provide a centralized view of the learner's learning state.

Required dashboard components
Current Goal
Overall Progress
Current Milestone
Skill Progress
Skill Gaps
Recommended Next Action
Learning Path
Recent Activity
MVP acceptance criteria

The dashboard is accepted when:

learner goal is visible
progress is visible
milestones are visible
skill gaps are visible
next action is visible
learning path is accessible
dashboard updates after relevant learner actions
6. P1 — Important Features

P1 features should be implemented if time permits without risking P0 stability.

6.1 Learner Feedback

Learners may provide recommendation feedback.

Possible feedback:

Useful
Not Useful
Too Easy
Too Difficult
Already Known
Not Relevant

The feedback should be stored.

6.2 Adaptive Recommendation

The system may use feedback and progress to influence subsequent recommendations.

Example:

Learner repeatedly marks beginner resources as "Too Easy"

        |
        v

Difficulty preference increases

        |
        v

Recommendation engine adjusts ranking

This feature should initially use simple deterministic adaptation.

Complex machine-learning personalization is not required for the MVP.

6.3 Learning Preferences

The learner may specify:

Weekly learning time
Preferred resource type
Preferred difficulty
Preferred learning format

These preferences may influence recommendation ranking.

6.4 Resource Filtering

Learners may filter resources by:

Difficulty
Type
Duration
Skill
Provider

This is useful but not more important than the core recommendation workflow.

7. P2 — Optional Features

P2 features may be implemented only after all P0 features are stable.

Potential P2 features:

administrator dashboard
live course-provider integration
external learning-platform APIs
automatic resource ingestion
advanced recommendation analytics
advanced learner analytics
resource freshness detection
notification system
email reminders
downloadable learning roadmap
roadmap sharing

These features must not delay the core MVP.

8. P3 — Future Features

The following features are explicitly outside the current competition MVP.

Potential future capabilities include:

large-scale collaborative filtering
reinforcement-learning-based recommendations
multi-language learning assistance
voice assistant
mobile applications
advanced predictive dropout detection
social learning
peer comparison
institution-wide analytics
enterprise LMS integrations
real-time learning-behavior modeling
production-scale distributed recommendation infrastructure

These features shall not be implemented unless the project scope is formally expanded.

9. Features Explicitly Excluded From MVP

The following capabilities are excluded from the MVP.

9.1 Full LMS

PathFinder AI is a recommendation and learning-path system.

It is not intended to become a complete learning-management platform.

The MVP will not provide:

full course hosting
video streaming infrastructure
assignment hosting
certificate generation
instructor management
payment processing
9.2 Large-Scale Web Crawling

The MVP will not depend on crawling thousands of websites.

A curated or controlled dataset is acceptable for the prototype.

9.3 Complex Recommendation Training

The MVP does not require training a large neural recommendation model.

A hybrid approach combining:

Rules
+
Structured Metadata
+
Semantic Similarity
+
Weighted Ranking
+
Optional LLM

is acceptable and preferred for prototype reliability.

9.4 Production-Scale Infrastructure

The project does not require:

Kubernetes
distributed databases
multi-region deployment
complex message queues
large-scale data warehouses

unless later required by architecture decisions.

10. MVP End-to-End Workflow

The complete MVP workflow shall be:

STEP 1
User opens PathFinder AI

        ↓

STEP 2
User registers / logs in

        ↓

STEP 3
User completes learner profile

        ↓

STEP 4
User enters learning goal

        ↓

STEP 5
AI processes goal

        ↓

STEP 6
System identifies target skills

        ↓

STEP 7
System compares target skills with learner skills

        ↓

STEP 8
System identifies skill gaps

        ↓

STEP 9
System retrieves candidate resources

        ↓

STEP 10
Recommendation engine scores resources

        ↓

STEP 11
Resources are ranked

        ↓

STEP 12
Prerequisites are evaluated

        ↓

STEP 13
Learning path is generated

        ↓

STEP 14
System explains recommendations

        ↓

STEP 15
Learner views dashboard

        ↓

STEP 16
Learner starts learning

        ↓

STEP 17
Learner updates progress

        ↓

STEP 18
System updates milestone progress

        ↓

STEP 19
Learner provides feedback

        ↓

STEP 20
Recommendation engine updates future recommendations

This workflow represents the primary competition demonstration scenario.

11. Primary Demo Scenario

The final prototype shall have one polished demonstration scenario.

Recommended scenario:

Target Role:
Machine Learning Engineer

Example learner:

Name:
Demo User

Experience:
Beginner

Current Skills:
Python — Basic
SQL — Basic

Interests:
Machine Learning
Artificial Intelligence
Data Science

Weekly Time:
8 hours

Goal:

I want to become a Machine Learning Engineer.

The system should then demonstrate:

Goal Understanding
        ↓
Required Skills
        ↓
Current Skills
        ↓
Skill Gaps
        ↓
Recommendations
        ↓
Personalized Roadmap
        ↓
Explanation
        ↓
Progress
        ↓
Feedback
        ↓
Updated Recommendation
12. Demo Acceptance Criteria

The competition demonstration shall satisfy the following.

AC-001 — User Access

Evaluator can access the application.

AC-002 — Registration/Login

Evaluator can register or use a provided demo account.

AC-003 — Profile

Evaluator can view or create learner profile.

AC-004 — Goal

Evaluator can enter a natural-language career/learning goal.

AC-005 — Goal Understanding

The system identifies relevant target skills.

AC-006 — Skill Gap

The system displays meaningful skill gaps.

AC-007 — Recommendation

The system generates relevant recommendations.

AC-008 — Explainability

The system explains why recommendations were generated.

AC-009 — Learning Path

The system generates an ordered roadmap.

AC-010 — Prerequisites

The roadmap respects important prerequisite relationships.

AC-011 — Dashboard

The dashboard displays learning progress and next actions.

AC-012 — Progress

The evaluator can mark an activity as completed.

AC-013 — Updated Progress

The dashboard reflects the changed progress.

AC-014 — AI Assistant

The evaluator can ask a learning-related question.

AC-015 — AI Context

The assistant responds using relevant learner/path context.

AC-016 — Feedback

The evaluator can provide recommendation feedback if the P1 feedback feature is implemented.

AC-017 — Adaptation

The recommendation system can demonstrate at least basic adaptation when learner state changes.

13. Technical Acceptance Criteria

The application shall satisfy the following technical requirements.

TAC-001 — Backend Starts Successfully

Backend must start without runtime errors using documented commands.

TAC-002 — Frontend Starts Successfully

Frontend must start without runtime errors using documented commands.

TAC-003 — Database Connection

Application must connect to the configured database successfully.

TAC-004 — Environment Configuration

Required environment variables must be documented.

Secrets must not be committed to Git.

TAC-005 — API Contract

Frontend and backend communication must follow DOC-08.

TAC-006 — Authentication Contract

Authentication implementation must follow DOC-12.

TAC-007 — Folder Architecture

Source code must follow DOC-13.

TAC-008 — Dependencies

Dependencies must follow DOC-14.

TAC-009 — Environment

Environment configuration must follow DOC-15.

TAC-010 — Git Workflow

Development must follow DOC-16.

TAC-011 — Testing

Critical workflows must follow DOC-18.

TAC-012 — Deployment

Deployment must follow DOC-19.

14. MVP Performance Expectations

Exact production performance targets are outside the competition MVP.

However, the prototype should provide an interactive experience.

Recommended targets:

Operation	Target
Login	< 2 seconds
Profile retrieval	< 1 second
Goal submission	< 5 seconds
Skill-gap analysis	< 5 seconds
Recommendation generation	< 5 seconds
Learning-path generation	< 7 seconds
Dashboard retrieval	< 2 seconds
Standard API request	< 2 seconds

AI/LLM operations may exceed these targets occasionally.

The UI shall display appropriate loading indicators.

15. MVP Reliability Requirements

The following workflows must not cause application crashes:

Invalid Login
Invalid Registration
Missing Goal
Malformed Goal
Empty Skill List
No Recommendations
AI Service Failure
Database Failure
External Resource Failure
Invalid Resource ID
Unauthorized API Request
Expired Authentication

The system should return controlled errors.

Example:

{
  "success": false,
  "error": {
    "code": "NO_RECOMMENDATIONS",
    "message": "No suitable learning resources were found for the current profile."
  }
}

The exact API error schema will be defined in DOC-08.

16. AI Reliability Requirements

AI-generated output must not directly overwrite critical database state without validation.

The system shall validate structured AI output before using it.

Example:

User Goal
   |
   v
LLM
   |
   v
Structured JSON
   |
   v
Schema Validation
   |
   +---- Invalid ----> Fallback
   |
   v
Application Logic

AI should not be responsible for:

authentication
authorization
database integrity
progress calculation
prerequisite enforcement
security decisions
17. Data Acceptance Criteria

The MVP database shall contain enough structured data to demonstrate meaningful recommendations.

Minimum recommended dataset:

Skills:
50+

Learning Resources:
100+

Skill Relationships:
50+

Goal/Role Profiles:
10+

Resource-Skill Relationships:
200+

These are prototype targets rather than strict competition requirements.

The dataset may be smaller during initial development but should be expanded before the final demonstration.

18. Minimum Recommendation Quality

The recommendation engine must not simply return random resources.

A recommendation should satisfy at least one strong relevance condition:

Goal relevance
OR
Skill-gap relevance
OR
Prerequisite relevance
OR
Learner preference relevance

The final ranking should combine multiple signals wherever possible.

19. Explainability Requirements

For each top recommendation, the system should be capable of producing structured reasoning.

Example:

{
  "resource_id": "RES-001",
  "reasons": [
    "Matches target role",
    "Addresses current skill gap",
    "Matches learner difficulty",
    "Satisfies prerequisite sequence"
  ]
}

The UI may convert this into human-readable text.

20. Security Acceptance Criteria

The MVP must satisfy:

No plaintext passwords
No secrets in Git
Authenticated protected APIs
Authorization checks
Validated user input
Safe error responses
Secure database access
Controlled AI prompts
No cross-user data leakage
21. Testing Acceptance Criteria

Before MVP approval, the following tests must pass.

Authentication Tests
Registration success
Duplicate registration failure
Login success
Invalid login failure
Logout
Unauthorized access
Profile Tests
Create profile
Read profile
Update profile
Skill update
Goal Tests
Create goal
Read goal
Update goal
Goal processing
Skill Gap Tests
Current skill retrieval
Required skill retrieval
Skill comparison
Gap generation
Recommendation Tests
Candidate retrieval
Filtering
Scoring
Ranking
Explanation
Path Tests
Path generation
Prerequisite ordering
Milestone generation
Next action
Progress Tests
Activity completion
Milestone progress
Overall progress
AI Tests
Valid goal
Malformed goal
AI unavailable
Invalid AI output
Context-aware response
22. MVP Completion Checklist

The MVP shall not be marked complete until the following checklist is satisfied.

Product
 User registration works
 User login works
 User logout works
 Learner profile works
 Goal creation works
 Natural-language goal processing works
 Current skills work
 Skill-gap analysis works
 Learning resources exist
 Recommendation engine works
 Recommendation ranking works
 Recommendation explanations work
 Prerequisite system works
 Learning path generation works
 Milestones work
 Progress tracking works
 Dashboard works
 AI assistant works
 Feedback works if included
 Basic adaptation works if included
Engineering
 Backend starts successfully
 Frontend starts successfully
 Database starts/connects successfully
 Environment variables documented
 API contracts implemented
 Authentication secured
 Error handling implemented
 Logging implemented
 Tests pass
 No secrets committed
 No unnecessary dependency folders committed
Documentation
 README complete
 Architecture documentation complete
 API documentation complete
 Database documentation complete
 AI/ML documentation complete
 Setup documentation complete
 Testing documentation complete
 Deployment documentation complete
 Integration checklist complete
 Traceability matrix updated
Competition
 Source code ZIP prepared
 GitHub repository accessible
 Solution documentation prepared
 Demo video recorded
 Demo video is 3–5 minutes
 Application deployed if possible
 Local setup instructions verified
 Demo account prepared
 Final demo scenario tested
 No critical demo failures
23. MVP Freeze Criteria

Before final competition submission, the MVP shall enter a feature freeze.

After feature freeze:

no major architectural changes
no unnecessary dependency changes
no database redesign
no API contract redesign
no major UI redesign

Only:

Bug fixes
Performance improvements
Security fixes
Demo improvements
Documentation corrections

should be allowed unless a critical issue requires architectural modification.

24. Scope Change Process

If a team member proposes a new feature, the following process must be followed.

Feature Proposal
      |
      v
Business Value Evaluation
      |
      v
Competition Relevance
      |
      v
Implementation Effort
      |
      v
Risk Evaluation
      |
      v
Dependency Evaluation
      |
      v
Decision

A new feature shall not be implemented immediately.

The team must first determine whether it affects:

requirements
database
API
frontend
backend
AI architecture
testing
deployment
documentation
timeline

Any approved scope change must be reflected in DOC-22 and DOC-23.

25. MVP-to-Architecture Mapping

The following mapping establishes the relationship between MVP requirements and future architecture documents.

MVP Area	Primary Document
Product Scope	DOC-01
Requirements	DOC-02
User Roles	DOC-03
Use Cases	DOC-04
MVP Scope	DOC-05
System Architecture	DOC-06
Database	DOC-07
API	DOC-08
AI/ML	DOC-09
Frontend	DOC-10
Backend	DOC-11
Security	DOC-12
Code Structure	DOC-13
Technology Stack	DOC-14
Environment	DOC-15
GitHub Workflow	DOC-16
GitHub Tasks	DOC-17
Testing	DOC-18
Deployment	DOC-19
Integration	DOC-20
AI Implementation	DOC-21
Architecture Decisions	DOC-22
Traceability	DOC-23
26. MVP Dependency Chain

The implementation dependencies are:

Authentication
      |
      v
Learner Profile
      |
      v
Goal Management
      |
      v
Skill Representation
      |
      v
Goal Understanding
      |
      v
Skill Gap Analysis
      |
      v
Resource Dataset
      |
      v
Recommendation Engine
      |
      v
Prerequisite Engine
      |
      v
Learning Path Generator
      |
      v
Progress Tracking
      |
      v
Dashboard
      |
      v
Feedback
      |
      v
Adaptive Recommendation

The AI assistant can operate across several stages:

Goal Understanding
Recommendation Explanation
Learning Path Explanation
Progress Questions
General Learning Questions
27. MVP Implementation Order

The recommended implementation order is:

Phase 1 — Foundation
Repository
Environment
Backend
Frontend
Database
Configuration
Phase 2 — Authentication
Registration
Login
Logout
Protected APIs
Phase 3 — Learner
Profile
Skills
Interests
Preferences
Phase 4 — Goals
Goal Creation
Goal Retrieval
Goal Processing
Phase 5 — Skill Intelligence
Skill Database
Required Skills
Skill Relationships
Skill Gap Analysis
Phase 6 — Resources
Resource Dataset
Resource APIs
Resource-Skill Mapping
Phase 7 — Recommendation
Candidate Generation
Filtering
Scoring
Ranking
Explanation
Phase 8 — Learning Path
Prerequisites
Path Generation
Stages
Milestones
Next Action
Phase 9 — Progress
Activity Status
Completion
Milestones
Overall Progress
Phase 10 — AI Assistant
Chat
Context
Explanation
Learning Questions
Fallback
Phase 11 — Dashboard
Goal
Progress
Skills
Gaps
Roadmap
Next Action
Phase 12 — Adaptation
Feedback
Preference Updates
Re-ranking
Phase 13 — Testing
Unit Tests
Integration Tests
API Tests
AI Tests
End-to-End Tests
Phase 14 — Deployment
Production Build
Deployment
Environment Configuration
Smoke Testing
Phase 15 — Competition Preparation
Demo Account
Demo Script
Demo Video
Documentation
Source ZIP
GitHub Verification
Final Testing
28. Definition of Done

A feature is considered DONE only when all of the following are true:

Requirement identified
        +
Design defined
        +
Database changes completed if required
        +
Backend implemented
        +
API implemented
        +
Frontend implemented if required
        +
Validation implemented
        +
Error handling implemented
        +
Tests written
        +
Tests passing
        +
Documentation updated
        +
GitHub issue completed
        +
Code reviewed
        +
Integration verified

A feature shall not be marked complete merely because its code exists.

29. Definition of MVP Complete

The PathFinder AI MVP is considered complete when:

All mandatory P0 features work.
The complete learner journey is executable.
The recommendation engine produces meaningful results.
Learning paths respect important prerequisites.
Recommendations have understandable explanations.
Learner progress is persisted.
Dashboard reflects learner state.
AI assistant works with relevant context.
Critical errors are handled.
Security requirements are satisfied.
Tests for critical workflows pass.
Documentation matches implementation.
GitHub repository contains the complete project.
Local setup has been verified from a clean environment.
The final demo scenario works from beginning to end.
30. Competition Evaluation Alignment

The MVP is intentionally designed around the official judging categories.

Problem Understanding & Solution Design — 20%

Demonstrated through:

clear learner problem definition
learner profiling
skill-gap analysis
personalized learning paths
explainable recommendations
system architecture
Functionality & Feature Completeness — 25%

Demonstrated through:

authentication
profile
goals
recommendations
learning path
progress
dashboard
AI assistant
AI/ML Implementation — 20%

Demonstrated through:

natural-language goal understanding
skill extraction
semantic matching
recommendation ranking
explainability
adaptive personalization
Innovation & Creativity — 15%

Demonstrated through:

goal-to-roadmap transformation
skill-gap intelligence
prerequisite-aware sequencing
explainable recommendations
adaptive learning paths
conversational learning assistant
User Experience & Interface — 10%

Demonstrated through:

simple onboarding
natural-language goal input
clear roadmap
progress visualization
clear next action
conversational interaction
Performance & Code Quality — 10%

Demonstrated through:

modular architecture
API separation
validation
testing
error handling
documentation
reproducible setup
31. Final Demo Success Condition

The strongest single demonstration should communicate the following story:

"I tell PathFinder AI where I want to go."

                ↓

"It understands my goal."

                ↓

"It understands where I currently stand."

                ↓

"It identifies what I am missing."

                ↓

"It finds what I should learn."

                ↓

"It puts everything in the right order."

                ↓

"It explains why."

                ↓

"I start learning."

                ↓

"It tracks my progress."

                ↓

"It adapts what I should learn next."

This is the core value proposition of PathFinder AI.

32. Document Dependencies

This document depends on:

DOC-01 Project Charter
DOC-02 Software Requirements Specification
DOC-03 User Roles and Journeys
DOC-04 Use Cases and User Stories

The following documents depend on this document:

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
DOC-16 Git/GitHub Development Workflow
DOC-17 GitHub Task Breakdown
DOC-18 Testing and QA Strategy
DOC-19 Deployment Architecture
DOC-20 Integration Checklist
DOC-21 Master AI Implementation Specification
DOC-22 Architecture Decision Records
DOC-23 Requirements Traceability Matrix
33. Document Status

Document ID: DOC-05

Status: DRAFT

Version: 1.0

Parent Documents:

DOC-01
DOC-02
DOC-03
DOC-04

Next Document:

DOC-06 — System Architecture

Implementation Status:

NOT STARTED

Architecture Status:

NOT FROZEN
34. Approval Criteria

DOC-05 can be considered approved when all team members agree on:

MVP boundaries
mandatory features
optional features
excluded features
primary demo scenario
implementation order
acceptance criteria
definition of done

Once approved, the team should avoid uncontrolled scope expansion.

35. Change Control

Any change to MVP scope must identify:

Change ID
Date
Requested By
Feature
Current Scope
Proposed Scope
Reason
Priority
Architecture Impact
Database Impact
API Impact
Frontend Impact
Backend Impact
AI/ML Impact
Testing Impact
Deployment Impact
Documentation Impact
Decision

All approved changes must be reflected in:

DOC-05
DOC-17
DOC-22
DOC-23
36. Final Statement

PathFinder AI MVP is not defined by the number of features implemented.

It is defined by whether the system can reliably demonstrate a complete personalized learning journey:

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
Prerequisites
  ↓
Personalized Path
  ↓
Explanation
  ↓
Progress
  ↓
Feedback
  ↓
Adaptation

The implementation team shall prioritize this complete journey above non-essential functionality.