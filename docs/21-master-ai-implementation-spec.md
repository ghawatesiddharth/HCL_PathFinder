DOC-21 — Master AI Implementation Specification

File: docs/21-master-ai-implementation-spec.md
Document ID: DOC-21
Project: PathFinder AI
Competition: HCLTech Round 2 — PathFinder Prototype
Team: AlgoX
Version: 1.0
Status: DRAFT
Parent Documents: DOC-01 through DOC-20
Implementation Owner: Siddharth Ghawate
Implementation Model: Single-developer implementation with documentation reflecting the complete product architecture

1. Purpose

This document is the master implementation specification for the artificial intelligence and personalization capabilities of PathFinder AI.

It translates the previously defined requirements, architecture, database design, API contracts, frontend architecture, backend architecture, security model, technology stack, deployment architecture, and integration requirements into an implementation-level AI specification.

The purpose of this document is to ensure that:

AI functionality is implemented consistently across the application.
AI components use the database and API contracts already defined.
Recommendation logic remains explainable.
Deterministic application logic is not unnecessarily delegated to an external LLM.
The system can operate with an AI-service failure fallback.
The implementation remains suitable for the HCLTech Round 2 prototype.
Every major AI capability can be demonstrated during the final presentation.
Future changes can be implemented without redesigning the complete application.
2. Relationship With Previous Documents

DOC-21 depends on the previously established project documents.

Document	Relationship
DOC-01	Project vision and objectives
DOC-02	Functional and AI requirements
DOC-03	User roles and journeys
DOC-04	Use cases and user stories
DOC-05	MVP scope and acceptance criteria
DOC-06	System architecture
DOC-07	Database design
DOC-08	API contract
DOC-09	ML/AI architecture
DOC-10	Frontend architecture
DOC-11	Backend architecture
DOC-12	Authentication and security
DOC-13	Code and folder architecture
DOC-14	Technology stack and dependencies
DOC-15	Environment and configuration
DOC-16	Git/GitHub workflow
DOC-17	Development task breakdown
DOC-18	Testing and QA
DOC-19	Deployment architecture
DOC-20	Integration checklist

DOC-21 does not replace these documents.

It provides the implementation-level coordination between them.

3. AI Product Objective

The AI subsystem shall transform learner information into personalized and explainable learning recommendations.

The core transformation is:

Learner Profile
      +
Learning Goal
      +
Current Skills
      +
Learning History
      +
Preferences
      +
Progress
      +
Feedback
      ↓
Learner State Representation
      ↓
Goal Understanding
      ↓
Required Skill Identification
      ↓
Skill Gap Analysis
      ↓
Candidate Resource Retrieval
      ↓
Candidate Scoring
      ↓
Prerequisite Validation
      ↓
Learning Path Construction
      ↓
Explanation Generation
      ↓
Personalized Learning Experience
4. Core Design Principle

PathFinder AI shall use a hybrid AI architecture.

The system shall not depend entirely on an external generative AI model.

The recommended architecture is:

                ┌──────────────────────┐
                │     Learner Input    │
                └──────────┬───────────┘
                           ↓
                ┌──────────────────────┐
                │ Goal Understanding   │
                │      AI / NLP        │
                └──────────┬───────────┘
                           ↓
                ┌──────────────────────┐
                │ Structured Learner   │
                │       State          │
                └──────────┬───────────┘
                           ↓
                ┌──────────────────────┐
                │ Skill Gap Engine     │
                │ Deterministic + AI   │
                └──────────┬───────────┘
                           ↓
                ┌──────────────────────┐
                │ Resource Retrieval   │
                └──────────┬───────────┘
                           ↓
                ┌──────────────────────┐
                │ Recommendation       │
                │ Ranking Engine        │
                └──────────┬───────────┘
                           ↓
                ┌──────────────────────┐
                │ Prerequisite Engine  │
                └──────────┬───────────┘
                           ↓
                ┌──────────────────────┐
                │ Path Generator       │
                └──────────┬───────────┘
                           ↓
                ┌──────────────────────┐
                │ Explanation Layer    │
                └──────────┬───────────┘
                           ↓
                ┌──────────────────────┐
                │ Personalized Path    │
                └──────────────────────┘
5. AI Components

The implementation shall contain the following logical AI components.

AI-COMP-001 — Goal Understanding

Responsible for converting natural-language learner goals into structured information.

Example:

Input:

"I want to become a machine learning engineer
and I currently know basic Python."

Expected structured representation:

{
  "target_role": "Machine Learning Engineer",
  "domain": "Artificial Intelligence",
  "known_skills": [
    "Python"
  ],
  "experience_level": "Beginner",
  "learning_objective": "Become job-ready in machine learning"
}
6. AI-COMP-002 — Skill Extraction

The system shall identify skills from:

learner goals
learner profile
learning resources
course descriptions
project descriptions
assessment metadata

Examples:

Python
SQL
Statistics
NumPy
Pandas
Machine Learning
Deep Learning
Natural Language Processing
Computer Vision
FastAPI
React
Git
Docker

Skills shall be normalized before being used by downstream components.

7. Skill Normalization

Different textual representations may refer to the same skill.

Examples:

Python programming
Python
Python language
Python 3

may be normalized to:

Python

Similarly:

Machine Learning
ML
Machine-learning
Machine learning techniques

may map to:

Machine Learning

Normalization shall occur before skill matching.

8. Skill Representation

Each skill shall have a structured representation.

Example:

{
  "skill_id": "skill_python",
  "name": "Python",
  "category": "Programming",
  "description": "General-purpose programming language",
  "level": 1
}

The exact database fields shall follow DOC-07.

The AI implementation must not introduce a conflicting schema.

9. Skill Proficiency Representation

The implementation shall use the proficiency model established by the project requirements.

0 = Not Known
1 = Beginner
2 = Basic
3 = Intermediate
4 = Advanced
5 = Expert

Example:

{
  "skill": "Python",
  "level": 2
}

means the learner has basic Python knowledge.

10. Goal Representation

A learner goal shall be represented using structured fields.

Example:

{
  "goal": "Become a Machine Learning Engineer",
  "target_role": "Machine Learning Engineer",
  "domain": "Artificial Intelligence",
  "experience_level": "Beginner",
  "time_available_per_week": 10
}

The implementation shall preserve the learner's original natural-language goal.

Both representations are useful:

Raw Goal
+
Structured Goal
11. Goal Understanding Pipeline

