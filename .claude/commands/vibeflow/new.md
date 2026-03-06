# New Flutter App

You are helping the user create a new Flutter app by describing their idea. You'll generate the complete product specification from their description.

## Step 1: Check Current State

Check if any spec files exist in `specs/` directory.

**If specs already exist:**

"I see you already have some project files set up.
Existing files found: [list of files]

**Options:**
1. Start fresh (overwrite existing specs)
2. Keep existing and add new features
3. Cancel and manage existing files manually"

Wait for user choice. If they choose 1 or 2, proceed. If 3, stop.

**If directory is clean:**
Proceed to Step 2.

---

## Step 2: Gather App Information

Ask the user for essential information:

**Quick Questions:**

1. "What's your app called?"
   - Wait for app name
   - **Validation:** App name must:
     - Be 2-30 characters long
     - Start with a letter
     - Contain only letters, numbers, spaces, and hyphens
     - Not use reserved words (flutter, dart, test, main)
     - Not start with a number or special character
   - If validation fails, ask again with guidance

2. "Describe your app in one sentence. What does it do?"
   - Wait for description

3. "What's the main problem it solves?"
   - Wait for problem description

4. "Who are your target users?"
   - Wait for user description

5. "What are the 3-5 key features?"
   - Wait for feature list (or infer from description)

That's it! No more questions needed.

---

## Step 3: Generate Product Specs

**Immediately create** all spec files with smart defaults.

### 3.1: Product Overview

Create `specs/overview.md` with:

```markdown
# Product Overview

## App Name
[App Name]

## Description
[User's description]

## Problems
- [Main problem from user input]

## Solutions
- [How the app solves the problem]

## Target Users
[User's description]

## Key Features
[Infer from description or use user's list]

1. [Feature 1]
2. [Feature 2]
3. [Feature 3]
```

---

### 3.2: Roadmap

**Immediately create** `specs/roadmap.md` with simple format:

**Infer features from:**
- User's feature list (if provided)
- Key problems mentioned
- App description (extract verbs/nouns)
- Mobile app patterns (auth, dashboard, lists, forms, settings)

**Generate 3-6 features** using the simple format:

```markdown
# Product Roadmap

## Features

### 1. [Core Feature 1]
- **ID:** F001
- **Priority:** P0
- **Effort:** medium
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core

[Description...]

### 2. [Core Feature 2]
- **ID:** F002
- **Priority:** P0
- **Effort:** medium
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core

[Description...]

### 3. [Supporting Feature]
- **ID:** F003
- **Priority:** P1
- **Effort:** small
- **Status:** pending
- **Dependencies:** F001
- **Phase:** phase-1
- **Tags:** ui

[Description...]
```

**Feature Selection Logic:**
- Always include: core value loop feature
- Always include: data entry/management feature
- Always include: view/list feature
- Optional: auth (if user mentions accounts, login, profiles)
- Optional: settings (include by default for completeness)
- Optional: search/filter (if data-heavy)

**Priority Assignment:**
- Core value features → P0
- Supporting features → P1
- Nice-to-have → P2
- Nice-to-have → P2

---

### 3.3: Data Shape

**Immediately create** `specs/data_shape.md`:

**Infer entities from:**
- Feature descriptions in roadmap
- App description (nouns = potential entities)
- Common patterns (User, Item, Transaction, etc.)

```markdown
# Data Shape

## Shared Entities (for lib/core/domain/models/)

Entities used by 3 or more features should be marked as shared.

### [Shared Entity 1]
[Description - mark as shared if used across multiple features]

## Feature-Specific Entities

### [Entity 1]
[Description based on its role in the app]

### [Entity 2]
...

## Relationships

- [Entity 1] has many [Entity 2]
- [Entity 2] belongs to [Entity 1]
...
```

**Entity Inference Rules:**
- Main "noun" in app name → primary entity
- Features mentioning "create", "manage", "track" → entities
- Dashboard/reports → analytics entity
- Categories/tags → classification entity

**Shared Entity Detection:**
- User, Profile, Account → Almost always shared
- Transaction, Item, Order → Often shared (used by create, list, detail, analytics features)
- Category, Tag → Often shared (used by multiple features for classification)

---

### 3.4: Feature Specs

**Automatically create** `specs/features/[feature_slug]/spec.md` for each roadmap item:

```markdown
# [Feature Name] Spec

## Purpose
[Inferred from roadmap description]

## User Flows
1. **[Flow 1]** — [Typical user journey]
2. **[Flow 2]** — [Alternative path]

## Screens
- **[Screen 1]** — [Purpose]
- **[Screen 2]** — [Purpose]

## States
- **Empty** — [When no data exists]
- **Loading** — [During data fetch]
- **Error** — [On failure]
- **Success** — [Normal operation]

## Interactions
- **[Action]** — [Response]

## Data Requirements
- [Entity 1] (read, create)
- [Entity 2] (read-only)
```

