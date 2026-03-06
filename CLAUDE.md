# Agent Directives for VibeFlow

VibeFlow is an **AI-powered Flutter development workflow** that helps you define your product vision, sketch data shapes, and implement beautiful Flutter UIs directly in your codebase using Clean Architecture.

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

### Step 3: Plan Development

```bash
/vibeflow:plan
# → Select focus area
# → AI resolves dependencies and prioritizes features
# → Development plan generated
```

### Step 4: Build Features

```bash
/vibeflow:feature
# → Select feature from development plan
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

**That's it!** 6 commands to build a complete Flutter app.

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

### The Core Components

1. **Product Overview** — What & why of the Flutter app
2. **Data Shape** — Domain entities & relationships (Dart classes later)
3. **Design System** — Colors → `ColorScheme`, Fonts → `TextTheme` via google_fonts
4. **Features** — Feature specifications that map to Clean Architecture layers

### Clean Architecture Principles

VibeFlow follows **Uncle Bob's Clean Architecture** with pure layer separation:

**Dependency Rule:** Dependencies point inward only — outer layers depend on inner layers, but inner layers know nothing about outer layers.

```
┌─────────────────────────────────────────────────────────────┐
│                    Presentation Layer                       │
│  (Screens, Widgets, Providers) → depends on Domain           │
├─────────────────────────────────────────────────────────────┤
│                       Domain Layer                          │
│           (Entities, Repositories, Use Cases)                │
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
  - `usecases/` — Business logic operations (GetNotes, CreateNote)

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

### State Management Architecture

VibeFlow uses a **props-driven, provider-based** state management pattern that keeps widgets pure and testable.

**Data Flow:**
```
data.json (dev) → *_view.dart → *_screen.dart
Use Cases (prod) → Provider → *_page.dart → *_screen.dart
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
- Use cases encapsulate business logic
- Repository pattern (interfaces in domain/, implementations in data/)
- Auto-fetch on initialization
- CRUD operations refresh data after each action

### Provider Registry Pattern

As features are added, manually updating main.dart becomes error-prone. Use a provider registry pattern:

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

      // Core providers (settings, theme, etc.)
      // ChangeNotifierProvider(
      //   create: (_) => SettingsProvider(...),
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

**When Building Features:**
1. Add provider import at top of `provider_registry.dart`
2. Add provider to `getProviders()` list
3. No main.dart edits needed after initial setup

---

## Core Reference

### Feature Slug Conversion

Convert feature names to slugs using these rules:
- Lowercase everything
- Replace spaces with underscore
- Remove special characters
- Keep words intact

**Examples:**
- "Dashboard" → `dashboard`
- "User Management" → `user_management`
- "Add Transaction Flow" → `add_transaction_flow`
- "Due Dates & Reminders" → `due_dates_reminders`

**Feature path construction:**
- **Spec path:** `[feature_slug]` → `specs/features/add_task/`
- **Implementation:** Distributed across layers (no feature folders)

---

### Progress Calculation

**Progress Percentage Formula:**
```
progress = (spec_complete * 25) +
           (sample_data * 25) +
           (types_documented * 10) +
           (entities_defined * 20) +
           (usecases_implemented * 15) +
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
- `/lib/domain/entities/` has entity .dart files → Entities defined (+20%)
- `/lib/domain/usecases/` has use case .dart files → Use cases implemented (+15%)
- `/lib/presentation/screens/` has screen .dart files → Screens implemented (+10%)

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

### 1. Note List
- **ID:** F001
- **Priority:** P0
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core, ui

The main screen displaying all notes in a scrollable list.

### 2. Create Note
- **ID:** F002
- **Priority:** P0
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core

Quick note creation screen with title and content.

### 3. Edit Note
- **ID:** F003
- **Priority:** P0
- **Status:** pending
- **Dependencies:** F002
- **Phase:** phase-1
- **Tags:** core

Edit existing notes with delete functionality.
```

**Metadata Fields:**
| Field | Type | Values | Description |
|-------|------|--------|-------------|
| `ID` | string | F001, F002, F003... (auto-generated) | Unique feature identifier |
| `Priority` | enum | P0, P1, P2 | Development priority |
| `Status` | enum | pending, started, in_progress, done, blocked | Implementation status |
| `Dependencies` | list | F001, F002... or "none" | Feature IDs this feature depends on |
| `Phase` | string | phase-1, phase-2... | Release phase assignment |
| `Tags` | list | core, ui, data, analytics... | Optional categorization |

---

### Data Shape Format

```markdown
# Data Shape

## Entities

### Note
The core entity representing a single note.

**Properties:**
- `id` — Unique identifier
- `title` — Note title (optional)
- `content` — Note body text (required)
- `color` — Visual color tag (optional)
- `createdAt` — Creation timestamp
- `updatedAt` — Last modification timestamp