The pipeline shall follow:

Natural Language Goal
        ↓
Text Cleaning
        ↓
Skill / Role Extraction
        ↓
Entity Normalization
        ↓
Goal Classification
        ↓
Required Skill Mapping
        ↓
Structured Goal
12. Goal Understanding Example

Input:

I want to become a data analyst.
I know Excel and basic Python.
I have about 8 hours per week.

Output:

{
  "target_role": "Data Analyst",
  "known_skills": [
    "Excel",
    "Python"
  ],
  "experience_level": "Beginner",
  "weekly_hours": 8
}

Required skills may then be retrieved from the project's skill knowledge base.

13. Required Skill Mapping

Each target role shall map to a collection of required skills.

Example:

Data Analyst
│
├── Excel
├── SQL
├── Statistics
├── Python
├── Pandas
├── Data Visualization
└── Power BI / Tableau

Another example:

Machine Learning Engineer
│
├── Python
├── Mathematics
├── Statistics
├── NumPy
├── Pandas
├── Machine Learning
├── Model Evaluation
├── Deep Learning
├── Deployment
└── MLOps

The exact mappings shall be maintained in the project database/resource knowledge base.

14. AI-COMP-003 — Skill Gap Engine

The Skill Gap Engine compares:

Required Skills
        VS
Learner Skills

and calculates deficiencies.

Example:

Required:

Python              3
Statistics          3
Machine Learning    3
SQL                 2

Learner:

Python              2
Statistics          1
Machine Learning    0
SQL                 0

Gap:

Python              1
Statistics          2
Machine Learning    3
SQL                 2
15. Skill Gap Formula

A basic deterministic gap score can be calculated as:

gap = required_level - current_level

with:

gap < 0 → 0

Therefore:

gap = max(required_level - current_level, 0)

Example:

Required = 4
Current = 2

Gap = 4 - 2
Gap = 2
16. Skill Gap Severity

The system may classify gap severity as:

0 → No Gap
1 → Low
2 → Moderate
3 → High
4+ → Critical

Example:

{
  "skill": "Machine Learning",
  "required_level": 4,
  "current_level": 0,
  "gap": 4,
  "severity": "Critical"
}
17. Skill Gap Priority

Skill gaps shall not all have equal recommendation priority.

Priority should consider:

Gap Size
+
Goal Importance
+
Prerequisite Importance
+
Resource Availability
+
Learner Context

A skill that is a prerequisite for several downstream skills should receive higher priority.

18. AI-COMP-004 — Resource Retrieval

The Resource Retrieval Engine shall identify learning resources relevant to:

target goal
missing skills
learner level
preferred resource type
available learning time
prerequisite state

Resources may include:

Course
Video
Article
Documentation
Project
Assessment
19. Resource Metadata

Resources should contain structured metadata such as:

{
  "resource_id": "resource_001",
  "title": "Python for Data Analysis",
  "description": "Learn Python concepts required for data analysis.",
  "resource_type": "Course",
  "difficulty": "Beginner",
  "duration_hours": 8,
  "skills": [
    "Python",
    "Pandas"
  ],
  "prerequisites": [
    "Basic Programming"
  ],
  "provider": "Example Provider",
  "url": "https://example.com/resource"
}

The actual schema shall follow DOC-07.

20. AI-COMP-005 — Semantic Matching

The recommendation system should support semantic similarity between:

Learner Need
        VS
Resource Description

Possible implementation approaches include:

TF-IDF
+
Keyword Matching
+
Cosine Similarity

for a lightweight prototype.

A stronger implementation may use:

Sentence Embeddings
+
Vector Similarity

The implementation should remain consistent with DOC-09 and DOC-14.

21. Hybrid Retrieval Strategy

The preferred retrieval pipeline is:

Candidate Filtering
        ↓
Keyword / Skill Matching
        ↓
Semantic Similarity
        ↓
Prerequisite Validation
        ↓
Personalization
        ↓
Ranking

This prevents irrelevant resources from being selected solely because their text is semantically similar.

22. Candidate Generation

Candidate generation shall first create a pool of potentially relevant resources.

Example:

Skill Gap:

SQL
Statistics
Python

Candidate resources:

SQL Basics
SQL Intermediate
Statistics Fundamentals
Statistics for Data Science
Python Basics
Python for Data Analysis
23. Candidate Filtering

Candidates may be filtered using:

Skill
Difficulty
Resource Type
Prerequisite
Duration
Language
Provider
Availability

Example:

If the learner is a beginner:

Advanced SQL Optimization

may be excluded unless prerequisite evidence indicates that it is appropriate.

24. Recommendation Scoring

Each candidate resource shall receive a relevance score.

A prototype scoring model may use:

Final Score =
Skill Match Score
+
Goal Alignment Score
+
Difficulty Fit Score
+
Semantic Similarity Score
+
Prerequisite Fit Score
+
Preference Score
+
Progress Adjustment
+
Feedback Adjustment

The individual components shall be normalized before combination.

25. Example Recommendation Formula

A prototype implementation may use:

Score =
0.30 × SkillMatch
+
0.20 × GoalAlignment
+
0.15 × SemanticSimilarity
+
0.15 × DifficultyFit
+
0.10 × PrerequisiteFit
+
0.05 × PreferenceFit
+
0.05 × FeedbackAdjustment

The exact weights are configurable.

They should not be hard-coded throughout the application.

26. Recommendation Configuration

Weights shall be maintained through configuration.

Example:

{
  "skill_match_weight": 0.30,
  "goal_alignment_weight": 0.20,
  "semantic_similarity_weight": 0.15,
  "difficulty_fit_weight": 0.15,
  "prerequisite_fit_weight": 0.10,
  "preference_fit_weight": 0.05,
  "feedback_weight": 0.05
}
27. Personalization

The same resource may receive different scores for different learners.

Example:

Learner A
Python = Advanced
SQL = Beginner
Learner B
Python = Beginner
SQL = Advanced

A Python resource should therefore rank differently for each learner.

This demonstrates true personalization rather than static recommendation.

28. AI-COMP-006 — Prerequisite Engine

The prerequisite engine ensures that recommendations follow logical learning dependencies.

Example:

Python
   ↓
NumPy
   ↓
Pandas
   ↓
Machine Learning

The system should avoid recommending advanced Machine Learning material before necessary foundations unless the learner already satisfies them.

29. Skill Dependency Graph

Skills may be represented as a directed graph.

Example:

