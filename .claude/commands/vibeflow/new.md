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

**Immediately create** `specs/data-shape.md`:

**Infer entities from:**
- Feature descriptions in roadmap
- App description (nouns = potential entities)
- Common patterns (User, Item, Transaction, etc.)

```markdown
# Data Shape

## Entities

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

## Step 4: Design System Selection

"Your app structure is ready! Choose a visual theme:

1. **modern** (recommended) — Clean indigo/purple, Inter font
2. **minimal** — Black/white, Geist font
3. **playful** — Colorful, rounded, Nunito font
4. **professional** — Blue/gray, SF Pro font
5. **nature** — Greens, earth tones, organic
6. **bold** — High contrast, strong colors
7. **Skip for now** — Decide later

Type a number (1-7):"

Wait for user input.

**If 1-6:** Generate design system files with preset (use `/vibeflow:theme` preset definitions)
**If 7:** Skip design system

---

## Step 5: Final Summary

Present a comprehensive summary:

"✅ Your [App Name] app is ready!

**📱 App:** [App Name]
**🎨 Design:** [Preset name / Not set]
**📋 Features:** [N] features defined
**📁 Structure:** Feature-first architecture ready

**Generated Files:**
- `specs/overview.md` — Product vision
- `specs/roadmap.md` — Feature roadmap with metadata
- `specs/data-shape.md` — Data entities
- `specs/features/*/spec.md` — [N] feature specifications
- `specs/design-system/*` — Design tokens (if theme selected)

**Next Steps:**

1. **Track progress:**
   ```bash
   /vibeflow:status
   ```

2. **Plan your sprint:**
   ```bash
   /vibeflow:sprint
   ```

3. **Build features:** Use `/vibeflow:feature` to implement

**Quick Reference:**
- View all features: `/vibeflow:status`
- Generate sprint plan: `/vibeflow:sprint`
- Change design: `/vibeflow:theme --preset [name]`

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

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking the user questions — never ask through text
- **Immediate file writing:** Never show drafts, always write directly
- **Spec structure:** Always use exact spec structure for consistency
- **Inference over questioning:** For custom apps, extract info from user input, don't ask everything
- **App name required:** Always ask for app name to personalize
- **Theme optional:** Don't block on theme selection, can do later
- **Progress tracking:** Mention `/vibeflow:status` in summary
- **First feature:** Always suggest which feature to start with
- **Design presets:** Reference `/vibeflow:theme` for preset definitions

---

## Design Presets Reference

Design presets are defined in `/vibeflow:theme`. Available presets:

- **modern** — Clean, indigo/purple, Manrope/Inter/JetBrains Mono
- **minimal** — Black/white, zinc, Geist
- **playful** — Colorful, pink/orange, Fredoka/Nunito
- **professional** — Blue/gray, SF Pro
- **nature** — Greens, earth tones, Nunito/Open Sans
- **bold** — High contrast, violet/fuchsia, Space Grotesk
