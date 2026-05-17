---
name: shadcn-ui-feel
description: Transform shadcn/ui interfaces from generic AI defaults into distinctive, designed UIs. Use this skill whenever building or restyling with shadcn/ui components, Tailwind, or any React/Next.js UI — especially when the result needs to feel crafted rather than generated. Forces intentional design decisions across color tokens, typography, radius, density, and motion before any component code is written.
---

# shadcn/ui Feel — Break the AI Default

shadcn/ui defaults are the fingerprint of AI-generated UI. When every project leaves `--radius: 0.5rem`, primary as blue-600, font-sans as Inter, and borders as gray-200 — the result is immediately recognizable. This skill breaks every default intentionally.

## Step 1 — Four Decisions Before Any Code

Make these four decisions explicitly and state them before writing components:

**1. Color axis** — Pick one dominant hue that isn't blue, purple, or gray. Examples:
- Warm amber-tinted neutrals (zinc-900 with amber-600 accent)
- Deep forest green with cream
- Charcoal-black with coral or terracotta
- Warm off-white with deep navy
- Slate with a vivid orange accent

**2. Type pairing** — Name two fonts. Neither should be Inter as the primary. Options:
- `Fraunces` (display) + `DM Sans` (body) — editorial, refined
- `Instrument Serif` (display) + `Geist` (body) — modern editorial
- `Syne` (display) + `IBM Plex Mono` (body) — technical, precise
- `Playfair Display` (display) + `Nunito Sans` (body) — warm, approachable
- `Cabinet Grotesk` (display) + `Switzer` (body) — clean, contemporary

**3. Radius personality** — Pick one extreme:
- `--radius: 0rem` — sharp, deliberate, editorial
- `--radius: 0.75rem` to `1rem` — soft, rounded, friendly
- **Never** `0.5rem` — it is the AI fingerprint

**4. Density** — Either compact (tight, information-dense) or spacious (generous padding). The medium default is invisible.

---

## Step 2 — Rewrite the CSS Variable Layer

Replace the default `globals.css` theme section completely. Never leave shadcn's init defaults in production:

```css
@layer base {
  :root {
    /* Surface — never pure white */
    --background: [H S% L%];      /* e.g. 45 30% 98% = warm off-white */
    --foreground: [H S% L%];      /* e.g. 220 15% 12% = soft near-black */

    /* Card — must visually separate from background */
    --card: [H S% L%];
    --card-foreground: var(--foreground);

    /* Primary — your chosen dominant hue */
    --primary: [H S% L%];
    --primary-foreground: [H S% L%];

    /* Secondary and muted — hue-tinted, never neutral gray */
    --secondary: [H S% L%];
    --secondary-foreground: [H S% L%];
    --muted: [H S% L%];           /* Add color, not just lightness */
    --muted-foreground: [H S% L%];

    /* Accent — a second unexpected color for hover/interactive states */
    --accent: [H S% L%];
    --accent-foreground: [H S% L%];

    /* Destructive */
    --destructive: 0 72% 51%;

    /* Borders — use a hue-tinted color, never gray-200 */
    --border: [H S% L%];
    --input: [H S% L%];
    --ring: [H S% L%];            /* Focus ring matches brand, not blue */

    /* Radius — from your Step 1 decision */
    --radius: [0rem | 0.75rem | 1rem];
  }

  .dark {
    /* Dark mode: deep, rich — not inverted light mode */
    --background: [H S% 8%];      /* Dark with color, not pure black */
    --foreground: [H S% 92%];     /* Warm white, not pure #fff */
    /* ... all tokens explicitly set */
  }
}
```

---

## Step 3 — Typography Setup

Load fonts via `next/font` or `@fontsource` — never CDN in production.

```tsx
// app/layout.tsx
import { Fraunces, DM_Sans } from 'next/font/google'

const display = Fraunces({ subsets: ['latin'], variable: '--font-display' })
const body = DM_Sans({ subsets: ['latin'], variable: '--font-body' })
```