### User
App user with authentication and preferences.

**Properties:**
- `id` — Unique identifier
- `email` — Email address
- `displayName` — Display name
- `preferences` — User settings
- `createdAt` — Account creation timestamp

## Relationships

- Note belongs to User
- User has many Notes
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
├── data_shape.md                  # Data entities & relationships
├── design_system/
│   ├── design_system.md          # Design aesthetic & principles
│   ├── colors.json                # Color tokens for light/dark modes
│   └── typography.json            # Typography definitions
├── plans/                         # Development plans
│   └── plan_[N].md                # Auto-generated plan files
└── features/
    └── [feature_slug]/
        ├── spec.md                # Feature specification
        ├── data.json              # Sample data
        └── models.md              # Domain entities & props
```

### Implementation Files (Clean Architecture)

```
lib/
├── domain/
│   ├── entities/
│   │   ├── note.dart
│   │   ├── user.dart
│   │   └── transaction.dart
│   ├── repositories/
│   │   ├── note_repository.dart
│   │   ├── user_repository.dart
│   │   └── transaction_repository.dart
│   └── usecases/
│       ├── get_notes.dart
│       ├── create_note.dart
│       ├── update_note.dart
│       ├── delete_note.dart
│       └── search_notes.dart
├── data/
│   ├── models/
│   │   ├── note_dto.dart
│   │   └── user_dto.dart
│   ├── repositories/
│   │   ├── note_repository_impl.dart
│   │   └── user_repository_impl.dart
│   └── datasources/
│       ├── note_local_datasource.dart
│       ├── note_remote_datasource.dart
│       └── user_datasource.dart
├── presentation/
│   ├── screens/
│   │   ├── note_list_screen.dart
│   │   ├── note_list_view.dart
│   │   ├── note_list_page.dart
│   │   ├── create_note_screen.dart
│   │   ├── create_note_view.dart
│   │   ├── create_note_page.dart
│   │   ├── edit_note_screen.dart
│   │   ├── note_details_screen.dart
│   │   └── settings_screen.dart
│   ├── widgets/
│   │   ├── note_card.dart
│   │   ├── search_bar.dart
│   │   └── empty_state.dart
│   ├── providers/
│   │   ├── note_list_provider.dart
│   │   ├── create_note_provider.dart
│   │   └── settings_provider.dart
│   └── routes/
│       └── app_routes.dart
└── core/
    ├── theme/
    │   └── app_theme.dart
    ├── constants/
    │   ├── app_colors.dart
    │   ├── app_strings.dart
    │   └── app_assets.dart
    ├── utils/
    │   ├── date_formatter.dart
    │   └── validators.dart
    └── services/
        ├── navigation_service.dart
        └── storage_service.dart
```

**Key Principles:**
- All domain entities in `lib/domain/entities/`
- Repository interfaces in `lib/domain/repositories/`
- Repository implementations in `lib/data/repositories/`
- Use cases in `lib/domain/usecases/` for business logic
- All screens in `lib/presentation/screens/`
- Shared widgets in `lib/presentation/widgets/`
- Providers in `lib/presentation/providers/`

---

## Use Cases Pattern Template

Use cases encapsulate single business operations. They depend on repository interfaces and are used by providers.

```dart
// lib/domain/usecases/get_notes.dart
class GetNotesUseCase {
  final NoteRepository _repository;

  GetNotesUseCase(this._repository);

  Future<List<Note>> execute() async {
    return await _repository.getNotes();
  }
}

// lib/domain/usecases/create_note.dart
class CreateNoteUseCase {
  final NoteRepository _repository;

  CreateNoteUseCase(this._repository);

  Future<Note> execute(Note note) async {
    return await _repository.createNote(note);
  }
}

// lib/domain/usecases/search_notes.dart
class SearchNotesUseCase {
  final NoteRepository _repository;

  SearchNotesUseCase(this._repository);

  Future<List<Note>> execute(String query) async {
    final notes = await _repository.getNotes();
    return notes.where((note) =>
      note.title.toLowerCase().contains(query.toLowerCase()) ||
      note.content.toLowerCase().contains(query.toLowerCase())
    ).toList();
  }
}
```

---

## Provider State Pattern Template

Providers use use cases for business logic and manage UI state.

```dart
// lib/presentation/providers/note_list_provider.dart
import 'package:flutter/foundation.dart';
import '../../domain/usecases/get_notes.dart';
import '../../domain/usecases/delete_note.dart';
import '../../domain/entities/note.dart';

class NoteListProvider extends ChangeNotifier {
  final GetNotesUseCase _getNotesUseCase;
  final DeleteNoteUseCase _deleteNoteUseCase;

