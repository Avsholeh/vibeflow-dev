# VibeFlow Specification Formats

This file contains all specification format templates used across VibeFlow. These are the single source of truth for spec structures.

---

## Product Overview Format

**File:** `specs/overview.md`

```markdown
# Product Overview

## App Name
[App Name]

## Description
[User's description]

## Problems
- [Main problem from user input]

## Solutions
- [How the app solves the problem]

## Target Users
[User's description]

## Key Features
[Infer from description or use user's list]

### [Module A]
1. [Feature 1]
2. [Feature 2]
3. [Feature 3]

### [Module B]
1. [Feature 1]
2. [Feature 2]
3. [Feature 3]
```

---

## Roadmap Format

**File:** `specs/roadmap.md`

```markdown
# Product Roadmap

## Features

### [Module A]

#### 1. [Feature Name]
- **ID:** F001
- **Priority:** P0
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core, ui

[Feature description...]

#### 2. [Feature Name]
- **ID:** F002
- **Priority:** P0
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core

[Feature description...]
```

**Metadata Fields:**

| Field | Type | Values | Description |
|-------|------|--------|-------------|
| `ID` | string | F001, F002, F003... | Unique feature identifier (auto-increment) |
| `Priority` | enum | P0, P1, P2 | Development priority |
| `Status` | enum | pending, started, in_progress, done, blocked | Implementation status |
| `Dependencies` | list | F001, F002... or "none" | Feature IDs this feature depends on |
| `Phase` | string | phase-1, phase-2... | Release phase assignment |
| `Tags` | list | core, ui, data, analytics... | Optional categorization |

**Priority Levels:**
- `P0` — Critical: Core value features, blocking other features
- `P1` — Important: Supporting features, nice to have soon
- `P2` — Nice to have: Enhancements, can be deferred

**Status Values:**
- `pending` — Not started (0%)
- `started` — Some work done, less than 50%
- `in_progress` — Work in progress, 50-99%
- `done` — Complete, 100%
- `blocked` — Cannot proceed due to dependencies

---

## Data Shape Format

**File:** `specs/data_shape.md`

```markdown
# Data Shape

## Entities

### [Entity 1]
[Description based on its role in the app]

**Properties:**
- `id` — Unique identifier
- `name` — Display name
- [Other properties...]

### [Entity 2]
[Description...]

## Relationships

- [Entity 1] has many [Entity 2]
- [Entity 2] belongs to [Entity 1]
```

---

## Feature Spec Format

**File:** `specs/modules/[module]/[feature_slug]/spec.md`

```markdown
# [Feature Name] Spec

## Purpose
[What this feature does and why it exists]

## User Flows
1. **[Flow 1]** — [Typical user journey]
2. **[Flow 2]** — [Alternative path]

## Screens
- **[Screen 1]** — [Purpose]
- **[Screen 2]** — [Purpose]

## States
- **Empty** — [When no data exists]
- **Loading** — [During data fetch]
- **Error** — [On failure]
- **Success** — [Normal operation]

## Interactions
- **[Action]** — [Response]

## Data Requirements
- [Entity 1] (read, create)
- [Entity 2] (read-only)

## Validation
- [Business rules and constraints]
```

---

## Design System Format

### Design System Spec

**File:** `specs/design_system/design_system.md`

```markdown
# Design System — [App Name]

## Aesthetic
[Description of visual style]

## Color Philosophy
[How colors are used]

## Typography Philosophy
[Font choices and readability]

## Component Patterns
[Key UI patterns]

## Motion & Interaction
[Animation patterns]
```

### Colors JSON

**File:** `specs/design_system/colors.json`

Must follow Material 3 format:

