# Development Planning

You are helping the user create development plans for their Flutter app based on roadmap features, priorities, and dependencies.

## Step 1: Check Prerequisites

First, check if `/specs/roadmap.md` exists.

If it does **not** exist:

"Before we can plan development, we need the product roadmap in place.
Please run `/vibeflow:new` to create the project first."

Stop here if the roadmap is missing.

---

## Step 2: Choose Planning Mode

Ask the user for planning mode using `AskUserQuestion`:

**Question: Planning Mode**
"What would you like to do?"
Options:
- "Continue working" — Plan development with existing features
- "Add new feature" — Add a new feature to the roadmap

**If user selects "Continue working":**
Proceed to Step 3: Parse Features.

**If user selects "Add new feature":**
Jump to Step 10: Add New Feature.

---

## Step 3: Parse Features

Read `/specs/roadmap.md` and extract all features with their metadata.

**Parse Features:**
- Extract metadata from list format under each feature heading
- **Fields:** ID, Priority, Status, Dependencies, Phase, Tags
- **Missing metadata:** Use sensible defaults (P1, pending, none, phase-1)

**Feature path construction:** `[feature_slug]` for all features (flat structure)

---

## Step 4: Gather Plan Parameters

Ask the user for planning focus using `AskUserQuestion`:

**Question: Development Focus**
"What should be the focus of this development plan?"
Options:
- "P0 features only" — Critical path to MVP
- "Specific phase" — Then ask which phase
- "All pending features" — Balanced approach
- "Continue in-progress" — Finish what's started

---

## Step 5: Parse and Filter Features

Read the roadmap and extract all features with their metadata:

**For each feature, extract:**
- Feature name
- Feature ID (F001, F002...)
- Priority (P0, P1, P2)
- Status (pending, in_progress, done, blocked)
- Dependencies (array of feature IDs)
- Phase

**Filter features based on focus area:**
- P0 only → `priority === "P0"`
- Specific phase → `phase === selectedPhase`
- All pending → `status !== "done"`
- Continue in-progress → `status === "in_progress"`

---

## Step 6: Resolve Dependencies

Implement topological sort to order features by dependencies:

**Algorithm:**
1. Create a map of feature ID → feature object
2. For each feature, recursively visit its dependencies
3. Add feature to sorted list only after all dependencies are visited
4. Detect and warn about circular dependencies

**Dependency Resolution:**
```python
sorted_features = []
visited = set()

def visit(feature):
    if feature.id in visited:
        return
    visited.add(feature.id)

    for dep_id in feature.dependencies:
        if dep_id in feature_map:
            visit(feature_map[dep_id])

    sorted_features.append(feature)

for feature in filtered_features:
    visit(feature)
```

---

## Step 7: Order Features by Priority

Starting with the highest priority features from the sorted list:

1. Group features by priority (P0 first, then P1, then P2)
2. Within each priority level, maintain dependency order
3. Include feature if all dependencies are either:
   - Already done (status === "done")
   - Already included in this plan

**Track:**
- Selected features
- Blocked features (dependencies not met)
- Deferred features (not in focus area)

---

## Step 8: Generate Development Plan

Format the development plan with the following structure:

```markdown
# Development Plan - [Focus Area]

## Plan Goals
[Summary based on focus area: "Complete P0 features for MVP Foundation"]

## Selected Features ([N] total)

### Priority: P0
1. **[Feature 1]** — [Feature ID]
   - **Dependencies:** [None or list]
   - **Deliverables:** [Key screens, use cases, entities]
   - **Acceptance:** [What "done" looks like]

2. **[Feature 2]** — [Feature ID]
   - **Dependencies:** F001 (must complete [Feature 1] first)
   - **Deliverables:** [Key screens, use cases, entities]
   - **Acceptance:** [What "done" looks like]

### Priority: P1
[Additional features if applicable]

## Blocked Features (Deferred)
[Features that couldn't be included due to unmet dependencies]

1. **[Feature]** — [Feature ID]
   - **Reason:** Waiting on [dependency ID]

## Quick Start

### Setup
```bash
# Build first feature
/vibeflow:feature
# Select "[Feature 1]"
```

### Implementation Order
1. [Feature 1] — Foundation for other features
2. [Feature 2] — Builds on Feature 1
3. [Feature 3] — Independent feature

### Daily Progress Check
```bash
# View overall progress
/vibeflow:status
```

## Plan Notes
- **Date:** [Today's date]
- **Focus:** [Focus area description]
```

---

## Step 9: Present to User

Display the development plan and provide next steps:

"I've generated a development plan for [App Name]:

**Plan Summary:**
- [X] features selected
- [Z] features blocked (dependencies)
- Focus: [Focus area]

**Priority Features:**
1. [Feature 1] — [Deliverables]
2. [Feature 2] — [Deliverables]

**Quick Start:**
```bash
/vibeflow:feature  # Build [Feature 1]
```

Would you like me to:
1. Save this development plan to `specs/plans/plan_[N].md`
2. Adjust the plan parameters (focus area)
3. Start implementing the first feature

