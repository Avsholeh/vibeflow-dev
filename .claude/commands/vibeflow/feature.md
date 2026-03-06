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
4. Display features from the development plan in implementation order

"If you have an active development plan, I'll show features from your latest plan.

**Development Plan:** [Plan Name]
**Focus:** [Focus area]

**Features for this plan:**

**Priority: P0**
1. **[Feature 1]** (F001) — Priority: P0 | Status: pending
2. **[Feature 2]** (F002) — Priority: P0 | Status: pending

**Priority: P1**
3. **[Feature 3]** (F003) — Priority: P1 | Status: pending

Type the feature name or number to build, or 'all' to see roadmap features:"

Wait for user input.

**If user types 'all' or no development plans exist:**
Read the roadmap and list all available features:

"Which feature would you like to build?

**Available Features:**

**Priority: P0**
1. **[Feature 1]** (F001) — Priority: P0 | Status: pending
2. **[Feature 2]** (F002) — Priority: P1 | Status: pending

**Priority: P1**
3. **[Feature 3]** (F003) — Priority: P1 | Status: pending

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
- Find the feature's metadata block
- Update `status: pending` → `status: in_progress`

### Phase 2: Generate Sample Data

Generate sample data files for the selected feature:

**Feature path construction:** `[feature_slug]` → `specs/features/add_task/`

- Read feature spec from the path above
- Create `data.json` in the feature path
- Create `models.md` in the feature path

### Phase 3: Implement Screens

**⚠️ MANDATORY: Use flutter-ui-design Skill**

Before writing any Flutter UI code, you MUST invoke the `flutter-ui-design` skill using the Skill tool:

**Skill Invocation:**
```
Use the Skill tool with:
- skill: "flutter-ui-design"
- args: "[Screen description] | Context: [feature context from spec] | Design tokens from specs/design_system/"
```

**Design Context to Compile:**
1. Read `specs/design_system/colors.json` - extract primary, secondary, semantic colors
2. Read `specs/design_system/typography.json` - extract font families and text styles
3. Read feature spec - understand screen purpose and user flows
4. Pass this context to the skill for consistent UI generation

**After Skill Returns:**
- Implement the screen using the design guidance
- Follow all flutter-ui-design principles (bold aesthetics, characterful typography, etc.)

Then create screen widgets:
- Read feature spec and sample data
- Use flutter-ui-design skill for UI implementation

**Screen paths (Clean Architecture):**
- Screen widgets in `lib/presentation/screens/`
- View wrappers using sample data
- Page wrappers for production

### Phase 4: Implement Logic

Create business logic following Clean Architecture:

**Domain Layer:**
- Domain entities in `lib/domain/entities/` (all entities go here, no feature folders)
- Repository interfaces in `lib/domain/repositories/`
- Use cases in `lib/domain/usecases/` (business logic operations)

**Data Layer:**
- Repository implementations in `lib/data/repositories/`
- Data sources in `lib/data/datasources/`
- DTOs in `lib/data/models/` (for data transfer)

**Presentation Layer:**
- Providers in `lib/presentation/providers/`

**Entity Deduplication & Merge Algorithm:**

When generating entities in `lib/domain/entities/`:

1. **Check if entity exists:**
   ```bash
   lib/domain/entities/[entity_name].dart
   ```

2. **If entity doesn't exist:**
   - Create new entity file with all properties
   - Proceed to next entity

3. **If entity exists:**
   - Read the existing entity file
   - Parse current properties (name, type, nullable)
   - Compare with new properties from spec

4. **Merge Decision:**
   - **If properties match exactly:** Use existing entity, skip generation
   - **If new properties added:** Append new properties to existing entity
   - **If properties conflict:** Ask user using AskUserQuestion:
     ```
     "Entity [EntityName] already exists with different properties:
     Existing: [list existing]
     New: [list new]
     What would you like to do?"
     Options:
     1. "Keep existing" — Use existing entity as-is
     2. "Merge properties" — Add new properties to existing
     3. "Replace with new" — Overwrite existing entity
     ```

5. **After merge:**
   - Update the entity file with merged properties
   - Add comment if properties were merged: `// Merged on [date]`

**Example Merge:**
```dart
// Existing: task.dart
class Task {
  final String id;
  final String title;
  final bool isCompleted;
}

// After merge (adding description, createdAt):
class Task {
  final String id;
  final String title;
  final bool isCompleted;
  final String? description;      // NEW
  final DateTime createdAt;        // NEW
}
```

### Phase 4.5: Update Routing

After generating screens, update the app routing:

**1. Create/Update:** `lib/presentation/routes/app_routes.dart`