```json
{
  "light": {
    "primary": "#3B82F6",
    "onPrimary": "#FFFFFF",
    "primaryContainer": "#DBEAFE",
    "onPrimaryContainer": "#1E3A8A",
    "secondary": "#64748B",
    "onSecondary": "#FFFFFF",
    "secondaryContainer": "#F1F5F9",
    "onSecondaryContainer": "#0F172A",
    "tertiary": "#10B981",
    "onTertiary": "#FFFFFF",
    "tertiaryContainer": "#D1FAE5",
    "onTertiaryContainer": "#064E3B",
    "error": "#EF4444",
    "onError": "#FFFFFF",
    "errorContainer": "#FEE2E2",
    "onErrorContainer": "#7F1D1D",
    "background": "#FAFAFA",
    "onBackground": "#0F172A",
    "surface": "#FFFFFF",
    "onSurface": "#0F172A",
    "surfaceVariant": "#F8FAFC",
    "onSurfaceVariant": "#64748B",
    "outline": "#E2E8F0",
    "outlineVariant": "#F1F5F9",
    "shadow": "#0A000000",
    "scrim": "#80000000",
    "inverseSurface": "#1E293B",
    "onInverseSurface": "#F1F5F9",
    "inversePrimary": "#60A5FA"
  },
  "dark": {
    "primary": "#60A5FA",
    "onPrimary": "#FFFFFF",
    "primaryContainer": "#1E3A8A",
    "onPrimaryContainer": "#DBEAFE",
    "secondary": "#94A3B8",
    "onSecondary": "#0F172A",
    "secondaryContainer": "#1E293B",
    "onSecondaryContainer": "#F1F5F9",
    "tertiary": "#34D399",
    "onTertiary": "#064E3B",
    "tertiaryContainer": "#064E3B",
    "onTertiaryContainer": "#D1FAE5",
    "error": "#F87171",
    "onError": "#7F1D1D",
    "errorContainer": "#7F1D1D",
    "onErrorContainer": "#FEE2E2",
    "background": "#0F172A",
    "onBackground": "#F1F5F9",
    "surface": "#0F172A",
    "onSurface": "#F1F5F9",
    "surfaceVariant": "#1E293B",
    "onSurfaceVariant": "#94A3B8",
    "outline": "#334155",
    "outlineVariant": "#1E293B",
    "shadow": "#1AFFFFFF",
    "scrim": "#88000000",
    "inverseSurface": "#F1F5F9",
    "onInverseSurface": "#0F172A",
    "inversePrimary": "#1E3A8A"
  },
  "semantic": {
    "success": "#10B981",
    "onSuccess": "#FFFFFF",
    "successContainer": "#D1FAE5",
    "onSuccessContainer": "#064E3B",
    "warning": "#F59E0B",
    "onWarning": "#FFFFFF",
    "warningContainer": "#FEF3C7",
    "onWarningContainer": "#78350F",
    "info": "#0EA5E9",
    "onInfo": "#FFFFFF",
    "infoContainer": "#E0F2FE",
    "onInfoContainer": "#075985"
  },
  "custom": {
    "border": "#E2E8F0",
    "borderDark": "#334155",
    "filled": "#F8FAFC",
    "filledDark": "#1E293B",
    "subtle": "#F1F5F9",
    "subtleDark": "#1E293B"
  }
}
```

### Typography JSON

**File:** `specs/design_system/typography.json`

Must follow Material 3 format:

```json
{
  "fontFamilies": {
    "primary": "Inter",
    "secondary": "Inter",
    "tertiary": "JetBrains Mono"
  },
  "textStyles": {
    "displayLarge": {
      "fontFamily": "Inter",
      "fontWeight": "w700",
      "fontSize": 57,
      "letterSpacing": -0.25,
      "lineHeight": 64,
      "color": "onSurface"
    },
    "headlineLarge": {
      "fontFamily": "Inter",
      "fontWeight": "w600",
      "fontSize": 32,
      "letterSpacing": 0,
      "lineHeight": 40,
      "color": "onSurface"
    },
    "titleLarge": {
      "fontFamily": "Inter",
      "fontWeight": "w600",
      "fontSize": 22,
      "letterSpacing": 0,
      "lineHeight": 28,
      "color": "onSurface"
    },
    "bodyLarge": {
      "fontFamily": "Inter",
      "fontWeight": "w400",
      "fontSize": 16,
      "letterSpacing": 0.5,
      "lineHeight": 24,
      "color": "onSurface"
    },
    "labelLarge": {
      "fontFamily": "Inter",
      "fontWeight": "w500",
      "fontSize": 14,
      "letterSpacing": 0.1,
      "lineHeight": 20,
      "color": "onSurface"
    }
  },
  "custom": {
    "overline": {
      "fontFamily": "Inter",
      "fontWeight": "w500",
      "fontSize": 10,
      "letterSpacing": 1.5,
      "lineHeight": 16,
      "color": "onSurfaceVariant"
    },
    "caption": {
      "fontFamily": "Inter",
      "fontWeight": "w400",
      "fontSize": 12,
      "letterSpacing": 0.4,
      "lineHeight": 16,
      "color": "onSurfaceVariant"
    },
    "mono": {
      "fontFamily": "JetBrains Mono",
      "fontWeight": "w400",
      "fontSize": 14,
      "letterSpacing": 0,
      "lineHeight": 20,
      "color": "onSurface"
    }
  },
  "weights": {
    "thin": "w100",
    "extraLight": "w200",
    "light": "w300",
    "regular": "w400",
    "medium": "w500",
    "semiBold": "w600",
    "bold": "w700",
    "extraBold": "w800",
    "black": "w900"
  },
  "sizes": {
    "xs": 10,
    "sm": 12,
    "base": 14,
    "md": 16,
    "lg": 18,
    "xl": 20,
    "2xl": 24,
    "3xl": 30,
    "4xl": 36,
    "5xl": 48,
    "6xl": 60
  }
}
```

---

## Sample Data Format

**File:** `specs/modules/[module]/[feature_slug]/data.json`

```json
[
  {
    "id": "1",
    "name": "Example Item 1",
    "description": "Sample data for development"
  },
  {
    "id": "2",
    "name": "Example Item 2",
    "description": "Another sample item"
  }
]
```

---

## Models Documentation Format

**File:** `specs/modules/[module]/[feature_slug]/models.md`

```markdown
# [Feature] Models

## Entity: [EntityName]

**Description:** [What this entity represents]

**Properties:**
| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `id` | String | Yes | Unique identifier |
| `name` | String | Yes | Display name |
| `description` | String? | No | Optional description |

**Relationships:**
- Belongs to: [OtherEntity]
- Has many: [RelatedEntity]
```
