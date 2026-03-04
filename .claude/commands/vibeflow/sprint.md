# Sprint Planning

You are helping the user create sprint plans for their Flutter app based on roadmap features, effort estimates, priorities, and dependencies.

## Step 1: Check Prerequisites

First, check if `/specs/roadmap.md` exists.

If it does **not** exist:

"Before we can plan sprints, we need the product roadmap in place.
Please run `/vibeflow:new` to create the project first."

Stop here if the roadmap is missing.

---

## Step 2: Parse Features

Read `/specs/roadmap.md` and extract all features with their metadata.

**Parse Features:**
- Extract metadata from list format under each feature heading
- **Fields:** ID, Priority, Effort, Status, Dependencies, Phase, Tags
- **Missing metadata:** Use sensible defaults (P1, medium, pending, none, phase-1)

---

## Step 3: Gather Sprint Parameters

Ask the user for sprint planning parameters using `AskUserQuestion`:

**Question 1: Sprint Duration**
"How long is your sprint?"
Options:
- "1 week" (Capacity: ~6 points)
- "2 weeks" (Capacity: ~10 points) — Recommended
- "1 month" (Capacity: ~20 points)
- "Custom" — Ask for number of weeks

**Question 2: Focus Area**
"What should be the focus of this sprint?"
Options:
- "P0 features only" — Critical path to MVP
- "Specific phase" — Then ask which phase
- "All pending features" — Balanced approach
- "Continue in-progress" — Finish what's started

---

## Step 4: Parse and Filter Features

Read the roadmap and extract all features with their metadata:

**For each feature, extract:**
- Feature name
- Feature ID (F001, F002...)
- Priority (P0, P1, P2)
- Effort (small, medium, large, xlarge)
- Status (pending, in_progress, done, blocked)
- Dependencies (array of feature IDs)
- Phase

**Convert effort to points:**
- `small` → 1 point
- `medium` → 3 points
- `large` → 5 points
- `xlarge` → 8 points

**Filter features based on focus area:**
- P0 only → `priority === "P0"`
- Specific phase → `phase === selectedPhase`
- All pending → `status !== "done"`
- Continue in-progress → `status === "in_progress"`

---

## Step 5: Resolve Dependencies

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

## Step 6: Select Features for Sprint

Starting with the highest priority features from the sorted list:

1. Calculate remaining capacity
2. Add feature if:
   - Feature's points <= remaining capacity
   - All dependencies are either:
     - Already done (status === "done")
     - Already included in this sprint
3. Subtract feature points from capacity
4. Continue until capacity is exhausted or no more features fit

**Track:**
- Selected features
- Blocked features (dependencies not met)
- Deferred features (not enough capacity)

---

## Step 7: Generate Sprint Plan

Format the sprint plan with the following structure:

```markdown
# [Duration] Sprint Plan ([Capacity] points)

## Sprint Goals
[Summary based on focus area: "Complete P0 features for MVP Foundation"]

## Selected Features ([Total Points]/[Capacity] points)

### Priority: P0
1. **[Feature 1]** ([X] points) — [Feature ID]
   - **Dependencies:** [None or list]
   - **Deliverables:** [Key screens, logic, data]
   - **Acceptance:** [What "done" looks like]

2. **[Feature 2]** ([X] points) — [Feature ID]
   - **Dependencies:** F001 (must complete [Feature 1] first)
   - **Deliverables:** [Key screens, logic, data]
   - **Acceptance:** [What "done" looks like]

### Remaining Capacity
[Any lower priority features that fit]

## Blocked Features (Deferred)
[Features that couldn't be included due to unmet dependencies or capacity]

1. **[Feature]** ([X] points) — [Feature ID]
   - **Reason:** Waiting on [dependency ID] or insufficient capacity

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

## Sprint Notes
- **Start Date:** [Today's date]
- **Target End Date:** [Calculated based on duration]
- **Focus:** [Focus area description]
```

---

## Step 8: Present to User

Display the sprint plan and provide next steps:

"I've generated a [duration] sprint plan for [App Name]:

**Sprint Summary:**
- [X] features selected ([Y] points)
- [Z] features blocked (dependencies)
- Capacity: [Y] points / [capacity] available

**Priority Features:**
1. [Feature 1] — [Deliverables]
2. [Feature 2] — [Deliverables]