  NoteListProvider({
    required GetNotesUseCase getNotesUseCase,
    required DeleteNoteUseCase deleteNoteUseCase,
  })  : _getNotesUseCase = getNotesUseCase,
        _deleteNoteUseCase = deleteNoteUseCase {
    fetchNotes();
  }

  // State
  List<Note> _notes = [];
  List<Note> get notes => _notes;

  bool _isLoading = false;
  bool get isLoading => _isLoading;

  String? _error;
  String? get error => _error;
  bool get hasError => _error != null;

  // Initial data fetch
  Future<void> fetchNotes() async {
    _setLoading(true);
    _clearError();

    try {
      _notes = await _getNotesUseCase.execute();
      _setLoading(false);
      notifyListeners();
    } catch (e) {
      _setError(e.toString());
      _setLoading(false);
      notifyListeners();
    }
  }

  // Delete operation
  Future<void> deleteNote(String id) async {
    _clearError();

    try {
      await _deleteNoteUseCase.execute(id);
      await fetchNotes(); // Refresh data
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
- `specs/overview.md`, `specs/roadmap.md`, `specs/data_shape.md`
- `specs/features/[feature_slug]/spec.md` for each feature
- `specs/design_system/*` (if theme selected)

---

### `/vibeflow:theme`

Set up design system with research-based theme recommendations.

**What it does:**
1. Analyzes app type and target users from overview.md
2. Researches industry standards for similar apps
3. Presents 3-4 theme options with research-backed explanations
4. Generates full Material 3 colors.json and typography.json

**Generated files:**
- `specs/design_system/design_system.md`
- `specs/design_system/colors.json`
- `specs/design_system/typography.json`

---

### `/vibeflow:feature`

Build a complete feature with sample data, screens, and business logic.

**What it does:**
1. Lists available features from roadmap
2. Checks feature dependencies
3. Generates in phases:
   - **Phase 1:** Sample data (data.json, models.md)
   - **Phase 2:** Screens (screen, view, page files) — Uses `flutter-ui-design` skill
   - **Phase 3:** Business logic (entities, repositories, use cases, providers)
   - **Phase 4:** Updates feature status

**Generated files:**
- Entities in `lib/domain/entities/`
- Repository interfaces in `lib/domain/repositories/`
- Use cases in `lib/domain/usecases/`
- Repository implementations in `lib/data/repositories/`
- Data sources in `lib/data/datasources/`
- Providers in `lib/presentation/providers/`
- Screens in `lib/presentation/screens/`

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
| Entities exist | +20% |
| Use cases exist | +15% |
| Screens exist | +10% |

---

### `/vibeflow:plan`

Create development plans based on priorities and dependencies.

**What it does:**
1. Parses roadmap for features with metadata
2. Prompts for focus area (P0 only, specific phase, all pending, in-progress)
3. Resolves dependencies using topological sort
4. Orders features by priority (P0 > P1 > P2)
5. Generates development plan

---

### `/vibeflow:validate`

Validate specification files for format consistency and completeness.

**What it does:**
1. Checks product overview, roadmap, data shape format
2. Validates design system JSON files
3. Verifies feature spec completeness
4. Reports inconsistencies with actionable recommendations

---

## Common Errors & Solutions

| Error | Cause | Solution |
|-------|-------|----------|
| Feature spec not found | `/specs/roadmap.md` missing | Run `/vibeflow:new` first |
| Design system exists | User already has colors/typography | Ask to replace or keep |
| Circular dependencies | Feature A depends on B, B depends on A | Break cycle, remove one dependency |
| Invalid dependency | Feature ID doesn't exist | Check roadmap for correct IDs |
| All features done | No pending features | All features marked as `done` |
| Dependencies Required | Feature depends on incomplete features | Build dependencies first |

---

## Implementation Guidelines (Flutter)

When implementing Flutter screens and widgets:

- **Mobile-first & Adaptive** — Use `MediaQuery`, `LayoutBuilder`, `OrientationBuilder`, `MediaQuery.sizeOf`. Support phone, foldable, tablet (NavigationRail), desktop (Sidebar/Rail).
- **Light & Dark Mode** — Always respect `Theme.of(context).brightness`. Use `ColorScheme` for semantic colors.
- **Use Design Tokens** — Map `colors.json` → `ColorScheme.fromSeed(seedColor: primary)`, custom extensions for semantic colors (success, warning, etc.). Use `GoogleFonts.[font]()` for `TextTheme`.
- **Props-Driven Widgets** — All widgets receive data & callbacks via constructor (`final` fields). Never import `data.json` in production widgets — only in development view wrappers.
- **Each Screen Manages Its Own Scaffold** — Feature screens include their own `Scaffold`, `AppBar`, and navigation elements. Use shared widgets from `presentation/widgets/` for consistency.
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
