# Theme Research & Design System

Set up design system with research-based theme recommendations and Material 3 format.

## Step 1: Check Current State

Check if `specs/design_system/colors.json` or `specs/design_system/typography.json` exists.

**If design system exists:** Use AskUserQuestion:
- "Replace with new theme" — Overwrites existing
- "Keep existing, research new options" — Research but keep current
- "Cancel" — Stop execution

**If no design system exists:** Proceed to Step 2.

---

## Step 2: Analyze App Context

Read `specs/overview.md` and `specs/roadmap.md`.

**Extract:**
- App type (SaaS, consumer, internal tool, game, etc.)
- Target audience (age, industry, region, technical proficiency)
- Key features
- Brand considerations

---

## Step 3: Research & Present Options

Based on app analysis, present 3-4 theme options using AskUserQuestion.

**Research considerations:**
- Industry standards for this app type
- Accessibility (WCAG AA/AAA compliance)
- Color psychology
- Use patterns (dark mode, screen time)
- Platform conventions

**Example presentation format:**

```
Based on your [app type] for [target users]:

**Option 1: [Name]**
- Colors: [palette description]
- Typography: [font choices]
- Research: [why this works]
- Best for: [use cases]

**Option 2: [Name]**
- Colors: [palette description]
- Typography: [font choices]
- Research: [why this works]
- Best for: [use cases]
```

Wait for user selection.

---

## Step 4: Generate Design System

**Immediately create** all design system files:

### 4.1 Design System Spec

Create `specs/design_system/design_system.md` — See `SPEC_FORMATS.md` for template.

### 4.2 Colors JSON

Create `specs/design_system/colors.json` — See `SPEC_FORMATS.md` for complete Material 3 format.

**Color format:** Use hex codes (`#3B82F6`).

### 4.3 Typography JSON

Create `specs/design_system/typography.json` — See `SPEC_FORMATS.md` for complete Material 3 format.

### 4.4 Generate Design Tokens

**First, generate Dart design tokens from JSON specs:**

Create `lib/core/theme/design_tokens.dart` by parsing the JSON files and generating static const Color values.

**Generation Instructions:**
1. Parse `specs/design_system/colors.json` for color values
2. Parse `specs/design_system/typography.json` for font configurations
3. Generate DesignTokens class with:
   - Primary colors (primaryLight, primaryDark, etc.)
   - Semantic colors (success, warning, error, info)
   - Custom colors (priorityHigh, priorityMedium, priorityLow)
   - Typography font names
   - Spacing and border radius constants

**Template:** See `CODE_TEMPLATES.md` for design_tokens.dart template

### 4.5 Flutter Theme Code

Create `lib/core/theme/app_theme.dart` — See `CODE_TEMPLATES.md` for template.

**Template uses:**
- DesignTokens for all color values
- ThemeExtension for custom semantic colors
- Material 3 ColorScheme.fromSeed() with DesignTokens.primaryLight/Dark
- Complete TextTheme with typography from DesignTokens
- Run `flutter analyze`. Fix any errors until clean.

---

## Step 5: Confirm with User

Present summary with theme name, colors, typography, and generated files list.

Provide next steps (plan, feature, status commands).

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion — see CLAUDE.md
- **Immediate file writing:** Always create files directly
- **Research-based:** Ground theme options in industry research
- **Material 3 format:** See `SPEC_FORMATS.md` for complete format
- **Accessibility:** Ensure WCAG AA compliance (4.5:1 normal text, 3:1 large text)
- **Google Fonts:** Recommend for easy integration
- **Reference:** All formats in `SPEC_FORMATS.md`, templates in `CODE_TEMPLATES.md`

---

## Research Reference (Quick)

**Color Psychology:**
- Blue — Trust, stability (finance, healthcare)
- Green — Growth, calm (wellness, productivity)
- Purple — Creativity, luxury (creative tools)
- Orange — Energy, friendliness (social, fitness)
- Red — Urgency, excitement (alerts, entertainment)

**Typography:**
- Geometric Sans — Modern, clean (Inter, Manrope, SF Pro)
- Humanist Sans — Friendly (Nunito, Open Sans)
- Grotesque Sans — Bold (Space Grotesk, Plus Jakarta Sans)
- Mono — Technical (JetBrains Mono, Fira Code)

**Industry Patterns:**
- Finance — Blue, gray, conservative fonts
- Health/SaaS — Green, blue, clean sans-serif
- Social — Vibrant colors, friendly fonts
- Entertainment — Bold colors, expressive typography
- Productivity — Calm colors, highly readable fonts