Python
  │
  ├────→ NumPy
  │        │
  │        └────→ Pandas
  │                  │
  └──────────────────┴──→ Machine Learning

Graph relationships shall be stored according to DOC-07.

30. Dependency Validation

For each candidate:

Check prerequisites
        ↓
Are prerequisites satisfied?
       / \
     YES  NO
      ↓    ↓
   Accept  Find prerequisite resource
31. AI-COMP-007 — Learning Path Generator

The Learning Path Generator converts ranked resources and skill dependencies into a structured roadmap.

Example:

Phase 1 — Foundations
    ↓
Phase 2 — Core Skills
    ↓
Phase 3 — Applied Skills
    ↓
Phase 4 — Project
    ↓
Phase 5 — Assessment
32. Learning Path Structure

A generated path should contain:

Goal
Overview
Estimated Duration
Stages
Milestones
Resources
Projects
Assessments
Next Action

Example:

{
  "goal": "Become a Data Analyst",
  "estimated_weeks": 12,
  "stages": [
    {
      "name": "SQL Foundations",
      "resources": []
    }
  ]
}

The actual persisted structure shall follow DOC-07 and DOC-08.

33. Learning Path Ordering

The path ordering should consider:

prerequisites
learner skill gaps
difficulty progression
goal importance
resource relevance
learner preferences
estimated workload
34. Milestones

Milestones provide meaningful checkpoints.

Example:

Milestone 1
Python Fundamentals Completed

Milestone 2
SQL Fundamentals Completed

Milestone 3
Data Analysis Project Completed

Milestone 4
Final Assessment Completed
35. Next Action Engine

The system shall identify a single recommended next action wherever possible.

Example:

Recommended Next Action:

Complete "SQL Fundamentals — Module 3"

This should be visible in the dashboard.

36. AI-COMP-008 — Explanation Engine

Every major recommendation should have an explanation.

Example:

Why this is recommended:

You currently have beginner-level SQL skills,
and SQL is a required skill for your Data Analyst goal.
This resource covers the SQL fundamentals needed
before the next stage of your learning path.
37. Explanation Requirements

Explanations should answer:

Why this resource?
Why now?
What skill does it develop?
How does it support the goal?
Why is it placed here?
38. Explainability Data

The recommendation engine should produce machine-readable reason codes.

Example:

{
  "reasons": [
    {
      "type": "SKILL_GAP",
      "skill": "SQL",
      "message": "SQL is a major gap for the selected goal."
    },
    {
      "type": "PREREQUISITE",
      "message": "This resource prepares you for the next learning stage."
    }
  ]
}

This allows the frontend to display explanations without relying entirely on generated text.

39. LLM Usage

An external LLM may be used for:

Natural-language understanding
Conversational responses
Explanation generation
Goal interpretation
Learning guidance

The LLM should not be the sole authority for:

Authentication
Progress calculation
Database updates
Skill-level arithmetic
Prerequisite validation
Recommendation eligibility
Security decisions
40. LLM Abstraction Layer

The backend should communicate with an AI provider through an abstraction.

Example:

AIService
   │
   ├── generate_response()
   ├── extract_goal()
   ├── extract_skills()
   └── generate_explanation()

The rest of the application should not directly depend on a provider-specific SDK.

41. Provider Independence

The AI layer should allow future provider replacement.

Conceptually:

Application
    ↓
AI Service Interface
    ↓
Provider Adapter
    ↓
LLM Provider

This prevents provider-specific implementation from spreading across the backend.

42. Prompt Design

Prompts shall provide structured context.

A recommendation explanation prompt may contain:

Learner Goal
Current Skills
Skill Gaps
Selected Resource
Resource Skills
Prerequisites
Progress

The model should be instructed to produce concise and learner-friendly explanations.

43. Structured AI Output

Whenever possible, AI outputs should follow a structured format.

Example:

{
  "target_role": "Data Analyst",
  "skills": [
    "SQL",
    "Statistics",
    "Python"
  ],
  "experience_level": "Beginner"
}

Structured output reduces downstream parsing errors.

44. AI Validation

AI-generated structured data must be validated before being used.

Validation includes:

Required fields
Data types
Allowed values
Skill existence
Maximum lengths
Unknown entities

Invalid output shall trigger fallback handling.

45. Hallucination Control

The system shall minimize unsupported AI-generated information.

The AI should not invent:

course URLs
provider names
resource availability
course ratings
skill prerequisites
certifications

when these values are not present in the application's trusted data.

46. Retrieval-Grounded Responses

When answering questions about available learning resources, the AI assistant should use application data as the source of truth.

Preferred flow:

User Question
      ↓
Intent Detection
      ↓
Retrieve Relevant Application Data
      ↓
Provide Context to AI
      ↓
Generate Response
47. Example Chat Query

User:

Why should I learn SQL before Power BI?

System:

Retrieve:
SQL prerequisite relationships
Power BI resource metadata
Learner skill state

Then generate:

SQL is recommended first because it supports
working with structured datasets that you will later
visualize in Power BI.
48. AI Assistant Context

The conversational assistant may use:

User Profile
Active Goal
Current Skills
Skill Gaps
Current Path
Current Milestone
Completed Resources
Recent Feedback

Only relevant information should be included.

49. Conversation Memory

The system should distinguish:

Short-Term Conversation Context

from:

Persistent Learner State

Conversation history should not automatically become permanent learner data.

Only meaningful learner-state information should be persisted.

50. AI-COMP-009 — Adaptive Recommendation

The recommendation engine shall adapt based on:

Progress
Feedback
Completion
Performance
Difficulty signals
Changed goals
Changed skills
51. Feedback Signals

Supported feedback may include:

Useful
Not Useful
Too Easy
Too Difficult
Already Known
Not Relevant

These signals can modify future recommendation scores.

52. Feedback Adjustment

Example:

If the learner repeatedly marks advanced resources as:

Too Difficult

the system may reduce the ranking of similar advanced resources.

If the learner repeatedly selects:

Project-based resources

the system may increase the preference score for project resources.

53. Progress Adaptation

Example:

Before completion:

SQL = Beginner

After assessment:

SQL = Intermediate

The next recommendation cycle should use the updated state.

54. Recommendation Regeneration

A new path should be generated when significant learner state changes.

Triggers may include:

Goal changed
Major skill completed
Assessment result changed
Multiple recommendations rejected
Learning preference changed
Major feedback received
55. AI Pipeline — Complete Flow

