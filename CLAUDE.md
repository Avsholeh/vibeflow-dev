# Agent Directives for VibeFlow

VibeFlow is an **AI-powered Flutter development workflow** that helps you define your product vision, sketch data shapes, and implement beautiful Flutter UIs using Clean Architecture.

---

## Quick Start

Build a complete Flutter app in 5 commands:

| What | Command |
|-------|----------|
| Start | `/vibeflow:new` |
| Style | `/vibeflow:theme` |
| Plan | `/vibeflow:plan` |
| Build | `/vibeflow:feature` |
| Track | `/vibeflow:status` |
| Validate | `/vibeflow:validate` |

---

## Mandatory Requirements

### flutter-ui-design Skill

**⚠️ MANDATORY** — When implementing any Flutter UI screens, agents MUST use the `flutter-ui-design` skill.

**Location:** `.claude/skills/flutter-ui-design/skill.md`

**Key Principles:**
1. Commit to one bold aesthetic — No middle-ground "pleasant modern app"
2. Characterful typography — Never default to system fonts
3. Strong color theme — Commit hard to a distinctive palette
4. Motion & delight — Implicit animations, hero moments
5. Spatial composition — Break grids intentionally when justified
6. Avoid — Generic Material 3, boring cards, overused color schemes

### AskUserQuestion Tool

**⚠️ MANDATORY** — When asking user questions, ALWAYS use the `AskUserQuestion` tool instead of asking through text.

**When to use:** Need user input for choices, clarifying requirements, presenting alternatives, getting approval.

---

## Clean Architecture

VibeFlow follows **Clean Architecture** with pure layer separation:

```
┌─────────────────────────────────────────────────────────────┐
│                    Presentation Layer                       │
│  (Screens, Widgets, Providers) → depends on Domain           │
├─────────────────────────────────────────────────────────────┤
│                       Domain Layer                          │
│           (Entities, Repositories)                            │
│                   ← depends on nothing                      │
├─────────────────────────────────────────────────────────────┤
│                        Data Layer                            │
│   (Repository Implementations, Data Sources, DTOs)           │
│              → depends on Domain                             │
└─────────────────────────────────────────────────────────────┘
```

**Layer Responsibilities:**

- **Domain Layer** (`lib/domain/`) — Core business logic, completely independent
  - `entities/` — Business objects (Note, User, Transaction)
  - `repositories/` — Repository interfaces (abstract contracts)

- **Data Layer** (`lib/data/`) — External data access, implements domain contracts
  - `repositories/` — Repository implementations (NoteRepositoryImpl)
  - `datasources/` — Data sources (LocalDataSource, RemoteDataSource)
  - `models/` — DTOs for data transfer (NoteDTO)

- **Presentation Layer** (`lib/presentation/`) — UI and state management
  - `screens/` — All screens (NoteListScreen, CreateNoteScreen)
  - `widgets/` — Shared reusable widgets (NoteCard, SearchBar)
  - `providers/` — State management with Provider pattern
  - `routes/` — Navigation/routing configuration

- **Core Layer** (`lib/core/`) — Shared utilities and configuration
  - `theme/` — Design system tokens and theme configuration
  - `constants/` — App-wide constants
  - `utils/` — Helper functions
  - `services/` — Global services (navigation, analytics)

### State Management

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
- Providers call repository methods directly
- Repository pattern (interfaces in domain/, implementations in data/)
- Auto-fetch on initialization
- CRUD operations refresh data after each action

### Provider Registry Pattern

**Create:** `lib/presentation/providers/provider_registry.dart`

```dart
import 'package:provider/provider.dart';
import 'package:flutter/material.dart';

// Import feature providers
// import '[feature]_provider.dart';

class ProviderRegistry {
  static List<SingleChildWidget> getProviders() {
    return [
      // Add feature providers here
      // ChangeNotifierProvider(
      //   create: (_) => [Feature]Provider(...),
      // ),
    ];
  }
}
```

**Usage in main.dart:**
```dart
void main() {
  runApp(
    MultiProvider(
      providers: ProviderRegistry.getProviders(),
      child: const MyApp(),
    ),
  );
}
```

---

## Core Reference

### Feature Slug Conversion

- Lowercase everything
- Replace spaces with underscore
- Remove special characters

**Examples:** "Dashboard" → `dashboard`, "User Management" → `user_management`

**Feature path:** `[module]/[feature_slug]` → `specs/modules/tasks/add_task/`

### Module Inference

