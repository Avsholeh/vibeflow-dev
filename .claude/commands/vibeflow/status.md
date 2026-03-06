# Development Status

You are helping the user track the implementation progress of their Flutter app by analyzing the roadmap, feature specs, and code implementation.

## Step 1: Check Prerequisites

First, check if `/specs/roadmap.md` exists.

If it does **not** exist:

"Before we can check development status, we need the product roadmap in place.
Please run `/vibeflow:new` to create the project first."

Stop here if the roadmap is missing.

---

## Step 2: Parse Roadmap

Read `/specs/roadmap.md` and extract all features.

**Parse Features:**
- Extract metadata from list format under each feature heading
- **Fields available:** ID, Priority, Effort, Status, Dependencies, Phase, Tags
- **Missing metadata:** Assign sensible defaults (P1, medium, pending, none, phase-1)

Extract for each feature:
- Feature name (from heading)
- Feature ID (from list or auto-generate as F001, F002...)
- Priority (P0, P1, P2 or default P1)
- Effort (small, medium, large, xlarge or default medium)
- Phase (from list or default "phase-1")

---

## Step 3: Check Implementation Status

For each feature, check the following paths:

**Spec Status:**
- `/specs/features/[feature_slug]/spec.md` exists → Spec complete (+25%)
- `/specs/features/[feature_slug]/data.json` exists → Sample data (+25%)
- `/specs/features/[feature_slug]/models.md` exists → Types documented (+10%)

**Implementation Status:**
- `/lib/features/[feature_slug]/domain/models/` has .dart files → Models defined (+20%)
- `/lib/features/[feature_slug]/providers/` has non-empty .dart files → Logic implemented (+15%)
- `/lib/features/[feature_slug]/screens/*_page.dart` files exist → Screens implemented (+10%)

**Feature slug conversion:**
- Convert feature name to lowercase, replace spaces with hyphens
- Example: "Add Transaction Flow" → `add_transaction_flow`

**Progress Calculation:**
```
progress = (spec_complete * 25) + (sample_data * 25) + (types_documented * 10) + (models_defined * 20) + (logic_implemented * 15) + (screens_implemented * 10)
```

**Determine Overall Status:**
- `progress = 0` → `pending`
- `0 < progress < 50` → `started`
- `50 <= progress < 100` → `in_progress`
- `progress = 100` → `done`

---

## Step 4: Generate Status Report

Format the status report with the following structure:

```markdown
# [App Name] Development Status

## Phase 1: [Phase Name]

| Feature | ID | Priority | Spec | Data | Types | Models | Logic | Screens | Progress |
|---------|----|----------|------|------|-------|--------|-------|---------|----------|
| [Feature 1] | F001 | P0 | [X] | [X] | [X] | [X] | [ ] | [ ] | 80% |
| [Feature 2] | F002 | P1 | [X] | [ ] | [ ] | [ ] | [ ] | [ ] | 25% |

**Phase Progress: [X]/[Y] features started ([average]% complete)**

[Repeat for each phase...]

## Overall Summary

- **Total Features:** [N]
- **Completed:** [X] features ([Y]%)
- **In Progress:** [X] features
- **Not Started:** [X] features

## Next Steps

[Based on current progress, recommend 3-5 actionable next steps such as:]
1. Complete [Feature Name] screens (currently at [X]%)
2. Generate sample data for [Feature Name]
3. Implement models for [Feature Name]
4. Add business logic for [Feature Name]

## Feature Details

[For features in progress, show breakdown:]

### [Feature Name] (F001) - [X]%
- [X] Spec complete
- [X] Sample data generated
- [ ] Models defined
- [ ] Logic implemented
- [ ] Screens implemented
```

**Status Indicators:**
- `[X]` — Complete/Exists
- `[ ]` — Not started/Does not exist
- `[~]` — Partial (for manual override when detected as incomplete but some work exists)

---

## Step 5: Present to User

Display the status report and provide guidance:

"[App Name] development is at [overall]% completion.

**Quick Summary:**
- [X] features completed
- [Y] features in progress
- [Z] features not started

**Priority Items:**
The following P0 features need attention:
- [List P0 features that aren't done]

You can update feature status directly in the roadmap file, or continue implementing with:
- `/vibeflow:plan` — Plan your next development
- `/vibeflow:feature` — Build a complete feature

Would you like details on any specific feature, or help with the next implementation step?"

---

## Important Notes

- **Auto-detection:** Status is determined by file existence, not manual entry
- **Feature IDs:** Use IDs from roadmap or auto-generate for tracking
- **Phase grouping:** Group features by phase if defined in roadmap
- **Priority focus:** Highlight P0 features that need attention
- **Actionable next steps:** Always provide concrete next actions based on current state
- **Slug conversion:** Be careful with special characters when converting feature names to slugs — see CLAUDE.md
- **Non-existent lib/features/:** If implementation directory doesn't exist at all, all implementation checks return false
- **Partial implementation:** Use `[~]` indicator when some files exist but not all in a category
- **Progress calculation:** See CLAUDE.md for detailed progress calculation formula
- **Reference:** All specification formats are in CLAUDE.md

---

## Edge Cases

**Missing feature slug directory:**
- If `/specs/features/[slug]/` doesn't exist → 0% progress, treat as not started

**Missing metadata:**
- If some fields are missing from the feature list → Use sensible defaults (P1, medium, pending, none, phase-1)

**Circular dependencies:**
- Not relevant for status checking (dependency resolution is for development planning)

**Empty roadmap:**
- Show "No features found in roadmap. Please add features first."
