# VibeFlow Workflow Reference

This document contains shared constants, rules, and formats used across all VibeFlow commands.

---

## Feature Slug Conversion

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

## Progress Calculation

### Progress Percentage Formula

```
progress = (spec_complete * 25) +
           (sample_data * 25) +
           (types_documented * 10) +
           (models_defined * 20) +
           (logic_implemented * 15) +
           (screens_implemented * 10)
```

### Status Determination

| Progress | Status |
|----------|--------|
| 0% | `pending` |
| 1-49% | `started` |
| 50-99% | `in_progress` |
| 100% | `done` |

### File Checks

**Spec Status:**
- `/specs/features/[feature_slug]/spec.md` exists → Spec complete (+25%)
- `/specs/features/[feature_slug]/data.json` exists → Sample data (+25%)
- `/specs/features/[feature_slug]/models.md` exists → Types documented (+10%)

**Implementation Status:**
- `/lib/features/[feature_slug]/domain/models/` has .dart files → Models defined (+20%)
- `/lib/features/[feature_slug]/providers/` has non-empty .dart files → Logic implemented (+15%)
- `/lib/features/[feature_slug]/screens/*_page.dart` files exist → Screens implemented (+10%)

---

## Effort Estimation

| Effort | Points | Duration |
|--------|--------|----------|
| `small` | 1 point | 1-2 days |
| `medium` | 3 points | 3-5 days |
| `large` | 5 points | 1-2 weeks |
| `xlarge` | 8 points | 2-4 weeks |

### Sprint Capacity Calculation

| Duration | Points (per developer) |
|----------|----------------------|
| 1 week | ~6 points |
| 2 weeks | ~10 points |
| 1 month | ~20 points |

**Team capacity:** Base capacity × Number of developers

---

## Priority Levels

| Priority | Meaning | When to Use |
|----------|---------|-------------|
| `P0` | Critical | Core value features, blocking other features |
| `P1` | Important | Supporting features, nice to have soon |
| `P2` | Nice to have | Enhancements, can be deferred |

---

## Status Values

| Status | Meaning |
|--------|---------|
| `pending` | Not started |
| `started` | Some work done, less than 50% |
| `in_progress` | Work in progress, 50-99% |
| `done` | Complete, 100% |
| `blocked` | Cannot proceed due to dependencies |

---

## Roadmap Format

### Feature Format

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
```

### Metadata Fields

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

## Theme Presets

Theme preset definitions are stored in `.claude/data/themes/[preset].json`

| Preset | Colors | Typography |
|--------|--------|------------|
| `modern` | indigo, purple, slate | Manrope, Inter, JetBrains Mono |
| `minimal` | zinc, stone, neutral | Geist (all variants) |
| `playful` | pink, orange, slate | Fredoka, Nunito, Comic Neue |
| `professional` | blue, cyan, gray | SF Pro Display, SF Pro Text, SF Mono |
| `nature` | emerald, lime, stone | Nunito, Open Sans, IBM Plex Mono |
| `bold` | violet, fuchsia, slate | Space Grotesk, Plus Jakarta Sans, Fira Code |

---

## File Structure Patterns

### Spec Files

```
specs/
├── overview.md
├── roadmap.md
├── data-shape.md
├── design-system/
│   ├── colors.json
│   └── typography.json
└── features/
    └── [feature_slug]/
        ├── spec.md
        ├── data.json
        └── models.md
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

## Screen File Pattern

Each screen has three files:

| File | Purpose | Data Source |
|-----|---------|-------------|
| `*_screen.dart` | Pure UI component (props-driven) | Constructor props |
| `*_view.dart` | Development wrapper | `data.json` |
| `*_page.dart` | Production wrapper | Provider |

---

## Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Feature spec not found | `/specs/roadmap.md` missing | Run `/vibeflow:new` first |
| Design system exists | User already has colors/typography | Ask to replace or keep |
| Circular dependencies | Feature A depends on B, B depends on A | Break cycle, remove one dependency |
| Invalid dependency | Feature ID doesn't exist | Check roadmap for correct IDs |
| All features done | No pending features | All features marked as `done` |
| No features fit | Smallest feature > capacity | Increase duration or reduce scope |

---

## UI Implementation Requirements

### Mandatory: Use flutter-ui-design Skill

When implementing any Flutter UI screens, agents MUST use the `flutter-ui-design` skill.

The skill provides:
- **Design thinking principles** (purpose, tone, platform reality, differentiation)
- **Flutter aesthetics guidelines** (typography, color, motion, layout, textures)
- **Anti-patterns to avoid** (generic Material 3, boring cards, overused colors)

### Key Principles

1. **Commit to one bold aesthetic** — No middle-ground "pleasant modern app"
2. **Characterful typography** — Never default to system fonts
3. **Strong color theme** — Commit hard to a distinctive palette
4. **Motion & delight** — Implicit animations, hero moments
5. **Spatial composition** — Break grids intentionally when justified
6. **Avoid** — Generic Material 3, boring cards, overused color schemes

### When to Use the Skill

Invoke `flutter-ui-design` whenever implementing:
- Screen widgets
- Custom widgets
- Navigation flows
- Full feature UIs
- Visual components

### Skill Location

`.claude/skills/flutter-ui-design/skill.md`

---

## User Interaction Guidelines

### Mandatory: Use AskUserQuestion Tool

When asking the user questions, ALWAYS use the `AskUserQuestion` tool instead of asking through text.

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

**Example:**
```
AskUserQuestion with:
- question: "Which visual style would you like for your app?"
- header: "Style"
- options: [modern, minimal, playful, professional, nature, bold]
```

**Do NOT ask questions through text.** Always use the AskUserQuestion tool for user interactions.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-03-01 | Initial reference document |
