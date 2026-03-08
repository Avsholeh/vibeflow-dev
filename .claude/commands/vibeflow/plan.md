# Development Planning

Create development plans based on roadmap features, priorities, and dependencies.

## Step 1: Check Prerequisites

Check if `/specs/roadmap.md` exists. If not: "Please run `/vibeflow:new` first."

---

## Step 2: Choose Planning Mode

Use AskUserQuestion:
- "Continue working" — Plan with existing features
- "Add new feature" — Add feature to roadmap

---

## Step 3: Parse Features

Read `/specs/roadmap.md` and extract features with metadata:
- Fields: ID, Priority, Status, Dependencies, Phase, Tags, Module
- Support module-grouped (### Module, #### Feature) and flat (### Feature) formats
- Use sensible defaults for missing metadata
- Infer module from parent heading

**Feature path construction:** `[module_slug]/[feature_slug]` — See `ALGORITHMS.md`

---

## Step 4: Gather Plan Parameters

Use AskUserQuestion for focus area:
- "All pending features"
- "P0 features only"
- "Specific phase"
- "Continue in-progress"

---

## Step 5: Filter & Resolve Dependencies

**Filter features** based on focus area.

**Resolve dependencies** using topological sort — See `ALGORITHMS.md` for algorithm.

**Edge cases:**
- Circular dependencies → Warn, break cycle, continue
- Invalid dependency → Warn, remove, continue
- All features done → Suggest adding features or viewing status
- Empty roadmap → Suggest running `/vibeflow:new`

---

## Step 6: Order by Priority

Group by priority (P0 > P1 > P2), maintain dependency order within each level.

Include feature if all dependencies are:
- Already done (status === "done")
- Already included in this plan

Track: selected, blocked, deferred features.

---

## Step 7: Generate Development Plan

Create `specs/plans/plan_[N].md` with:

```markdown
# Development Plan - [Focus Area]

## Plan Goals
[Summary based on focus area]

## Selected Features ([N] total)

### Priority: P0
1. **[Feature 1]** — [Feature ID]
   - **Dependencies:** [None or list]
   - **Deliverables:** [Key screens, entities]
   - **Acceptance:** [What "done" looks like]

### Priority: P1
[Additional features if applicable]

## Blocked Features (Deferred)
[Features with unmet dependencies]

## Quick Start

### Setup
/vibeflow:feature  # Select first feature

### Implementation Order
1. [Feature 1] — Foundation
2. [Feature 2] — Builds on Feature 1

### Daily Progress Check
/vibeflow:status

## Plan Notes
- **Date:** [Today's date]
- **Focus:** [Focus area description]
```

---

## Step 8: Present to User

Display summary with:
- X features selected
- Z features blocked
- Focus area
- Quick start command

Use AskUserQuestion for next action:
- Save plan
- Adjust parameters
- Start implementing first feature

---

## Step 9: Add New Feature (Optional)

When user selects "Add new feature":

**1. Gather Details** using AskUserQuestion:
- Feature name
- Priority (P0/P1/P2)
- Dependencies (feature IDs)
- Phase (phase-1, phase-2, custom)
- Tags (core, ui, data, analytics) — multiSelect

**2. Generate Spec** — See `SPEC_FORMATS.md` for template

**3. Update Roadmap:**
Append new feature with auto-incremented ID.

**4. Confirm & Next Steps:**
Review spec, build feature, or plan new development.

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion — see CLAUDE.md
- **Dependency resolution:** Respect dependencies — see `ALGORITHMS.md`
- **Priority ordering:** P0 > P1 > P2 — see CLAUDE.md
- **Status awareness:** Skip "done" features
- **Spec format:** See `SPEC_FORMATS.md`
