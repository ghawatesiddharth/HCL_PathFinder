# PathFinder AI — Frontend Architecture

**Document ID:** DOC-10  
**Project:** PathFinder AI  
**Competition:** HCLTech Round 2 — PathFinder Prototype  
**Team:** AlgoX  
**Version:** 1.0  
**Status:** DRAFT  
**Parent Documents:** DOC-01, DOC-02, DOC-03, DOC-04, DOC-05, DOC-06, DOC-07, DOC-08, DOC-09  
**Next Document:** DOC-11 Backend Architecture  

---

# 1. Purpose

This document defines the frontend architecture of PathFinder AI.

The purpose of this document is to establish a complete and implementation-ready frontend design before frontend coding begins.

The frontend architecture defines:

- frontend technology
- application structure
- page structure
- routing
- reusable components
- state management
- API communication
- authentication handling
- form handling
- error handling
- loading states
- AI chat interface
- learning-path visualization
- dashboard
- responsive behavior
- accessibility
- frontend security
- testing
- performance
- frontend-backend integration
- environment configuration
- coding standards

The frontend must implement the requirements defined in DOC-02 and the system architecture defined in DOC-06.

No frontend component should introduce business logic that conflicts with the backend, database, API, or AI architecture.

---

# 2. Architectural Principle

The frontend is responsible for:

