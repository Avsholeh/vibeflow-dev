---
name: flutter-ui-design
description: Create distinctive, production-grade Flutter interfaces with exceptional visual character. Use this skill when the user asks to build screens, components, flows, or full Flutter apps. Generates creative, polished Dart/Flutter code that avoids generic Material "slop" and cookie-cutter designs.
license: Complete terms in LICENSE.txt
---

This skill guides the creation of **unforgettable Flutter UIs** — production-ready, performant screens and components that feel genuinely designed rather than AI-defaulted.

The user provides Flutter requirements: a screen, custom widget, navigation flow, full feature, or complete app UI. They may include context (target audience, brand vibe, platform priorities iOS/Android/web, performance constraints, accessibility needs, etc.).

## Design Thinking

Before writing any Dart code, deeply understand the context and **commit to one BOLD, coherent aesthetic direction**:

- **Purpose** — What core job is this screen/feature doing? Who is the actual human using it?
- **Tone & Personality** — Choose an extreme and own it: brutal minimalism with razor-sharp micro-details • maximalist joyful chaos • retro vaporwave/neon • brutalist raw concrete textures • organic/hand-drawn fluidity • luxury high-fashion restraint • playful toy-like exaggeration • editorial Swiss precision • cyberpunk glitch/distortion • soft-monochrome melancholy • industrial heavy-metal weight • art-deco geometry • lo-fi grainy nostalgia — etc. Avoid middle-ground "pleasant modern app".
- **Platform reality** — Mobile-first (touch, thumb zones, safe areas, dynamic type, foldables), but consider desktop/web if requested.
- **Differentiation** — What single visual or interaction choice will make someone remember this screen 3 weeks later?

**CRITICAL**: Lock in **one** clear conceptual north star and execute it relentlessly. Half-hearted eclecticism kills memorability. Bold maximalism and extreme restraint both succeed — mediocrity fails.

Then deliver clean, idiomatic Flutter code that is:
- Fully functional and production-grade (no placeholder logic, proper state, error handling where relevant)
- Visually striking and cohesive
- Performant (const constructors, efficient rebuilds, judicious use of animations)
- Accessible (semantics, sufficient contrast, dynamic type scaling)
- Meticulously refined (pixel-perfect spacing, harmonious proportions, thoughtful micro-details)

## Flutter Aesthetics Guidelines

Focus intensely on these levers to escape generic Flutter UIs:

- **Typography** — Never default to system fonts or plain Google Fonts Roboto/Inter. Choose characterful pairings:
  - Display: something unexpected (e.g. Playfair Display, Space Grotesk alternatives like GT Walsheim, Cabinet Grotesk, Neue Haas Grotesk, hyper-bold experimental fonts, hand-drawn scripts when appropriate)
  - Body: refined & highly legible (e.g. Source Serif, Literata, Satoshi, Apfel Grotesk, recursive variable fonts)
  - Use Google Fonts, custom .ttf/.otf assets, variable fonts, FontFeature, TextStyle.letterSpacing / height tuning aggressively.

- **Color & Theme** — Commit hard. Define a ThemeData (or custom class) with semantic tokens (primary, accent, surface, danger, etc.). Prefer:
  - Strong dominant hue + sharp contrasting accent
  - Dramatic dark/light modes with true personality difference (not just hue shift)
  - Gradients, duotones, subtle mesh gradients, color-burn/blend modes via shaders when justified
  - Avoid pastel-everything, corporate blue-purple gradients, or Material 3 defaults without heavy customization

- **Motion & Delight** — Motion is personality.
  - Prefer implicit animations (AnimatedContainer, AnimatedOpacity, Hero, AnimatedSwitcher)
  - Use flutter_animate, rive, lottie only when needed — CSS-like elegance first
  - Focus on **hero moments**: staggered page entrance (stagger delays + curves), satisfying drag physics, surprise hover/scroll effects
  - Custom curves (Curves.easeInOutCubicEmphasized, custom Cubic), spring simulations
  - Scroll-driven effects (SliverAppBar collapse, parallax, shader scrolling)

- **Spatial Composition & Layout** — Break grids intentionally (when it serves the vision)
  - Asymmetry, dramatic negative space OR joyful density
  - Overlapping elements (Stack + ClipPath / ClipRRect with custom shapes)
  - Diagonal flows, rotated containers, custom clippers (bezier paths, wave cuts)
  - Generous use of Slivers for rich scrolling experiences
  - Custom painters for unique backgrounds (noise, grain, geometric patterns, liquid blobs, mesh gradients)

- **Textures & Atmosphere** → Depth creators
  - Subtle noise/grain overlays (CustomPainter or shader)
  - Glassmorphic / neumorphic effects (BackdropFilter + shadows + borders)
  - Layered transparencies, dramatic inner/outer shadows
  - Custom cursors/shapes (on web), ripple effects with personality
  - Decorative SVG assets, hand-crafted icons (not just MaterialIcons)

**NEVER** default to:
- Plain Material 3 / Cupertino without heavy personality override
- Overused color schemes (Material default blue/purple, tailwind-ish grays)
- Boring symmetric centered cards
- Roboto / system font stacks
- Generic neumorphism / glassmorphism without context
- Cookie-cutter bottom nav + FAB placement every time

**Vary wildly** across generations — different fonts, color stories, motion signatures, layout languages. Never converge on the same "nice modern app" look.

**IMPORTANT**: Match code complexity to the vision
- Maximalist → elaborate CustomPaint, multiple Animations, shaders, clippers, Rive if needed
- Restrained luxury/minimal → surgical precision in ThemeData, spacing constants, typography scale, subtle implicit animations only

Flutter gives incredible power — Impeller rendering, variable fonts, advanced Material 3 components, shaders, slivers, custom painters. Use them boldly when the aesthetic calls for it.

Remember: You are capable of extraordinary, context-specific creative work. Commit fully to a distinctive vision. Show what Flutter can really become when design intent leads the code.