# New Flutter App

You are helping the user create a new Flutter app by describing their idea. You'll generate the complete product specification from their description.

## Step 1: Check Current State

Check if any spec files exist in `specs/` directory.

**If specs already exist:**

Use AskUserQuestion to present options:

```text
Question: "I see you already have some project files set up. What would you like to do?"

Options:
1. "Start fresh" — Overwrite existing specs with new app configuration
2. "Keep existing" — Add new features to existing project files
3. "Cancel" — Stop and manage existing files manually
```

- **If Start fresh:** Proceed to Step 2 (will overwrite existing specs)
- **If Keep existing:** Proceed to Step 2 (will preserve and merge with existing specs)
- **If Cancel:** Stop execution

**If directory is clean:**
Proceed to Step 2.

---

## Step 2: Gather App Information

First, ask the user to share their raw notes, ideas, or thoughts about the product they want to build. Be warm and open-ended:

"I'd love to help you define your product overview. Tell me about the product you're building - share any notes, ideas, or rough thoughts you have. What problem are you trying to solve? Who is it for? Don't worry about structure yet, just share what's on your mind."

Wait for their response before proceeding.

---

## Step 3: Ask Clarifying Questions

After receiving their input, use the AskUserQuestion tool to ask targeted questions covering all three areas. Ask questions one or two at a time, conversationally, with follow-ups as needed.

### Product Overview Questions

Shape the core product definition:

- The product name - A clear, concise name for the product
- The core product description (1-3 sentences that capture the essence)
- The key problems the product solves (1-5 specific pain points)
- How the product solves each problem (concrete solutions)
- The main features that make this possible

**Important:** If the user hasn't already provided a product name, ask them:

- "What would you like to call this product? (A short, memorable name)"

Example questions (adapt based on their input):

- "Who is the primary user of this product? Can you describe them?"
- "What's the single biggest pain point you're addressing?"
- "How do people currently solve this problem without your product?"
- "What makes your approach different or better?"
- "What are the 3-5 most essential features?"

### Roadmap Questions

Identify the main areas/sections of the product:

- "What are the main areas or screens of this product? (e.g., Dashboard, Settings, Invoices)"
- "What would you consider the most critical area to build first?"
- "Are there any areas that should be separate from the core functionality?"

### Data Shape Questions

Identify the core entities ("nouns") of the product:

- "What are the main 'things' users will create, view, or manage in this product? (e.g., Projects, Invoices, Clients)"
- "How do these things relate to each other?"

The goal is to gather enough information for all three files before proceeding. Don't need exhaustive detail on every entity — just the core nouns and their relationships.

## Step 4: Generate Product Specs

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

### 4.5: Review & Iterate

After generating all specs, present summary to user and ask for confirmation:

```text
"✅ I've generated your product specs:

📋 Product Overview: [App name]
🗺️ Roadmap: [N] features
📦 Data Shape: [N] entities
📄 Feature Specs: [N] feature files generated

Would you like to:"
```

Use AskUserQuestion with options:
```text
Options:
1. "Approve" — Proceed to design system setup
2. "Edit overview" — Modify product overview
3. "Edit roadmap" — Add/remove/change features
4. "Edit data shape" — Modify entities and relationships
```

- **If Approve:** Proceed to Step 5
- **If Edit:** Apply changes, regenerate affected files, re-ask confirmation (iterate until satisfied)

---

## Step 5: Design System Research & Generation

Use AskUserQuestion to present options:

```text
Question: "Your app structure is ready! Would you like to set up your design system now?"

Options:
1. "Generate now" — Analyze app type and generate theme with research-backed recommendations
2. "Skip for now" — You can run `/vibeflow:theme` later
```

- **If Generate now:** Run `/vibeflow:theme` command workflow
- **If Skip for now:** Continue to Step 6 (Final Summary)

---

## Step 6: Final Summary

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
