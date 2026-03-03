# Theme Research & Design System

You are helping the user set up a design system for their Flutter app using research-based theme recommendations and Material 3 format.

## Step 1: Check Current State

Check if `specs/design-system/colors.json` or `specs/design-system/typography.json` exists.

**If design system already exists:**

"You already have a design system configured:
- Colors: [yes/no]
- Typography: [yes/no]

**Options:**
1. Replace with new research-based theme (overwrites existing)
2. Keep existing, research new options
3. Cancel and keep current setup"

Wait for user choice.

**If no design system exists:**

Proceed to Step 2.

---

## Step 2: Analyze App Context

Read the following files to understand the app context:
- `specs/overview.md` — App description, target users, problems, solutions
- `specs/roadmap.md` — Features and functionality

**Extract:**
- App type (SaaS, consumer, internal tool, game, etc.)
- Target audience (age, industry, region, technical proficiency)
- Key features (what users will do most often)
- Brand considerations (existing brand, new brand, personality)

---

## Step 3: Research & Present Options

Based on the app analysis, research and present 3-4 theme options with explanations.

**Research considerations:**
1. **Industry standards** — What colors and fonts are commonly used for this app type?
2. **Accessibility** — WCAG AA/AAA compliance, color contrast ratios
3. **Psychology of color** — What emotions do different colors evoke?
4. **Use patterns** — Dark mode usage, screen time expectations
5. **Platform conventions** — Material Design, iOS patterns

**Present options using AskUserQuestion:**

```
Based on your [app type] for [target users], here are researched theme options:

**Option 1: [Name]**
- Colors: [palette description]
- Typography: [font choices]
- Research: [why this works for this app type]
- Best for: [use cases]

**Option 2: [Name]**
- Colors: [palette description]
- Typography: [font choices]
- Research: [why this works for this app type]
- Best for: [use cases]

**Option 3: [Name]**
- Colors: [palette description]
- Typography: [font choices]
- Research: [why this works for this app type]
- Best for: [use cases]
```

**Example presentation for a finance app:**

```
Based on your finance app for tracking daily expenses, here are researched theme options:

**Option 1: Professional Trust**
- Colors: Blue/Gray palette (proven to build trust in finance apps)
- Typography: Inter / SF Pro (industry standard for readability)
- Research: 78% of banking apps use blue as primary color; blue conveys trust and stability
- Best for: Trust-sensitive applications, professional users

**Option 2: Modern Clarity**
- Colors: Indigo/Purple with clean neutrals (distinctive, memorable)
- Typography: Manrope headings, Inter body (geometric, friendly)
- Research: SaaS trend toward distinct branding while maintaining clarity
- Best for: Users who interact daily, want personality without sacrificing clarity

**Option 3: Calm Focus**
- Colors: Green/Emerald with warm neutrals (reduces financial anxiety)
- Typography: Nunito / Open Sans (friendly, approachable)
- Research: Green reduces cognitive load and anxiety in task apps; warm tones feel less clinical
- Best for: Wellness-focused finance, users who find money management stressful
```

Wait for user selection.

---

## Step 4: Generate Design System

**Immediately create** all design system files based on selected option:

### 4.1: Create design-system.md

```markdown
# Design System — [App Name]

## Aesthetic
[Visual style description - 2-3 sentences]

## Color Philosophy
[How colors are used - e.g., "Subtle borders, purposeful accents for CTAs"]

## Typography Philosophy
[Font choices and readability - e.g., "Geometric sans for headings, readable sans for body"]

## Component Patterns
[Key UI patterns - e.g., "Cards with subtle borders", "Rounded buttons"]

## Motion & Interaction
[Animation patterns - e.g., "Micro-interactions on hover", "Smooth page transitions"]
```

### 4.2: Create colors.json

**Must follow Material 3 format** (see CLAUDE.md for complete format).

Include:
- `light` — All Material 3 color roles (primary, secondary, tertiary, error, background, surface, etc.)
- `dark` — Complete dark mode variants
- `semantic` — success, warning, info colors
- `custom` — Border, filled, subtle colors

**Color format:** Use hex codes (`#3B82F6`), not color names.

### 4.3: Create typography.json

**Must follow Material 3 format** (see CLAUDE.md for complete format).

Include:
- `fontFamilies` — primary, secondary, tertiary
- `textStyles` — All Material 3 text styles (displayLarge, headlineLarge, titleLarge, bodyLarge, labelLarge, etc.)
- `custom` — overline, caption, mono, button
- `weights` — All font weight mappings
- `sizes` — Font size scale

---

## Step 5: Confirm with User

After creating the files, confirm:

"I've set up your design system:

**Theme:** [Option name]
**Colors:** [primary], [secondary], [neutral] base
**Typography:** [heading] (headings), [body] (body), [mono] (code)

Files created:
- `specs/design-system/design-system.md` — Design principles
- `specs/design-system/colors.json` — Material 3 color tokens
- `specs/design-system/typography.json` — Material 3 text styles

You can:
- Manually edit the design system files to customize
- Run `/vibeflow:theme` again to choose different options

**What's next:**
- `/vibeflow:feature` — Build your first feature
- `/vibeflow:status` — Check your progress"

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking the user questions — never ask through text
- **Immediate file writing:** Always create files directly, no drafts
- **Research-based:** Theme options should be grounded in industry research, not arbitrary choices
- **Material 3 format:** Always use complete Material 3 format for colors.json and typography.json (see CLAUDE.md)
- **Accessibility:** Ensure WCAG AA compliance for color contrast (4.5:1 for normal text, 3:1 for large text)
- **Google Fonts:** Recommend Google Fonts for easy integration (free, reliable)
- **Customizable:** Users can manually edit generated files after creation
- **Reference:** See CLAUDE.md for complete Material 3 format specifications

---

## Research Reference

**Color Psychology:**
- Blue — Trust, stability, professionalism (finance, healthcare, enterprise)
- Green — Growth, health, calm (wellness, productivity, environmental)
- Purple — Creativity, luxury, wisdom (creative tools, premium apps)
- Orange — Energy, friendliness, enthusiasm (social, fitness, food)
- Red — Urgency, passion, excitement (alerts, notifications, entertainment)

**Typography Categories:**
- Geometric Sans — Modern, clean, scalable (Inter, Manrope, SF Pro)
- Humanist Sans — Friendly, approachable (Nunito, Open Sans, DM Sans)
- Grotesque Sans — Bold, distinctive (Space Grotesk, Plus Jakarta Sans)
- Mono — Technical, precise (JetBrains Mono, Fira Code, IBM Plex Mono)

**Industry Patterns:**
- Finance — Blue, gray, conservative fonts
- Health/SaaS — Green, blue, clean sans-serif
- Social — Vibrant colors, friendly fonts
- Entertainment — Bold colors, expressive typography
- Productivity — Calm colors, highly readable fonts