The complete pipeline is:

1. Receive learner request
        ↓
2. Authenticate learner
        ↓
3. Load learner profile
        ↓
4. Load active goal
        ↓
5. Load current skills
        ↓
6. Understand goal
        ↓
7. Map goal to required skills
        ↓
8. Calculate skill gaps
        ↓
9. Retrieve candidate resources
        ↓
10. Filter candidates
        ↓
11. Calculate semantic relevance
        ↓
12. Apply personalization
        ↓
13. Validate prerequisites
        ↓
14. Rank resources
        ↓
15. Construct learning path
        ↓
16. Generate explanation
        ↓
17. Persist path
        ↓
18. Return structured response
56. Recommendation Request Flow
Frontend
   ↓
POST /recommendations
   ↓
Backend Controller
   ↓
Recommendation Service
   ↓
Learner Context Service
   ↓
Skill Gap Service
   ↓
Resource Retrieval
   ↓
Ranking Engine
   ↓
Explanation Service
   ↓
Database
   ↓
API Response
   ↓
Frontend

The actual endpoint path must follow DOC-08.

57. Learning Path Generation Flow
Goal
 ↓
Required Skills
 ↓
Skill Gaps
 ↓
Skill Dependencies
 ↓
Candidate Resources
 ↓
Resource Ranking
 ↓
Prerequisite Ordering
 ↓
Stage Construction
 ↓
Milestone Construction
 ↓
Path Persistence
 ↓
Frontend Visualization
58. Chat Flow
User
 ↓
Chat Interface
 ↓
Authenticated API
 ↓
Intent Detection
 ↓
Context Retrieval
 ↓
Relevant Data Retrieval
 ↓
AI Service
 ↓
Response Validation
 ↓
Chat Response
59. AI Failure Fallback

If the external AI provider fails:

AI Request
    ↓
Failure
    ↓
Fallback Logic

The application should continue to provide deterministic functionality where possible.

For example:

Skill gap analysis
Resource ranking
Prerequisite ordering
Progress tracking
Dashboard

should remain functional without an LLM.

60. Fallback Chat Response

If a conversational response cannot be generated:

"I'm temporarily unable to generate an AI response.
You can still view your learning path and recommendations."

The system must not expose:

API keys
provider errors
stack traces
internal URLs
database credentials
61. AI API Error Handling

AI-related failures should distinguish:

Timeout
Rate Limit
Authentication Error
Invalid Response
Provider Unavailable
Malformed Output

The backend should convert these into controlled application errors.

62. AI Timeout

AI requests should have a configured timeout.

Example:

AI_TIMEOUT_SECONDS=30

The exact configuration shall be managed through DOC-15 environment configuration.

63. AI Rate Limiting

Repeated AI requests should be controlled to prevent unnecessary provider usage.

Potential controls:

Per-user request limits
Request debouncing
Caching
Maximum prompt size
Maximum response size
64. Prompt Security

User-generated content must be treated as untrusted input.

The system should avoid allowing user content to override system-level AI instructions.

65. Prompt Injection Awareness

The assistant should not follow user instructions that attempt to expose:

system prompts
API credentials
internal configuration
private learner data
database structure
security information
66. Privacy

AI requests should contain only the learner information required for the requested operation.

Sensitive or unnecessary data should not be sent to external AI services.

67. Data Minimization

For a recommendation explanation, the AI may only require:

Goal
Relevant Skills
Skill Gap
Resource

It should not receive unrelated information such as:

Password
JWT
Internal identifiers
Database credentials
68. AI Logging

Logs may contain:

AI operation
Request ID
User ID reference where appropriate
Latency
Success/failure
Provider status

Logs should not contain:

Passwords
JWT tokens
API keys
Full sensitive learner data
69. Recommendation Auditability

For each recommendation, the system should retain enough information to understand why it was selected.

Example:

{
  "resource_id": "resource_001",
  "score": 0.86,
  "reasons": [
    "Addresses SQL skill gap",
    "Matches beginner difficulty",
    "Satisfies prerequisite state"
  ]
}
70. Recommendation Determinism

Given identical:

Learner State
Goal
Resource Dataset
Configuration

the deterministic ranking component should produce consistent results.

If randomness is introduced by an AI service, it should be controlled where practical.

71. Recommendation Diversity

The engine should avoid recommending several nearly identical resources.

Example:

SQL Course A
SQL Course B
SQL Course C
SQL Course D

may provide poor learning diversity.

The system should attempt to balance:

Courses
Videos
Projects
Assessments
Documentation

when appropriate.

72. Learning Resource Deduplication

Duplicate or near-duplicate resources should be detected where possible.

Potential signals:

Same URL
Same provider + title
High semantic similarity
Same resource identifier
73. Difficulty Matching

Difficulty should be matched against learner proficiency.

Example:

Learner = Beginner

Preferred:
Beginner
Beginner → Intermediate

Lower priority:
Advanced
Expert

Unless the learner explicitly requests advanced content.

74. Time Personalization

If the learner provides weekly learning time:

Weekly Time = 8 hours

the generated path should avoid unrealistic workloads.

Example:

Estimated Workload = 20 hours/week

should trigger an adjustment.

75. Learning Pace

The path generator should estimate:

Resource Duration
+
Weekly Availability

to calculate approximate completion time.

76. Example Pace Calculation
Total learning effort = 40 hours

Weekly availability = 10 hours

Estimated duration = 40 / 10

Estimated duration = 4 weeks

This is an estimate and should be presented as such.

77. Project Recommendations

Projects should be included where they strengthen practical skill development.

Example:

Learn Python
     ↓
Learn Pandas
     ↓
Data Analysis Project
     ↓
Assessment
78. Assessment Recommendations

Assessments may be used to verify skill development.

Possible assessment types:

Quiz
Coding Exercise
Project Review
Skill Test
Knowledge Check
79. Skill Verification

If an assessment indicates strong performance, the system may update the learner's effective skill level.

This update should be governed by deterministic application rules.

The LLM should not directly assign permanent proficiency levels without validation.

80. AI Confidence

Where AI extraction is used, the system may store confidence.

Example:

{
  "skill": "Python",
  "confidence": 0.94
}

Low-confidence extractions should be reviewed or treated conservatively.

81. Confidence Threshold

Example configurable thresholds:

>= 0.80 → High confidence
0.60–0.79 → Medium confidence
< 0.60 → Low confidence

These values are implementation parameters and may be adjusted during testing.

