# Agent Directives for VibeFlow

VibeFlow is an **AI-powered Flutter development workflow** that helps you define your product vision, sketch data shapes, and implement beautiful Flutter UIs directly in your codebase using feature-first modular architecture.

---

## Quick Start — Build in 5 Commands

**New to VibeFlow?** This is all you need:

### Step 1: Start Your App

```bash
/vibeflow:new
# → Describe your app
# → Answer 5 quick questions
# → Done! Everything generated
```

### Step 2: Choose Visual Style

```bash
/vibeflow:theme
# → AI analyzes your app and presents 3-4 researched theme options
# → Select the option that best fits your vision
# → Full Material 3 design system generated
```

### Step 3: Plan Sprints

```bash
/vibeflow:sprint
# → Select sprint duration and focus area
# → AI resolves dependencies and selects features that fit capacity
# → Sprint plan generated
```

### Step 4: Build Features

```bash
/vibeflow:feature
# → Select feature from sprint plan
# → Automatically generates:
#    ✓ Sample data
#    ✓ Screens (using flutter-ui-design skill)
#    ✓ Business logic
#    ✓ State management
```

### Step 5: Track Progress

```bash
/vibeflow:status
# → View implementation progress across all features
# → See what's done, in progress, and pending
```

**That's it!** 5 commands to build a complete Flutter app.

| What | Command |
|-------|----------|
| Start | `/vibeflow:new` |
| Style | `/vibeflow:theme` |
| Plan | `/vibeflow:sprint` |
| Build | `/vibeflow:feature` |
| Track | `/vibeflow:status` |

---

## Mandatory Requirements

### flutter-ui-design Skill

**⚠️ MANDATORY** — When implementing any Flutter UI screens, agents MUST use the `flutter-ui-design` skill.

This ensures all VibeFlow UIs are distinctive and production-grade, avoiding generic Material 3 defaults.

**When to use:**
- Screen widgets
- Custom widgets
- Navigation flows
- Full feature UIs
- Visual components

**Skill location:** `.claude/skills/flutter-ui-design/skill.md`

**Key Principles:**
1. **Commit to one bold aesthetic** — No middle-ground "pleasant modern app"
2. **Characterful typography** — Never default to system fonts
3. **Strong color theme** — Commit hard to a distinctive palette
4. **Motion & delight** — Implicit animations, hero moments
5. **Spatial composition** — Break grids intentionally when justified
6. **Avoid** — Generic Material 3, boring cards, overused color schemes

---

### AskUserQuestion Tool

**⚠️ MANDATORY** — When asking the user questions, ALWAYS use the `AskUserQuestion` tool instead of asking through text.

**When to use:**
- Need user input for choices (options, confirmations)
- Clarifying requirements or preferences
- Presenting alternatives for user selection
- Getting approval before proceeding

**How to use:**
- Provide clear question text ending with "?"
- Present 2-4 options with labels and descriptions
- Use multiSelect: true when user can select multiple options
- Include markdown previews for visual comparisons when appropriate

**Do NOT ask questions through text.** Always use the AskUserQuestion tool for user interactions.

---

## Workflow Overview

### The Three Pillars

1. **Product Overview** — What & why of the Flutter app
2. **Data Shape** — Domain models & relationships (Dart classes later)
3. **Design System** — Colors → `ColorScheme`, Fonts → `TextTheme` via google_fonts
4. **Features** — Independent feature modules with clean architecture layers

### Feature-First Architecture Principles

- **Features are independent** — Each feature is a self-contained module
- **Clean Architecture layers** — data/, domain/, screens/, widgets/, providers/ within each feature
- **Core for shared** — Only truly app-wide utilities go in `core/` (shared widgets, routing, theme)
- **Repository pattern** — Interfaces in domain/, implementations in data/
- **Props-driven widgets** — All data received via constructor, callbacks via constructor parameters

### State Management Architecture

