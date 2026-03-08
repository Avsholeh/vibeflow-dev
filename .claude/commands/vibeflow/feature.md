# Build Feature

Generate complete feature with sample data, screens, and business logic.

## Step 1: Check Prerequisites

Check if `specs/roadmap.md` exists. If not: "Please run `/vibeflow:new` first."

---

## Step 2: List Available Features

**If development plans exist** (`/specs/plans/`):
1. Read latest development plan
2. Display features from plan in implementation order

**If no plans or user types 'all':**
Read roadmap and list all available features by priority.

Display format:
```
**Priority: P0**
1. **[Feature 1]** (F001) — Status: pending
2. **[Feature 2]** (F002) — Status: pending
```

Wait for user input (feature name or number).

---

## Step 3: Feature Selection & Validation

Map number/name to feature. If no match, show available features again.

---

## Step 4: Build Complete Feature

### Phase 1: Update Status

Mark feature as `in_progress` in roadmap.

### Phase 2: Generate Sample Data

**Feature path construction:** See `ALGORITHMS.md` for slug conversion.

Create:
- `specs/modules/[module]/[feature_slug]/data.json`
- `specs/modules/[module]/[feature_slug]/models.md`

### Phase 3: Implement Screens

**⚠️ MANDATORY: Use flutter-ui-design Skill**

Before writing any Flutter UI code, invoke the `flutter-ui-design` skill:
- skill: "flutter-ui-design"
- args: "[Screen description] | Context: [feature context] | Design tokens from specs/design_system/"

**Design Context:**
1. Read `specs/design_system/colors.json`
2. Read `specs/design_system/typography.json`
3. Read feature spec
4. Pass context to skill

**After Skill Returns:** Implement screen using design guidance.

**Screen paths:**
- Screen widgets in `lib/presentation/screens/[module]/`
- View wrappers using sample data
- Page wrappers for production

See `CODE_TEMPLATES.md` for screen templates.

### Phase 4: Implement Logic

**Domain Layer:**
- Domain entities in `lib/domain/entities/` (flat, no feature folders)
- Repository interfaces in `lib/domain/repositories/[module]/`

**Data Layer:**
- Repository implementations in `lib/data/repositories/[module]/`
- Data sources in `lib/data/datasources/[module]/`
- DTOs in `lib/data/models/` (if needed)

**Presentation Layer:**
- Providers in `lib/presentation/providers/`

**Entity Deduplication:** See `ALGORITHMS.md` for merge algorithm.

### Phase 5: Update Routing

Update `lib/presentation/routes/app_routes.dart` — See `CODE_TEMPLATES.md` for routing template.

### Phase 6: Code Quality Check

Run `flutter analyze`. Fix any errors until clean.

### Phase 7: Update Feature Status

Mark feature as `done` in roadmap.

---

## Step 5: Summary

Present summary with generated files list and next steps (test, status, plan, build another).

---

## Important Notes

- **Development plan integration:** When plans exist, prioritize showing features from latest plan
- **flutter-ui-design Skill:** MANDATORY for distinctive UI — see CLAUDE.md
- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion — see CLAUDE.md
- **Atomic execution:** If any phase fails, report error and stop
- **Slug conversion:** See `ALGORITHMS.md`
- **Progress calculation:** See `ALGORITHMS.md`
- **File structure:** Follow VibeFlow Clean Architecture — see CLAUDE.md
- **Provider setup:** See `CODE_TEMPLATES.md`
- **Immediate writing:** Never show drafts
- **Reference:** All formats in `SPEC_FORMATS.md`, templates in `CODE_TEMPLATES.md`

---

## Dependency Checking

Before building, check feature dependencies. If incomplete:

"⚠️ **Dependencies Required:** This feature depends on: [list]

Current status: [dependency statuses]

**Recommendation:** Build dependencies first."

Use AskUserQuestion:
- "Continue anyway" — Proceed with build
- "Cancel and build dependencies first" — Stop execution

---

## Definition of Done

A feature is complete when:

**Spec & Data (60%):**
- [ ] `spec.md` exists with complete description
- [ ] `data.json` exists with realistic sample data
- [ ] `models.md` exists with Dart model definitions

**Domain Layer (24%):**
- [ ] Domain entity classes exist
- [ ] Repository interface exists

**Data Layer (N/A):**
- [ ] Repository implementation exists
- [ ] Data source exists (mock/API/database)

**Screens (12%):**
- [ ] `*_screen.dart` files exist (props-driven)
- [ ] `*_view.dart` files exist (development)
- [ ] `*_page.dart` files exist (production)
- [ ] flutter-ui-design skill used

**Checks:**
- [ ] Code compiles (`flutter analyze` passes)
- [ ] Feature status in roadmap is `done`