**User Flow Inference:**
- Create screens → "New", "Edit", "Delete" flows
- List screens → "View", "Search", "Filter" flows
- Dashboard → "Summary", "Drill-down" flows
- Detail screens → "View details", "Related items" flows

---

## Step 4: Design System Research & Generation

"Your app structure is ready! Let's set up your design system.

I'll analyze your app type and target users to research and recommend theme options."

Use `/vibeflow:theme` command to research and generate your design system.

Would you like to:
1. "Generate design system now" — Run `/vibeflow:theme`
2. "Skip for now" — You can run `/vibeflow:theme` later

**If 1:** Run the theme command workflow
**If 2:** Continue to final summary

---

## Step 5: Final Summary

Present a comprehensive summary:

"✅ Your [App Name] app is ready!

**📱 App:** [App Name]
**🎨 Design:** [Theme selected / Not set]
**📋 Features:** [N] features defined
**📁 Structure:** Feature-first architecture ready

**Generated Files:**
- `specs/overview.md` — Product vision
- `specs/roadmap.md` — Feature roadmap with metadata
- `specs/data_shape.md` — Data entities
- `specs/features/*/spec.md` — [N] feature specifications
- `specs/design_system/*` — Design tokens (if theme selected)

**Next Steps:**

1. **Set up your design system:**
   ```bash
   /vibeflow:theme
   ```

2. **Plan your development:**
   ```bash
   /vibeflow:plan
   ```

3. **Build features:**
   ```bash
   /vibeflow:feature
   ```

4. **Track progress:**
   ```bash
   /vibeflow:status
   ```

**Quick Reference:**
- Set up design: `/vibeflow:theme`
- Plan development: `/vibeflow:plan`
- Build feature: `/vibeflow:feature`
- View progress: `/vibeflow:status`

You're all set! Start building your first feature."

---

## Supporting Sections

### Feature Inference Database

Use these patterns to infer features from app descriptions:

| Keywords in Description | Suggested Features |
|------------------------|-------------------|
| "track", "log", "record" | Dashboard, Add Entry, List View, Reports |
| "social", "community", "connect" | Feed, Post, Profile, Comments, Notifications |
| "shopping", "store", "buy" | Catalog, Cart, Checkout, Orders, Profile |
| "tasks", "todo", "manage" | Task List, Add Task, Categories, Reminders |
| "notes", "write", "document" | Note List, Editor, Folders, Search |
| "finance", "money", "budget" | Dashboard, Add Transaction, Categories, Reports, History |
| "fitness", "health", "workout" | Dashboard, Log Workout, Exercises, Progress, Goals |
| "learn", "education", "course" | Course List, Lesson View, Progress, Quizzes |
| "chat", "message", "conversation" | Chat List, Conversation View, Message Input, Media Sharing, Notifications |
| "travel", "booking", "reservation" | Search, Booking Flow, Trip Details, Confirmation, Itinerary |
| "food", "delivery", "restaurant" | Restaurant List, Menu View, Cart, Order Tracking, Reviews |
| "property", "real estate", "rent" | Property Search, Listing Details, Favorites, Contact Agent, Map View |
| "event", "ticket", "concert" | Event Discovery, Event Details, Ticket Purchase, Calendar Integration, QR Tickets |
| "weather", "forecast", "temperature" | Current Weather, Forecast View, Location Search, Alerts, Settings |
| "music", "audio", "playlist" | Library, Player Controls, Playlist Management, Search, Downloads |
| "map", "navigation", "direction" | Map View, Route Planning, Location Search, Turn-by-Turn, Saved Places |
| "sell", "marketplace", "listing" | Product Feed, Seller Dashboard, Messaging, Reviews, Wallet |
| "doctor", "medical", "appointment" | Appointment Booking, Doctor Search, Medical Records, Prescriptions, Video Consult |
| "game", "quiz", "trivia" | Game Lobby, Gameplay, Score Tracking, Leaderboards, Achievements |
| "news", "article", "content" | Article Feed, Categories, Bookmarks, Sharing, Notifications |
| "portfolio", "showcase", "gallery" | Project Gallery, Project Details, About Section, Contact, Categories |
| "survey", "form", "questionnaire" | Form Builder, Survey List, Response Collection, Analytics, Export |

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking the user questions — never ask through text
- **Immediate file writing:** Never show drafts, always write directly
- **Spec structure:** Always use exact spec structure for consistency (see CLAUDE.md)
- **Inference over questioning:** For custom apps, extract info from user input, don't ask everything
- **App name required:** Always ask for app name to personalize
- **Theme system:** Use `/vibeflow:theme` for research-based theme recommendations
- **Progress tracking:** Mention `/vibeflow:status` in summary
- **First feature:** Always suggest which feature to start with
- **Reference:** All specification formats are in CLAUDE.md