VibeFlow uses a **props-driven, provider-based** state management pattern that keeps widgets pure and testable.

**Data Flow:**
```
data.json (dev) → *_view.dart → *_screen.dart
Repository (prod) → Provider → *_page.dart → *_screen.dart
```

**Three-File Pattern:**
| File | Purpose | Data Source |
|-----|---------|-------------|
| `*_screen.dart` | Pure UI (props-driven) | Constructor props |
| `*_view.dart` | Development wrapper | `data.json` |
| `*_page.dart` | Production wrapper | Provider |

**Key Pattern:**
- Props-driven widgets (all data via constructor)
- Provider pattern with ChangeNotifier
- Repository pattern (interfaces in domain/, implementations in data/)
- Auto-fetch on initialization
- CRUD operations refresh data after each action

---

## Core Reference

### Feature Slug Conversion

Convert feature names to slugs using these rules:
- Lowercase everything
- Replace spaces with hyphens
- Remove special characters
- Keep words intact

**Examples:**
- "Dashboard" → `dashboard`
- "User Management" → `user_management`
- "Add Transaction Flow" → `add_transaction_flow`
- "Due Dates & Reminders" → `due_dates_reminders`

---

### Progress Calculation

**Progress Percentage Formula:**
```
progress = (spec_complete * 25) +
           (sample_data * 25) +
           (types_documented * 10) +
           (models_defined * 20) +
           (logic_implemented * 15) +
           (screens_implemented * 10)
```

**Status Determination:**

| Progress | Status |
|----------|--------|
| 0% | `pending` |
| 1-49% | `started` |
| 50-99% | `in_progress` |
| 100% | `done` |

**File Checks:**

*Spec Status:*
- `/specs/features/[feature_slug]/spec.md` exists → Spec complete (+25%)
- `/specs/features/[feature_slug]/data.json` exists → Sample data (+25%)
- `/specs/features/[feature_slug]/models.md` exists → Types documented (+10%)

*Implementation Status:*
- `/lib/features/[feature_slug]/domain/models/` has .dart files → Models defined (+20%)
- `/lib/features/[feature_slug]/providers/` has non-empty .dart files → Logic implemented (+15%)
- `/lib/features/[feature_slug]/screens/*_page.dart` files exist → Screens implemented (+10%)

---

### Effort Estimation

| Effort | Points | Duration |
|--------|--------|----------|
| `small` | 1 point | 1-2 days |
| `medium` | 3 points | 3-5 days |
| `large` | 5 points | 1-2 weeks |
| `xlarge` | 8 points | 2-4 weeks |

**Sprint Capacity Calculation:**

| Duration | Points |
|----------|--------|
| 1 week | ~6 points |
| 2 weeks | ~10 points |
| 1 month | ~20 points |

---

### Priority Levels

| Priority | Meaning | When to Use |
|----------|---------|-------------|
| `P0` | Critical | Core value features, blocking other features |
| `P1` | Important | Supporting features, nice to have soon |
| `P2` | Nice to have | Enhancements, can be deferred |

---

### Status Values

| Status | Meaning |
|--------|---------|
| `pending` | Not started |
| `started` | Some work done, less than 50% |
| `in_progress` | Work in progress, 50-99% |
| `done` | Complete, 100% |
| `blocked` | Cannot proceed due to dependencies |

---

## Specification Formats

### Roadmap Format

```markdown
# Product Roadmap

## Features

### 1. Dashboard
- **ID:** F001
- **Priority:** P0
- **Effort:** medium
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core, ui

The main screen showing balance and recent activity.

### 2. Add Transaction
- **ID:** F002
- **Priority:** P0
- **Effort:** small
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core

A form for quick transaction entry with amount, category, and notes.

### 3. Transaction History
- **ID:** F003
- **Priority:** P1
- **Effort:** medium
- **Status:** pending
- **Dependencies:** F001, F002
- **Phase:** phase-1
- **Tags:** ui

A list view of all transactions with filtering and search.
```

