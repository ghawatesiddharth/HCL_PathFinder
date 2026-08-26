# PathFinder AI — Software Requirements Specification

**Document ID:** DOC-02  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT  
**Parent Document:** DOC-01 Project Charter  

---

# 1. Purpose

This Software Requirements Specification defines the functional and non-functional requirements for PathFinder AI.

The purpose of this document is to convert the project vision established in DOC-01 into precise, testable software requirements.

Every implementation feature must trace back to one or more requirements defined in this document.

---

# 2. Requirement Identification Convention

Requirements use the following identifiers:

| Prefix | Meaning |
|---|---|
| FR | Functional Requirement |
| NFR | Non-Functional Requirement |
| AI | AI/ML Requirement |
| SEC | Security Requirement |
| INT | Integration Requirement |
| DATA | Data Requirement |
| UX | User Experience Requirement |
| OPS | Operations Requirement |

Example:

```text
FR-001
represents a functional requirement.

3. Product Overview

PathFinder AI is a personalized learning assistant that converts a learner's goal and current learning state into an actionable learning roadmap.

The system combines:

learner profiling
natural-language goal understanding
skill representation
skill-gap analysis
learning-resource recommendation
prerequisite reasoning
personalized path generation
AI explanations
progress tracking
learner feedback
adaptive recommendations
4. Actors
ACT-001 — Learner

The primary user.

The learner can:

register
authenticate
create a profile
define goals
specify skills
interact with the AI assistant
receive recommendations
view learning paths
track progress
provide feedback
ACT-002 — System Administrator

An administrative role may manage:

learning resources
skill definitions
prerequisite relationships
datasets
system configuration

Administrator functionality will be limited in the MVP and finalized during architecture design.

ACT-003 — External AI Service

An optional external AI/LLM service may provide:

natural-language understanding
conversational responses
recommendation explanations
structured information extraction

External AI services must not become the sole source of truth for deterministic application logic.

ACT-004 — External Learning Resource Provider

External providers may supply learning resources such as:

courses
videos
documentation
articles

The exact integrations will be defined later.

5. Functional Requirements
5.1 Authentication
FR-001 — User Registration

The system shall allow a new learner to create an account.

Required information may include:

name
email
password

The final registration schema will be defined in DOC-07.

FR-002 — User Login

The system shall allow registered learners to authenticate securely.

FR-003 — Session Management

The system shall maintain authenticated user sessions using a secure authentication mechanism.

FR-004 — Logout

The system shall allow a learner to terminate their authenticated session.

5.2 Learner Profile
FR-005 — Profile Creation

The system shall allow a learner to create a learning profile.

FR-006 — Profile Update

The learner shall be able to update their profile.

FR-007 — Experience Level

The profile shall support an experience-level representation.

Possible values may include:

Beginner
Intermediate
Advanced
Professional

The final values will be finalized in DOC-07.

FR-008 — Current Skills

The system shall allow learners to specify skills they already possess.

FR-009 — Interests

The system shall allow learners to specify areas of interest.

FR-010 — Learning History

The system shall maintain relevant completed learning activities.

Examples:

completed courses
completed projects
completed assessments
FR-011 — Learning Preferences

The system may capture learning preferences such as:

preferred resource type
estimated weekly learning time
difficulty preference
preferred learning format
5.3 Goal Management
FR-012 — Goal Creation

The learner shall be able to define one or more learning goals.

FR-013 — Natural-Language Goal

The learner shall be able to describe a goal using natural language.

Example:

"I want to become a Machine Learning Engineer."
FR-014 — Goal Editing

The learner shall be able to modify an existing goal.

FR-015 — Goal Status

Goals shall support status tracking.

Possible states:

Active
Paused
Completed
Archived
5.4 Conversational Interface
FR-016 — Chat Interface

The system shall provide a conversational interface for learners.

FR-017 — Natural-Language Questions

The learner shall be able to ask questions about:

their learning path
skills
recommended resources
prerequisites
progress
next steps
FR-018 — Context Awareness

The AI assistant should consider relevant learner context when responding.

Relevant context may include:

active goal
current skills
completed resources
progress
current milestone
FR-019 — Recommendation Explanation

The assistant shall be able to explain why a learning resource was recommended.

5.5 Skill Management
FR-020 — Skill Representation

The system shall represent learner skills in a structured format.

FR-021 — Skill Levels

Skills should support proficiency levels.

Example:

0 — Not known
1 — Beginner
2 — Basic
3 — Intermediate
4 — Advanced
5 — Expert

The final scoring system will be finalized in the AI/ML and database documents.

FR-022 — Required Skills

The system shall identify skills associated with a target goal.

FR-023 — Skill Gap

The system shall compare:

Current learner skills
        VS
Required goal skills

and identify skill gaps.

FR-024 — Skill Gap Severity

The system should classify skill gaps according to their importance or deficiency.

5.6 Learning Resource Management
FR-025 — Resource Representation

Learning resources shall contain structured metadata.

Potential metadata:

title
description
URL
provider
resource type
difficulty
duration
skills
prerequisites
rating
language
FR-026 — Resource Types

The system should support multiple resource categories.

Examples:

Course
Video
Article
Documentation
Project
Assessment
FR-027 — Resource Search

The system shall be capable of retrieving resources relevant to a learner's goal or skill gap.

FR-028 — Resource Filtering

Resources may be filtered according to:

skill
difficulty
type
duration
provider
learner preference
5.7 Recommendation Engine
FR-029 — Personalized Recommendations

The system shall generate recommendations based on learner context.

The recommendation process should consider:

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
+
Progress
FR-030 — Candidate Generation

The system shall identify a set of candidate learning resources before final ranking.

FR-031 — Recommendation Ranking

The system shall rank candidate resources according to relevance.

FR-032 — Goal Alignment

Recommendations shall be relevant to the learner's active goal.

FR-033 — Skill Gap Alignment

Recommendations should address identified skill gaps.

FR-034 — Personalization

Two learners with different profiles may receive different recommendations for the same goal.

FR-035 — Recommendation Reasons

Each recommendation should have machine-readable reasoning information that can be used to explain the recommendation.

5.8 Prerequisite System
FR-036 — Skill Dependencies

The system shall support relationships between skills.

Example:

Python
   ↓
NumPy
   ↓
Machine Learning
FR-037 — Resource Prerequisites

Learning resources may specify prerequisite skills or resources.

FR-038 — Prerequisite Validation

The system shall consider prerequisite relationships when constructing learning paths.

FR-039 — Prerequisite Ordering

The generated learning path should respect prerequisite dependencies wherever applicable.

5.9 Learning Path Generation
FR-040 — Path Generation

The system shall generate a personalized learning path for an active goal.

FR-041 — Path Stages

A learning path shall contain logically ordered stages.

Example:

Foundation
Core Skills
Applied Skills
Projects
Advanced Topics
Assessment
FR-042 — Milestones

The system shall support milestones within a learning path.

FR-043 — Learning Activities

A path may contain:

courses
videos
articles
projects
assessments
FR-044 — Estimated Effort

The system should provide an estimated learning effort where sufficient data is available.

FR-045 — Next Action

The system shall identify the recommended next learning action.

FR-046 — Path Regeneration

The system shall be capable of generating an updated path when significant learner information changes.

5.10 Explainable Recommendations
FR-047 — Recommendation Explanation

The system shall explain why a resource is relevant.

FR-048 — Skill Explanation

The system should explain what skill a resource develops.

FR-049 — Sequence Explanation

The system should explain why an item appears at a particular point in the roadmap.

FR-050 — Gap Explanation

The system should explain why a skill is considered a gap.

5.11 Progress Tracking
FR-051 — Activity Status

Learning activities shall support statuses such as:

Not Started
In Progress
Completed
Skipped
FR-052 — Completion Tracking

The system shall record completed learning activities.

FR-053 — Milestone Progress

The system shall calculate milestone completion.

FR-054 — Overall Progress

The system shall calculate overall learning-path progress.

FR-055 — Skill Development

The system should reflect progress toward relevant skills.

5.12 Feedback
FR-056 — Resource Feedback

The learner shall be able to provide feedback about a recommendation.

FR-057 — Recommendation Feedback

The learner may indicate whether a recommendation was useful.

Possible signals:

Useful
Not Useful
Too Easy
Too Difficult
Already Known
Not Relevant
FR-058 — Path Feedback

The learner should be able to provide feedback on the generated learning path.

5.13 Adaptive Recommendation
FR-059 — Feedback-Based Adaptation

The system shall use relevant learner feedback to influence future recommendations.

FR-060 — Progress-Based Adaptation

The system shall consider learner progress when selecting future recommendations.

FR-061 — Difficulty Adaptation

The system should adjust recommended difficulty based on learner performance where sufficient evidence exists.

FR-062 — Re-ranking

The system shall be capable of re-ranking recommendations when learner state changes.

5.14 Dashboard
FR-063 — Learner Dashboard

The system shall provide a dashboard showing the learner's current learning state.

FR-064 — Progress Visualization

The dashboard shall display learning progress.

FR-065 — Skill Visualization

The dashboard should display relevant skill development.

FR-066 — Milestone Visualization

The dashboard shall display milestone status.

FR-067 — Next Recommendation

The dashboard shall display the learner's recommended next action.

6. AI/ML Requirements
AI-001 — Natural-Language Understanding

The system shall process natural-language learner goals.

AI-002 — Goal Information Extraction

The system should extract useful structured information from learner goals.

Possible outputs:

Target Role
Domain
Required Skills
Experience Expectations
Learning Objective
AI-003 — Semantic Matching

The recommendation engine should support semantic matching between learner needs and learning resources.

AI-004 — Skill Extraction

The system should identify relevant skills from learner input and learning-resource metadata.

AI-005 — Skill Gap Reasoning

The AI layer shall support identification of missing or insufficient skills.

AI-006 — Recommendation Ranking

The AI/recommendation layer shall rank resources according to learner relevance.

AI-007 — Explainability

The AI layer shall provide understandable recommendation explanations.

AI-008 — Adaptive Personalization

The AI/recommendation system should use learner state changes to improve future recommendations.

AI-009 — AI Fallback

The system should degrade gracefully if an external AI service is unavailable.

Critical deterministic functionality must remain operational where possible.

7. Data Requirements
DATA-001 — Learner Data

The system shall store learner information required for personalization.

DATA-002 — Goal Data

The system shall store learner goals.

DATA-003 — Skill Data

The system shall maintain structured skill information.

DATA-004 — Skill Relationship Data

The system shall support prerequisite/dependency relationships between skills.

DATA-005 — Resource Data

The system shall store learning-resource metadata.

DATA-006 — Learning Path Data

The system shall store generated learning paths.

DATA-007 — Progress Data

The system shall store learner progress.

DATA-008 — Feedback Data

The system shall store relevant learner feedback.

8. User Experience Requirements
UX-001 — Simple Onboarding

A new learner should be able to begin personalization without excessive setup.

UX-002 — Clear Goal Entry

The goal-input experience should support natural-language input.

UX-003 — Understandable Roadmap

The generated learning path must be understandable without requiring technical knowledge of the recommendation system.

UX-004 — Clear Next Action

The interface should clearly identify what the learner should do next.

UX-005 — Explainability Visibility

Recommendation explanations should be accessible from the learning-path interface.

UX-006 — Responsive Interface

The application should provide a usable experience across supported screen sizes.

9. Security Requirements
SEC-001 — Password Security

Passwords shall never be stored in plaintext.

SEC-002 — Authentication

Protected resources shall require authentication.

SEC-003 — Authorization

Users shall only access resources they are authorized to access.

SEC-004 — Secret Management

API keys, database credentials, JWT secrets, and other secrets shall not be committed to Git.

SEC-005 — Input Validation

User-controlled input shall be validated before processing.

SEC-006 — Error Safety

Internal implementation details and secrets shall not be exposed through API error responses.

10. Integration Requirements
INT-001 — Frontend/API Contract

Frontend communication with backend services shall use documented API contracts.

INT-002 — Database Access

Application services shall access the database through the defined backend data-access layer.

INT-003 — AI Service

AI services shall be accessed through an abstraction layer rather than being tightly coupled to UI components.

INT-004 — External Resources

External resource integrations shall have defined timeout and failure behavior.

11. Non-Functional Requirements
NFR-001 — Maintainability

The codebase shall use modular components and clearly defined responsibilities.

NFR-002 — Reliability

Critical user workflows should handle expected failures gracefully.

NFR-003 — Performance

Common API operations should return within an acceptable interactive response time under prototype-scale load.

Exact performance targets will be established during architecture and testing.

NFR-004 — Scalability

The architecture should allow future expansion of:

learners
resources
skills
recommendation volume

without requiring a complete architectural rewrite.

NFR-005 — Testability

Critical business logic shall be independently testable.

NFR-006 — Reproducibility

A developer should be able to reproduce the application using the repository documentation and defined dependencies.

NFR-007 — Observability

Important application failures should produce useful logs without exposing secrets.

NFR-008 — Documentation

Critical APIs, configuration, architecture, setup procedures, and workflows shall be documented.

12. Error Handling Requirements
FR-068 — Invalid Input

The system shall return meaningful validation feedback for invalid user input.

FR-069 — Empty Recommendations

The system shall provide a graceful response when no suitable recommendation is available.

FR-070 — AI Failure

If an AI service fails, the application shall display a controlled fallback response.

FR-071 — External API Failure

External resource failures shall not crash the complete application.

FR-072 — Database Failure

Database failures shall produce controlled application errors.

13. MVP Priority Classification

Requirements will use the following priority levels.

Priority	Meaning
P0	Mandatory for MVP/demo
P1	Important
P2	Enhancement
P3	Future

Initial priority allocation:

Requirement Area	Priority
Authentication	P0
Learner Profile	P0
Goal Management	P0
Conversational Interface	P0
Skill Management	P0
Skill Gap Analysis	P0
Recommendation Engine	P0
Prerequisite System	P0
Learning Path Generation	P0
Explainability	P0
Progress Tracking	P0
Feedback	P1
Adaptive Recommendation	P1
Dashboard	P0
Administration	P2
Advanced integrations	P2
14. Primary End-to-End Requirement

The most important system requirement is the complete learner journey.

FR-E2E-001 — Personalized Learning Journey

The system shall allow a learner to:

Register
   ↓
Create Profile
   ↓
Define Goal
   ↓
Provide Current Skills
   ↓
Analyze Skill Gap
   ↓
Generate Recommendations
   ↓
Generate Learning Path
   ↓
Understand Recommendations
   ↓
Track Progress
   ↓
Provide Feedback
   ↓
Receive Updated Recommendations

The complete workflow must be demonstrable in the final prototype.

15. Requirement Traceability Foundation

Initial traceability:

Requirement	Related Capability
FR-001–FR-004	Authentication
FR-005–FR-011	Learner Profiling
FR-012–FR-015	Goal Management
FR-016–FR-019	Conversational Assistant
FR-020–FR-024	Skill Intelligence
FR-025–FR-028	Resource Management
FR-029–FR-035	Recommendation Engine
FR-036–FR-039	Prerequisite Engine
FR-040–FR-046	Learning Path Generator
FR-047–FR-050	Explainability
FR-051–FR-055	Progress
FR-056–FR-058	Feedback
FR-059–FR-062	Adaptation
FR-063–FR-067	Dashboard
AI-001–AI-009	AI/ML
SEC-001–SEC-006	Security
INT-001–INT-004	Integrations
NFR-001–NFR-008	Quality Attributes
16. Requirements That Must Be Resolved Later

The following decisions are intentionally deferred.

R-DEC-001

Exact technology stack.

Defined in DOC-14.

R-DEC-002

Exact database schema.

Defined in DOC-07.

R-DEC-003

Exact API endpoints.

Defined in DOC-08.

R-DEC-004

Exact AI/ML algorithms.

Defined in DOC-09.

R-DEC-005

Frontend architecture.

Defined in DOC-10.

R-DEC-006

Backend architecture.

Defined in DOC-11.

R-DEC-007

Deployment platform.

Defined in DOC-19.

17. Requirement Acceptance Rules

A requirement is considered implementation-ready only when it has:

unique identifier
clear description
defined actor
defined expected behavior
testable outcome
known dependencies
defined priority

Requirements that cannot be tested must be refined before implementation.

18. SRS Completion Criteria

DOC-02 is considered ready for approval when:

all major user capabilities are identified
functional requirements have unique IDs
AI requirements are identified
security requirements are defined
data requirements are defined
integration requirements are defined
non-functional requirements are defined
MVP priorities are established
the end-to-end learner journey is defined
unresolved architectural decisions are explicitly documented
19. Document Status

Status: DRAFT

Parent: DOC-01 Project Charter

Next Document: DOC-03 User Roles and Journeys

Implementation Status: NOT STARTED

Architecture Status: NOT FROZEN

20. Change Control

Changes to this document after approval must identify:

changed requirement
reason
affected architecture
affected API
affected database
affected implementation
affected tests
affected GitHub issues

All approved changes must be reflected in DOC-23 Requirements Traceability Matrix.