| Feature Pattern | Module |
|-----------------|--------|
| "Task List", "Add Task" | `tasks` |
| "User Profile", "User Settings" | `user` |
| "Dashboard", "Home Screen" | `dashboard` |
| "Authentication", "Login" | `auth` |
| "Settings", "Preferences" | `settings` |
| "Notifications", "Alerts" | `notifications` |

### Priority Levels

| Priority | Meaning |
|----------|---------|
| `P0` | Critical |
| `P1` | Important |
| `P2` | Nice to have |

### Status Values

| Status | Progress |
|--------|----------|
| `pending` | 0% |
| `started` | 1-49% |
| `in_progress` | 50-99% |
| `done` | 100% |
| `blocked` | Dependencies incomplete |

---

## Specification & Code Templates

**All spec formats:** See `.claude/vibeflow/SPEC_FORMATS.md`

**All code templates:** See `.claude/vibeflow/CODE_TEMPLATES.md`

**Algorithms & formulas:** See `.claude/vibeflow/ALGORITHMS.md`

---

## Commands Reference

### `/vibeflow:new`

Create a new Flutter app with complete specification files.

**Generated files:**
- `specs/overview.md`, `specs/roadmap.md`, `specs/data_shape.md`
- `specs/modules/[module]/[feature_slug]/spec.md` for each feature
- `specs/design_system/*` (if theme selected)
- `lib/main.dart`

---

### `/vibeflow:theme`

Set up design system with research-based theme recommendations.

**Generated files:**
- `specs/design_system/design_system.md`
- `specs/design_system/colors.json`
- `specs/design_system/typography.json`

---

### `/vibeflow:feature`

Build a complete feature with sample data, screens, and business logic.

**Generated files:**
- Entities in `lib/domain/entities/`
- Repository interfaces in `lib/domain/repositories/`
- Repository implementations in `lib/data/repositories/`
- Data sources in `lib/data/datasources/`
- Providers in `lib/presentation/providers/`
- Screens in `lib/presentation/screens/`

---

### `/vibeflow:status`

View implementation progress across all features.

**Progress calculation:**
| Check | Progress |
|-------|----------|
| `spec.md` exists | +30% |
| `data.json` exists | +30% |
| `models.md` exists | +12% |
| Entities exist | +24% |
| Screens exist | +12% |

---

### `/vibeflow:plan`

Create development plans based on priorities and dependencies.

**Process:**
1. Parse roadmap for features with metadata
2. Prompt for focus area
3. Resolve dependencies using topological sort
4. Order features by priority (P0 > P1 > P2)
5. Generate development plan

---

### `/vibeflow:validate`

Validate specification files for format consistency and completeness.

---

## Implementation Guidelines (Flutter)

When implementing Flutter screens and widgets:

- **Mobile-first & Adaptive** — Use `MediaQuery`, `LayoutBuilder`, `OrientationBuilder`
- **Light & Dark Mode** — Always respect `Theme.of(context).brightness`
- **Use Design Tokens** — Map `colors.json` → `ColorScheme`, use `GoogleFonts` for `TextTheme`
- **Props-Driven Widgets** — All widgets receive data & callbacks via constructor
- **Each Screen Manages Its Own Scaffold** — Include `Scaffold`, `AppBar`, navigation elements
- **Bold Aesthetics** — Follow `flutter-ui-design` skill strictly

### Flutter-Specific Guidelines
- Use **Material 3** (`useMaterial3: true`) with heavy customization
- Prefer **StatelessWidget**; use `StatefulWidget` only when needed
- Animations: implicit first (`AnimatedOpacity`, `AnimatedContainer`)
- Responsiveness: thumb zones, safe areas, dynamic type scaling
- Icons: `Icons.` + Material Symbols

---

## Software Engineering Principles

### SOLID
- **Single Responsibility** — Each class has one reason to change
- **Open/Closed** — Open for extension, closed for modification
- **Liskov Substitution** — Subtypes must be substitutable for base types
- **Interface Segregation** — Prefer small, specific interfaces
- **Dependency Inversion** — Depend on abstractions, not concretions

### DRY (Don't Repeat Yourself)
- Extract reusable logic into shared functions or widgets
- Avoid duplicating code across features

### KISS (Keep It Simple, Stupid)
- Favor simple solutions over complex ones
- Avoid over-engineering and premature optimization

### YAGNI (You Aren't Gonna Need It)
- Only implement what's needed now
- Avoid speculative features