**Metadata Fields:**
| Field | Type | Values |
|-------|------|--------|
| `ID` | string | F001, F002, F003... (auto-generated) |
| `Priority` | enum | P0, P1, P2 |
| `Effort` | enum | small, medium, large, xlarge |
| `Status` | enum | pending, started, in_progress, done, blocked |
| `Dependencies` | list | F001, F002... or "none" |
| `Phase` | string | phase-1, phase-2... - Release phase assignment |
| `Tags` | list | core, ui, data, analytics... - Optional categorization |

---

### Data Shape Format

```markdown
# Data Shape

## Entities

### Task
The core entity representing a single todo item with all its properties.

**Properties:**
- `id` — Unique identifier
- `title` — Task name (required)
- `description` — Additional details (optional)
- `isCompleted` — Completion status
- `categoryId` — Reference to category (optional)
- `createdAt` — Creation timestamp
- `completedAt` — Completion timestamp (optional)

### Category
A classification for organizing related tasks together.

**Properties:**
- `id` — Unique identifier
- `name` — Category display name
- `color` — Visual identifier for UI
- `icon` — Optional icon for visual distinction

## Relationships

- Task belongs to Category (optional)
- Category has many Tasks
```

---

### Feature Spec Format

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

### Design System Format

#### Design System Spec (`specs/design_system/design_system.md`)

```markdown
# Design System — [App Name]

## Aesthetic
[Description of visual style - e.g., "Clean, minimal, refined with subtle borders"]

## Color Philosophy
[How colors are used - e.g., "Subtle borders, purposeful color accents for CTAs"]

## Typography Philosophy
[Font choices and readability considerations - e.g., "Geometric sans for headings, readable serif for body"]

## Component Patterns
[Key UI patterns - e.g., "Cards with subtle borders", "Rounded buttons", "Floating panels"]

## Motion & Interaction
[Animation and interaction patterns - e.g., "Micro-interactions on hover", "Smooth page transitions"]
```

#### Colors JSON (`specs/design_system/colors.json`)

**Must follow Material 3 format:**

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

#### Typography JSON (`specs/design_system/typography.json`)

**Must follow Material 3 format:**

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

## File Structure Patterns

### Spec Files

```
specs/
├── overview.md                    # Product overview
├── roadmap.md                     # Feature roadmap
├── data_shape.md                  # Data models & relationships
├── design_system/
│   ├── design_system.md          # Design aesthetic & principles
│   ├── colors.json                # Color tokens for light/dark modes
│   └── typography.json            # Typography definitions
└── features/
    └── [feature_slug]/
        ├── spec.md                # Feature specification
        ├── data.json              # Sample data
        └── models.md              # Domain models & props
```

### Implementation Files

```
lib/features/[feature_slug]/
├── domain/
│   ├── models/
│   │   └── [model].dart
│   └── repositories/
│       └── [feature]_repository.dart
├── data/
│   ├── datasources/
│   │   └── [feature]_datasource.dart
│   └── repositories/
│       └── [feature]_repository_impl.dart
├── providers/
│   └── [feature]_provider.dart
├── screens/
│   ├── [screen]_screen.dart    # Pure UI (props-driven)
│   ├── [screen]_view.dart      # Dev wrapper (data.json)
│   └── [screen]_page.dart      # Production wrapper (Provider)
├── widgets/
│   └── [widget].dart
└── routes.dart
```

---

## Provider State Pattern Template