**Tip:** Run `/vibeflow:status` anytime to track progress."

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking the user questions — never ask through text
- **Dependency resolution:** Always respect dependencies — a feature cannot be started before its dependencies are done
- **Priority ordering:** P0 > P1 > P2 within same dependency level — see CLAUDE.md
- **Status awareness:** Skip features already marked as "done" in roadmap
- **Blocked features:** Clearly explain why each feature is blocked (missing dependency)
- **Acceptance criteria:** Provide concrete "done" definition for each feature
- **Interactive questions:** Use `AskUserQuestion` with clear options
- **Reference:** All specification formats are in CLAUDE.md

---

## Development Planning Algorithm (Detailed)

**Input:**
- `features`: Array of feature objects with metadata
- `focus`: Filter criteria (priority, phase, status)

**Output:**
- `selected`: Features that match criteria
- `blocked`: Features blocked by dependencies
- `deferred`: Features deferred due to focus criteria

**Pseudocode:**
```
filtered = filter_features(features, focus)
sorted = topological_sort(filtered)  # Resolve dependencies
selected = []
blocked = []
deferred = []

for feature in sorted:
    if can_include(feature, selected):
        selected.append(feature)
    elif dependencies_not_met(feature, selected):
        blocked.append(feature)
    else:
        deferred.append(feature)

return selected, blocked, deferred
```

---

## Edge Cases

**Circular dependencies:**
- Detect and warn user
- Break cycle by removing last dependency link
- Continue planning

**Invalid dependency reference:**
- Warn about missing feature ID
- Remove invalid dependency
- Continue planning

**All features done:**
- Show "All features are complete! Congratulations!"
- Ask user what they'd like to do:
  - "Add a new feature" — Guide them to add feature to roadmap (see Step 10: Add New Feature)
  - "View progress" — Suggest running `/vibeflow:status`
  - "Plan new development" — Re-run planning with different parameters

**Empty roadmap:**
- Show "No features found in roadmap."
- Suggest running `/vibeflow:new` first

---

## Step 10: Add New Feature

When the user selects "Add new feature" in Step 2, or when all features are complete:

**1. Gather Feature Details using AskUserQuestion:**

Ask for feature name, then these questions:

**Question: Priority**
"What is the priority level for this feature?"
- "P0 - Critical" — Core value features, blocking other features
- "P1 - Important" — Supporting features, nice to have soon
- "P2 - Nice to have" — Enhancements, can be deferred

**Question: Dependencies**
"Does this feature depend on any existing features?"
- "No dependencies" — Feature can be built independently
- "Has dependencies" — Ask to list feature IDs (F001, F002, etc.)

**Question: Phase**
"Which release phase does this belong to?"
- "phase-2" — Next release
- "phase-1" — Current release
- "Custom" — Specify phase name

**Question: Tags** (optional - multiSelect)
"What categories apply to this feature?"
- "core" — Core functionality
- "ui" — User interface focus
- "data" — Data/processing focus
- "analytics" — Analytics/reporting

**2. Generate Feature Spec:**

Create the feature spec file at:
`/specs/features/[feature_slug]/spec.md`

```markdown
# [Feature Name] Spec

## Purpose
[Generate based on feature name and context]

## User Flows
1. **[Primary Flow]** — [Describe main user journey]
2. **[Secondary Flow]** — [Alternative or error paths]

## Screens
- **[Screen 1]** — [Purpose and key elements]
- **[Screen 2]** — [Purpose and key elements]

## States
- **Empty** — [When no data exists]
- **Loading** — [During data fetch]
- **Error** — [On failure]
- **Success** — [Normal operation]

## Interactions
- **[Action]** — [Expected response]

## Data Requirements
- [List entities and their access patterns]
- [Entity 1] (read, create, update)
- [Entity 2] (read-only)

## Validation
- [Business rules and constraints]
```

**3. Update Roadmap and Data Shape:**

**Update Roadmap:**
Append the new feature to `/specs/roadmap.md` with auto-incremented ID:

```markdown
### [Feature Name]
- **ID:** F[XXX] (next available number)
- **Priority:** [P0/P1/P2]
- **Status:** pending
- **Dependencies:** [none or F001, F002...]
- **Phase:** [phase-X]
- **Tags:** [core, ui, data, analytics...]
```

**Update Data Shape (if needed):**
If the feature introduces new entities, update `/specs/data_shape.md`:

```markdown
## Entities

### [Entity Name]
[Description...]

## Relationships

- [Relationship description]
```

**4. Confirm and Next Steps:**

"New feature added to roadmap!

**Feature Details:**
- **Name:** [Feature Name]
- **ID:** F[XXX]
- **Priority:** [P0/P1/P2]
- **Phase:** [phase-X]

**What's Next:**
1. Review the generated spec at `/specs/features/[feature_slug]/spec.md`
2. Run `/vibeflow:feature` to start building
3. Or run `/vibeflow:plan` to create a new development plan

Would you like to:
1. Start building this feature now
2. Plan new development with this feature included
3. Add another feature"
