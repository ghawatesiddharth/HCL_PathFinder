# PathFinder AI — Project Charter

**Project:** PathFinder AI — Personalized Learning Path Recommender
**Competition:** HCLTech Round 2 — PathFinder Prototype
**Institution:** Sanjivani University, Kopargaon
**Team:** AlgoX
**Team Size:** 5
**Document ID:** DOC-01
**Version:** 1.0
**Status:** DRAFT — Architecture Foundation
**Date:** 26 August 2026

---

# 1. Document Purpose

This Project Charter establishes the foundational definition of the PathFinder AI project.

It defines:

* the problem being solved
* the product vision
* project objectives
* target users
* high-level solution
* project scope
* major capabilities
* success criteria
* assumptions
* constraints
* stakeholders
* project governance
* documentation governance
* architectural principles

This document is the foundation for all subsequent project specifications.

All later documents must remain consistent with the decisions established here.

---

# 2. Project Identity

## 2.1 Project Name

**PathFinder AI**

## 2.2 Project Type

AI-powered personalized learning path recommendation platform.

## 2.3 Competition Context

The project is being developed for the HCLTech Round 2 — PathFinder Prototype challenge.

The challenge requires an intelligent learning assistant capable of understanding learner goals, profiling learners, recommending learning resources, generating structured learning paths, explaining recommendations, and adapting recommendations using learner progress and feedback.

---

# 3. Official Problem Statement

Online learning platforms provide thousands of courses and learning resources across different domains. However, learners often struggle to determine:

* what they should learn
* where they should begin
* which skills they are missing
* which courses should be taken first
* what prerequisites are required
* which projects will reinforce their learning
* whether their current learning path is appropriate for their goal

A generic recommendation system may suggest individually relevant courses without considering the complete sequence required to achieve a specific learning or career objective.

PathFinder AI addresses this problem by creating a personalized, prerequisite-aware learning roadmap based on the learner's goals, current skills, experience, interests, learning history, preferences, progress, and feedback.

---

# 4. Problem Definition

## 4.1 Core Problem

A learner may know the destination but not the route.

For example:

> "I want to become a Machine Learning Engineer."

The learner may not know:

```text
What do I already know?
        ↓
What skills am I missing?
        ↓
What should I learn first?
        ↓
What are the prerequisites?
        ↓
Which resources should I use?
        ↓
What project should I build?
        ↓
How do I measure progress?
        ↓
What should I do next?
```

PathFinder AI converts this ambiguous goal into a structured, personalized learning journey.

---

# 5. Product Vision

The vision of PathFinder AI is:

> **To act as an intelligent learning companion that understands a learner's destination, determines their current position, identifies the gap between the two, and continuously guides them through the most appropriate learning path.**

The product should move beyond simple course recommendation and provide:

```text
Goal Understanding
        ↓
Learner Profiling
        ↓
Skill Assessment
        ↓
Skill Gap Analysis
        ↓
Prerequisite Reasoning
        ↓
Resource Recommendation
        ↓
Learning Path Generation
        ↓
AI Explanation
        ↓
Progress Tracking
        ↓
Feedback
        ↓
Adaptive Re-planning
```

---

# 6. Product Mission

PathFinder AI will help learners transform a broad learning or career goal into an actionable roadmap consisting of:

* skills
* learning resources
* courses
* projects
* assessments
* milestones
* progress indicators
* next recommended actions

The system should explain **why** each recommendation is made rather than presenting unexplained recommendations.

---

# 7. Target Users

## 7.1 Primary User — Learner

A student or professional who wants to achieve a specific learning or career goal.

Examples:

* Become a Data Analyst
* Become a Machine Learning Engineer
* Learn Full-Stack Development
* Learn Cloud Computing
* Prepare for a specific technical role
* Transition into a new technology domain

---

## 7.2 Secondary User — Mentor/Reviewer

A mentor, teacher, or evaluator may inspect the learner's generated roadmap and progress.

Mentor functionality is not necessarily part of the initial MVP and will be evaluated during requirements analysis.

