# Validation Command

Validate VibeFlow specification files for format consistency and completeness.

## Step 1: Check Prerequisites

Check if `specs/` directory exists. If not: "Please run `/vibeflow:new` first."

---

## Step 2: Validate Product Overview

Check `specs/overview.md` for required sections:
- App Name, Description, Problems, Solutions, Target Users, Key Features

**Report:** ✅ Valid / ⚠️ Missing sections / ❌ Not found

---

## Step 3: Validate Roadmap

Check `specs/roadmap.md`:
- Features section with ## Features heading
- Module groupings (### [Module Name]) - optional but recommended
- Feature metadata: ID (F###), Priority (P0/P1/P2), Status, Dependencies, Phase, Tags
- Sequential feature IDs, no duplicates, valid dependency references

**Report:** ✅ Valid / ⚠️ Warnings (list issues) / ❌ Not found

---

## Step 4: Validate Data Shape

Check `specs/data_shape.md`:
- ## Entities section
- ## Relationships section
- Entity names, descriptions, properties lists

**Report:** ✅ Valid / ⚠️ Warnings / ❌ Not found

---

## Step 5: Validate Design System

Check `specs/design_system/`:
- `colors.json` exists, valid JSON, has light/dark/semantic/custom sections, valid hex codes
- `typography.json` exists, valid JSON, has fontFamilies/textStyles/weights/sizes

**Report:** ✅ Valid / ⚠️ Warnings / ❌ Missing files

**Complete formats:** See `SPEC_FORMATS.md`

---

## Step 6: Validate Feature Specs

Check each `specs/modules/[module]/[feature_slug]/`:
- `spec.md` exists with required sections (Purpose, User Flows, Screens, States, Interactions, Data Requirements)
- `data.json` exists and is valid JSON
- `models.md` exists with Dart model definitions

**Report:** ✅ All features valid / ⚠️ Feature issues (list) / ❌ No features found

**Complete format:** See `SPEC_FORMATS.md`

---

## Step 7: Generate Validation Report

Present comprehensive report with:

```markdown
# VibeFlow Validation Report

## Overview
- Product Overview: [Status]
- Roadmap: [Status]
- Data Shape: [Status]
- Design System: [Status]
- Feature Specs: [Status]

## Detailed Results
### [Each Section]
[✅ Valid / ⚠️ Warnings / ❌ Errors]
[Specific counts and issues]

## Recommendations
[Actionable suggestions]
```

---

## Step 8: Present to User

Display validation report with quick actions:
- Fix issues and run `/vibeflow:validate` again
- Use `/vibeflow:feature` to complete missing feature files
- Use `/vibeflow:theme` to fix design system issues

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion — see CLAUDE.md
- **Read-only:** Only reads files, never modifies them
- **Comprehensive:** Validates all spec files for format and completeness
- **Helpful errors:** Provide specific guidance on fixes
- **Reference:** All formats in `SPEC_FORMATS.md`
