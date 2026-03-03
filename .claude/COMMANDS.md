# VibeFlow Command Reference

Complete reference for all VibeFlow commands.

---

## Quick Reference

| Command | Purpose |
|---------|---------|
| `/vibeflow:new` | Create new Flutter app with specs |
| `/vibeflow:theme` | Set up design system with presets |
| `/vibeflow:feature` | Build complete feature implementation |
| `/vibeflow:status` | Track development progress |
| `/vibeflow:sprint` | Plan sprints based on effort |

---

## `/vibeflow:new`

Create a new Flutter app with complete specification files.

### Usage

```bash
/vibeflow:new
```

### What It Does

1. Checks for existing spec files
2. Prompts for app description (5 questions for custom app)
3. Generates `specs/` directory with:
   - `overview.md` - Product vision
   - `roadmap.md` - Features with metadata
   - `data-shape.md` - Domain entities
   - `features/*/spec.md` - Feature specifications
4. Optionally generates design system files

### Options

After starting, you can choose to:
- **Describe custom app** - Answer 5 questions, specs generated from your description
- **View Todo example** - See complete example at `.claude/examples/todo-app-spec.md`
- **Generate Todo app** - Use Todo app as starting point

### Generated Files

```
specs/
├── overview.md
├── roadmap.md
├── data-shape.md
└── features/
    └── [feature_name]/
        └── spec.md
```

### See Also