---

# 8. Core User Scenario

A learner enters:

> "I want to become a Data Analyst. I know basic Python and Excel but I have never worked with SQL or Power BI."

PathFinder AI should be capable of interpreting the goal and profile, identifying the learner's existing capabilities, determining missing skills, and generating an ordered roadmap.

Conceptually:

```text
Goal:
Data Analyst

Current Skills:
Python
Excel

Missing Skills:
SQL
Statistics
Data Visualization
Power BI

Prerequisites:
Statistics → Visualization
SQL → Data Analysis
Power BI → Dashboarding

Generated Path:

1. SQL Fundamentals
2. SQL for Data Analysis
3. Statistics for Data Analytics
4. Data Cleaning
5. Data Visualization
6. Power BI
7. Analytics Project
8. Capstone Assessment
```

The actual recommendation algorithm will be defined in the AI/ML Architecture document.

---

# 9. Core Product Capabilities

The system will provide six foundational capabilities corresponding to the competition requirements.

## CAP-001 — Conversational Goal Understanding

The learner can describe their goal using natural language.

Example:

```text
"I want to become an ML engineer within the next year."
```

The system should extract relevant information such as:

* target role
* domain
* skills
* experience
* constraints
* interests
* learning preferences where available

---

## CAP-002 — Learner Profiling

The system maintains a learner profile containing relevant information such as:

* current skills
* experience level
* interests
* completed courses
* learning objectives
* learning history
* progress
* preferences
* feedback

---

## CAP-003 — Personalized Recommendation

The system recommends resources based on the learner rather than simply popularity.

Recommendations may include:

* courses
* videos
* articles
* documentation
* projects
* assessments

The recommendation architecture will be defined in `09-ml-ai-architecture.md`.

---

## CAP-004 — Prerequisite-Aware Learning Path

The system generates an ordered roadmap.

The ordering should consider:

```text
Skill dependencies
        +
Prerequisites
        +
Learner's current skills
        +
Target skills
```

The roadmap should contain milestones and meaningful learning stages.

---

## CAP-005 — Explainable AI Assistant

The system should explain:

* why a resource was recommended
* why it appears at a particular position
* what skill it develops
* what prerequisite it satisfies
* how it contributes to the learner's goal
* what the learner should do next

---

## CAP-006 — Progress and Adaptive Recommendations

The dashboard should display:

* completed learning items
* current milestone
* skill development
* roadmap progress
* pending activities
* recommended next action

The system should use learner progress and feedback to adapt future recommendations.

---

# 10. High-Level Product Flow

```text
                    LEARNER
                       │
                       ▼
              Goal / Conversation
                       │
                       ▼
              Learner Profile
                       │
                       ▼
              Skill Assessment
                       │
                       ▼
              Skill Gap Analysis
                       │
                       ▼
          Candidate Resource Retrieval
                       │
                       ▼
           Recommendation & Ranking
                       │
                       ▼
          Prerequisite-Aware Ordering
                       │
                       ▼
           Personalized Learning Path
                       │
                       ▼
             AI Explanation Layer
                       │
                       ▼
               Learner Dashboard
                       │
                       ▼
             Progress / Feedback
                       │
                       └───────────────┐
                                       │
                                       ▼
                             Adaptive Re-ranking
```

---

# 11. Project Objectives

## OBJ-001 — Personalization

Generate learning paths based on individual learner characteristics.

## OBJ-002 — Goal Alignment

Ensure recommendations are relevant to the learner's stated objective.

## OBJ-003 — Skill Gap Identification

Identify skills required for the target goal that are missing or insufficient.

## OBJ-004 — Prerequisite Reasoning

Ensure learning resources are ordered according to skill dependencies.

## OBJ-005 — Explainability

Provide understandable reasons for recommendations.

## OBJ-006 — Adaptability

Update recommendations based on progress and feedback.

## OBJ-007 — Usability

Provide a clear and intuitive learner experience.

## OBJ-008 — Demonstrable AI

Use meaningful AI/ML techniques rather than presenting a static rule-based course list.