```dart
import 'package:flutter/material.dart';
import '../screens/[feature]_screen.dart';
// ... other screen imports

class AppRoutes {
  static const String [FEATURE_ROUTE] = '/[feature]';
  // Add other routes

  static Map<String, WidgetBuilder> getRoutes() {
    return {
      [FEATURE_ROUTE]: (context) => const [Feature]Page(),
      // Add other routes
    };
  }

  static Route<T>? onGenerateRoute<T>(RouteSettings settings) {
    switch (settings.name) {
      case [FEATURE_ROUTE]:
        return MaterialPageRoute(builder: (context) => const [Feature]Page());
      // Add other cases
      default:
        return null;
    }
  }
}
```

**2. Update main.dart (if first feature):**
```dart
MaterialApp(
  // ...
  initialRoute: AppRoutes.[FEATURE_ROUTE],
  onGenerateRoute: AppRoutes.onGenerateRoute,
  // or
  routes: AppRoutes.getRoutes(),
)
```

**3. For navigation between features:**
```dart
Navigator.pushNamed(context, AppRoutes.[FEATURE_ROUTE]);
```

**Note:** The [Feature]Page wraps [Feature]Screen with Provider connectivity.

### Phase 5: Code Quality Check

Run code quality checks to ensure the implementation is correct:

```bash
flutter analyze
```

- If any warnings or errors are found, iterate and fix them
- Re-run `flutter analyze` until no errors remain
- Only proceed to the next phase once the code is clean

### Phase 6: Update Feature Status

Update `specs/roadmap.md` to mark the feature as `done`:
- Find the feature's metadata block
- Update `status: in_progress` → `status: done`

---

## Step 5: Summary

Present a comprehensive summary:

"✅ Feature **[Feature Name]** is now complete!

**Generated Files:**

**Spec & Data:**
- `specs/features/[feature_slug]/spec.md`
- `specs/features/[feature_slug]/data.json`
- `specs/features/[feature_slug]/models.md`

**Implementation (Clean Architecture):**

**Domain Layer:**
- `lib/domain/entities/` — Domain entities (if new)
- `lib/domain/repositories/` — Repository interfaces
- `lib/domain/usecases/` — Business logic use cases

**Data Layer:**
- `lib/data/repositories/` — Repository implementations
- `lib/data/datasources/` — Data sources
- `lib/data/models/` — DTOs (if needed)

**Presentation Layer:**
- `lib/presentation/screens/` — UI screens
- `lib/presentation/widgets/` — Shared widgets (if any)
- `lib/presentation/providers/` — State providers

**Status Updated:** [Feature Name] is now marked as `done` in the roadmap.

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
- Screens use sample data in development — they'll connect to real providers in production
- **Clean Architecture:** Entities in domain/, use cases for business logic, providers depend on use cases"

---

## Important Notes

- **Development plan integration:** When development plans exist in `/specs/plans/`, prioritize showing features from the latest development plan. Users can type 'all' to see all roadmap features
- **Development plan parsing:** Read development plan files to extract "Selected Features" section with feature names, IDs, priorities, and implementation order
- **Roadmap fallback:** If no development plans exist or user types 'all', fall back to showing all features from the roadmap
- **flutter-ui-design Skill:** MANDATORY - Use the skill before implementing any screens to ensure distinctive, production-grade UI (see CLAUDE.md)
- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking the user questions — never ask through text
- **Atomic execution:** If any phase fails, report the error and stop — don't partially implement
- **Feature slug detection:** Use the same slug conversion as other commands (lowercase, underscore) — see CLAUDE.md
- **Status tracking:** Always update the roadmap status to reflect implementation progress
- **Dependency awareness:** Check if dependencies are implemented; if not, warn user
- **File structure:** Follow the exact VibeFlow Clean Architecture — see CLAUDE.md
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
- Replace spaces with underscore
- Remove special characters
- Keep words intact

**Examples:**
- "Dashboard" → `dashboard`
- "User Management" → `user_management`
- "Add Transaction Flow" → `add_transaction_flow`
- "Due Dates & Reminders" → `due_dates_reminders`

---

## Definition of Done

A feature is considered complete when all of the following are met:

### Spec & Data (50%)
- [ ] `specs/features/[feature_slug]/spec.md` exists with complete feature description
- [ ] `specs/features/[feature_slug]/data.json` exists with realistic sample data
- [ ] `specs/features/[feature_slug]/models.md` exists with Dart model definitions

### Domain Layer (20%)
- [ ] Domain entity classes exist in `lib/domain/entities/`
- [ ] Entities have all required properties and types
- [ ] Repository interface exists in `lib/domain/repositories/`
- [ ] Use cases exist in `lib/domain/usecases/`

### Data Layer (15%)
- [ ] Repository implementation exists in `lib/data/repositories/`
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
3. Business logic — Create entities, repositories, use cases, providers

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
