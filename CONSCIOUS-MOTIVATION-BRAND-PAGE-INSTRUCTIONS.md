# Conscious Motivation — Brand Page Build Instructions

## WHAT YOU'RE BUILDING

A single-page brand site for Conscious Motivation (consciousmotivation.com). One self-contained HTML file — all CSS, JS inline. No frameworks, no build tools. This is the public-facing brand page, not the coaching site (that's Walk Through Fire, a sibling site).

The page must visually match or exceed the Walk Through Fire site (walk-through-fires.surge.sh) in craft and atmosphere. Same design family, different personality. WTF is dark, intense, narrative-heavy. This page is light, spacious, confident, minimal.

---

## DESIGN SYSTEM

### Color Palette

```css
:root {
  --bone: #FAF5EF;        /* Primary background — warm white */
  --ash: #1a1a1a;          /* Primary text */
  --smoke: #2d2d2d;        /* Secondary dark */
  --ember: #E8530E;        /* Accent — used sparingly, high impact */
  --ember-glow: #FF6B2B;   /* Hover/glow states */
  --gold: #C49A6C;         /* Labels, dividers, subtle accents */
  --gold-light: #D4B896;   /* Lighter gold for gradients */
  --slate: #8A8A8A;        /* Body text — softer than ash */
  --charcoal: #111111;     /* Vault section background */
  --white: #FFFFFF;        /* Cards, overlays */
}
```

The page is **light-dominant** (bone background) — the inverse of WTF's dark design. The single dark section is The Vault (Section 4). This contrast makes The Vault feel like entering a different space.

### Typography

Use the exact same font stack as WTF for brand family recognition:

```css
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;0,900;1,400;1,700&family=Inter:wght@300;400;500;600&family=JetBrains+Mono:wght@300;400&display=swap');
```

- **Playfair Display** (900, 700): Headlines. Massive. Confident. `clamp(3rem, 8vw, 7rem)` for hero, scale down for sections.
- **Inter** (300, 400): Body text. Clean, modern, light weight preferred. 1.7 line-height for breathing room.
- **JetBrains Mono** (300, 400): Labels, CTAs, section markers. Uppercase, letter-spacing: 0.2–0.3em. Small size (0.75–0.85rem).

### Spacing

Generous. Every section should feel like it has room to breathe.

- Section padding: `10rem 2rem` minimum on desktop, `5rem 1.5rem` on mobile
- Max content width: `900px` for text, `1200px` for product grid
- Between elements: at least `2rem`, prefer `3rem`
- Hero: full viewport height (`100vh`)

### Interactions & Animation

All animations serve the "rising" metaphor — elements enter from below and fade in.

```css
/* Base reveal animation — elements rise into view */
[data-scroll] {
  opacity: 0;
  transform: translateY(40px);
  transition: opacity 0.8s ease-out, transform 0.8s ease-out;
}

[data-scroll].visible {
  opacity: 1;
  transform: translateY(0);
}
```

Use IntersectionObserver to trigger `.visible` class on scroll, same pattern as WTF.

**Hover states on CTAs:**
- Ghost button CTAs: border + text in ember, hover fills with ember background, text goes bone/ash
- Subtle box-shadow glow on hover: `0 0 30px rgba(232, 83, 14, 0.2)`
- Transition: `all 0.4s ease`

**Hover states on product cards:**
- Slight lift: `transform: translateY(-4px)`
- Soft shadow appears
- Keep it subtle — no bouncing or scaling

---

## PARTICLE SYSTEM

Instead of WTF's fire embers, create a **phoenix ash / rising mote** particle system:

- Canvas-based, fixed position, behind all content (z-index: 0)
- Particles rise gently upward (slower than WTF's embers — this site is calmer)
- Color: gold/amber tones, `hsla(35, 60%, 55%, opacity)` range
- Fewer particles than WTF (40 on desktop, 20 on mobile)
- Smaller particle size (1–2px, with 3px glow radius)
- Gentle lateral drift with sine wave motion
- Mouse interaction: particles gently drift away from cursor (same as WTF but softer)
- Opacity: lower overall (0.15–0.4 range) — these are background texture, not a feature
- On the dark Vault section, particles should be more visible (increase opacity to 0.3–0.6)

---

## PHOENIX LOGO

The phoenix logo files are in the project folder:
- `logo-web.png` (123KB, transparent background — use this)
- `Conscious Motivation Logo.jpg` (315KB, with background)

Place the logo:
- **Header**: Small (40–50px height), top-left, fixed position with subtle background on scroll. The logo should be visible but not dominant.
- **The Vault section**: Centered above the headline, slightly larger (80–100px), with a CSS glow effect:

```css
.vault-phoenix {
  filter: drop-shadow(0 0 20px rgba(232, 83, 14, 0.4));
  animation: subtleGlow 3s ease-in-out infinite alternate;
}

@keyframes subtleGlow {
  from { filter: drop-shadow(0 0 15px rgba(232, 83, 14, 0.3)); }
  to { filter: drop-shadow(0 0 25px rgba(232, 83, 14, 0.5)); }
}
```

Convert `logo-web.png` to base64 and embed inline (same approach as WTF's embedded images).

---

## PAGE STRUCTURE — 5 SECTIONS

### SECTION 1: HERO — THE DECLARATION

Full viewport. Centered. Massive whitespace. The page opens with confidence.

```
[Phoenix logo — small, centered above headline]

Out of the ashes,
into your next level of being.

[CTA: "Enter" — ghost button, ember border]

[Scroll indicator — thin gold line descending, pulsing]
```

**Technical specs:**
- Background: `var(--bone)` with a subtle radial gradient adding warmth at center: `radial-gradient(ellipse 80% 60% at 50% 60%, rgba(196, 154, 108, 0.08), transparent)`
- Headline: Playfair Display 900, `clamp(3rem, 8vw, 7rem)`, color `var(--ash)`, line-height 1.0
- "Out of the ashes," on first line, "into your next level of being." on second
- Both lines centered, stacked
- Stagger fade-in animations (logo 0.5s, headline 1s, CTA 1.5s, scroll indicator 2s)
- CTA: JetBrains Mono, 0.8rem, uppercase, letter-spacing 0.25em, ember border ghost button
- Scroll indicator: 1px wide, 50px tall gold line with `scrollPulse` animation (same as WTF)

### SECTION 2: THE WORK — DAILY PRACTICE

Light background. Clean vertical stack. Declarative copy blocks.

```
[Section label: "THE PRACTICE" — JetBrains Mono, gold, tiny]

Daily insights.
Simple lifts.
Steady rise.

One spark of awareness.
One honest action.
One level at a time.

This is the work.

[CTA: "See Today's Insight" — ghost button linking to Instagram or placeholder]
```

**Technical specs:**
- Background: `var(--bone)` — seamless continuation from hero
- Section label: JetBrains Mono 300, 0.75rem, letter-spacing 0.3em, uppercase, `var(--gold)`
- First three lines: Playfair Display 700, `clamp(1.8rem, 4vw, 3rem)`, `var(--ash)`, each line stacked, `line-height: 1.3`
- Middle three lines: Inter 300, `clamp(1rem, 2vw, 1.2rem)`, `var(--slate)`, generous spacing between lines
- "This is the work.": Playfair Display 400 italic, `clamp(1.2rem, 2.5vw, 1.6rem)`, `var(--ash)`
- Each text block gets `data-scroll` for staggered reveal
- Thin gold divider line (`1px, 60px wide, centered`) between copy blocks
- CTA at bottom, centered

### SECTION 3: WEAR YOUR RISE — MERCH

Light background with white product cards. This section should feel aspirational.

```
[Section label: "COLLECTION" — JetBrains Mono, gold]

Wear Your Rise

Physical reminders of the work you're doing.

[Product grid — 3 cards in a row, responsive to 1 column on mobile]

[CTA: "Shop the Collection" — ghost button]
```

**Technical specs:**
- Headline: Playfair Display 900, `clamp(2rem, 5vw, 3.5rem)`, `var(--ash)`
- Subline: Inter 300, `var(--slate)`, max-width 500px, centered
- Product grid: CSS Grid, `grid-template-columns: repeat(auto-fit, minmax(280px, 1fr))`, gap `2rem`
- Product cards:
  - Background: `var(--white)`
  - Aspect ratio: 4:5 for image area (placeholder with light ash background and centered phoenix icon in gold)
  - Padding below image: product name in Inter 500, category in JetBrains Mono
  - Border: none. Subtle `box-shadow: 0 2px 20px rgba(0,0,0,0.06)`
  - Hover: `translateY(-4px)`, shadow deepens to `0 8px 30px rgba(0,0,0,0.1)`
  - Transition: `all 0.3s ease`
- Placeholder product names: "The Rise Tee", "Phoenix Daily Card Set", "The Vault Notebook"
- CTA below grid, centered

### SECTION 4: THE VAULT — FREE TRAINING

**This is the dark section.** Background shifts to charcoal. This should feel like entering a different space — the contrast from the light sections above creates an immersive moment.

```
[Phoenix logo with glow — centered]

The Next Level Being Vault

Where insights become practice.
Contemplate. Investigate. Conversate.

Free. Private. Feather-light.

[CTA: "Enter the Vault" — SOLID ember button, the only filled CTA on the page]
```

**Technical specs:**
- Background: `var(--charcoal)` (#111111)
- Full-width section, generous padding (`12rem 2rem`)
- Phoenix logo: centered, 80–100px, with CSS glow animation
- Headline: Playfair Display 900, `clamp(2rem, 5vw, 3.5rem)`, `var(--bone)`
- Body text: Inter 300, `var(--slate)` (lighter here for contrast), centered
- "Contemplate. Investigate. Conversate." — JetBrains Mono 300, letter-spacing 0.15em, `var(--gold)` — this is a methodology label
- "Free. Private. Feather-light." — Inter 400, `var(--bone)`, slightly larger
- CTA: **This is the only solid/filled button on the entire page.** Background `var(--ember)`, color `var(--bone)`, padding `1.2rem 3rem`. On hover: brighter ember + glow shadow. This is the primary action of the site.
- The particle system should be more visible in this section (increase particle opacity over dark background)
- Subtle top/bottom borders: thin gradient lines transitioning from bone to charcoal

### SECTION 5: COACHING — SELECTIVE

Back to light. Minimal. Almost whispered. This is not the main offer — it's a door for those who need more.

```
[Section label: "FOR HEAVIER SEASONS" — JetBrains Mono, gold]

High-touch support for those
in the heaviest seasons.

Coaching exists for exactly these moments.

[CTA: "Learn More" — text link with arrow, not a button. Links to walk-through-fires.surge.sh]
```

**Technical specs:**
- Background: `var(--bone)`
- Less padding than other sections (`6rem 2rem`) — intentionally smaller/quieter
- Headline: Playfair Display 700, `clamp(1.5rem, 3.5vw, 2.5rem)`, `var(--ash)`
- Body: Inter 300, `var(--slate)`
- CTA: Not a button. Just a text link — Inter 400, `var(--ember)`, with a right arrow (→), hover underline. Links to `https://walk-through-fires.surge.sh`
- This section should feel like a quiet aside, not a sales pitch

---

## FOOTER

Minimal. Grounded. Confident.

```
[Phoenix logo — small, centered]

Conscious Motivation

"Out of the ashes, into your next level of being."

[Links: Instagram · LinkedIn · Contact]

© 2026 Keenan Benjamin. All rights reserved.
```

**Technical specs:**
- Background: `var(--ash)` (dark footer, same as WTF)
- Text: `var(--bone)` and `var(--gold)` for links
- Phoenix logo: small (30–40px), centered
- Brand name: JetBrains Mono, 0.85rem, letter-spacing 0.3em, uppercase
- Tagline: Playfair Display italic, 1rem, `var(--gold)`
- Links: Inter 400, 0.85rem, `var(--gold)`, hover `var(--ember)`
- Contact link: `mailto:keenan@consciousmotivation.com`
- Instagram: `https://www.instagram.com/consciousmotivation` (or placeholder #)
- LinkedIn: `https://www.linkedin.com/in/keenanbenjamin`
- Generous spacing, everything centered

---

## RESPONSIVE DESIGN

Follow WTF's responsive patterns:

- **Desktop** (>768px): Full layouts, larger type, 80 particles
- **Mobile** (≤768px): Single column, reduced type scale, 20 particles, touch-friendly tap targets (min 44px), `{ passive: true }` on scroll listeners for performance
- Product grid collapses to single column on mobile
- Hero headline scales down via `clamp()` — already specified above
- Section padding reduces: `5rem 1.5rem`
- Navigation/header: hamburger not needed (no nav links, just logo)

---

## PERFORMANCE

- Embed the phoenix logo as base64 inline (same approach as WTF)
- Particle canvas: use `requestAnimationFrame`, fewer particles on mobile
- Scroll animations: use IntersectionObserver, not scroll event listeners
- Parallax: desktop only, disabled on mobile with `{ passive: true }` scroll listeners
- No external dependencies. No jQuery. No frameworks. Vanilla JS only.
- Target: single HTML file under 100KB (no embedded photos like WTF — this is a cleaner, lighter page)

---

## WHAT NOT TO BUILD

- No password gate
- No timeline/narrative sections (that's WTF's territory)
- No testimonials
- No pricing
- No about/bio section (coaching page handles that)
- No blog or content sections
- No sticky header navigation (logo only)
- No cookie banners or popups

---

## DEPLOYMENT

Deploy to Surge at `consciousmotivation.surge.sh` (or whatever domain Keenan specifies). Same deployment process as WTF:

```bash
npx surge <folder> <domain>
```

---

## THE VISUAL TEST

When someone lands on this page, they should feel three things in the first 2 seconds:

1. **Calm** — the whitespace and light palette say "you're not being rushed"
2. **Weight** — the typography and precision say "this is serious, not a hobby"
3. **Rise** — the particles, the scroll animations, the vertical rhythm all move upward

If it feels like a wellness blog, it's wrong. If it feels like a corporate site, it's wrong. If it feels like a place where transformation begins in stillness — it's right.