---

# 12. MVP Definition

The Minimum Viable Product will focus on demonstrating the complete personalized learning cycle.

The MVP must support:

```text
MVP-001
Learner registration/login

MVP-002
Learner profile creation

MVP-003
Natural-language learning goal input

MVP-004
Skill/profile capture

MVP-005
Skill gap identification

MVP-006
Personalized resource recommendation

MVP-007
Prerequisite-aware learning path generation

MVP-008
AI explanation of recommendations

MVP-009
Learning progress tracking

MVP-010
Learner feedback

MVP-011
Adaptive next recommendation

MVP-012
Progress dashboard
```

The detailed acceptance criteria will be defined in Document 05.

---

# 13. Innovation Direction

The project should differentiate itself from a conventional course recommender.

The proposed innovation direction is:

## "Goal-to-Roadmap Intelligence"

Instead of asking:

> "Which course is similar to this course?"

PathFinder AI asks:

> "What does this learner need to learn next to reach their target?"

This requires reasoning across:

```text
Learner
   +
Goal
   +
Current Skills
   +
Required Skills
   +
Prerequisites
   +
Learning Resources
   +
Progress
   +
Feedback
```

---

# 14. AI/ML Direction

The project will use a layered intelligence approach.

The final algorithms will be selected during the AI/ML architecture phase.

The conceptual pipeline is:

```text
Natural Language Goal
        ↓
Goal/Skill Extraction
        ↓
Semantic Representation
        ↓
Learner Skill Representation
        ↓
Skill Gap Detection
        ↓
Candidate Resource Retrieval
        ↓
Relevance Scoring
        ↓
Prerequisite Graph Ordering
        ↓
Personalization
        ↓
Explanation
        ↓
Feedback-Based Adaptation
```

The implementation must distinguish between:

* deterministic business logic
* graph-based reasoning
* machine-learning components
* generative AI components

This separation is important for reliability and explainability.

---

# 15. High-Level System Boundary

PathFinder AI will contain the following conceptual subsystems:

```text
┌─────────────────────────────────────────────┐
│              PathFinder AI                  │
│                                             │
│  ┌─────────────┐       ┌───────────────┐   │
│  │ Frontend    │◄─────►│ Backend API   │   │
│  └─────────────┘       └───────┬───────┘   │
│                                │           │
│        ┌───────────────────────┼────────┐  │
│        │                       │        │  │
│        ▼                       ▼        ▼  │
│  Profile Engine        Recommendation  AI │
│                           Engine         │ │
│        │                       │        │  │
│        └───────────┬───────────┘        │  │
│                    ▼                    │  │
│              Skill Graph                │  │
│                    │                    │  │
│                    ▼                    │  │
│                Database                 │  │
└─────────────────────────────────────────────┘
```

The exact architecture will be finalized in Document 06.

---

# 16. Scope

## 16.1 In Scope

The project includes:

* learner authentication
* learner profile
* goal capture
* conversational interaction
* skill representation
* skill-gap analysis
* course/resource recommendation
* learning path generation
* prerequisite relationships
* milestones
* projects
* assessments
* recommendation explanations
* progress tracking
* learner feedback
* adaptive recommendation
* dashboard
* AI/ML recommendation functionality

---

## 16.2 Potentially Out of MVP Scope

The following will not automatically be included:

* complete course delivery platform
* video hosting
* payment processing
* enterprise LMS integration
* instructor management
* real-time multiplayer learning
* certification issuance
* advanced mentor administration
* mobile native applications

These may be considered future scope if they do not improve the Round 2 prototype score sufficiently.

---

# 17. Success Criteria

The project will be considered successful when a new learner can complete the following journey:

```text
Register
   ↓
Create Profile
   ↓
Describe Goal
   ↓
Provide Current Skills
   ↓
Receive Skill Gap
   ↓
Receive Personalized Roadmap
   ↓
Understand Recommendation Reasons
   ↓
Start Learning
   ↓
Update Progress
   ↓
Provide Feedback
   ↓
Receive Updated Recommendation
```

