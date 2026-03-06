# Build Feature

You are helping the user build a complete feature in one go — generating sample data, implementing screens, and adding business logic all together.

## Step 1: Check Prerequisites

Check if `specs/roadmap.md` exists.

If it does **not** exist:

"Before building features, we need the product setup in place.
Please run `/vibeflow:new` first."

Stop here if prerequisites are missing.

---

## Step 2: List Available Features

First, check if development plan files exist in `/specs/plans/`.

**If development plans exist:**
1. Find the latest development plan (highest N in `plan_[N].md`)
2. Read the development plan file
3. Extract selected features from the "Selected Features" section
4. Display features from the development plan in implementation order, grouped by category

"If you have an active development plan, I'll show features from your latest plan.

**Development Plan:** [Plan Name]
**Focus:** [Focus area]

**Features for this plan:**

**Group: Tasks**
1. **[Feature 1]** (F001) — Priority: P0 | Status: pending
2. **[Feature 2]** (F002) — Priority: P0 | Status: pending

**Group: Backup**
3. **[Feature 3]** (F003) — Priority: P1 | Status: pending

**Ungrouped Features**
4. **[Feature 4]** (F004) — Priority: P1 | Status: pending

Type the feature name or number to build, or 'all' to see roadmap features:"

Wait for user input.

**If user types 'all' or no development plans exist:**
Read the roadmap and list all available features, grouped by category:

"Which feature would you like to build?

**Available Features:**

**Group: Tasks**
1. **[Feature 1]** (F001) — Priority: P0 | Status: pending
2. **[Feature 2]** (F002) — Priority: P1 | Status: pending

**Group: Backup**
3. **[Feature 3]** (F003) — Priority: P1 | Status: pending

**Ungrouped Features**
4. **[Feature 4]** (F004) — Priority: P2 | Status: pending

Type the feature name or number:"

Wait for user input.

---

## Step 3: Feature Selection & Validation

**If user typed a number:**
- Map to the corresponding feature from the roadmap

**If user typed a name:**
- Find matching feature (fuzzy match)

**If no match found:**
- Show available features again
- Ask user to select

---

## Step 4: Build Complete Feature

Once feature is selected, build it in phases:

### Phase 1: Update Feature Status

Update `specs/roadmap.md` to mark the feature as `in_progress`:
- Find the feature's YAML block
- Update `status: pending` → `status: in_progress`
- Update `lastUpdated` in frontmatter

### Phase 2: Generate Sample Data

Generate sample data files for the selected feature:

**Feature path construction:**
- **With group:** `specs/features/[group_slug]/[feature_slug]/`
- **Without group:** `specs/features/[feature_slug]/`

- Read feature spec from the path above
- Create `data.json` in the feature path
- Create `models.md` in the feature path

### Phase 3: Implement Screens

**⚠️ MANDATORY: Use flutter-ui-design Skill**

Before writing any Flutter UI code, you MUST use the `flutter-ui-design` skill to generate distinctive, production-grade UIs.

**How to use:**
- Invoke the skill when implementing screens, widgets, or visual components
- The skill provides design thinking principles and Flutter aesthetics guidelines
- Follow the skill's guidance for typography, color, motion, and layout

Then create screen widgets:
- Read feature spec and sample data
- Use flutter-ui-design skill for UI implementation

**Feature path for implementation:**
- **With group:** `lib/features/[group_slug]/[feature_slug]/`
- **Without group:** `lib/features/[feature_slug]/`

Generate:
- Screen widgets in `[feature_path]/screens/`
- View wrappers using sample data
- Page wrappers for production

### Phase 4: Implement Logic

Create business logic:

**Determine model location:**
1. Check `specs/data_shape.md` for shared entities section
2. If entity is marked as "shared", generate in `lib/core/domain/models/`
3. If entity is used by 3+ features, generate in `lib/core/domain/models/`
4. Otherwise, generate in feature-specific domain models

**Feature path for implementation:**
- **With group:** `lib/features/[group_slug]/[feature_slug]/`
- **Without group:** `lib/features/[feature_slug]/`

**Generate files:**
- Domain models (shared → `lib/core/domain/models/` or feature-specific → `[feature_path]/domain/models/`)
- Repository interfaces in `[feature_path]/domain/repositories/`
- Repository implementations in `[feature_path]/data/repositories/`
- Data sources in `[feature_path]/data/datasources/`
- Providers in `[feature_path]/providers/`

**Note:** If generating models in `lib/core/domain/models/`, check if the model file already exists. If it does, import the existing model instead of creating a duplicate.

### Phase 5: Update Feature Status

Update `specs/roadmap.md` to mark the feature as `done`:
- Find the feature's YAML block
- Update `status: in_progress` → `status: done`
- Update `lastUpdated` in frontmatter

---

## Step 5: Summary

Present a comprehensive summary:

"✅ Feature **[Feature Name]** is now complete!

**Generated Files:**

**Spec & Data:**
- `specs/features/[feature_path]/spec.md`
- `specs/features/[feature_path]/data.json`
- `specs/features/[feature_path]/models.md`

**Implementation:**
- `lib/core/domain/models/` — Shared domain models (if any)
- `lib/features/[feature_path]/domain/models/` — Feature-specific models
- `lib/features/[feature_path]/domain/repositories/` — Repository interfaces
- `lib/features/[feature_path]/data/repositories/` — Repository implementations
- `lib/features/[feature_path]/data/datasources/` — Data sources
- `lib/features/[feature_path]/providers/` — State providers
- `lib/features/[feature_path]/screens/` — UI screens