**Quick Start:**
```bash
/vibeflow:feature  # Build [Feature 1]
```

Would you like me to:
1. Save this sprint plan to `specs/sprints/sprint_[N].md`
2. Adjust the sprint parameters (duration, focus)
3. Start implementing the first feature

**Tip:** Run `/vibeflow:status` anytime to track progress through the sprint."

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking the user questions — never ask through text
- **Effort estimation:** Use roadmap values or default to medium (3 points) — see CLAUDE.md
- **Capacity calculation:** 1 week = 6 points, 2 weeks = 10 points, 1 month = 20 points
- **Dependency resolution:** Always respect dependencies — a feature cannot be started before its dependencies are done
- **Priority ordering:** P0 > P1 > P2 within same dependency level — see CLAUDE.md
- **Status awareness:** Skip features already marked as "done" in roadmap
- **Blocked features:** Clearly explain why each feature is blocked (missing dependency or capacity)
- **Acceptance criteria:** Provide concrete "done" definition for each feature
- **Backward compatible:** Works with legacy roadmaps (using default effort values)
- **Interactive questions:** Use `AskUserQuestion` with clear options for each parameter
- **Reference:** All specification formats are in CLAUDE.md

---

## Sprint Planning Algorithm (Detailed)

**Input:**
- `features`: Array of feature objects with metadata
- `duration`: Sprint duration in weeks
- `focus`: Filter criteria (priority, phase, status)

**Output:**
- `selected`: Features that fit in sprint
- `blocked`: Features blocked by dependencies
- `deferred`: Features deferred due to capacity

**Pseudocode:**
```
capacity = calculate_capacity(duration)
filtered = filter_features(features, focus)
sorted = topological_sort(filtered)  # Resolve dependencies
selected = []
blocked = []
deferred = []

for feature in sorted:
    if can_include(feature, selected, capacity):
        selected.append(feature)
        capacity -= feature.points
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
  - "Add a new feature" — Guide them to add feature to roadmap (see Step 9: Add New Feature)
  - "View progress" — Suggest running `/vibeflow:status`
  - "Plan new sprint" — Re-run sprint planning with different parameters

**No features fit:**
- Show "Smallest feature ([X] points) exceeds capacity ([Y] points)"
- Suggest increasing duration

**Empty roadmap:**
- Show "No features found in roadmap."
- Suggest running `/vibeflow:new` first

---

## Step 9: Add New Feature (When All Features Done)

When all features are complete and the user selects "Add a new feature":

**1. Gather Feature Details using AskUserQuestion:**

Ask for feature name, then these questions:

**Question: Priority**
"What is the priority level for this feature?"
- "P0 - Critical" — Core value features, blocking other features
- "P1 - Important" — Supporting features, nice to have soon
- "P2 - Nice to have" — Enhancements, can be deferred

**Question: Effort**
"How much effort is required for this feature?"
- "Small (1 point)" — 1-2 days
- "Medium (3 points)" — 3-5 days — Recommended default
- "Large (5 points)" — 1-2 weeks
- "X-Large (8 points)" — 2-4 weeks

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

Create the feature spec file at `/specs/features/[feature_slug]/spec.md`:

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

**3. Update Roadmap:**

Append the new feature to `/specs/roadmap.md` with auto-incremented ID:

```markdown
### [Feature Name]
- **ID:** F[XXX] (next available number)
- **Priority:** [P0/P1/P2]
- **Effort:** [small/medium/large/xlarge]
- **Status:** pending
- **Dependencies:** [none or F001, F002...]
- **Phase:** [phase-X]
- **Tags:** [core, ui, data, analytics...]
```

**4. Confirm and Next Steps:**

"New feature added to roadmap!

**Feature Details:**
- **Name:** [Feature Name]
- **ID:** F[XXX]
- **Priority:** [P0/P1/P2]
- **Effort:** [X] points
- **Phase:** [phase-X]

**What's Next:**
1. Review the generated spec at `/specs/features/[feature_slug]/spec.md`
2. Run `/vibeflow:feature` to start building
3. Or run `/vibeflow:sprint` to plan a new sprint

Would you like to:
1. Start building this feature now
2. Plan a new sprint with this feature included
3. Add another feature"