The complete flow should be demonstrable during the competition demo.

---

# 18. Competition-Oriented Success Metrics

The project should optimize for the judging categories.

## Problem Understanding & Solution Design — 20%

Demonstrate:

* clear problem definition
* logical architecture
* complete learner journey
* well-defined system boundaries
* meaningful personalization

## Functionality & Feature Completeness — 25%

Demonstrate:

* working end-to-end flow
* recommendations
* roadmap
* explanations
* dashboard
* feedback
* adaptation

## AI/ML Implementation — 20%

Demonstrate:

* meaningful AI/ML pipeline
* semantic understanding
* recommendation/ranking
* skill-gap reasoning
* explainability
* adaptive behavior

## Innovation & Creativity — 15%

Demonstrate:

* goal-to-roadmap intelligence
* prerequisite reasoning
* adaptive learning
* learner-specific explanations
* useful learning projects/assessments

## User Experience — 10%

Demonstrate:

* intuitive onboarding
* conversational interaction
* understandable roadmap
* visual progress
* clear next action

## Performance & Code Quality — 10%

Demonstrate:

* modular architecture
* maintainable code
* validation
* testing
* efficient APIs
* reproducible setup
* clean Git history

---

# 19. Team

The current team consists of five members:

| Member                    | Email                                                             | Project Role    |
| ------------------------- | ----------------------------------------------------------------- | --------------- |
| Siddharth Ramnath Ghawate | [siddharthaghawate@gmail.com](mailto:siddharthaghawate@gmail.com) | To be finalized |
| Gayatri Balasaheb Autade  | [autadegayatri56@gmail.com](mailto:autadegayatri56@gmail.com)     | To be finalized |
| Shruti Yuvaraj Gawali     | [shruti.gawali2026@gmail.com](mailto:shruti.gawali2026@gmail.com) | To be finalized |
| Palak Atul Desai          | [desaipalak020@gmail.com](mailto:desaipalak020@gmail.com)         | To be finalized |
| Vivek Dnyaneshwar Shinde  | [vivekshinde.it@gmail.com](mailto:vivekshinde.it@gmail.com)       | To be finalized |

Technical ownership will be assigned in the Git/GitHub Development Workflow and Task Breakdown documents after the architecture is finalized.

---

# 20. Major Project Constraints

## CON-001 — Competition Deadline

Round 2 submissions close:

**31 August 2026 at 11:59 PM IST.**

Therefore, the architecture must prioritize a demonstrable MVP over unnecessary complexity.

## CON-002 — Team Size

The project is developed by a five-member team.

## CON-003 — Prototype Requirement

The system must be sufficiently complete to demonstrate the primary learner journey.

## CON-004 — Reproducibility

The source repository and setup instructions must allow evaluators to run the project.

## CON-005 — AI Credibility

AI/ML functionality must be demonstrable and explainable.

---

# 21. Assumptions

The following assumptions are provisional and may be refined in later documents:

* Learners can provide their current skills either manually or through conversational interaction.
* A structured learning-resource dataset can be assembled for the prototype.
* Learning resources can be represented using metadata such as title, skills, difficulty, duration, prerequisites, and resource type.
* A skill graph can represent prerequisite relationships.
* The recommendation system can combine semantic similarity with structured learner/resource information.
* Progress and feedback can be stored for adaptive recommendations.
* External APIs may be used where appropriate, subject to availability, reliability, licensing, and competition constraints.

---

# 22. Architectural Principles

The project will follow these principles.

## AP-001 — Contract First

Interfaces are defined before implementation.

## AP-002 — Separation of Concerns

Frontend, backend, AI/ML, database, and external integrations must have clear boundaries.

## AP-003 — Backend-Owned Intelligence

Core recommendation and path-generation logic should not be duplicated inside the frontend.

## AP-004 — Explainability

Recommendations should have traceable reasons.

## AP-005 — Reproducibility

The project must be reproducible from the GitHub repository.

## AP-006 — Configuration Separation

Environment-specific configuration must not be hard-coded.