82. AI Evaluation Dataset

A small internal evaluation dataset should be created for the prototype.

It should contain examples such as:

Learner Goal
Expected Role
Expected Skills
Expected Experience Level

Example:

Input:
"I want to become a frontend developer."

Expected:
Role = Frontend Developer

Skills:
HTML
CSS
JavaScript
React
Git
83. Recommendation Evaluation Dataset

The team should prepare test cases containing:

Learner Profile
Goal
Expected Skill Gaps
Expected Top Recommendations
Expected Path Order

This enables regression testing.

84. AI Metrics

The prototype may evaluate:

Goal extraction
Accuracy
Precision
Recall
Skill extraction
Precision
Recall
F1-score
Recommendation
Precision@K
Recall@K
NDCG@K
User feedback
Recommendation acceptance rate
Recommendation rejection rate
85. Explainability Evaluation

Recommendations should be evaluated for:

Correctness
Relevance
Clarity
Consistency

An explanation must correspond to actual recommendation signals.

86. AI Testing Strategy

Tests shall cover:

Normal input
Empty input
Invalid input
Ambiguous goal
Unknown skill
Missing resource
AI provider failure
Malformed AI output
Database failure
87. Goal Extraction Tests

Example:

Input:
"I want to learn data science."

Expected:
Domain = Data Science

Another:

Input:
"I already know Python and want to learn machine learning."

Expected:
Known Skill = Python
Target = Machine Learning
88. Skill Gap Tests

Example:

Required:
Python = 3

Current:
Python = 1

Expected:
Gap = 2
89. Recommendation Tests

Given:

Goal = Data Analyst
Gap = SQL
Level = Beginner

a beginner SQL resource should rank above an advanced SQL optimization resource.

90. Prerequisite Tests

Given:

Python → Pandas → Machine Learning

Machine Learning should not appear before required prerequisites unless the learner already satisfies them.

91. Path Generation Tests

The generated path should:

Contain the active goal
Contain required stages
Respect prerequisites
Contain milestones
Provide next action
92. Feedback Tests

Input:

Too Difficult

Expected:

Future recommendations should reduce preference
for similar difficulty where appropriate.
93. AI Integration Test

The AI provider adapter should be independently testable.

Use mocks during automated tests.

Production provider calls should not be required for every test.

94. Cost Control

The system should minimize unnecessary external AI calls.

Strategies:

Cache repeated requests
Use deterministic logic first
Send concise context
Avoid repeated explanation generation
Use AI only where it adds value
95. Caching

Potential cache targets:

Goal interpretation
Resource embeddings
Repeated explanations
Static skill mappings

Caching must not cause stale learner-specific recommendations.

96. Embedding Strategy

If embeddings are used:

Resource Description
        ↓
Embedding Model
        ↓
Vector
        ↓
Vector Storage

Learner query:

Goal / Skill Need
        ↓
Embedding
        ↓
Similarity Search

The exact vector-storage mechanism shall follow the selected technology stack.

97. Prototype-Friendly AI Architecture

For the HCLTech prototype, implementation complexity should remain controlled.

Recommended priority:

Phase 1:
Deterministic skill matching

Phase 2:
Semantic similarity

Phase 3:
AI goal extraction

Phase 4:
AI explanations

Phase 5:
Adaptive recommendations

This ensures a working application even if advanced AI functionality is incomplete.

98. MVP AI Scope

The MVP must demonstrate:

Natural-language goal
        ↓
Goal understanding
        ↓
Skill extraction
        ↓
Skill gap
        ↓
Personalized recommendations
        ↓
Learning path
        ↓
Explanation
        ↓
Progress
99. Non-MVP AI Scope

Potential future enhancements:

Advanced collaborative filtering
Reinforcement learning
Knowledge graph expansion
Real-time external course crawling
Advanced learner modeling
Automated assessment generation
Voice assistant
Multilingual conversational learning

These are not required for the initial prototype.

100. Demo Scenario

The primary demo should use a realistic learner.

Example:

Goal:
"I want to become a Data Analyst."

Current Skills:
Python — Basic
Excel — Intermediate

Weekly Availability:
8 hours

Learning Preference:
Projects + Videos
101. Demo AI Flow

During the demonstration:

1. User enters goal.
2. AI understands goal.
3. System identifies required skills.
4. Existing skills are displayed.
5. Skill gaps are calculated.
6. Recommendations appear.
7. Personalized path is generated.
8. User opens recommendation explanation.
9. User completes an activity.
10. Progress changes.
11. Recommendation changes.
12. Dashboard reflects new state.
102. Demo Differentiation

A second learner profile should optionally demonstrate personalization.

Learner A
Python = Basic
SQL = Beginner
Learner B
Python = Advanced
SQL = Intermediate

The system should produce different learning paths.

This provides strong evidence for the personalization requirement.

103. AI Architecture Components

Recommended backend modules:

ai/
├── service.py
├── prompts.py
├── schemas.py
├── goal_parser.py
├── skill_extractor.py
├── embeddings.py
├── similarity.py
├── skill_gap.py
├── recommender.py
├── ranking.py
├── prerequisites.py
├── path_generator.py
├── explainer.py
├── adaptive.py
└── fallback.py

Actual folder placement must remain consistent with DOC-13.

104. Service Responsibilities
goal_parser.py

Responsible for:

Goal interpretation
Role extraction
Goal normalization
skill_extractor.py

Responsible for:

Skill extraction
Skill normalization
skill_gap.py

Responsible for:

Current skill vs required skill comparison
recommender.py

Responsible for:

Candidate generation
Recommendation orchestration
ranking.py

Responsible for:

Scoring
Sorting
Ranking
prerequisites.py

Responsible for:

Dependency validation
Prerequisite ordering
path_generator.py

Responsible for:

Stage generation
Milestones
Learning order
explainer.py

Responsible for:

Recommendation explanation
105. AI Data Flow
Database
   ↓
Learner Context
   ↓
AI Orchestration Service
   ├── Goal Parser
   ├── Skill Extractor
   ├── Skill Gap Engine
   ├── Resource Retriever
   ├── Ranking Engine
   ├── Prerequisite Engine
   ├── Path Generator
   └── Explanation Engine
   ↓
Persist Result
   ↓
API
   ↓
Frontend
106. API Integration

AI services shall be exposed through backend APIs defined in DOC-08.

Potential logical operations include:

