# New Flutter App

Generate complete product specification from user's description.

## Step 1: Check Existing Specs

Check if `specs/` directory has existing files.

**If specs exist:** Use AskUserQuestion to present options:
- "Start fresh" — Overwrite existing specs
- "Keep existing" — Add new features to existing project
- "Cancel" — Stop execution

**If directory is clean:** Proceed to Step 2.

---

## Step 2: Gather App Information

Ask user to share their raw notes, ideas, or thoughts about the product. Be warm and open-ended:

"I'd love to help you define your product overview. Tell me about the product you're building - share any notes, ideas, or rough thoughts you have. What problem are you trying to solve? Who is it for? Don't worry about structure yet, just share what's on your mind."

Wait for response before proceeding.

---

## Step 3: Ask Clarifying Questions

Use AskUserQuestion to gather information covering:

**Product Overview:**
- Product name (short, memorable)
- Core description (1-3 sentences)
- Key problems (1-5 pain points)
- How the app solves each problem
- Main features

**Roadmap:**
- Main areas/sections of the product
- Most critical area to build first

**Data Shape:**
- Main "things" users will create, view, or manage
- How these things relate to each other

Ask questions conversationally, one or two at a time.

---

## Step 4: Generate Product Specs

**Immediately create** all spec files with smart defaults.

### 4.1 Product Overview

Create `specs/overview.md` — See `SPEC_FORMATS.md` for template.

**Module Inference:** Group related features under common module headings. Use module inference rules from `ALGORITHMS.md`. Aim for 2-5 modules.

### 4.2 Roadmap

Create `specs/roadmap.md` — See `SPEC_FORMATS.md` for template.

**Infer features from:**
- User's feature list (if provided)
- Key problems mentioned
- App description (extract verbs/nouns)
- Mobile app patterns (auth, dashboard, lists, forms, settings)

**Priority Assignment:** Core value → P0, Supporting → P1, Nice-to-have → P2

**Feature Inference:** See `ALGORITHMS.md` for keyword-to-feature mappings.

### 4.3 Data Shape

Create `specs/data_shape.md` — See `SPEC_FORMATS.md` for template.

**Infer entities from:** Feature descriptions, app description (nouns = entities), common patterns.

### 4.4 Feature Specs

Create `specs/modules/[module]/[feature_slug]/spec.md` for each roadmap item.

**Feature path construction:** See `ALGORITHMS.md` for slug conversion and module inference.

**User Flow Inference:** Create screens → "New", "Edit", "Delete" flows. List screens → "View", "Search", "Filter" flows.

### 4.5 Main Entry Point

Create `lib/main.dart` — See `CODE_TEMPLATES.md` for template.

**Note:** This is a starter template. Update TODO sections as you build features with `/vibeflow:feature`.

### 4.6 Review & Iterate

Present summary and use AskUserQuestion for confirmation:
- "Approve" — Proceed to design system setup
- "Edit overview" — Modify product overview
- "Edit roadmap" — Add/remove/change features
- "Edit data shape" — Modify entities and relationships

Iterate until satisfied.

---

## Step 5: Design System (Optional)

Use AskUserQuestion:
- "Generate now" — Run `/vibeflow:theme` workflow
- "Skip for now" — Continue to Step 6

---

## Step 6: Final Summary

Present comprehensive summary with generated files list and next steps (theme, plan, feature, status commands).

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking questions — see CLAUDE.md
- **Immediate file writing:** Never show drafts, always write directly
- **Spec structure:** See `SPEC_FORMATS.md` for exact formats
- **Inference over questioning:** Extract info from user input, don't ask everything
- **App name required:** Always ask for app name
- **Slug conversion:** See `ALGORITHMS.md`
- **Module inference:** See `ALGORITHMS.md`
