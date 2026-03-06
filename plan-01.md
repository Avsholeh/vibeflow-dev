# Plan: Hierarchical Sub-Features for VibeFlow

## Context

VibeFlow currently uses a **flat feature organization** where all features are listed directly under `features/` with IDs like F001, F002, etc. The user wants to organize features into logical groups/categories for better project structure.

**Current structure:**
```
features/
├── add_task
├── edit_task
├── backup_google_drive
├── backup_dropbox
└── send_email
```

**Desired structure:**
```
features/
├── tasks/
│   ├── add_task
│   └── edit_task
├── backup/
│   ├── google_drive
│   └── dropbox
└── notifications/
    ├── send_email
    └── push_notification
```

## Implementation Approach

### 1. New Roadmap Format with Groups

Add a **`Group`** field to feature metadata and introduce group-level organization:

```markdown
# Product Roadmap

## Features

### Group: Tasks

#### 1. Add Task
- **ID:** F001
- **Group:** tasks
- **Priority:** P0
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core

Quick task creation with title and description.

#### 2. Edit Task
- **ID:** F002
- **Group:** tasks
- **Priority:** P1
- **Status:** pending
- **Dependencies:** F001
- **Phase:** phase-1
- **Tags:** core

Edit existing task details.

### Group: Backup

#### 3. Google Drive Backup
- **ID:** F003
- **Group:** backup
- **Priority:** P1
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-2
- **Tags:** integration

Backup data to Google Drive.

#### 4. Dropbox Backup
- **ID:** F004
- **Group:** backup
- **Priority:** P2
- **Status:** pending
- **Dependencies:** F003
- **Phase:** phase-2
- **Tags:** integration

Backup data to Dropbox.
```

### 2. Feature Slug Enhancement

Feature slugs will now include the group prefix:
- **Without group:** `add_task` → `specs/features/add_task/`
- **With group:** `tasks/add_task` → `specs/features/tasks/add_task/`

**Slug conversion rules:**
1. Group slug: lowercase, spaces → hyphens (e.g., "Tasks" → `tasks`)
2. Feature slug: lowercase, spaces → hyphens (e.g., "Add Task" → `add_task`)
3. Combined: `[group_slug]/[feature_slug]` (e.g., `tasks/add_task`)

### 3. Files to Modify

#### Core Command Files
1. **`.claude/commands/vibeflow/new.md`** (lines 119-169)
   - Update roadmap generation to include `Group` field
   - Add group detection from user input (keywords like "backup", "tasks", "notifications")
   - Generate grouped roadmap format

2. **`.claude/commands/vibeflow/plan.md`** (lines 36-109)
   - Update feature parsing to extract `Group` field
   - Modify dependency resolution to handle group-based paths
   - Update plan generation to show grouped features

3. **`.claude/commands/vibeflow/feature.md`** (lines 72-126)
   - Update feature selection to show grouped hierarchy
   - Modify file paths to use `[group]/[feature]` format
   - Update spec generation with group-aware paths

4. **`.claude/commands/vibeflow/status.md`** (lines 36-111)
   - Update progress calculation for nested feature paths
   - Modify status report to show features grouped by category
   - Add group-level progress summaries

5. **`.claude/commands/vibeflow/validate.md`** (lines 60-94)
   - Add validation for `Group` field
   - Validate group slug format
   - Check for duplicate group names

#### Documentation File
6. **`CLAUDE.md`** - Multiple sections need updates:
   - **Roadmap Format** (lines 254-306): Add `Group` field to metadata table
   - **File Structure Patterns** (lines 604-660): Update spec and implementation paths
   - **Progress Calculation** (lines 195-227): Update file path checks
   - **Feature Slug Conversion** (lines 179-192): Add group slug rules

### 4. New Metadata Field

| Field | Type | Values | Description |
|-------|------|--------|-------------|
| `Group` | string | tasks, backup, notifications, etc. | Logical grouping for features |

### 5. Backward Compatibility

- **Optional field:** `Group` is optional; existing roadmaps without groups continue to work
- **Default behavior:** Features without `Group` are placed directly in `features/`
- **Flat fallback:** If no groups are defined, use the original flat structure

### 6. File Structure Updates

**Spec files:**
```
specs/features/
├── [group_slug]/
│   └── [feature_slug]/
│       ├── spec.md
│       ├── data.json
│       └── models.md
└── [feature_slug]/  # for features without group
    ├── spec.md
    ├── data.json
    └── models.md
```

**Implementation files:**
```
lib/features/
├── [group_slug]/
│   └── [feature_slug]/
│       ├── domain/
│       ├── data/
│       ├── providers/
│       ├── screens/
│       └── widgets/
└── [feature_slug]/  # for features without group
    └── ...
```

## Critical Files Summary

| File | Changes |
|------|---------|
| `.claude/commands/vibeflow/new.md` | Add group detection, update roadmap generation format |
| `.claude/commands/vibeflow/plan.md` | Parse Group field, update dependency resolution, grouped plan output |
| `.claude/commands/vibeflow/feature.md` | Update feature selection UI, use nested paths for generation |
| `.claude/commands/vibeflow/status.md` | Add group-aware progress calculation, grouped status report |
| `.claude/commands/vibeflow/validate.md` | Add Group field validation |
| `CLAUDE.md` | Update Roadmap Format, File Structure, Progress Calculation, Feature Slug sections |

## Verification

1. **Test new app creation:** Run `/vibeflow:new` and verify grouped roadmap generation
2. **Test plan generation:** Run `/vibeflow:plan` and verify grouped features in plan
3. **Test feature building:** Run `/vibeflow:feature` and verify correct nested paths
4. **Test status tracking:** Run `/vibeflow:status` and verify grouped progress report
5. **Test validation:** Run `/vibeflow:validate` and verify Group field validation
6. **Backward compatibility:** Test with existing flat roadmap (no groups)

## Notes

- The `Phase` field is kept separate from `Group` - phases are for release planning, groups for logical organization
- Feature IDs (F001, F002) remain sequential across all groups, not per-group
- The `Group` field is opt-in; users can continue using flat structure if preferred