- [Feature Spec Format](WORKFLOW_REFERENCE.md#roadmap-format)
- [Todo Example](examples/todo-app-spec.md)

---

## `/vibeflow:theme`

Set up design system with preset themes.

### Usage

```bash
# Interactive selection
/vibeflow:theme

# Direct preset selection
/vibeflow:theme --preset modern

# View available presets
/vibeflow:theme --help
```

### Flags

| Flag | Description |
|------|-------------|
| `--preset [name]` | Skip interactive selection, use specified preset |
| `--help` | Show available presets |

### Available Presets

| Preset | Style | Best For |
|--------|-------|----------|
| `modern` | Clean, indigo/purple | SaaS, productivity |
| `minimal` | Black/white | Content, readers |
| `playful` | Colorful, rounded | Kids, games |
| `professional` | Blue/gray | Business, finance |
| `nature` | Greens, earth tones | Health, wellness |
| `bold` | High contrast | Strong brands |

### Generated Files

```
specs/design-system/
├── colors.json
└── typography.json
```

### Preset Data

Theme presets are stored in `.claude/data/themes/[preset].json`

### See Also

- [Theme Presets](WORKFLOW_REFERENCE.md#theme-presets)

---

## `/vibeflow:feature`

Build a complete feature with sample data, screens, and business logic.

### Usage

```bash
/vibeflow:feature
```

### What It Does

1. Lists available features from roadmap
2. Checks feature dependencies
3. Generates in 4 phases:
   - **Phase 1:** Sample data (`data.json`, `models.md`)
   - **Phase 2:** Screens (screen, view, page files) — Uses `flutter-ui-design` skill
   - **Phase 3:** Business logic (models, repositories, providers)
   - **Phase 4:** Updates feature status to `in_progress`

**Important:** When implementing screens (Phase 2), the `flutter-ui-design` skill is used to ensure distinctive, production-grade UI that avoids generic Material 3 defaults.

### Feature Selection

Features are listed with:
- Feature name
- Feature ID (F001, F002...)
- Priority (P0, P1, P2)
- Effort (small, medium, large, xlarge)
- Current status

### Dependency Checking

If feature has dependencies, you'll see:

```
⚠️ Dependencies Required:
This feature depends on: [Feature A], [Feature B]

Current status:
- [Feature A]: [status]
- [Feature B]: [status]

Options:
1. Continue anyway (may have incomplete integration)
2. Cancel and build dependencies first
```

### Generated Files

```
specs/features/[feature_slug]/
├── spec.md
├── data.json
└── models.md

lib/features/[feature_slug]/
├── domain/
│   ├── models/
│   └── repositories/
├── data/
│   ├── datasources/
│   └── repositories/
├── providers/
├── screens/
│   ├── [screen]_screen.dart
│   ├── [screen]_view.dart
│   └── [screen]_page.dart
├── widgets/
└── routes.dart
```

### See Also

- [File Structure Patterns](WORKFLOW_REFERENCE.md#file-structure-patterns)
- [Provider State Pattern](WORKFLOW_REFERENCE.md#provider-state-pattern-template)

---

## `/vibeflow:status`

View implementation progress across all features.

### Usage

```bash
/vibeflow:status
```

### What It Does

1. Parses roadmap for all features
2. Checks file existence for progress calculation
3. Generates status report by phase
4. Shows priority items needing attention

### Progress Calculation

Progress is calculated based on file existence:

| Check | Progress |
|-------|----------|
| `spec.md` exists | +25% |
| `data.json` exists | +25% |
| `models.md` exists | +10% |
| Domain models exist | +20% |
| Logic implemented | +15% |
| Screens implemented | +10% |

### Status Report Format

```markdown
# [App Name] Development Status

## Phase 1: [Phase Name]

| Feature | ID | Priority | Spec | Data | Types | Models | Logic | Screens | Progress |
|---------|----|----------|------|------|-------|--------|-------|---------|----------|
| [Feature 1] | F001 | P0 | [X] | [X] | [X] | [X] | [ ] | [ ] | 80% |

**Phase Progress:** [X]/[Y] features started ([average]% complete)

## Overall Summary
- **Total Features:** [N]
- **Completed:** [X] features ([Y]%)
- **In Progress:** [X] features
- **Not Started:** [X] features

## Next Steps
[Actionable recommendations based on current progress]
```

### See Also

- [Progress Calculation](WORKFLOW_REFERENCE.md#progress-calculation)

---

## `/vibeflow:sprint`

Plan sprints based on effort estimates and dependencies.

### Usage

```bash
/vibeflow:sprint
```

### What It Does

1. Parses roadmap for features with metadata
2. Prompts for sprint parameters:
   - Sprint duration (1 week, 2 weeks, 1 month, custom)
   - Focus area (P0 only, specific phase, all pending, in-progress)
   - Team size (1, 2, or 3+ developers)
3. Resolves dependencies using topological sort
4. Selects features that fit capacity
5. Generates sprint plan

### Questions Asked

**1. Sprint Duration**
- "1 week" (~6 points per dev)
- "2 weeks" (~10 points per dev) - Recommended
- "1 month" (~20 points per dev)
- "Custom" - Specify weeks

**2. Focus Area**
- "P0 features only" - Critical path to MVP
- "Specific phase" - Then asks which phase
- "All pending features" - Balanced approach
- "Continue in-progress" - Finish what's started

**3. Team Size**
- "1 developer" - Solo development
- "2 developers" - Small team
- "3+ developers" - Larger team

### Sprint Plan Format

```markdown
# [Duration] Sprint Plan ([Team Size] dev × [Weeks] weeks = [Points] points)

## Sprint Goals
[Summary based on focus area]

## Selected Features ([Total Points]/[Capacity] points)

### Priority: P0
1. **[Feature 1]** ([X] points) — [Feature ID]
   - **Dependencies:** [None or list]
   - **Deliverables:** [Key screens, logic, data]
   - **Acceptance:** [What "done" looks like]

## Blocked Features (Deferred)
[Features that couldn't be included]

## Quick Start
### Setup
/vibeflow:feature

### Implementation Order
1. [Feature 1] — Foundation for other features
2. [Feature 2] — Builds on Feature 1

### Daily Progress Check
/vibeflow:status
```

### See Also

- [Effort Estimation](WORKFLOW_REFERENCE.md#effort-estimation)
- [Priority Levels](WORKFLOW_REFERENCE.md#priority-levels)

---

## Common Patterns

### Checking Prerequisites

Most commands check if `specs/roadmap.md` exists first. If missing:

```
Before we can proceed, we need the product roadmap in place.
Please run `/vibeflow:new` to create the project first.
```

### Feature Slug Conversion

Feature names are converted to slugs consistently:
- Lowercase
- Spaces to hyphens
- Remove special characters

Example: "Due Dates & Reminders" → `due_dates_reminders`

### Atomic Execution

Commands fail fast and don't partially implement:
- If any phase fails, report error and stop
- Don't leave half-generated files
- Provide clear error messages

---

## Workflow Examples

### New App Workflow

```bash
# 1. Create app specs
/vibeflow:new
# → Describe your app
# → Specs generated

# 2. Set up design system
/vibeflow:theme --preset modern
# → Design tokens created

# 3. Check progress
/vibeflow:status
# → Shows all features pending

# 4. Plan sprint
/vibeflow:sprint
# → Sprint plan generated

# 5. Build features (repeat for each)
/vibeflow:feature
# → Select feature from list
# → Complete feature generated

# 6. Track progress
/vibeflow:status
# → Shows implementation progress
```

### Quick Start

```bash
# Create and build in one flow
/vibeflow:new      # Describe app
/vibeflow:theme    # Choose style
/vibeflow:feature  # Build first feature
/vibeflow:status   # Check progress
```

---

## Error Messages

| Error | Cause | Solution |
|-------|-------|----------|
| `Before building features, we need the product setup` | No roadmap | Run `/vibeflow:new` |
| `Feature spec not found` | Feature missing from specs | Run `/vibeflow:new` |
| `Dependencies Required` | Feature depends on incomplete features | Build dependencies first |
| `All features are complete` | No pending features | All done! |
| `Smallest feature exceeds capacity` | Can't fit any feature | Increase duration |

---

## Data Files

### Theme Presets

`.claude/data/themes/[preset].json` - Theme definitions

### Example App

`.claude/examples/todo-app-spec.md` - Complete workflow example

### Reference

`.claude/WORKFLOW_REFERENCE.md` - Shared constants and rules

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-03-01 | Initial command reference |