Create/interpret goal
Generate recommendations
Generate learning path
Get skill gaps
Get explanation
Chat with assistant
Submit recommendation feedback
Regenerate path

The exact endpoint names must remain those defined in DOC-08.

107. Database Integration

AI services may interact with:

Learners
Goals
Skills
Learner Skills
Resources
Resource Skills
Prerequisites
Learning Paths
Path Items
Progress
Feedback

The exact table/entity names must follow DOC-07.

108. Frontend Integration

The frontend should consume AI results through APIs.

The frontend should not contain:

Recommendation scoring
Skill-gap calculation
LLM credentials
Prerequisite graph logic

These remain backend responsibilities.

109. Frontend AI Presentation

The UI should display:

AI-generated goal interpretation
Skill gaps
Recommendation cards
Recommendation explanations
Learning roadmap
Next action
Progress
110. Recommendation Card

Each recommendation card should ideally show:

Title
Resource Type
Difficulty
Estimated Duration
Relevant Skill
Match Score or Match Label
Why Recommended
Start / Open Action
111. Skill Gap UI

Example:

Your Skill Gaps

SQL                 ███████░░░  High
Statistics          █████░░░░░  Medium
Python              ██░░░░░░░░  Low
Data Visualization  ██████░░░░  High

The frontend should use the structured backend values rather than calculating them itself.

112. Learning Path UI

The roadmap should visually communicate:

Completed
    ↓
Current
    ↓
Upcoming
    ↓
Future

Each stage should expose:

Resources
Skills
Duration
Milestone
Status
113. Chat UI

The assistant should provide:

User message
AI response
Optional supporting recommendations
Optional related path action

Example:

User:
Why do I need SQL?

Assistant:
SQL is currently one of your largest skill gaps
for the Data Analyst goal. It is also a foundation
for working with structured datasets.
114. Chat-to-Action

Where appropriate, the assistant should provide actionable links/buttons.

Example:

Assistant:
I recommend starting with SQL Fundamentals.

[View Resource]
[Add to Path]

The actual action must use the existing backend APIs.

115. AI Security Boundary

The architecture must maintain:

Frontend
   ↓
Backend Security Boundary
   ↓
AI Service

The browser must never directly contain:

LLM API Key
Database Credentials
Internal Service Credentials
JWT Signing Secret
116. Configuration

AI configuration shall use environment variables or approved configuration mechanisms.

Potential variables:

AI_PROVIDER
AI_MODEL
AI_API_KEY
AI_TIMEOUT_SECONDS
AI_MAX_TOKENS
AI_TEMPERATURE

Actual names shall follow DOC-15.

117. Temperature

For deterministic extraction tasks, a low temperature should be preferred.

For example:

Goal extraction:
low randomness

For conversational explanations:

slightly higher flexibility

Exact settings should be determined during implementation testing.

118. Token Control

Prompts should avoid unnecessarily large context.

The system should send only relevant:

skills
resources
progress
goal information
119. Context Window Protection

Large resource datasets should not be sent directly to the LLM.

Instead:

Database
   ↓
Retrieval
   ↓
Top Relevant Resources
   ↓
AI Context
120. AI Orchestrator

A central orchestration service should coordinate AI operations.

Conceptually:

class AIOrchestrator:
    def understand_goal(...)
    def analyze_skills(...)
    def generate_recommendations(...)
    def generate_path(...)
    def explain_recommendation(...)
    def answer_question(...)

The exact implementation must follow DOC-13 and DOC-11.

121. Deterministic vs AI Responsibilities
Function	Preferred Implementation
Authentication	Deterministic
Authorization	Deterministic
Skill arithmetic	Deterministic
Progress calculation	Deterministic
Prerequisite validation	Deterministic
Goal extraction	AI-assisted
Semantic similarity	ML
Recommendation ranking	Hybrid
Explanation	AI-assisted
Chat	AI
Dashboard calculations	Deterministic
122. AI Decision Boundary

The following rule shall be followed:

AI may interpret and assist; deterministic application logic shall decide critical system state.

This prevents unpredictable AI behavior from corrupting application data.

123. Recommendation Explainability Rule

Every recommendation returned by the recommendation engine should have at least one deterministic reason.

Examples:

Addresses skill gap
Matches learner level
Matches preferred resource type
Satisfies prerequisite
Aligns with goal

AI-generated prose may then convert these reasons into natural language.

124. Recommendation Response Contract

Conceptually:

{
  "resource_id": "resource_001",
  "title": "SQL Fundamentals",
  "score": 0.91,
  "reasons": [
    {
      "type": "SKILL_GAP",
      "skill": "SQL"
    },
    {
      "type": "DIFFICULTY_MATCH",
      "value": "Beginner"
    }
  ],
  "explanation": "This resource is recommended because SQL is an important gap for your current goal."
}

The actual API response must follow DOC-08.

125. Path Response Contract

Conceptually:

{
  "goal_id": "goal_001",
  "estimated_duration_weeks": 12,
  "stages": [],
  "milestones": [],
  "next_action": {
    "resource_id": "resource_001"
  }
}

The actual response must follow DOC-08.

126. AI Observability

Important AI metrics should be logged:

Request count
Success count
Failure count
Latency
Provider errors
Fallback count
Recommendation generation time
127. Performance Targets

For prototype-scale operation:

Database operations:
fast and predictable

Deterministic recommendation:
interactive

AI request:
acceptable interactive latency

Dashboard:
fast enough for normal user interaction

Exact thresholds should follow DOC-18 testing targets.

128. Performance Optimization

Potential optimizations:

Database indexing
Resource pre-computation
Embedding caching
Result caching
Lazy loading
Batch operations
Prompt reduction
129. AI Versioning

Changes to:

Prompt
Model
Embedding model
Ranking weights
Skill mappings

should be versioned.

Example:

AI_CONFIG_VERSION=1.0

This assists reproducibility.

130. Recommendation Versioning

Generated learning paths should record the configuration/version used to create them where supported by the database design.

131. Reproducibility

A developer should be able to reproduce an AI result using:

Same learner state
Same resource dataset
Same configuration
Same model/provider
Same algorithm version

where deterministic reproduction is possible.

132. AI Documentation

Implementation documentation should include:

Model/provider
Prompt purpose
Input schema
Output schema
Fallback behavior
Scoring formula
Configuration
Testing strategy
Known limitations
133. Known AI Limitations

The prototype may have limitations such as:

Incomplete skill taxonomy
Limited resource dataset
Limited learner history
Limited feedback volume
External AI dependency
Potential semantic matching errors
Limited cold-start personalization

These limitations should be disclosed during the final presentation.

134. Cold Start

For a new learner with no history:

Profile
+
Goal
+
Self-declared skills
+
Preferences

shall provide enough information to generate an initial learning path.

135. New Learner Strategy

The first recommendation cycle should rely more heavily on:

Goal
Required Skills
Self-reported Skills
Experience Level
Preferences

As progress data accumulates, the system can incorporate:

Completion
Feedback
Assessment
Behavior
136. Returning Learner Strategy

For returning learners:

Historical progress
+
Completed resources
+
Feedback
+
Current skills
+
Current goal

should influence the next recommendation cycle.

137. Goal Change

If the learner changes:

Data Analyst

to:

Machine Learning Engineer

the system should regenerate:

Required Skills
Skill Gaps
Recommendations
Learning Path
Milestones
Next Action

without deleting historical learning activity.

138. Progress Preservation

Changing a goal should not automatically erase:

Completed courses
Completed projects
Skill evidence
Historical feedback

These may remain useful for future personalization.

139. Feedback Preservation

Feedback should be associated with the relevant recommendation/resource context where supported by DOC-07.

140. Recommendation Refresh

The frontend may provide:

Refresh Recommendations

but should avoid generating duplicate AI requests unnecessarily.

141. Regeneration Rules

A regeneration operation should:

Load latest learner state
Load latest goal
Recalculate skill gaps
Retrieve candidates
Re-rank
Rebuild path
Persist updated path
142. AI Implementation Phases
Phase 1 — Foundation

Implement:

Skill model
Goal model
Resource model
Skill gap logic
Phase 2 — Recommendation

Implement:

Candidate retrieval
Scoring
Ranking
Phase 3 — Path Generation

Implement:

Prerequisites
Stages
Milestones
Next action
Phase 4 — LLM

Implement:

Goal extraction
Explanations
Chat
Phase 5 — Adaptation

Implement:

Feedback
Progress-based adaptation
Re-ranking
143. Implementation Priority
Component	Priority
Skill Gap	P0
Recommendation	P0
Path Generator	P0
Prerequisites	P0
Explanation	P0
Goal Understanding	P0
Dashboard AI integration	P0
Chat	P0
Feedback adaptation	P1
Advanced semantic retrieval	P1
Advanced learner modeling	P2
144. Minimum Demonstrable AI

The final prototype must demonstrate:

Natural-language goal
        ↓
Structured interpretation
        ↓
Skill gap
        ↓
Personalized recommendation
        ↓
Learning roadmap
        ↓
Explanation
145. AI Acceptance Criteria

AI functionality is accepted when:

AC-AI-001
The system can interpret a natural-language learning goal.

AC-AI-002
The system identifies relevant target skills.

AC-AI-003
The system calculates learner skill gaps.

AC-AI-004
The system retrieves relevant resources.

AC-AI-005
The system ranks resources according to learner context.

AC-AI-006
The system respects prerequisites.

AC-AI-007
The system generates an ordered learning path.

AC-AI-008
The system explains recommendations.

AC-AI-009
The system tracks changes in learner state.

AC-AI-010
The system can adapt recommendations based on feedback.

AC-AI-011
The system provides controlled AI failure behavior.

AC-AI-012
The system does not expose AI credentials.
146. End-to-End AI Acceptance Test

Input:

Goal:
"I want to become a Data Analyst."

Current Skills:

Python = Basic
Excel = Intermediate

Weekly Time:

8 hours

Expected:

1. Goal recognized.
2. Data Analyst role identified.
3. Required skills retrieved.
4. Existing skills recognized.
5. Missing skills identified.
6. Skill gaps ranked.
7. Relevant resources retrieved.
8. Resources personalized.
9. Prerequisites respected.
10. Learning path generated.
11. Explanations available.
12. Progress can be tracked.
13. Feedback can influence future recommendations.
147. AI Quality Gate

Before integration with the frontend, the backend AI implementation should pass:

Goal extraction tests
Skill extraction tests
Skill gap tests
Recommendation tests
Prerequisite tests
Path generation tests
Explanation tests
Fallback tests
Security tests
API contract tests
148. Integration With DOC-18

AI tests shall be incorporated into the project's testing and QA strategy.

No AI module should be considered complete merely because it works manually.

It should have automated or repeatable validation where practical.

149. Integration With DOC-19

Deployment shall provide the configuration required by the AI service.

Production deployment must support:

AI provider configuration
Secure secrets
Database connectivity
API connectivity
Frontend/backend integration
150. Integration With DOC-20

The final integration checklist must verify:

AI API reachable
AI credentials configured
Database accessible
Resources loaded
Skills loaded
Recommendation engine operational
Path generation operational
Chat operational
Fallback operational
Frontend displays AI responses
151. Integration With Git Workflow

AI changes should be committed using focused commits.

Examples:

feat(ai): add skill gap engine

feat(ai): add recommendation ranking

feat(ai): add learning path generator

feat(ai): add goal extraction

feat(ai): add recommendation explanations
152. Code Review Requirements

Before merging AI functionality, verify:

No secrets
No hardcoded credentials
No duplicated scoring logic
API contract respected
Database schema respected
Tests included
Fallback included
Logging safe
153. AI Configuration Management

Do not hard-code:

API keys
Model credentials
Database credentials
Production endpoints

Use environment configuration defined in DOC-15.

154. Development Environment

Development AI configuration should be separate from production configuration.

Example:

.env
.env.example

The real .env file must not be committed.

155. Resource Dataset Strategy

The prototype should maintain a controlled resource dataset.

The dataset should include enough diversity to demonstrate:

Different goals
Different skills
Different difficulties
Different resource types
Different prerequisites
156. Seed Data

A seed process should create:

Skills
Skill relationships
Learning resources
Resource-skill mappings
Role-skill mappings

This allows the application to be reproduced consistently.

157. Seed Data Principle

Seed data is application data, not AI hallucination.

It should be explicitly defined and version-controlled where appropriate.

158. Resource URL Validation

Resource URLs should be validated before being presented to users.

The recommendation engine must not invent URLs.

159. External Resource Integration

If external resource providers are added later:

Provider Adapter
      ↓
Normalization
      ↓
Validation
      ↓
Application Resource Model

External provider formats must not leak into the frontend.

160. AI Resource Trust Boundary

