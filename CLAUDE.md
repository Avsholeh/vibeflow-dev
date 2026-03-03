# Agent Directives for VibeFlow
VibeFlow is an **AI-powered Flutter development workflow** that helps you define your product vision, sketch data shapes, and implement beautiful Flutter UIs directly in your codebase using feature-first modular architecture.

---

## Getting Started — The Development Flow

**🚀 New to VibeFlow?** See [Quick Start — Build in 4 Commands](#quick-start--build-in-4-commands)

**The VibeFlow Workflow:**
1. `/vibeflow:new` — Set up your app (custom app. Todo app example available for reference)
2. `/vibeflow:theme` — Choose visual style
3. `/vibeflow:feature` — Build features
4. `/vibeflow:status` — Track progress

---

## Quick Start — Build in 4 Commands

**New to VibeFlow?** This is all you need:

### Step 1: Start Your App

```bash
/vibeflow:new
# → Describe your app, I'll generate everything from scratch
# → View Todo app example for reference (optional)
# → Enter app name
# → Done! Everything generated
```

---

### Step 2: Choose Visual Style
```bash
/vibeflow:theme --preset modern
```

| Preset | Style | Best For |
|--------|-------|----------|
| `modern` | Clean, indigo/purple | SaaS, productivity |
| `minimal` | Black/white | Content, readers |
| `playful` | Colorful, rounded | Kids, games |
| `professional` | Blue/gray | Business, finance |
| `nature` | Greens, earth tones | Health, wellness |
| `bold` | High contrast | Strong brands |

---

### Step 3: Build Features
```bash
/vibeflow:feature
# → Select feature from list
# → Automatically generates:
#    ✓ Sample data
#    ✓ Screens
#    ✓ Business logic
#    ✓ State management
```

Repeat for each feature in your roadmap.

---

### Step 4: Track Progress
```bash
/vibeflow:status         # View progress
/vibeflow:sprint         # Plan sprints
```

---

**That's it!** 4 commands to build a complete Flutter app.

| What | Command |
|-------|----------|
| Start | `/vibeflow:new` |
| Style | `/vibeflow:theme --preset [name]` |
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

**See:** [UI Implementation Requirements](.claude/WORKFLOW_REFERENCE.md#ui-implementation-requirements)

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

**See:** [User Interaction Guidelines](.claude/WORKFLOW_REFERENCE.md#user-interaction-guidelines)

---

## Roadmap Format

VibeFlow uses roadmaps with metadata in a clean list format for features.

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
| Field | Values | Description |
|-------|--------|-------------|
| `ID` | F001, F002... | Unique identifier for tracking |
| `Priority` | P0, P1, P2 | P0=critical, P1=important, P2=nice-to-have |
| `Effort` | small, medium, large, xlarge | 1, 3, 5, 8 points respectively |
| `Status` | pending, started, in_progress, done, blocked | Current implementation state |
| `Dependencies` | F001, F002... or "none" | Features that must be completed first |
| `Phase` | phase-1, phase-2... | Release phase assignment |
| `Tags` | core, ui, data... | Optional categorization |

---
## File Structure
```
.claude/                         # VibeFlow configuration
├── commands/
│   └── vibeflow/               # Workflow commands (new, feature, theme, status, sprint)
├── data/
│   └── themes/                 # Theme preset JSON files (modern, minimal, playful, etc.)
├── examples/
│   └── todo-app-spec.md        # Complete example showing workflow output
├── skills/
│   └── flutter-ui-design/      # UI design skill for distinctive Flutter interfaces
├── WORKFLOW_REFERENCE.md       # Shared constants, rules, and patterns
└── COMMANDS.md                 # Comprehensive command reference

specs/                          # Spec definition files
├── overview.md
├── roadmap.md
│
├── data-shape.md               # Entities & relationships
│
├── design-system/
│   ├── colors.json            # { "primary": "teal", "secondary": "amber", "neutral": "slate" }
│   └── typography.json        # { "heading": "Manrope", "body": "Inter", "mono": "JetBrains Mono" }
│
└── features/
    └── [feature_name]/
        ├── spec.md
        ├── data.json          # Realistic sample data for previews
        ├── models.md         # Dart models, enums, props classes
        └── *.png              # Screenshots

lib/                            # Flutter implementation (feature-first)
├── main.dart
│
├── core/                       # App-wide utilities
│   ├── theme/
│   │   ├── app_theme.dart     # ThemeData configuration
│   │   └── color_extensions.dart
│   ├── router/
│   │   └── app_router.dart    # Route constants and navigation helpers
│   ├── widgets/               # Shared widgets (AppBars, FABs, etc.)
│   └── utils/
│
└── features/                   # Feature modules
    └── [feature_name]/        # e.g., tasks, auth, settings
        ├── data/
        │   ├── datasources/        # API, local storage, mock implementations
        │   └── repositories/       # Repository implementations
        ├── domain/
        │   ├── models/             # Domain entities (core business objects)
        │   └── repositories/       # Repository interfaces (abstract contracts)
        ├── providers/              # ChangeNotifier providers
        ├── screens/
        │   ├── [screen_name]_screen.dart    # Pure UI component (props-driven)
        │   ├── [screen_name]_view.dart      # Development wrapper (uses data.json)
        │   └── [screen_name]_page.dart      # Production wrapper (uses Provider)
        ├── widgets/                # Feature-specific widgets
        └── routes.dart
```

---
## Implementation Guidelines (Flutter)
When implementing Flutter screens and widgets:

- **Mobile-first & Adaptive** — Use `MediaQuery`, `LayoutBuilder`, `OrientationBuilder`, `MediaQuery.sizeOf`. Support phone, foldable, tablet (NavigationRail), desktop (Sidebar/Rail).
- **Light & Dark Mode** — Always respect `Theme.of(context).brightness`. Use `ColorScheme` for semantic colors.
- **Use Design Tokens** — Map `colors.json` → `ColorScheme.fromSeed(seedColor: primary)`, custom extensions for semantic colors (success, warning, etc.). Use `GoogleFonts.[font]()` for `TextTheme`.
- **Props-Driven Widgets** — All widgets receive data & callbacks via constructor (`final` fields). Never import `data.json` in production widgets — only in development view wrappers.
- **Each Screen Manages Its Own Scaffold** — Feature screens include their own `Scaffold`, `AppBar`, and navigation elements. Use shared widgets from `core/widgets/` for consistency.
- **Bold Aesthetics** — Strictly follow the `flutter-ui-design` skill: commit to one extreme visual direction (brutal minimal, maximalist joy, retro-futurist, luxury restraint, etc.). Use `AnimatedSwitcher`, `Hero`, `flutter_animate`, `CustomPaint`, shaders, clippers, etc. Avoid generic Material 3 look.

---
## Flutter-Specific Guidelines
- Use **Material 3** (`useMaterial3: true`) with heavy customization
- Prefer **StatelessWidget**; use `StatefulWidget` or packages only when needed
- Animations: implicit first (`AnimatedOpacity`, `AnimatedContainer`), then `flutter_animate` or Rive if vision demands
- Responsiveness: thumb zones, safe areas, dynamic type scaling (`MediaQuery.textScalerOf`)
- Icons: `Icons.` + Material Symbols (via `Icons` or custom SVGs)
- No Tailwind CSS utility classes → use `Theme.of(context)`, `ColorScheme`, custom `ThemeExtension`s

---
## The Three Pillars (Flutter Context)
1. **Product Overview** — What & why of the Flutter app
2. **Data Shape** — Domain models & relationships (Dart classes later)
3. **Design System** — Colors → `ColorScheme`, Fonts → `TextTheme` via google_fonts
Plus **Features** — Independent feature modules with clean architecture layers

---
## Design System Scope
- **Color Systems Available**: Material 3 Dynamic, Tailwind-inspired palette names, Semantic naming, Brand-first generation
- **Product Flutter App**: uses user-defined tokens (primary/secondary/neutral → `ColorScheme`, fonts → `GoogleFonts`)

---
## Feature-First Architecture Principles
- **Features are independent** — Each feature is a self-contained module
- **Clean Architecture layers** — data/, domain/, screens/, widgets/, providers/ within each feature
- **Core for shared** — Only truly app-wide utilities go in `core/` (shared widgets, routing, theme)
- **Repository pattern** — Interfaces in domain/, implementations in data/
- **Props-driven widgets** — All data received via constructor, callbacks via constructor parameters

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

---
## Development Workflow
VibeFlow implements Flutter widgets directly in your codebase:
- Production widgets are implemented in `lib/features/` with proper data sources and state management
- Development view wrappers use `data.json` for hot-reload friendly design iteration
- State management, persistence, auth, and backend integration are implemented as part of each feature
- All code follows Flutter best practices and is ready for production use

---

## State Management Architecture

VibeFlow uses a **props-driven, provider-based** state management pattern that keeps widgets pure and testable.

**For complete implementation details, see:** [State Management in WORKFLOW_REFERENCE.md](.claude/WORKFLOW_REFERENCE.md#state-management-architecture)

### Quick Reference

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

## Command Reference

### 🔥 Core Commands (Essential Workflow)

**Start → Style → Build**

| Command | Purpose |
|---------|---------|
| `/vibeflow:new` | **Start app** — Custom app (5 questions). Todo app example available for reference |
| `/vibeflow:theme` | **Visual style** — Choose a preset theme (modern, minimal, playful, professional, nature, bold) |
| `/vibeflow:feature` | **Build feature** — Generate sample data + screens + logic in one command |
| `/vibeflow:status` | **Progress tracking** — View implementation progress across all features |
| `/vibeflow:sprint` | **Sprint planning** — Generate sprint plans based on effort and dependencies |

**That's it!** 4 commands to build a complete Flutter app.