```text
Presentation
User Interaction
Client-Side Validation
Navigation
UI State
API Communication
Visualization
User Feedback

The frontend is NOT responsible for:

Database Access
Authentication Secret Generation
Password Hashing
Recommendation Business Logic
Skill Gap Calculation
Learning Path Generation
LLM API Calls
Sensitive AI Provider Credentials
Administrative Database Operations

These responsibilities belong to backend and AI services.

3. Frontend Technology Stack

The MVP frontend shall use the following technology stack.

Layer	Technology
Language	TypeScript
Framework	React
Build Tool	Vite
Routing	React Router
HTTP Client	Axios
Styling	Tailwind CSS
UI Components	Reusable custom components
Forms	React Hook Form
Validation	Zod
Server State	TanStack Query
Client State	Zustand
Icons	Lucide React
Charts	Recharts
Testing	Vitest
Component Testing	React Testing Library
E2E Testing	Playwright
Code Quality	ESLint
Formatting	Prettier

The exact package versions must be frozen in:

package.json
package-lock.json

The versions used during implementation must be documented in DOC-14.

4. Frontend Responsibilities

The frontend shall provide the following major capabilities:

Authentication UI
Learner onboarding
Learner profile
Goal creation
Skill selection
Skill-level management
AI conversational interface
Skill-gap visualization
Learning-path visualization
Recommendation cards
Recommendation explanations
Progress tracking
Milestone tracking
Feedback submission
Dashboard
Error handling
Loading states
Responsive interface
Accessibility support
5. Frontend Application Architecture

The frontend follows a modular architecture.

Browser
   |
   v
React Application
   |
   +-----------------------------+
   |                             |
   v                             v
Pages                         Global UI
   |                             |
   v                             v
Features                    Components
   |
   v
Hooks
   |
   v
Services
   |
   v
API Client
   |
   v
Backend API

The frontend must never directly communicate with the database.

6. High-Level Frontend Architecture
+--------------------------------------------------+
|                   Browser                        |
+--------------------------------------------------+
                     |
                     v
+--------------------------------------------------+
|                React Application                 |
+--------------------------------------------------+
                     |
        +------------+------------+
        |                         |
        v                         v
+---------------+        +----------------+
| React Router  |        | Global State   |
+---------------+        +----------------+
        |                         |
        v                         v
+---------------------------------------------+
|                Page Components              |
+---------------------------------------------+
        |
        +-------------------+
        |                   |
        v                   v
+---------------+   +------------------------+
| Feature Layer |   | Shared Components      |
+---------------+   +------------------------+
        |
        v
+---------------------------------------------+
|              Custom Hooks                   |
+---------------------------------------------+
        |
        v
+---------------------------------------------+
|         API / Service Layer                 |
+---------------------------------------------+
        |
        v
+---------------------------------------------+
|              Backend API                    |
+---------------------------------------------+
7. Recommended Folder Structure

The frontend shall use the following structure.

frontend/
│
├── public/
│   ├── favicon.svg
│   └── assets/
│
├── src/
│   │
│   ├── app/
│   │   ├── App.tsx
│   │   ├── router.tsx
│   │   ├── providers.tsx
│   │   └── constants.ts
│   │
│   ├── assets/
│   │   ├── images/
│   │   ├── icons/
│   │   └── illustrations/
│   │
│   ├── components/
│   │   ├── common/
│   │   ├── layout/
│   │   ├── navigation/
│   │   ├── forms/
│   │   ├── feedback/
│   │   ├── loading/
│   │   └── error/
│   │
│   ├── features/
│   │   │
│   │   ├── auth/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   ├── types.ts
│   │   │   └── schemas.ts
│   │   │
│   │   ├── onboarding/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   ├── types.ts
│   │   │   └── schemas.ts
│   │   │
│   │   ├── profile/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   └── types.ts
│   │   │
│   │   ├── goals/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   ├── types.ts
│   │   │   └── schemas.ts
│   │   │
│   │   ├── skills/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   └── types.ts
│   │   │
│   │   ├── recommendations/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   └── types.ts
│   │   │
│   │   ├── learning-path/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   └── types.ts
│   │   │
│   │   ├── progress/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   └── types.ts
│   │   │
│   │   ├── feedback/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   └── types.ts
│   │   │
│   │   └── chat/
│   │       ├── components/
│   │       ├── hooks/
│   │       ├── services/
│   │       └── types.ts
│   │
│   ├── pages/
│   │   ├── LandingPage.tsx
│   │   ├── LoginPage.tsx
│   │   ├── RegisterPage.tsx
│   │   ├── OnboardingPage.tsx
│   │   ├── DashboardPage.tsx
│   │   ├── ProfilePage.tsx
│   │   ├── GoalsPage.tsx
│   │   ├── GoalDetailsPage.tsx
│   │   ├── LearningPathPage.tsx
│   │   ├── RecommendationsPage.tsx
│   │   ├── ChatPage.tsx
│   │   ├── ProgressPage.tsx
│   │   ├── NotFoundPage.tsx
│   │   └── ErrorPage.tsx
│   │
│   ├── hooks/
│   │   ├── useAuth.ts
│   │   ├── useDebounce.ts
│   │   ├── useLocalStorage.ts
│   │   └── useMediaQuery.ts
│   │
│   ├── lib/
│   │   ├── axios.ts
│   │   ├── queryClient.ts
│   │   ├── storage.ts
│   │   └── utils.ts
│   │
│   ├── services/
│   │   └── api/
│   │       ├── authApi.ts
│   │       ├── profileApi.ts
│   │       ├── goalsApi.ts
│   │       ├── skillsApi.ts
│   │       ├── recommendationsApi.ts
│   │       ├── learningPathApi.ts
│   │       ├── progressApi.ts
│   │       ├── feedbackApi.ts
│   │       └── chatApi.ts
│   │
│   ├── store/
│   │   ├── authStore.ts
│   │   ├── uiStore.ts
│   │   └── onboardingStore.ts
│   │
│   ├── types/
│   │   ├── api.ts
│   │   ├── auth.ts
│   │   ├── learner.ts
│   │   ├── goal.ts
│   │   ├── skill.ts
│   │   ├── resource.ts
│   │   ├── learningPath.ts
│   │   ├── progress.ts
│   │   └── recommendation.ts
│   │
│   ├── utils/
│   │   ├── formatters.ts
│   │   ├── validators.ts
│   │   └── errorHandlers.ts
│   │
│   ├── styles/
│   │   ├── globals.css
│   │   └── components.css
│   │
│   ├── main.tsx
│   └── vite-env.d.ts
│
├── .env.example
├── .gitignore
├── index.html
├── package.json
├── package-lock.json
├── tsconfig.json
├── vite.config.ts
├── eslint.config.js
├── prettier.config.js
└── README.md

This structure is authoritative for the MVP unless an Architecture Decision Record changes it.

8. Application Entry Point

The application entry point is:

frontend/src/main.tsx

Responsibilities:

initialize React
initialize providers
initialize router
initialize query client
mount application

It must not contain business logic.

9. App Component

The main application component is:

frontend/src/app/App.tsx

Responsibilities:

render the application shell
manage high-level layout
expose routing structure
render global UI elements
10. Provider Architecture

The application shall use centralized providers.

Recommended provider hierarchy:

React.StrictMode
    |
    v
QueryClientProvider
    |
    v
BrowserRouter
    |
    v
AuthProvider / Auth State
    |
    v
App

Providers must remain lightweight.

11. Routing Architecture

React Router shall be used.

The application shall define public and protected routes.

Public Routes
/
 /login
 /register
Protected Routes
/dashboard
/onboarding
/profile
/goals
/goals/:goalId
/learning-path/:pathId
/recommendations
/progress
/chat
12. Route Table
Route	Page	Authentication
/	LandingPage	Public
/login	LoginPage	Public
/register	RegisterPage	Public
/onboarding	OnboardingPage	Required
/dashboard	DashboardPage	Required
/profile	ProfilePage	Required
/goals	GoalsPage	Required
/goals/:goalId	GoalDetailsPage	Required
/learning-path/:pathId	LearningPathPage	Required
/recommendations	RecommendationsPage	Required
/progress	ProgressPage	Required
/chat	ChatPage	Required
*	NotFoundPage	Any
13. Protected Route

Protected routes must verify authentication.

Conceptual flow:

Request Protected Page
        |
        v
Check Authentication
        |
   +----+----+
   |         |
Authenticated Not Authenticated
   |         |
   v         v
Render      Redirect
Page        Login

The frontend must not treat client-side authentication checks as the final security boundary.

The backend must independently validate authentication.

14. Landing Page

The landing page introduces PathFinder AI.

It should communicate:

project purpose
personalized learning concept
key benefits
core workflow
call-to-action
login
registration

Primary CTA:

Build My Learning Path
15. Login Page

The login page shall contain:

email field
password field
login button
validation messages
loading state
authentication error
registration link

Validation:

Email must be valid.
Password must not be empty.

The frontend sends credentials through the authentication API.

16. Registration Page

The registration page shall contain:

name
email
password
confirm password
registration button

Validation:

Name required.
Email valid.
Password required.
Password confirmation must match.

Password requirements must match backend validation rules.

The frontend must never hash passwords as a replacement for backend password hashing.

17. Onboarding Architecture

The onboarding experience is one of the most important frontend workflows.

The onboarding flow should minimize friction.

Recommended sequence:

Step 1
Basic Information
        |
        v
Step 2
Experience Level
        |
        v
Step 3
Current Skills
        |
        v
Step 4
Interests
        |
        v
Step 5
Learning Preferences
        |
        v
Step 6
Career / Learning Goal
        |
        v
Generate Learning Path
18. Onboarding State

Onboarding state may temporarily contain:

name
experienceLevel
skills
interests
learningPreferences
goal

The state may be maintained through:

Zustand

or local component state where appropriate.

Sensitive information should not be unnecessarily persisted in local storage.

19. Goal Entry Interface

The goal input must support natural-language input.

Example:

I want to become a machine learning engineer

The interface may provide example prompts:

I want to become a Data Analyst.
I want to become a Full Stack Developer.
I want to learn Cloud Computing.
I want to become an AI Engineer.

The goal is sent to the backend AI pipeline.

20. Skill Selection Interface

Learners should be able to:

search skills
select skills
remove skills
assign proficiency
view selected skills

Example:

Python        Intermediate
SQL           Basic
Statistics    Beginner
Machine Learning   None
21. Skill Level UI

The UI should represent skill levels consistently.

Recommended:

0 = Not Known
1 = Beginner
2 = Basic
3 = Intermediate
4 = Advanced
5 = Expert

The backend remains the authoritative source for validation.

22. Dashboard Architecture

The dashboard is the primary authenticated landing page.

The dashboard shall show:

Welcome
Active Goal
Current Progress
Skill Gaps
Learning Path
Current Milestone
Recommended Next Action
Recent Activity
AI Assistant
23. Dashboard Layout

Recommended structure:

+--------------------------------------------------+
| Header                                           |
+--------------------------------------------------+
| Sidebar | Main Content                           |
|         |                                        |
| Home    | Welcome                                |
| Goals   | Active Goal                            |
| Path    | Progress                               |
| Skills  | Skill Gaps                             |
| Chat    | Next Action                            |
| Profile | Recommendations                       |
|         |                                        |
+--------------------------------------------------+

Mobile layouts may replace the sidebar with a bottom navigation or menu.

24. Dashboard Components

The dashboard should use reusable components.

Recommended:

DashboardHeader
GoalSummaryCard
ProgressCard
SkillGapCard
MilestoneCard
NextActionCard
RecommendationCard
RecentActivityList
QuickChatCard
25. Goal Summary Card

Displays:

goal title
goal status
target role/domain
overall progress
path duration estimate
current milestone

Example:

Goal:
Become a Machine Learning Engineer

Progress:
42%

Current Stage:
Core Machine Learning

Next:
Learn supervised learning
26. Progress Card

Displays:

percentage completion
completed activities
total activities
current milestone

Progress must be calculated using backend data.

Frontend should not independently calculate business-critical progress values unless explicitly defined by the API contract.

27. Skill Gap Card

Displays:

Skill Gap

Python
██████████ 90%

Statistics
██████░░░░ 60%

Machine Learning
██░░░░░░░░ 20%

The actual skill values must come from backend responses.

28. Next Action Card

The next-action card is a high-priority UI element.

It should answer:

What should I do next?

Example:

Next Recommended Action

Complete:
Introduction to Supervised Learning

Reason:
Your target role requires strong supervised-learning fundamentals.

Estimated Time:
3 hours

[Start Learning]
29. Learning Path Page

The learning path page visualizes the complete personalized roadmap.

Recommended structure:

Goal
 |
 v
Foundation
 |
 v
Core Skills
 |
 v
Applied Skills
 |
 v
Projects
 |
 v
Assessment
 |
 v
Advanced Topics
30. Learning Path Components

Recommended components:

LearningPathHeader
PathProgress
StageList
StageCard
MilestoneCard
LearningActivityCard
PrerequisiteBadge
CompletionBadge
RecommendationReason
NextActionCard
31. Learning Path Stage

Each stage should display:

stage name
description
progress
activities
estimated duration
completion status

Example:

Stage 2 — Core Machine Learning

Progress: 60%

1. Regression
2. Classification
3. Model Evaluation
4. Feature Engineering
32. Learning Activity Card

Each activity may contain:

Title
Type
Provider
Difficulty
Duration
Skills
Prerequisites
Status
Reason
Action

Example:

Machine Learning Fundamentals

Type: Course
Difficulty: Intermediate
Duration: 6 hours

Skills:
Machine Learning
Python

Status:
Not Started

[Open Resource]
33. Recommendation Card

Recommendation cards must make personalization visible.

Each card should include:

resource title
type
difficulty
duration
relevance
skills
recommendation reason
action
34. Recommendation Explanation

The frontend must display backend-generated explanation data.

Example:

Why this is recommended

This resource is recommended because:

1. Machine Learning is required for your target role.
2. Your current proficiency is below the target level.
3. You have already completed Python fundamentals.
4. This resource satisfies a prerequisite for your next milestone.

The frontend should not invent recommendation reasoning.

35. Chat Interface

The AI assistant is a core product feature.

The chat interface should support:

user messages
assistant responses
loading indicator
error handling
conversation history
contextual questions
recommendation questions
learning-path questions
36. Chat UI Structure
+---------------------------------------------+
| PathFinder AI Assistant                     |
+---------------------------------------------+
|                                             |
| User: What should I learn next?             |
|                                             |
| AI: Based on your current progress...       |
|                                             |
| User: Why this course?                      |
|                                             |
| AI: This course is recommended because...   |
|                                             |
+---------------------------------------------+
| Ask PathFinder AI...                [Send]  |
+---------------------------------------------+
37. Chat Context

The frontend may send contextual identifiers such as:

userId
goalId
learningPathId
conversationId

The backend should determine the authoritative learner context.

The frontend must not attempt to construct the complete AI context independently.

38. Chat Message Model

Frontend representation:

ChatMessage {
    id
    role
    content
    timestamp
}

Allowed roles:

user
assistant
system

System messages may remain hidden from normal users.

39. Chat Loading State

While waiting for an AI response:

AI is thinking...

The UI must prevent accidental duplicate submissions when appropriate.

40. Chat Error Handling

If AI service fails:

I couldn't process that request right now.
Please try again.

The UI may offer:

Retry

The actual error details must remain hidden from the learner.

41. Profile Page

The profile page shall allow users to view and update:

Name
Experience Level
Interests
Skills
Learning Preferences

The page should support:

View
Edit
Save
Cancel
42. Goals Page

The goals page should display:

Active Goals
Completed Goals
Paused Goals
Archived Goals

Each goal should have:

Goal title
Status
Progress
Created date
Updated date
43. Goal Details Page

The goal details page should display:

goal description
target skills
current skills
skill gaps
learning path
recommendations
progress
next action
44. Progress Page

The progress page shall provide deeper progress information.

It should show:

Overall Progress
Completed Activities
Current Stage
Completed Milestones
Skill Development
Learning Time
Recent Activity
45. Feedback Interface

Feedback should be simple.

Possible options:

Useful
Not Useful
Too Easy
Too Difficult
Already Known
Not Relevant

The user may optionally provide text feedback.

46. Feedback UX

After a recommendation:

Was this recommendation useful?

[👍 Useful] [👎 Not Useful]

Additional feedback may be shown after selecting an option.

47. API Communication Architecture

All backend communication must pass through the API service layer.

Incorrect:

Component
   |
   +--> axios.post(...)

Correct:

Component
   |
   v
Hook
   |
   v
API Service
   |
   v
Axios Client
   |
   v
Backend

This separation makes testing and maintenance easier.

48. Axios Client

A centralized Axios instance shall be created:

src/lib/axios.ts

Responsibilities:

base URL
request configuration
response handling
authentication behavior
common error handling
timeout
49. API Base URL

The frontend shall obtain the backend URL from environment configuration.

Example:

VITE_API_BASE_URL=http://localhost:8000/api/v1

Production will use the deployed backend URL.

The URL must never be hardcoded throughout components.

50. API Service Modules

API services shall be divided by feature.

authApi.ts
profileApi.ts
goalsApi.ts
skillsApi.ts
recommendationsApi.ts
learningPathApi.ts
progressApi.ts
feedbackApi.ts
chatApi.ts

Each module should expose typed functions.

Example conceptual structure:

login()
register()
logout()
getProfile()
updateProfile()
createGoal()
getGoals()
getGoal()
generateLearningPath()
getRecommendations()
submitFeedback()
sendChatMessage()
51. Server State Management

TanStack Query shall manage server state.

Use it for:

API responses
caching
refetching
loading state
mutations
invalidation

Examples:

useQuery
useMutation
queryClient.invalidateQueries()
52. Client State Management

Zustand shall be used only for appropriate client-side state.

Examples:

Authentication state
Onboarding state
UI preferences
Sidebar state
Temporary interface state

Server data must not be duplicated unnecessarily inside Zustand.

53. State Ownership Rules

Use:

React local state

for component-only state.

Use:

Zustand

for global client state.

Use:

TanStack Query

for server state.

This distinction prevents state-management conflicts.

54. Authentication State

Authentication state may contain:

isAuthenticated
user
accessToken

depending on the final backend authentication architecture.

Authentication implementation must follow DOC-12.

55. Token Storage

The final token-storage strategy must be consistent with DOC-12.

Preferred secure architecture:

HTTP-only secure cookie

where supported by the backend architecture.

If token-based browser storage is required for the prototype, the risks must be documented in DOC-12.

The frontend must never expose secrets in source code.

56. Form Architecture

Forms shall use:

React Hook Form
+
Zod

Example:

Form
 |
 v
React Hook Form
 |
 v
Zod Validation
 |
 v
API Mutation
57. Validation

Frontend validation should provide immediate feedback.

Examples:

Required field
Invalid email
Password mismatch
Invalid goal
Invalid skill level
Invalid duration

However, frontend validation does not replace backend validation.

58. Loading States

Every asynchronous operation should have a defined loading state.

Examples:

Loading dashboard...
Generating your learning path...
Finding recommendations...
Saving profile...
Sending message...
Updating progress...

Avoid blank screens during API operations.

59. Skeleton Loading

For major pages, skeleton UI should be preferred where appropriate.

Example:

Dashboard
----------------
████████████
██████░░░░░
████████░░░

Skeletons should resemble the final layout.

60. Empty States

The frontend must define empty states.

Examples:

No goals yet.
Create your first learning goal to begin.
No recommendations available.
Try updating your goal or skills.
No completed activities yet.
Start your first learning activity.
61. Error States

Each major feature should have a controlled error state.

Example:

Something went wrong.

We couldn't load your learning path.

[Try Again]

Technical stack traces must never be displayed.

62. API Error Mapping

Backend errors should be converted into user-friendly messages.

Example:

400
Invalid input

401
Please log in again

403
You don't have permission to perform this action

404
Requested resource not found

409
This item already exists

429
Too many requests

500
Something went wrong on the server
63. Global Error Boundary

The application shall use a React error boundary.

Purpose:

catch unexpected rendering errors
prevent complete blank screen
display recovery interface
log useful diagnostic information

Example:

Something went wrong.

[Return to Dashboard]
64. Notifications

The application may use toast notifications for short-lived events.

Examples:

Profile updated successfully.
Goal created.
Feedback submitted.
Learning activity completed.

Notifications should not replace important inline validation.

65. Responsive Design

The application must support:

Desktop
Tablet
Mobile

Recommended breakpoints should be defined centrally through Tailwind configuration.

The interface must remain usable without horizontal scrolling.

66. Mobile Navigation

On mobile:

Desktop Sidebar

may become:

Mobile Header
+
Bottom Navigation

or a collapsible menu.

Primary navigation should remain accessible.

67. Accessibility

The frontend should follow WCAG-oriented accessibility principles.

Requirements include:

semantic HTML
keyboard navigation
visible focus states
accessible form labels
accessible buttons
alt text for meaningful images
sufficient contrast
screen-reader-friendly status messages
no interaction dependent solely on color
68. Component Architecture

Components should be reusable and focused.

Bad:

Dashboard.tsx

containing thousands of lines.

Preferred:

DashboardPage
 |
 +-- DashboardHeader
 +-- GoalSummaryCard
 +-- ProgressCard
 +-- SkillGapCard
 +-- NextActionCard
 +-- RecommendationCard
69. Component Naming Convention

Use PascalCase for React components.

Examples:

GoalCard.tsx
SkillBadge.tsx
ProgressCard.tsx
ChatMessage.tsx
LearningPathStage.tsx
70. File Naming Convention

React components:

PascalCase.tsx

Hooks:

useSomething.ts

Services:

somethingApi.ts

Types:

something.ts

Utilities:

camelCase.ts
71. TypeScript Rules

The frontend shall use strict TypeScript.

Avoid:

any

unless there is a documented reason.

Prefer explicit interfaces and types.

72. API Types

API response types must be defined centrally or feature-wise.

Example:

ApiResponse<T>
ApiError
PaginatedResponse<T>

The frontend must match DOC-08 API contracts.

73. Example API Type

Conceptual:

interface Recommendation {
    id: string;
    title: string;
    description: string;
    resourceType: string;
    difficulty: string;
    durationMinutes?: number;
    skills: string[];
    recommendationReason: string;
}

Actual implementation must match DOC-08 and DOC-07.

74. Learning Path Type

Conceptual:

interface LearningPath {
    id: string;
    goalId: string;
    title: string;
    description: string;
    progress: number;
    stages: LearningPathStage[];
}

The backend remains authoritative.

75. UI Design Principles

The UI should follow these principles:

Simple
Clear
Modern
Professional
Action-oriented
Explainable
Responsive
Consistent

The interface should prioritize the learner's next action.

76. Visual Hierarchy

The most important information should be visually prominent.

Priority:

1. Goal
2. Progress
3. Skill gaps
4. Next action
5. Recommendations
6. Supporting information
77. Recommendation Transparency

AI recommendations must not feel like unexplained black boxes.

Every important recommendation should provide:

What?
Why?
How?
Next?

Example:

What:
Learn Python for Data Science

Why:
You need stronger Python skills for your target role.

How:
Complete this 4-hour course.

Next:
Practice with the attached project.
78. Dashboard Information Architecture

The dashboard should answer five questions:

Where am I going?
What do I know?
What am I missing?
What should I learn?
How am I progressing?
79. Navigation Model

Primary navigation:

Dashboard
My Goals
Learning Path
Recommendations
Progress
AI Assistant
Profile
80. Breadcrumbs

Breadcrumbs may be used on deeper pages.

Example:

Dashboard
 >
Goals
 >
Machine Learning Engineer
 >
Learning Path
81. Resource External Links

External learning resources should open safely.

Where appropriate:

target="_blank"
rel="noopener noreferrer"

The frontend must never trust arbitrary URL input without backend validation.

82. Search UX

Resource and skill searches should support:

debouncing
loading state
empty state
result count
keyboard accessibility

Search requests should not be sent on every keystroke.

83. Debouncing

Search input should use a debounce mechanism.

Example:

User types
    |
    v
Wait 300–500 ms
    |
    v
Send API request

The exact value may be configured during implementation.

84. Pagination

Large datasets should use pagination or controlled infinite loading.

Potential areas:

Resources
Recommendations
Learning history
Chat history

The API contract must define pagination behavior.

85. Caching

TanStack Query shall cache appropriate server responses.

Recommended caching candidates:

Profile
Skills
Goals
Learning Path
Recommendations

Highly dynamic data should use shorter stale times.

86. Query Invalidation

When mutations change server data, relevant queries must be invalidated.

Example:

Complete Activity
      |
      +--> invalidate progress
      +--> invalidate learning path
      +--> invalidate recommendations
87. Recommendation Refresh

When learner state changes significantly:

Profile Updated
      |
      v
Goal State Updated
      |
      v
Skill State Updated
      |
      v
Recommendations Refreshed

The frontend triggers appropriate backend APIs rather than performing recommendation logic locally.

88. Progress Update Flow

Recommended flow:

Learner clicks Complete
        |
        v
Frontend validation
        |
        v
POST/PATCH Progress API
        |
        v
Backend updates progress
        |
        v
Response
        |
        v
Invalidate progress/path queries
        |
        v
UI refresh
89. Feedback Flow
Learner selects feedback
        |
        v
Feedback component
        |
        v
Feedback API
        |
        v
Backend stores feedback
        |
        v
Success response
        |
        v
Recommendation state refreshed if required
90. Goal Creation Flow
Goal Form
    |
    v
Client Validation
    |
    v
Create Goal API
    |
    v
Backend
    |
    v
Goal Created
    |
    v
Generate / Request Learning Path
    |
    v
Display Result
91. Learning Path Generation UX

Learning path generation may take longer than normal CRUD operations.

The interface should display:

Understanding your goal...
Analyzing your skills...
Identifying skill gaps...
Selecting learning resources...
Building your roadmap...
Finalizing recommendations...

These are UI progress messages and should not falsely claim internal processing stages unless the backend exposes real status.

92. AI Generation Loading

For AI operations:

Generating personalized roadmap...

The user should understand that generation may take a few seconds.

93. Cancellation

Long-running frontend operations should support cancellation where technically appropriate.

Example:

Generate Path
       |
       v
[Generating...]
       |
       v
[Cancel]

Cancellation must not leave the backend in an inconsistent state.

94. Frontend Security

Frontend security requirements:

never commit secrets
never expose private API keys
validate user input
escape/render untrusted content safely
use secure authentication mechanisms
protect routes
avoid unsafe HTML rendering
use HTTPS in production
use secure external links
handle expired authentication gracefully
95. XSS Protection

Avoid:

dangerouslySetInnerHTML

unless absolutely necessary and content is sanitized.

AI-generated content should be treated as untrusted content.

96. AI Response Rendering

AI responses may contain:

plain text
markdown
lists
code snippets
links

If markdown rendering is implemented:

sanitize generated HTML
restrict unsafe elements
validate links
prevent script execution
97. Authentication Expiration

If an authenticated session expires:

API returns 401
      |
      v
Clear invalid auth state
      |
      v
Redirect to login
      |
      v
Show message:
"Your session has expired. Please log in again."
98. Unauthorized Access

If backend returns:

403 Forbidden

the frontend must display an appropriate permission message.

It must not attempt to bypass authorization.

99. Offline Behavior

The MVP does not require complete offline support.

However:

network failures must be detected
user should receive useful feedback
unsaved form data should not be silently lost where practical
100. Performance Requirements

The frontend should aim for:

fast initial load
efficient component rendering
optimized API requests
lazy-loaded pages where useful
optimized images
minimal unnecessary dependencies
101. Code Splitting

Large feature areas may be lazy-loaded.

Example:

Dashboard
Chat
Learning Path

can be dynamically imported when appropriate.

102. Image Optimization

Images should:

use appropriate dimensions
use modern formats where practical
avoid unnecessarily large files
provide alt text
103. Bundle Management

The project should periodically inspect bundle size.

Avoid adding large libraries when lightweight alternatives exist.

104. Logging

Frontend logging must not expose:

passwords
tokens
API keys
private user information

Development logs may be more verbose.

Production logs must be controlled.

105. Analytics

Analytics are not required for the MVP unless explicitly approved.

If analytics are added later, privacy implications must be documented.

106. Frontend Testing Strategy

Testing shall occur at multiple levels.

Unit Tests
Integration Tests
Component Tests
E2E Tests
107. Unit Testing

Unit tests should cover:

utility functions
formatters
validators
state logic
pure business presentation helpers

Framework:

Vitest
108. Component Testing

React Testing Library should test:

rendering
user interaction
form validation
loading states
error states
accessibility behavior

Avoid testing implementation details.

109. API Integration Testing

API-related frontend logic should test:

success
validation error
authentication error
server error
network failure

Mocking should be used where appropriate.

110. End-to-End Testing

Playwright should cover critical workflows.

Minimum E2E flows:

Registration
Login
Onboarding
Goal Creation
Learning Path Generation
Dashboard
Progress Update
Feedback
AI Chat
Logout
111. Critical E2E Journey

The most important test:

Register
   ↓
Login
   ↓
Complete Profile
   ↓
Enter Goal
   ↓
Select Skills
   ↓
Generate Path
   ↓
View Recommendations
   ↓
Open Learning Path
   ↓
Complete Activity
   ↓
View Progress
   ↓
Ask AI Assistant
   ↓
Submit Feedback

This workflow must remain functional before release.

112. Test Data

Frontend tests should use controlled test fixtures.

Test data must not depend on production users.

Example:

test learner
test goal
test skills
test resources
test learning path
113. Environment Configuration

Frontend environments:

Development
Testing
Production

Environment files may include:

.env
.env.example
.env.test

Actual production secrets must not be committed.

114. Required Frontend Environment Variables

Initial example:

VITE_API_BASE_URL=
VITE_APP_NAME=
VITE_APP_ENV=

Only values safe for browser exposure may use the VITE_ prefix.

No secret API keys should be exposed through frontend environment variables.

115. Development Environment

Example:

VITE_API_BASE_URL=http://localhost:8000/api/v1
VITE_APP_NAME=PathFinder AI
VITE_APP_ENV=development

Actual values must match backend configuration.

116. Production Environment

Example:

VITE_API_BASE_URL=https://api.example.com/api/v1
VITE_APP_NAME=PathFinder AI
VITE_APP_ENV=production

The actual deployment URL will be finalized in DOC-19.

117. Frontend Build

Development:

npm install
npm run dev

Production build:

npm run build

Preview:

npm run preview

Exact scripts must be defined in package.json.

118. Recommended package.json Scripts
{
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "preview": "vite preview",
    "lint": "eslint .",
    "format": "prettier --write .",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:e2e": "playwright test"
  }
}

The final package configuration must be verified during implementation.

119. ESLint

ESLint shall enforce:

TypeScript quality
React rules
unused variables
common mistakes
consistent coding practices

Linting must pass before merge.

120. Prettier

Prettier shall be used for formatting.

Developers should not manually maintain inconsistent formatting.

121. Git Frontend Rules

Frontend changes should use feature branches.

Example:

feature/frontend-auth
feature/frontend-dashboard
feature/frontend-learning-path
feature/frontend-chat

Changes should be merged through pull requests.

122. Frontend Commit Examples

Good:

feat(frontend): add login page
feat(frontend): add dashboard layout
feat(frontend): add learning path visualization
fix(frontend): handle expired authentication
test(frontend): add onboarding flow tests
123. Frontend Feature Ownership

Features should remain isolated.

Example:

features/goals

should contain goal-specific:

components
hooks
services
types
schemas

This prevents unrelated code from becoming tightly coupled.

124. Dependency Direction

Preferred dependency direction:

Pages
  |
  v
Features
  |
  v
Hooks
  |
  v
Services
  |
  v
API Client

Avoid:

API Service
   |
   v
UI Component
125. Circular Dependencies

Circular dependencies must be avoided.

Example of problematic architecture:

Component A
   |
   v
Component B
   |
   v
Component A

Shared logic should move to:

components/common
hooks
utils
types
126. Reusable UI Components

Common reusable components may include:

Button
Input
Textarea
Select
Checkbox
Radio
Modal
Dialog
Card
Badge
ProgressBar
Spinner
Skeleton
Alert
Toast
Dropdown
Tabs
Tooltip
127. Layout Components

Recommended:

AppLayout
DashboardLayout
AuthLayout
Header
Sidebar
MobileNavigation
Footer
PageContainer
128. Form Components

Recommended:

FormField
FormLabel
FormError
TextInput
PasswordInput
SkillSelector
GoalInput
ExperienceSelector
129. AI Components

Recommended:

ChatWindow
ChatMessage
ChatInput
TypingIndicator
SuggestedPrompt
RecommendationExplanation
AIErrorState
130. Learning Components

Recommended:

LearningPath
LearningPathStage
LearningActivity
Milestone
ResourceCard
PrerequisiteList
ProgressIndicator
131. Dashboard Components

Recommended:

DashboardHeader
GoalSummary
ProgressSummary
SkillGapSummary
MilestoneSummary
NextAction
RecommendationList
RecentActivity
132. Component Props

Props must be explicit and typed.

Avoid:

function Card(props: any)

Prefer:

interface CardProps {
    title: string;
    description?: string;
}
133. Component Responsibility

A component should ideally have one clear responsibility.

Example:

SkillGapCard

should display skill-gap information.

It should not:

call database
generate recommendations
calculate AI embeddings
modify backend state directly
134. Business Logic Boundary

Frontend business logic should be limited to presentation-related concerns.

Example allowed:

format duration
format percentage
display status
sort already-loaded UI data

Example not allowed:

calculate skill-gap intelligence
generate recommendation score
determine prerequisite graph
call LLM provider
135. Recommendation Score

If recommendation scores are returned by backend, frontend may display or transform them for visualization.

The frontend must not independently calculate the authoritative recommendation score.

136. Progress Score

The frontend should display backend-calculated progress.

If the backend returns:

progressPercentage: 72

the frontend displays:

72%

rather than independently calculating a conflicting value.

137. Date Handling

All timestamps should be handled consistently.

Backend timestamps should use a defined standard.

Frontend should format timestamps for the user's locale.

138. Duration Handling

Backend duration values should use a consistent unit.

Recommended:

minutes

Frontend may display:

180 minutes

as:

3 hours
139. Status Representation

Frontend status values must match backend API enums.

Example:

NOT_STARTED
IN_PROGRESS
COMPLETED
SKIPPED

Do not create incompatible frontend-only values.

140. Goal Status

Recommended API values:

ACTIVE
PAUSED
COMPLETED
ARCHIVED

UI may display:

Active
Paused
Completed
Archived
141. Accessibility for Chat

Chat must support:

keyboard input
focus management
screen-reader announcements
readable message structure
accessible send button

When a new AI response arrives, accessibility announcements should be considered.

142. Accessibility for Progress

Progress should not rely only on color.

Example:

Progress: 72%

should accompany the visual progress bar.

143. Accessibility for Skill Levels

Skill levels should include text labels.

Do not communicate proficiency only through color.

144. Frontend Data Flow

General data flow:

User Interaction
       |
       v
React Component
       |
       v
Custom Hook
       |
       v
API Service
       |
       v
Axios Client
       |
       v
Backend API
       |
       v
Backend Service
       |
       v
Database / AI Layer
       |
       v
API Response
       |
       v
TanStack Query
       |
       v
React Component
       |
       v
UI
145. Authentication Data Flow
Login Form
   |
   v
Auth Hook
   |
   v
authApi.login()
   |
   v
Backend /auth/login
   |
   v
Authentication
   |
   v
Session / Token
   |
   v
Frontend Auth State
   |
   v
Dashboard
146. Profile Data Flow
Profile Page
   |
   v
useProfile()
   |
   v
profileApi.getProfile()
   |
   v
Backend
   |
   v
Database
   |
   v
Profile Response
   |
   v
TanStack Query
   |
   v
UI
147. Goal Data Flow
Goal Form
   |
   v
Validation
   |
   v
createGoal()
   |
   v
Backend
   |
   v
Goal Database Record
   |
   v
Response
   |
   v
Invalidate Goals
   |
   v
Goal List
148. Learning Path Data Flow
Goal
 +
Profile
 +
Skills
      |
      v
Backend AI Pipeline
      |
      v
Learning Path
      |
      v
Frontend API
      |
      v
LearningPathPage
      |
      v
Stage Components
149. Recommendation Data Flow
Learner Context
       |
       v
Recommendation API
       |
       v
Candidate Resources
       |
       v
Ranking
       |
       v
Personalized Recommendations
       |
       v
Frontend
       |
       v
Recommendation Cards
150. Chat Data Flow
User Question
      |
      v
Chat Input
      |
      v
chatApi.sendMessage()
      |
      v
Backend AI Service
      |
      v
AI Response
      |
      v
Chat UI
151. Feedback Data Flow
User Feedback
      |
      v
Feedback Component
      |
      v
feedbackApi
      |
      v
Backend
      |
      v
Feedback Database
      |
      v
Recommendation System
152. Frontend and Backend Contract

Frontend implementation must follow DOC-08.

For every API endpoint:

Method
URL
Request
Response
Status Codes
Authentication
Validation
Error Format

must be known before implementation.

153. API Contract Example

Conceptual:

POST /api/v1/goals

Request:

{
  "title": "Machine Learning Engineer",
  "description": "Become an ML Engineer"
}

Response:

{
  "id": "goal-id",
  "status": "ACTIVE"
}

The actual contract must be taken from DOC-08.

154. API Error Contract

All backend errors should follow a predictable format.

Recommended:

{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request",
    "details": {}
  }
}

The final schema must match DOC-08.

155. Frontend Integration Contract

The frontend must not assume undocumented backend behavior.

Before integrating a feature:

Requirement
    |
    v
API Contract
    |
    v
Backend Implementation
    |
    v
Frontend Service
    |
    v
Frontend Component
156. Mock API Development

Frontend development may initially use mocked responses.

Mocks must follow the exact DOC-08 response format.

This prevents integration mismatch later.

157. Mock Data Rules

Mock data must:

match real API schemas
use realistic IDs
use realistic enums
contain required fields
avoid undocumented fields
158. Integration Readiness

A frontend feature is integration-ready when:

API contract is defined
request type exists
response type exists
service exists
hook exists
UI exists
loading state exists
error state exists
success state exists
tests exist
159. Definition of Done

Frontend feature is considered complete only when:

Code implemented
+
TypeScript passes
+
ESLint passes
+
Unit/component tests pass
+
API contract verified
+
Responsive behavior verified
+
Accessibility checked
+
Error states handled
+
Loading states handled
+
Git commit created
+
Pull request reviewed
160. Frontend Development Sequence

Frontend implementation should follow this order.

Phase 1
Project Setup
        |
        v
Phase 2
Routing + Layout
        |
        v
Phase 3
Authentication
        |
        v
Phase 4
Onboarding
        |
        v
Phase 5
Profile + Goals
        |
        v
Phase 6
Dashboard
        |
        v
Phase 7
Skills + Skill Gaps
        |
        v
Phase 8
Learning Path
        |
        v
Phase 9
Recommendations
        |
        v
Phase 10
Progress
        |
        v
Phase 11
Feedback
        |
        v
Phase 12
AI Chat
        |
        v
Phase 13
Testing
        |
        v
Phase 14
Integration
161. Frontend MVP Priority

P0:

Project setup
Routing
Authentication
Onboarding
Profile
Goals
Dashboard
Skill management
Learning path
Recommendations
Progress
AI chat
Responsive UI
Error handling

P1:

Feedback
Advanced visualizations
Adaptive UI behavior
Enhanced chat experience

P2:

Advanced animations
Advanced analytics
Advanced personalization UI
162. Frontend Acceptance Criteria

The frontend MVP must satisfy the following.

AC-FE-001

User can register.

AC-FE-002

User can log in.

AC-FE-003

Authenticated user reaches dashboard.

AC-FE-004

User can complete onboarding.

AC-FE-005

User can create a goal.

AC-FE-006

User can define current skills.

AC-FE-007

User can view skill gaps.

AC-FE-008

User can generate a learning path.

AC-FE-009

User can view learning stages.

AC-FE-010

User can view recommendations.

AC-FE-011

User can understand recommendation reasons.

AC-FE-012

User can mark learning activity progress.

AC-FE-013

Dashboard updates progress.

AC-FE-014

User can provide feedback.

AC-FE-015

User can interact with AI assistant.

AC-FE-016

Application handles API failures gracefully.

AC-FE-017

Application works on desktop and mobile.

AC-FE-018

Critical workflows pass E2E tests.

163. Frontend Traceability
Requirement	Frontend Capability
FR-001	Registration
FR-002	Login
FR-003	Session handling
FR-004	Logout
FR-005	Profile creation
FR-006	Profile update
FR-007	Experience selector
FR-008	Skill selector
FR-009	Interests
FR-010	Learning history
FR-011	Learning preferences
FR-012	Goal creation
FR-013	Natural-language goal input
FR-014	Goal editing
FR-015	Goal status
FR-016	Chat interface
FR-017	Chat questions
FR-018	Context-aware chat
FR-019	Recommendation explanation
FR-020	Skill UI
FR-021	Skill levels
FR-023	Skill-gap visualization
FR-025	Resource cards
FR-028	Resource filters
FR-029	Recommendation UI
FR-035	Recommendation reasoning
FR-040	Learning path page
FR-041	Path stages
FR-042	Milestones
FR-043	Learning activities
FR-045	Next action
FR-046	Path regeneration UI
FR-047	Recommendation explanation
FR-051	Activity status
FR-052	Completion tracking
FR-053	Milestone progress
FR-054	Overall progress
FR-055	Skill development
FR-056	Resource feedback
FR-057	Recommendation feedback
FR-059	Feedback interaction
FR-063	Dashboard
FR-064	Progress visualization
FR-065	Skill visualization
FR-066	Milestone visualization
FR-067	Next recommendation
AI-001	Goal input
AI-002	Structured goal display
AI-003	Recommendation display
AI-007	Explainability UI
UX-001	Onboarding
UX-002	Goal entry
UX-003	Learning roadmap
UX-004	Next action
UX-005	Recommendation explanations
UX-006	Responsive design
164. Dependencies on Other Documents

Frontend architecture depends on:

DOC-01 Project Charter
DOC-02 Software Requirements Specification
DOC-03 User Roles and Journeys
DOC-04 Use Cases and User Stories
DOC-05 MVP Scope and Acceptance Criteria
DOC-06 System Architecture
DOC-07 Database Design
DOC-08 API Contract
DOC-09 ML/AI Architecture

Frontend must not contradict these documents.

165. Dependency on DOC-08

DOC-08 is the authoritative source for:

API endpoints
HTTP methods
Request schemas
Response schemas
Authentication requirements
Error responses
Pagination
166. Dependency on DOC-09

DOC-09 is authoritative for:

AI behavior
goal parsing
skill extraction
skill-gap analysis
recommendation generation
ranking
learning-path generation
explanations
adaptive behavior

The frontend only visualizes and interacts with these capabilities.

167. Dependency on DOC-12

DOC-12 is authoritative for:

authentication
authorization
token/session behavior
password security
security controls
168. Dependency on DOC-13

DOC-13 defines the complete repository architecture.

Frontend folder structure in this document must remain consistent with DOC-13.

169. Dependency on DOC-14

DOC-14 defines exact:

frontend framework
libraries
versions
dependency installation

This document defines the architectural role of those technologies.

170. Dependency on DOC-15

DOC-15 defines:

environment variables
development configuration
production configuration
local setup
171. Dependency on DOC-18

DOC-18 defines the project-wide testing and QA strategy.

Frontend-specific tests described here must integrate with DOC-18.

172. Dependency on DOC-20

DOC-20 defines final integration checks.

Frontend must satisfy frontend-related integration checklist items before release.

173. Dependency on DOC-21

DOC-21 will act as the master implementation specification.

Frontend implementation instructions must ultimately be consolidated into DOC-21.

174. Architecture Rules

The following rules are mandatory.

Rule 1

Frontend never accesses database directly.

Rule 2

Frontend never contains private API keys.

Rule 3

Frontend never calls LLM providers directly unless explicitly approved by architecture.

Rule 4

Frontend follows DOC-08.

Rule 5

Frontend uses typed API models.

Rule 6

Server state is managed through TanStack Query.

Rule 7

Global client state is limited.

Rule 8

Components remain modular.

Rule 9

Critical workflows are tested.

Rule 10

Business-critical recommendation logic remains backend-owned.

175. Anti-Patterns

The following must be avoided.

Huge monolithic components
Hardcoded API URLs
Hardcoded recommendation logic
Duplicated API calls
Duplicated server state
Excessive global state
Any type everywhere
Secrets in frontend
Direct database access
LLM calls from UI components
Unsafe HTML rendering
Missing error states
Missing loading states
Uncontrolled form state
176. Frontend Code Review Checklist

Before merging frontend code:

[ ] TypeScript compiles
[ ] ESLint passes
[ ] Prettier passes
[ ] No secrets committed
[ ] API contract followed
[ ] Types defined
[ ] Loading state implemented
[ ] Error state implemented
[ ] Empty state implemented
[ ] Responsive behavior checked
[ ] Accessibility checked
[ ] Tests added
[ ] No unnecessary dependencies
[ ] No duplicate server state
[ ] No business logic moved into UI
177. Frontend Integration Checklist

Before backend integration:

[ ] API base URL configurable
[ ] API client implemented
[ ] Authentication integration ready
[ ] API request types ready
[ ] API response types ready
[ ] Error mapping ready
[ ] Query hooks ready
[ ] Mutation hooks ready
[ ] Loading states ready
[ ] Empty states ready
[ ] Error states ready
[ ] Mock API tested
178. Frontend Release Checklist

Before release:

[ ] Production build succeeds
[ ] No TypeScript errors
[ ] No ESLint errors
[ ] Unit tests pass
[ ] Component tests pass
[ ] E2E tests pass
[ ] Authentication verified
[ ] Dashboard verified
[ ] Learning path verified
[ ] Recommendations verified
[ ] Progress verified
[ ] AI chat verified
[ ] Responsive layout verified
[ ] Accessibility verified
[ ] Environment variables verified
[ ] Production API URL verified
[ ] HTTPS verified
179. Demo Readiness

The frontend must support a clean 3–5 minute competition demonstration.

Recommended demo flow:

Landing Page
      |
      v
Register / Login
      |
      v
Onboarding
      |
      v
Enter Goal
      |
      v
Select Skills
      |
      v
Generate Learning Path
      |
      v
Dashboard
      |
      v
Skill Gaps
      |
      v
Recommendations
      |
      v
Learning Path
      |
      v
Progress Update
      |
      v
AI Assistant
180. Demo Optimization Principle

The demo should minimize:

waiting
typing
navigation
configuration

Test data may be prepared for demonstration, provided the final submission remains reproducible.

181. Competition-Oriented UX

Because the project is evaluated on:

Problem Understanding
Functionality
AI/ML
Innovation
UX
Performance
Code Quality

the frontend should visibly demonstrate:

Personalization
AI reasoning
Skill-gap intelligence
Learning-path sequencing
Progress adaptation
Clear user experience
182. Important UI Demonstration Elements

During the competition demo, evaluators should be able to see:

Learner Goal
Current Skills
Skill Gaps
Recommended Resources
Why Recommendations Were Chosen
Learning Sequence
Progress
AI Conversation
Adaptation
183. Frontend Architecture Decision Summary

The frontend architecture chooses:

React
+
TypeScript
+
Vite
+
React Router
+
Tailwind CSS
+
TanStack Query
+
Zustand
+
React Hook Form
+
Zod
+
Axios
+
Vitest
+
React Testing Library
+
Playwright

These technologies provide a modular and testable frontend suitable for the PathFinder AI MVP.

184. Future Frontend Extensions

Future versions may include:

Advanced learning analytics
Gamification
Achievements
Leaderboards
Social learning
Calendar integration
Learning reminders
Offline learning
Mobile application
Voice assistant
Advanced AI tutor
Real-time collaboration

These are outside the current MVP unless explicitly promoted.

185. Final Frontend Architecture

The complete frontend architecture is:

                         USER
                           |
                           v
                  +----------------+
                  | React Frontend |
                  +----------------+
                           |
             +-------------+-------------+
             |                           |
             v                           v
        React Router                Global State
             |                     Zustand / Query
             |                           |
             +-------------+-------------+
                           |
                           v
                    Feature Modules
                           |
       +-------------------+-------------------+
       |                   |                   |
       v                   v                   v
 Authentication        Learning           AI Chat
 Onboarding            Path               Interface
 Profile               Goals
 Skills                Progress
 Recommendations       Feedback
       |                   |                   |
       +-------------------+-------------------+
                           |
                           v
                      Custom Hooks
                           |
                           v
                      API Services
                           |
                           v
                     Axios Client
                           |
                           v
                     Backend API
                           |
             +-------------+-------------+
             |                           |
             v                           v
       Application Services          AI Services
             |                           |
             v                           v
          Database                 Recommendation
                                   Learning Path
                                   Explanation
186. Final Architectural Principle

PathFinder AI frontend must remain a presentation and interaction layer over the backend intelligence platform.

The architecture must preserve this boundary:

FRONTEND
    |
    | User Interaction
    | Presentation
    | State
    | Visualization
    |
    v
BACKEND
    |
    | Business Logic
    | Authentication
    | Data Processing
    | Recommendation Orchestration
    |
    +----------------------+
    |                      |
    v                      v
DATABASE                 AI/ML
                           |
                           v
                    Recommendation
                    Skill Intelligence
                    Learning Path

The frontend must never become the source of truth for business-critical intelligence.

187. Document Completion Criteria

DOC-10 is considered complete when:

[ ] Frontend technology stack defined
[ ] Folder architecture defined
[ ] Routing defined
[ ] Page architecture defined
[ ] Component architecture defined
[ ] State management defined
[ ] API communication defined
[ ] Authentication behavior defined
[ ] Onboarding defined
[ ] Dashboard defined
[ ] Learning path UI defined
[ ] Recommendation UI defined
[ ] AI chat defined
[ ] Progress UI defined
[ ] Feedback UI defined
[ ] Error handling defined
[ ] Loading states defined
[ ] Responsive design defined
[ ] Accessibility defined
[ ] Testing defined
[ ] Security defined
[ ] Environment configuration defined
[ ] Integration flow defined
[ ] Acceptance criteria defined
[ ] Traceability defined
188. Document Status
Document ID: DOC-10
Document Name: Frontend Architecture
Status: DRAFT
Implementation Status: NOT STARTED
Architecture Status: DEFINED
Parent: DOC-06 System Architecture
Depends On: DOC-08 API Contract, DOC-09 ML/AI Architecture
Next Document: DOC-11 Backend Architecture
189. Change Control

Any change to frontend architecture must identify:

Changed component
Reason for change
Affected requirement
Affected API
Affected database interaction
Affected AI integration
Affected tests
Affected deployment configuration
Affected documentation
Affected GitHub issue

Architectural changes must be recorded in:

docs/22-architecture-decision-records.md

and relevant requirements must be updated in:

docs/23-requirements-traceability-matrix.md