External AI may recommend among application resources.

It should not independently invent external resources as authoritative application recommendations unless an explicit external-resource integration is implemented.

161. Security Principle

The AI subsystem must follow:

Least Privilege
+
Minimal Context
+
Validated Output
+
Controlled Access
+
Secure Configuration
162. Failure Scenarios

The implementation must handle:

AI provider unavailable
AI timeout
AI malformed JSON
Unknown skill
No matching resource
No prerequisite resource
Empty learner profile
Empty goal
Database unavailable
Invalid authentication
163. Graceful Degradation

If AI fails:

Goal
 ↓
Deterministic skill mapping
 ↓
Skill gap
 ↓
Recommendation
 ↓
Path

should continue where possible.

164. No-Recommendation Scenario

If no suitable resource exists:

No exact learning resource was found for this skill.

The system may suggest:

Broader resource category
Alternative learning activity

but must not fabricate a resource.

165. Unknown Goal Scenario

If the goal is unclear:

I need a little more information to personalize your path.
What role or skill are you aiming to learn?

The system should request clarification rather than inventing a goal.

166. Unknown Skill Scenario

If a skill is not in the knowledge base:

Unknown Skill
      ↓
Normalization attempt
      ↓
Alias lookup
      ↓
If unresolved:
flag for review / fallback
167. Recommendation Transparency

Users should be able to understand:

What was recommended
Why it was recommended
What skill it addresses
Where it fits in the path
168. AI UX Principle

AI should reduce learner confusion rather than create additional complexity.

The UI should avoid exposing technical concepts such as:

embedding vectors
cosine similarity
ranking weights
prompt tokens

unless shown in an administrator/debug interface.

169. User-Facing Language

Prefer:

Why this is recommended

instead of:

Similarity score = 0.873

Prefer:

This addresses your SQL skill gap.

instead of:

SkillMatchWeight = 0.30
170. Developer-Facing Diagnostics

Developer logs may include:

candidate count
ranking scores
reason codes
AI latency
fallback reason

but should not expose secrets.

171. AI Monitoring

During development, monitor:

Recommendation quality
AI response latency
AI failures
Fallback frequency
User feedback
172. Feedback Loop

The long-term architecture is:

Learner
   ↓
Recommendation
   ↓
Interaction
   ↓
Feedback
   ↓
Learner State Update
   ↓
Recommendation Re-ranking
   ↓
Improved Recommendation
173. Learning Loop

PathFinder AI should gradually become more personalized as learner information increases.

Initial Profile
      ↓
Initial Recommendation
      ↓
Learning Activity
      ↓
Progress
      ↓
Feedback
      ↓
Updated Learner Model
      ↓
Improved Recommendation
174. Final AI Architecture

The final logical architecture is:

┌──────────────────────────────────────────────┐
│                  FRONTEND                    │
│                                              │
│ Goal Input │ Chat │ Dashboard │ Roadmap      │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│                    API                       │
│                                              │
│ Auth │ Goals │ Skills │ Recommendations      │
│ Paths │ Progress │ Feedback │ Chat            │
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│             AI ORCHESTRATION                 │
│                                              │
│ Goal Parser                                  │
│ Skill Extractor                              │
│ Skill Gap Engine                             │
│ Resource Retrieval                           │
│ Semantic Matching                            │
│ Recommendation Ranking                       │
│ Prerequisite Engine                          │
│ Path Generator                               │
│ Explanation Engine                           │
│ Adaptive Engine                              │
│ AI Fallback                                  │
└──────────────────────┬───────────────────────┘
                       │
             ┌─────────┴──────────┐
             ▼                    ▼
┌─────────────────────┐   ┌───────────────────┐
│      DATABASE       │   │   EXTERNAL AI     │
│                     │   │                   │
│ Learners            │   │ LLM Provider      │
│ Goals               │   │ Embedding Model   │
│ Skills              │   │                   │
│ Resources           │   │                   │
│ Paths               │   │                   │
│ Progress            │   │                   │
│ Feedback            │   │                   │
└─────────────────────┘   └───────────────────┘
175. Final Implementation Rule

PathFinder AI shall be implemented as a hybrid personalized learning system rather than a simple chatbot.

The core value proposition is:

UNDERSTAND
    ↓
ANALYZE
    ↓
RECOMMEND
    ↓
SEQUENCE
    ↓
EXPLAIN
    ↓
TRACK
    ↓
ADAPT

The AI layer must remain integrated with the learner profile, skill graph, resource database, recommendation engine, learning-path generator, progress system, feedback mechanism, backend API, frontend dashboard, security architecture, testing strategy, and deployment architecture already defined in DOC-01 through DOC-20.

176. Document Completion Criteria

DOC-21 is considered complete when:

AI components are defined
AI responsibilities are defined
AI/deterministic boundaries are defined
Goal understanding is defined
Skill extraction is defined
Skill gap analysis is defined
Recommendation logic is defined
Prerequisite logic is defined
Learning path generation is defined
Explanation generation is defined
Adaptive recommendation is defined
Fallback behavior is defined
AI security is defined
AI testing is defined
AI deployment requirements are defined
Frontend integration is defined
Backend integration is defined
Database integration is defined
End-to-end AI flow is defined
177. Document Status

Document: DOC-21 — Master AI Implementation Specification

Status: DRAFT

Implementation Status: NOT STARTED

AI Architecture Status: DEFINED

Parent Documents: DOC-01 through DOC-20

Next Document: DOC-22 — Architecture Decision Records

Final Documentation: DOC-23 — Requirements Traceability Matrix

178. Change Control

Any change to the AI architecture must identify:

Changed AI component
Reason for change
Affected requirement
Affected API
Affected database
Affected frontend
Affected backend
Affected tests
Affected deployment configuration
Affected documentation

Changes must remain consistent with the complete PathFinder AI architecture.

179. Final Cross-Document Consistency Rule

No AI implementation decision shall silently contradict:

DOC-02 Requirements
DOC-05 MVP Scope
DOC-07 Database Design
DOC-08 API Contract
DOC-09 ML/AI Architecture
DOC-10 Frontend Architecture
DOC-11 Backend Architecture
DOC-12 Security
DOC-13 Code Architecture
DOC-14 Technology Stack
DOC-15 Environment Configuration
DOC-18 Testing
DOC-19 Deployment
DOC-20 Integration

If an implementation change requires modifying one of these documents, the corresponding document shall be updated before or alongside the implementation change.