```dart
import 'package:flutter/foundation.dart';

class [FeatureName]Provider extends ChangeNotifier {
  final [FeatureName]Repository _repository;

  [FeatureName]Provider({required [FeatureName]Repository repository})
      : _repository = repository {
    fetch[Model]s(); // Auto-load on initialization
  }

  // State
  List<[Model]> _items = [];
  List<[Model]> get items => _items;

  bool _isLoading = false;
  bool get isLoading => _isLoading;

  String? _error;
  String? get error => _error;
  bool get hasError => _error != null;

  // Initial data fetch
  Future<void> fetch[Model]s() async {
    _setLoading(true);
    _clearError();

    try {
      _items = await _repository.get[Model]s();
      _setLoading(false);
      notifyListeners();
    } catch (e) {
      _setError(e.toString());
      _setLoading(false);
      notifyListeners();
    }
  }

  // CRUD operations
  Future<void> create[Model]([Model] [model]) async {
    _setLoading(true);
    _clearError();

    try {
      await _repository.create[Model]([model]);
      await fetch[Model]s(); // Refresh data
    } catch (e) {
      _setError(e.toString());
      _setLoading(false);
      notifyListeners();
    }
  }

  Future<void> update[Model]([Model] [model]) async {
    _clearError();

    try {
      await _repository.update[Model]([model]);
      await fetch[Model]s(); // Refresh data
    } catch (e) {
      _setError(e.toString());
      notifyListeners();
    }
  }

  Future<void> delete[Model](String id) async {
    _clearError();

    try {
      await _repository.delete[Model](id);
      await fetch[Model]s(); // Refresh data
    } catch (e) {
      _setError(e.toString());
      notifyListeners();
    }
  }

  // Private state helpers
  void _setLoading(bool value) {
    _isLoading = value;
    notifyListeners();
  }

  void _setError(String error) {
    _error = error;
  }

  void _clearError() {
    _error = null;
  }
}
```

---

## Commands Reference

### `/vibeflow:new`

Create a new Flutter app with complete specification files.

**What it does:**
1. Checks for existing spec files (prompts to overwrite/keep/cancel if found)
2. Gathers app information (name, description, problems, users, features)
3. Generates spec files (overview.md, roadmap.md, data_shape.md, feature specs)
4. Optionally generates design system files

**Generated files:**
```
specs/
├── overview.md
├── roadmap.md
├── data_shape.md
└── features/
    └── [feature_name]/
        └── spec.md
```

---

### `/vibeflow:theme`

Set up design system with research-based theme recommendations.

**What it does:**
1. Analyzes app type and target users from overview.md
2. Researches industry standards for similar apps
3. Presents 3-4 theme options with research-backed explanations
4. Generates full Material 3 colors.json and typography.json

**Example options presented:**
- **Professional Trust** — Blue/Gray palette for trust-sensitive apps
- **Modern Productivity** — Indigo/Purple for SaaS tools
- **Calm Focus** — Green/Emerald for wellness and productivity

**Generated files:**
```
specs/design_system/
├── design_system.md
├── colors.json
└── typography.json
```

---

### `/vibeflow:feature`

Build a complete feature with sample data, screens, and business logic.

**What it does:**
1. Lists available features from roadmap
2. Checks feature dependencies
3. Generates in 4 phases:
   - **Phase 1:** Sample data (data.json, models.md)
   - **Phase 2:** Screens (screen, view, page files) — Uses `flutter-ui-design` skill
   - **Phase 3:** Business logic (models, repositories, providers)
   - **Phase 4:** Updates feature status to `in_progress`

**Generated files:**
```
specs/features/[feature_slug]/
├── data.json
└── models.md

lib/features/[feature_slug]/
├── domain/models/
├── domain/repositories/
├── data/datasources/
├── data/repositories/
├── providers/
└── screens/
```

---

### `/vibeflow:status`

View implementation progress across all features.

**What it does:**
1. Parses roadmap for all features
2. Checks file existence for progress calculation
3. Generates status report by phase
4. Shows priority items needing attention

**Progress calculation:**
| Check | Progress |
|-------|----------|
| `spec.md` exists | +25% |
| `data.json` exists | +25% |
| `models.md` exists | +10% |
| Domain models exist | +20% |
| Logic implemented | +15% |
| Screens implemented | +10% |

---

### `/vibeflow:sprint`

Plan sprints based on effort estimates and dependencies.