## AP-007 — Security by Default

Secrets, credentials, and sensitive data must never be committed.

## AP-008 — Testability

Major services must be independently testable.

## AP-009 — Traceability

Requirements must map to implementation and testing.

## AP-010 — Incremental Integration

The system will be integrated incrementally rather than merged as one large final integration.

---

# 23. Documentation Governance

The following documents will collectively form the PathFinder AI specification.

```text
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
DOC-16 Git/GitHub Development Workflow
DOC-17 GitHub Task Breakdown
DOC-18 Testing and QA Strategy
DOC-19 Deployment Architecture
DOC-20 Integration Checklist
DOC-21 Master AI Implementation Specification
DOC-22 Architecture Decision Records
DOC-23 Requirements Traceability Matrix
```

---

# 24. Source-of-Truth Hierarchy

When contradictions occur, the following hierarchy applies:

```text
Official HCL Challenge Requirements
                ↓
Project Charter
                ↓
Software Requirements Specification
                ↓
Architecture Documents
                ↓
API / Database / AI Specifications
                ↓
Implementation
```

Implementation must not override an approved architectural decision without an Architecture Decision Record.

---

# 25. Change Management

After a document is marked **FROZEN**, changes require:

1. identification of the affected decision
2. explanation of the change
3. identification of dependent documents
4. update of affected documentation
5. update of the traceability matrix
6. implementation impact assessment
7. approval before implementation

This prevents silent architecture drift.

---

# 26. Existing Prototype Reuse Policy

A previous PathFinder prototype exists and may be used as a reference.

Existing prototype components may be:

* reused
* redesigned
* refactored
* replaced
* discarded

based on the new architecture.

The previous implementation is **not considered the architectural source of truth**.

The new specification defined in this documentation set takes precedence.

---

# 27. Prototype-to-Production Direction

The project will be rebuilt in layers:

```text
Phase 1
Requirements

        ↓

Phase 2
Architecture

        ↓

Phase 3
Data + AI Design

        ↓

Phase 4
Development Environment

        ↓

Phase 5
Backend Foundation

        ↓

Phase 6
AI/ML Engine

        ↓

Phase 7
Frontend

        ↓

Phase 8
Integration

        ↓

Phase 9
Testing

        ↓

Phase 10
Deployment

        ↓

Phase 11
Demo & Submission
```

---

# 28. Definition of Project Completion

PathFinder AI will not be considered complete merely because the frontend loads.

The MVP is complete only when:

* learner can authenticate
* learner can create/update profile
* learner can specify a goal
* system can identify relevant skills
* system can identify meaningful gaps
* system can recommend learning resources
* system can generate an ordered learning path
* prerequisites are respected
* recommendations have explanations
* learner can track progress
* learner can provide feedback
* recommendations can adapt
* dashboard displays meaningful progress
* backend APIs are documented
* database schema is documented
* tests exist for critical functionality
* project can be reproduced from the repository

---

# 29. Phase-1 Exit Criteria

Document 01 will be considered approved when the team agrees on:

* [ ] Problem definition
* [ ] Product vision
* [ ] Target user
* [ ] Core capabilities
* [ ] MVP direction
* [ ] High-level system boundary
* [ ] AI/ML direction
* [ ] Scope
* [ ] Success criteria
* [ ] Architectural principles
* [ ] Documentation governance

After approval, this document becomes **FROZEN VERSION 1.0**.

---

# 30. Dependencies

This document is the foundation for:

```text
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
DOC-16 Git/GitHub Development Workflow
DOC-17 GitHub Task Breakdown
DOC-18 Testing and QA Strategy
DOC-19 Deployment Architecture
DOC-20 Integration Checklist
DOC-21 Master AI Implementation Specification
DOC-22 Architecture Decision Records
DOC-23 Requirements Traceability Matrix
```

---

# 31. Document Status

**Current Status:** DRAFT

**Next Review:** Team review

**Next Document:** DOC-02 — Software Requirements Specification

**Freeze Required Before:** DOC-02 is finalized