Apply in `globals.css`:
```css
:root {
  --font-display: 'Fraunces', serif;
  --font-body: 'DM Sans', sans-serif;
}

body { font-family: var(--font-body); }
h1, h2, h3, .display { font-family: var(--font-display); }
```

**Type scale to use consistently:**
- Display/Hero: `text-5xl font-bold tracking-tight` + display font
- H1: `text-3xl font-semibold tracking-tight`
- H2: `text-xl font-medium`
- Body: `text-base leading-relaxed`
- Caption/Label: `text-sm text-muted-foreground`
- Overline: `text-xs font-medium uppercase tracking-wider`

---

## Step 4 — Component Patterns That Feel Designed

**Buttons — enforce hierarchy:**
- One `default` button per primary action in any view
- `outline` for secondary actions
- `ghost` for tertiary actions
- Never more than 2 button variants visible simultaneously

Add hover lift to interactive cards:
```tsx
<Card className="hover:shadow-md hover:-translate-y-0.5 transition-all duration-200 cursor-pointer">
```

**Forms — always label above, never placeholder-only:**
```tsx
<FormField>
  <FormLabel>Email address</FormLabel>
  <FormControl><Input placeholder="you@example.com" /></FormControl>
  <FormDescription>Used for account notifications.</FormDescription>
  <FormMessage />  {/* Error state */}
</FormField>
```

**Badges and status indicators:**
- Use semantic color variants, not the default gray
- Add `dot` indicators for live/active states
- Keep badge text under 2 words

**Data tables:**
- Alternating rows: subtle, not striped-zebra obvious
- Selected row: brand color at 10% opacity, not the shadcn default blue
- Column headers: `text-xs font-medium uppercase tracking-wider text-muted-foreground`

---

## Step 5 — Motion That Feels Considered

Every interactive element should have deliberate transitions:

```tsx
// Standard interactive class combos
"transition-colors duration-150"              // Color changes
"transition-all duration-200"                 // Lift + shadow
"transition-transform duration-150 ease-out"  // Scale effects
```

Page-level animations (framer-motion):
```tsx
// Stagger children — never animate everything at once
const container = { hidden: {}, show: { transition: { staggerChildren: 0.05 } } }
const item = { hidden: { opacity: 0, y: 8 }, show: { opacity: 1, y: 0 } }
```

Rules:
- `ease-out` for enters, `ease-in` for exits
- No bouncing springs on professional/serious UIs
- Stagger delay: 40–80ms per item, never more

---

## Anti-Patterns Checklist

Run through this before calling any shadcn UI complete:

- [ ] Default blue primary color left unchanged
- [ ] `--radius: 0.5rem` (the default — must be deliberately changed)
- [ ] Inter as the only font with no display font
- [ ] Pure white `#ffffff` or `hsl(0 0% 100%)` background
- [ ] Pure black text
- [ ] `border-gray-200` or `border-border` with no hue tint
- [ ] Gradient text on hero heading (purple-to-blue)
- [ ] All buttons in a view have the same visual weight
- [ ] Placeholder text as the only form label
- [ ] Default shadcn toast at bottom-right when UX calls for something else
- [ ] `cn()` with no custom classes — all components using shadcn defaults only
- [ ] Dark mode that's just the inverted light palette

---

## Quick Wins (Always Apply These)

1. **Warm the background**: `hsl(45 30% 98%)` instead of pure white — instant human feel
2. **Tint borders**: Add 10–20% hue saturation to `--border`, remove pure gray
3. **Card separation**: Cards 2–3 lightness steps different from background
4. **Focus rings**: Set `--ring` to match your primary, remove the default blue
5. **One accent**: Pick one unexpected hover/focus color — used only on interactive states
6. **Shadow with color**: `shadow-[0_2px_8px_hsl(var(--primary)/0.12)]` instead of gray shadows
7. **Monochrome baseline**: Build light mode in grayscale first, add color only where it earns its place