**Note:** `[feature_path]` is `[group]/[feature]` for grouped features, or `[feature]` for standalone features.

**Status Updated:** [Feature Name] is now marked as `in_progress` in the roadmap.

**Next Steps:**

1. **Test the feature:**
   ```bash
   flutter run
   # Navigate to your feature and test the UI and interactions
   ```

2. **View overall progress:**
   ```bash
   /vibeflow:status
   ```

3. **Plan your next development:**
   ```bash
   /vibeflow:plan
   ```

4. **Build another feature:**
   ```bash
   /vibeflow:feature
   # Select the next feature from the development plan
   ```

**Implementation Notes:**
- Data sources use mock implementations by default — replace with real API/database when ready
- State management uses the Provider pattern — update `main.dart` if needed
- Screens use sample data in development — they'll connect to real providers in production"

---

## Important Notes

- **Development plan integration:** When development plans exist in `/specs/plans/`, prioritize showing features from the latest development plan. Users can type 'all' to see all roadmap features
- **Development plan parsing:** Read development plan files to extract "Selected Features" section with feature names, IDs, priorities, and implementation order
- **Roadmap fallback:** If no development plans exist or user types 'all', fall back to showing all features from the roadmap
- **flutter-ui-design Skill:** MANDATORY - Use the skill before implementing any screens to ensure distinctive, production-grade UI (see CLAUDE.md)
- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking the user questions — never ask through text
- **Atomic execution:** If any phase fails, report the error and stop — don't partially implement
- **Feature slug detection:** Use the same slug conversion as other commands (lowercase, hyphens) — see CLAUDE.md
- **Status tracking:** Always update the roadmap status to reflect implementation progress
- **Dependency awareness:** Check if dependencies are implemented; if not, warn user
- **File structure:** Follow the exact VibeFlow feature-first architecture — see CLAUDE.md
- **Provider setup:** Ensure main.dart has the feature's provider registered — see Provider template in CLAUDE.md
- **Immediate writing:** Never show drafts — always write files directly
- **Reference:** All specification formats are in CLAUDE.md

---

## Dependency Checking

Before building, check the feature's dependencies:

**If feature has dependencies:**

"⚠️ **Dependencies Required:**
This feature depends on: [Dependency 1], [Dependency 2]

Current status:
- [Dependency 1]: [status]
- [Dependency 2]: [status]

**Recommendation:** Build dependencies first for a smoother development experience.

**Options:**
1. Continue anyway (may have incomplete integration)
2. Cancel and build dependencies first

Type 1 or 2:"

Wait for user input.

**If 1:** Continue with feature build
**If 2:** Stop and suggest building dependencies

---

## Feature Slug Conversion

Convert feature names to slugs using these rules:
- Lowercase everything
- Replace spaces with hyphens
- Remove special characters
- Keep words intact

**Examples (without group):**
- "Dashboard" → `dashboard`
- "User Management" → `user_management`

**Examples (with group):**
- Group: "Tasks" → `tasks`
- Feature: "Add Task" → `add_task`
- Combined: `tasks/add_task`

---

## Definition of Done

A feature is considered complete when all of the following are met:

**Note:** `[feature_path]` is `[group]/[feature]` for grouped features, or `[feature]` for standalone features.

### Spec & Data (50%)
- [ ] `specs/features/[feature_path]/spec.md` exists with complete feature description
- [ ] `specs/features/[feature_path]/data.json` exists with realistic sample data
- [ ] `specs/features/[feature_path]/models.md` exists with Dart model definitions

### Domain Models (20%)
- [ ] Domain model classes exist in `lib/features/[feature_path]/domain/models/`
- [ ] Models have all required properties and types
- [ ] Repository interface exists in `lib/features/[feature_path]/domain/repositories/`

### Business Logic (15%)
- [ ] Repository implementation exists in `lib/features/[feature_path]/data/repositories/`
- [ ] Data source exists (mock, API, or database)
- [ ] Provider exists with state management (ChangeNotifier)
- [ ] Provider is registered in `main.dart`

### Screens (10%)
- [ ] `*_screen.dart` files exist for all screens (props-driven UI)
- [ ] `*_view.dart` files exist for development (uses data.json)
- [ ] `*_page.dart` files exist for production (uses Provider)
- [ ] Screens use flutter-ui-design skill for distinctive UI

### Additional Checks
- [ ] Code compiles without errors (`flutter analyze` passes)
- [ ] Hot reload works for development views
- [ ] Feature status in roadmap updated to `done`

---

## Integration with Existing Commands

This command handles the complete feature implementation:
1. Sample data generation — Create data.json and models.md
2. Screen implementation — Generate UI widgets and wrappers
3. Business logic — Create domain models, repositories, providers

Benefits:
- Runs all phases in sequence automatically
- Updates feature status in roadmap
- Provides unified summary
- Checks dependencies upfront

---

## Error Handling

**If spec file doesn't exist:**
"Feature spec not found. Please run `/vibeflow:new` to generate feature specs first."

**If sample data generation fails:**
"Failed to generate sample data: [error details]
Please fix the issue and try again."

**If screen implementation fails:**
"Failed to implement screens: [error details]
Sample data was generated successfully. You can continue manually."

**If logic implementation fails:**
"Failed to implement business logic: [error details]
Screens and sample data were generated successfully. You can continue manually."

---

## Progress Indicators

Show progress during the build:

"Building [Feature Name]...
[✓] Generating sample data...
[✓] Implementing screens...
[✓] Adding business logic...
[✓] Updating feature status..."
