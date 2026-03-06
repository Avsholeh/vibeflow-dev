# Validation Command

You are helping the user validate their VibeFlow specification files for format consistency and completeness.

## Step 1: Check Prerequisites

Check if `specs/` directory exists.

If it does **not** exist:

"No specs directory found. Please run `/vibeflow:new` to create your project first."

Stop here if specs directory is missing.

---

## Step 2: Validate Product Overview

Check `specs/overview.md`:

**Required sections:**
- App Name
- Description
- Problems
- Solutions
- Target Users
- Key Features

**Validation:**
```markdown
# Product Overview

## App Name
[App Name]

## Description
[Description]

## Problems
- [Problem]

## Solutions
- [Solution]

## Target Users
[Target Users]

## Key Features
1. [Feature]
2. [Feature]
3. [Feature]
```

**Report:**
- ✅ Valid — All required sections present
- ⚠️ Missing sections: [list]
- ❌ Not found

---

## Step 3: Validate Roadmap

Check `specs/roadmap.md`:

**Required format:**
- Features section with ## Features heading
- Each feature has numbered heading (### 1., ### 2., etc. or #### 1., #### 2., etc. under groups)
- Each feature has metadata fields:
  - ID (F### format)
  - Group (optional - lowercase with underscores/hyphens)
  - Priority (P0, P1, or P2)
  - Effort (small, medium, large, or xlarge)
  - Status (pending, started, in_progress, done, or blocked)
  - Dependencies (none or F###, F### list)
  - Phase (phase-1, phase-2, etc.)
  - Tags (comma-separated list)

**Validation checks:**
- Feature IDs are sequential (F001, F002, F003...)
- No duplicate feature IDs
- All dependencies reference valid feature IDs
- Status values are valid
- Priority values are valid
- Effort values are valid
- Group slugs are valid format (lowercase, hyphens/underscores only) - if present
- Group sections have proper headings (if groups are used)

**Report:**
- ✅ Valid — [N] features with valid metadata
- ⚠️ Warnings:
  - Duplicate IDs: [list]
  - Invalid dependencies: [list]
  - Invalid status values: [list]
  - Invalid priority values: [list]
  - Invalid effort values: [list]
  - Invalid group slugs: [list]
  - Inconsistent group formatting: [list]
- ❌ Not found or malformed

---

## Step 4: Validate Data Shape

Check `specs/data_shape.md`:

**Required sections:**
- ## Entities section
- ## Relationships section
- Each entity has name and description
- Each entity has properties list with **Property:** format

**Validation checks:**
- Entities referenced in relationships exist
- Relationship format is consistent

**Report:**
- ✅ Valid — [N] entities with proper format
- ⚠️ Warnings:
  - Orphaned entities in relationships
  - Inconsistent formats
- ❌ Not found or malformed

---

## Step 5: Validate Design System

Check if design system files exist:

**Required files:**
- `specs/design_system/colors.json`
- `specs/design_system/typography.json`

**Validation checks for colors.json:**
- Valid JSON format
- Contains light section
- Contains dark section
- Contains semantic section
- Contains custom section
- All color values are valid hex codes (#RRGGBB or #RRGGBBAA)

**Validation checks for typography.json:**
- Valid JSON format
- Contains fontFamilies section
- Contains textStyles section
- Contains weights section
- Contains sizes section
- Font family names are valid

**Report:**
- ✅ Valid — Both files present with correct format
- ⚠️ Warnings:
  - Missing sections: [list]
  - Invalid color codes: [list]
  - Invalid font names: [list]
- ❌ Missing files or invalid format

---

## Step 6: Validate Feature Specs

Check each feature directory in `specs/features/`:

**For grouped features:**
- Check `specs/features/[group_slug]/[feature_slug]/` directories

**For ungrouped features:**
- Check `specs/features/[feature_slug]/` directories

For each feature directory:
- `spec.md` exists and has required sections (Purpose, User Flows, Screens, States, Interactions, Data Requirements)
- `data.json` exists and is valid JSON
- `models.md` exists and contains Dart model definitions

**Report:**
- ✅ All features valid — [N] features complete
- ⚠️ Feature issues:
  - [Group/Feature]: Missing spec.md
  - [Group/Feature]: Invalid data.json
  - [Group/Feature]: Missing models.md
- ❌ No features found

---

## Step 7: Generate Validation Report

Present a comprehensive validation report:

```markdown
# VibeFlow Validation Report

## Overview
- Product Overview: [Status]
- Roadmap: [Status]
- Data Shape: [Status]
- Design System: [Status]
- Feature Specs: [Status]

## Detailed Results

### Product Overview
[✅ Valid / ⚠️ Warnings / ❌ Errors]

### Roadmap
[✅ Valid / ⚠️ Warnings / ❌ Errors]
- Features: [N] total
- Issues: [list if any]

### Data Shape
[✅ Valid / ⚠️ Warnings / ❌ Errors]
- Entities: [N] total
- Issues: [list if any]

### Design System
[✅ Valid / ⚠️ Warnings / ❌ Errors]
- Colors: [Status]
- Typography: [Status]
- Issues: [list if any]

### Feature Specs
[✅ Valid / ⚠️ Warnings / ❌ Errors]
- Features validated: [N] total
- Issues: [list if any]

## Recommendations
[Actionable suggestions based on findings]
```

---

## Step 8: Present to User

Display the validation report and provide next steps:

"[Validation Summary]

**Overall Status:** [All Valid / Has Warnings / Has Errors]

**Quick Actions:**
- Fix issues and run `/vibeflow:validate` again
- Use `/vibeflow:feature` to complete missing feature files
- Use `/vibeflow:theme` to fix design system issues

**Tip:** Run validation anytime after making changes to ensure consistency."

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking the user questions — never ask through text
- **Read-only:** This command only reads files, never modifies them
- **Comprehensive:** Validates all VibeFlow spec files for format and completeness
- **Helpful errors:** Provide specific guidance on how to fix any issues found
- **Reference:** All specification formats are documented in CLAUDE.md
