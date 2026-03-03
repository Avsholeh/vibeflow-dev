# Theme Presets

You are helping the user quickly set up a design system for their Flutter app using preset themes or custom configuration.

## Step 1: Check Current State

Check if `specs/design-system/colors.json` or `specs/design-system/typography.json` exists.

**If design system already exists:**

"You already have a design system configured:
- Colors: [yes/no]
- Typography: [yes/no]

**Options:**
1. Replace with preset theme (overwrites existing)
2. Keep existing, choose different preset
3. Cancel and keep current setup"

Wait for user choice.

**If no design system exists:**

Proceed to Step 2.

---

## Step 2: Check for Preset Flag

Check if user specified a preset via flag:

```
/vibeflow:theme --preset [name]
```

**If preset flag provided:**
- Skip interactive selection
- Use the specified preset directly
- Proceed to Step 4

**If no preset flag:**
- Proceed to Step 3 for interactive selection

---

## Step 3: Interactive Selection (if no preset flag)

Present preset options:

"Choose a theme preset for your app:

| Preset | Style | Best For |
|--------|-------|----------|
| **modern** | Clean, indigo/purple, Inter font | Most apps, SaaS, productivity |
| **minimal** | Black/white, Geist font | Content-focused, readers, tools |
| **playful** | Colorful, rounded, Nunito font | Kids apps, games, social |
| **professional** | Blue/gray, SF Pro font | Business, finance, enterprise |
| **nature** | Green/earth tones, organic | Health, wellness, sustainability |
| **bold** | High contrast, strong colors | Brands with strong identity |

Type a preset name to select it:"

Wait for user input.

**If user types a valid preset name:**
- Valid presets are: `modern`, `minimal`, `playful`, `professional`, `nature`, `bold`
- Use that preset
- Proceed to Step 4

**If user types an invalid preset name:**
- Show error: "Invalid preset name. Valid options: modern, minimal, playful, professional, nature, bold"
- Show available presets list with descriptions
- Ask user to type again
- Suggest using `/vibeflow:theme --help` to see all options

**Note:** After selecting a preset, you can manually edit `specs/design-system/colors.json` and `specs/design-system/typography.json` to customize colors and fonts.

---

## Step 4: Generate Design System

**Immediately create** `specs/design-system/colors.json` and `specs/design-system/typography.json` based on the preset.

**Read preset data from** `.claude/data/themes/[preset].json` and create the spec files.

**Available Presets:**

| Preset | Colors | Typography | Description |
|--------|--------|------------|-------------|
| `modern` | indigo, purple, slate | Manrope, Inter, JetBrains Mono | Clean and contemporary |
| `minimal` | zinc, stone, neutral | Geist (all variants) | Ultra-minimal black and white |
| `playful` | pink, orange, slate | Fredoka, Nunito, Comic Neue | Vibrant and friendly |
| `professional` | blue, cyan, gray | SF Pro Display, SF Pro Text, SF Mono | Business-ready |
| `nature` | emerald, lime, stone | Nunito, Open Sans, IBM Plex Mono | Earthy and natural |
| `bold` | violet, fuchsia, slate | Space Grotesk, Plus Jakarta Sans, Fira Code | High contrast |

---

## Step 5: Confirm with User

After creating the files, reply:

"I've set up your design system with the **[preset name]** preset:

**Colors:** [primary], [secondary], [neutral] base
**Typography:** [heading] (headings), [body] (body), [mono] (code)

Files created:
- `specs/design-system/colors.json`
- `specs/design-system/typography.json`

You can:
- Change preset: `/vibeflow:theme --preset [name]`
- Manually edit the design system files to customize

**What's next:**
- `/vibeflow:new` — Start your app
- `/vibeflow:feature` — Build your first feature"

---

## Color Palette Reference

All presets use Tailwind-inspired color names. Full palette:

| Primary Options | Secondary Options | Neutral Options |
|-----------------|-------------------|-----------------|
| indigo | purple | slate |
| blue | cyan | gray |
| sky | teal | zinc |
| violet | fuchsia | stone |
| purple | pink | neutral |
| pink | rose | |
| red | orange | |
| orange | amber | |
| amber | yellow | |
| yellow | lime | |
| lime | green | |
| green | emerald | |
| emerald | teal | |
| teal | cyan | |
| cyan | sky | |

---

## Font Reference

All presets use Google Fonts (except SF Pro variants). Available fonts:

**Headings:**
- Manrope, Inter, Geist, SF Pro Display, Fredoka, Nunito, Space Grotesk, Plus Jakarta Sans, Outfit, Montserrat

**Body:**
- Inter, Geist, SF Pro Text, Nunito, Open Sans, Plus Jakarta Sans, Inter, DM Sans, work Sans

**Mono:**
- JetBrains Mono, Geist Mono, Comic Neue, SF Mono, IBM Plex Mono, Fira Code, JetBrains Mono

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking the user questions — never ask through text
- **Immediate file writing:** Always create files directly, no drafts
- **Presets are starting points:** Users can customize after selecting a preset
- **Backward compatible:** Existing custom colors/typography are preserved unless user chooses to replace
- **Google Fonts:** Most presets use Google Fonts (free, easy integration)
- **SF Pro:** Requires Apple devices or licensing; suggest alternatives for cross-platform
- **Preset flag:** Allows non-interactive usage for scripting/power users
- **Help command:** `/vibeflow:theme --help` shows all presets
- **Data files:** Preset definitions are stored in `.claude/data/themes/[preset].json`

---

## Examples

```bash
# Interactive selection
/vibeflow:theme

# Direct preset selection
/vibeflow:theme --preset modern

# View available presets
/vibeflow:theme --help
```
