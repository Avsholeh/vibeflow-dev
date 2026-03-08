# Development Status

Track implementation progress by analyzing roadmap, feature specs, and code implementation.

## Step 1: Check Prerequisites

Check if `/specs/roadmap.md` exists. If not: "Please run `/vibeflow:new` first."

---

## Step 2: Parse Roadmap

Read `/specs/roadmap.md` and extract all features:
- Support module-grouped (### Module, #### Feature) and flat (### Feature) formats
- Fields: ID, Priority, Status, Dependencies, Phase, Tags, Module
- Assign sensible defaults for missing metadata

**Feature path construction:** See `ALGORITHMS.md` for slug conversion.

---

## Step 3: Check Implementation Status

For each feature, check:

**Spec Status:**
- `spec.md` exists → +30%
- `data.json` exists → +30%
- `models.md` exists → +12%

**Implementation Status:**
- `lib/domain/entities/` has entity files → +24%
- `lib/presentation/screens/` has screen files → +12%

**Progress formula:** See `ALGORITHMS.md`

**Status indicators:**
- `[X]` — Complete/Exists
- `[ ]` — Not started
- `[~]` — Partial

---

## Step 4: Generate Status Report

```markdown
# [App Name] Development Status

## Phase 1: [Phase Name]

| Feature | ID | Priority | Spec | Data | Types | Entities | Screens | Progress |
|---------|----|----------|------|------|-------|----------|---------|----------|
| [Feature 1] | F001 | P0 | [X] | [X] | [X] | [X] | [ ] | 72% |

**Phase Progress:** [X]/[Y] features started ([average]%)

## Overall Summary
- **Total Features:** [N]
- **Completed:** [X] features ([Y]%)
- **In Progress:** [X] features
- **Not Started:** [X] features

## Next Steps
1. Complete [Feature Name] screens (currently [X]%)
2. Generate sample data for [Feature Name]
3. Implement entities for [Feature Name]

## Feature Details
### [Feature Name] (F001) - [X]%
- [X] Spec complete
- [X] Sample data generated
- [ ] Entities defined
- [ ] Screens implemented
```

---

## Step 5: Present to User

Display status report with:
- Overall completion percentage
- Quick summary (completed, in progress, not started)
- Priority items needing attention (P0 features not done)
- Next steps (plan, feature commands)

---

## Important Notes

- **Auto-detection:** Status determined by file existence
- **Feature IDs:** Use from roadmap or auto-generate
- **Phase grouping:** Group by phase if defined
- **Priority focus:** Highlight P0 features needing attention
- **Actionable next steps:** Provide concrete actions based on state
- **Slug conversion:** See `ALGORITHMS.md`
- **Progress formula:** See `ALGORITHMS.md`
- **Non-existent lib/:** All implementation checks return false

---

## Edge Cases

**Missing feature path:** Directory doesn't exist → 0% progress, not started

**Missing metadata:** Use sensible defaults (P1, pending, none, phase-1)

**Empty roadmap:** Show "No features found. Please add features first."