**What it does:**
1. Parses roadmap for features with metadata
2. Prompts for sprint parameters (duration, focus)
3. Resolves dependencies using topological sort
4. Selects features that fit capacity
5. Generates sprint plan

**Questions asked:**
1. Sprint duration (1 week, 2 weeks, 1 month, custom)
2. Focus area (P0 only, specific phase, all pending, in-progress)

---

## Common Errors & Solutions

| Error | Cause | Solution |
|-------|-------|----------|
| Feature spec not found | `/specs/roadmap.md` missing | Run `/vibeflow:new` first |
| Design system exists | User already has colors/typography | Ask to replace or keep |
| Circular dependencies | Feature A depends on B, B depends on A | Break cycle, remove one dependency |
| Invalid dependency | Feature ID doesn't exist | Check roadmap for correct IDs |
| All features done | No pending features | All features marked as `done` |
| No features fit | Smallest feature > capacity | Increase duration |
| Dependencies Required | Feature depends on incomplete features | Build dependencies first |

---

## Implementation Guidelines (Flutter)

When implementing Flutter screens and widgets:

- **Mobile-first & Adaptive** — Use `MediaQuery`, `LayoutBuilder`, `OrientationBuilder`, `MediaQuery.sizeOf`. Support phone, foldable, tablet (NavigationRail), desktop (Sidebar/Rail).
- **Light & Dark Mode** — Always respect `Theme.of(context).brightness`. Use `ColorScheme` for semantic colors.
- **Use Design Tokens** — Map `colors.json` → `ColorScheme.fromSeed(seedColor: primary)`, custom extensions for semantic colors (success, warning, etc.). Use `GoogleFonts.[font]()` for `TextTheme`.
- **Props-Driven Widgets** — All widgets receive data & callbacks via constructor (`final` fields). Never import `data.json` in production widgets — only in development view wrappers.
- **Each Screen Manages Its Own Scaffold** — Feature screens include their own `Scaffold`, `AppBar`, and navigation elements. Use shared widgets from `core/widgets/` for consistency.
- **Bold Aesthetics** — Strictly follow the `flutter-ui-design` skill: commit to one extreme visual direction (brutal minimal, maximalist joy, retro-futurist, luxury restraint, etc.). Use `AnimatedSwitcher`, `Hero`, `flutter_animate`, `CustomPaint`, shaders, clippers, etc. Avoid generic Material 3 look.

### Flutter-Specific Guidelines
- Use **Material 3** (`useMaterial3: true`) with heavy customization
- Prefer **StatelessWidget**; use `StatefulWidget` or packages only when needed
- Animations: implicit first (`AnimatedOpacity`, `AnimatedContainer`), then `flutter_animate` or Rive if vision demands
- Responsiveness: thumb zones, safe areas, dynamic type scaling (`MediaQuery.textScalerOf`)
- Icons: `Icons.` + Material Symbols (via `Icons` or custom SVGs)
- No Tailwind CSS utility classes → use `Theme.of(context)`, `ColorScheme`, custom `ThemeExtension`s

---

## Software Engineering Principles

VibeFlow follows these core principles to maintain code quality:

### SOLID
- **Single Responsibility** — Each class/module has one reason to change
- **Open/Closed** — Open for extension, closed for modification
- **Liskov Substitution** — Subtypes must be substitutable for base types
- **Interface Segregation** — Prefer small, specific interfaces over large ones
- **Dependency Inversion** — Depend on abstractions (interfaces), not concretions

### DRY (Don't Repeat Yourself)
- Extract reusable logic into shared functions or widgets
- Avoid duplicating code across features
- Use inheritance and composition appropriately

### KISS (Keep It Simple, Stupid)
- Favor simple solutions over complex ones
- Avoid over-engineering and premature optimization
- Write code that's easy to understand and modify

### YAGNI (You Aren't Gonna Need It)
- Only implement what's needed now
- Avoid speculative features
- Add complexity only when requirements demand it
