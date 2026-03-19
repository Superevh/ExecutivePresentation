---
name: executive-presentation
description: >
  Transform any raw content — speaking notes, resource lists, research summaries, meeting notes,
  workshop materials, or reference collections — into a polished, executive-ready HTML handout.
  Use this skill whenever the user wants to create a professional reference document, reading list,
  resource guide, summary sheet, or leave-behind for stakeholders. Trigger when the user says things
  like "make a handout", "create a resource guide", "turn this into a handout", "executive summary",
  "leave-behind document", "reading list with links", or "summarise this for stakeholders". Also
  trigger whenever content needs to be presented in a scannable, beautifully formatted single-page
  document with chapter sections, citations, buy links, or reference links. If there's any content
  that needs to be packaged for an audience — use this skill.
---

# Executive Presentation Skill

Transform raw user content into a single-file HTML presentation with executive polish.
The output is always a **single self-contained `.html` file** structured as full-viewport slides —
each slide fills exactly one screen, with keyboard/scroll navigation between them.
It also prints cleanly to PDF (one slide per page).

---

## Core Philosophy

1. **Content First** — Extract, validate, and organise the user's actual content. Never invent resources or fabricate links.
2. **Executive Polish** — Every output should look like it came from a top-tier consulting firm or in-house design team.
3. **Zero Dependencies** — Single HTML file. Inline CSS/JS. No external libraries required (Google Fonts via CDN is OK).
4. **Distinctive Design** — No generic "AI slop." Every handout must feel custom-crafted for its context. Avoid Inter, Roboto, Arial, or system-ui. Avoid converging on the same choices across handouts.
5. **Scannable Structure** — Chapter headers, précis paragraphs, and resource cards make it fast to skim and easy to act on.
6. **Live Links** — All books, media, and references include working links. Validate structure before outputting.
7. **Atmosphere Over Flat Colour** — Use layered CSS gradients, geometric patterns, or grain overlays. Depth and texture make the presentation feel crafted, not generated.
8. **One Slide = One Viewport (NON-NEGOTIABLE)** — Every slide must fit exactly within `100vh`. No scrolling within a slide, ever. If content overflows, split it into two slides. Never cram.

---

## ICON DESIGN — MINIMALIST & PALETTE-ALIGNED

**Icons must always be:**
- **Minimalist line-style only** — use simple SVG paths, strokes, or Unicode symbols. No filled-blob icons, emoji, or clipart aesthetics.
- **Palette-matched** — icon colour must come exclusively from the design palette (`--teal`, `--dark-teal`, `--charcoal`, `--yellow`, `--mid-grey`). Never use black, blue, green, or any off-palette colour for icons.
- **Proportionate** — icon size must scale with surrounding text using `clamp()`. Never hard-code icon dimensions in px.
- **Purposeful** — only use icons when they reinforce content meaning. If an icon doesn't add clarity, omit it.

**Recommended icon approach** — inline SVG with `currentColor` so icons inherit palette context:
```html
<svg width="clamp(14px, 1.4vw, 18px)" height="clamp(14px, 1.4vw, 18px)" viewBox="0 0 24 24"
     fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"
     aria-hidden="true">
  <!-- path here -->
</svg>
```

Never use icon fonts (FontAwesome, Material Icons) or emoji as icons.

---

## ABSOLUTE COLOUR CONSTRAINT — NO RED

**Never use red or any red-adjacent colour** in any output from this skill. This applies to:
- Link button backgrounds (Netflix, YouTube, and other red-branded platforms must use palette alternatives)
- Badge backgrounds, borders, hover states
- Any decorative element, rule, or accent

**Safe replacements for streaming/media platforms:**
- Netflix → `--charcoal` background with `--yellow` text
- YouTube → `--dark-teal` background with `--offwhite` text
- Any other red-defaulting brand → remap to the palette, never use brand red

---

## Step 1: Content Extraction & Validation

When the user provides content, extract these elements:

### A. Structural Elements
- **Title** — infer from the content theme if not explicitly stated
- **Subtitle / event context** — session name, date, audience if mentioned
- **Chapter/section headers** — key themes or topic clusters
- **Key points** — the core ideas under each chapter (2–5 per section)

### B. Resources (extract ALL of these)
For each resource mentioned, capture:

| Field | Notes |
|-------|-------|
| `title` | Full title of the book/film/article/tool |
| `author` / `creator` | Author, director, or source |
| `type` | book / film / article / podcast / tool / website / quote |
| `précis` | 1–2 sentence summary of the key idea from the content |
| `links` | See link generation rules below |

### C. Link Generation Rules

**Books:**
- Amazon AU buy link: `https://www.amazon.com.au/s?k={title+author}`
- Booktopia: `https://www.booktopia.com.au/search.ep?q={title}`
- Audible AU: `https://www.audible.com.au/search?keywords={title}`

**Films / Documentaries / Series:**
- Netflix: `https://www.netflix.com/search?q={title}` — charcoal/yellow button (NOT red)
- Prime Video: `https://www.amazon.com.au/s?k={title}&i=instant-video`
- YouTube: `https://www.youtube.com/results?search_query={title}` — dark-teal/offwhite button (NOT red)

**Podcasts:**
- Spotify: `https://open.spotify.com/search/{title}`
- Apple Podcasts: `https://podcasts.apple.com/search?term={title}`

**Articles / Websites:**
- Use the direct URL if provided
- Otherwise: `https://www.google.com/search?q={title+author}`

**Note**: Always URL-encode spaces as `+`. These are search links, not direct product pages — do not fabricate direct product URLs.

---

## Step 2: Design System

### Colour Palette (LOCKED — preserve exactly)
```css
--dark-teal:  #1A6B5A;
--teal:       #00857C;
--yellow:     #FFD100;   /* PRIMARY brand accent — use on card tops, rules, highlights */
--offwhite:   #F5F5F0;
--charcoal:   #1C2B39;
--mid-grey:   #6B7C8D;
--light-grey: #E8EAE6;
--white:      #ffffff;
/* NEVER use red, crimson, rose, coral, or any red-family colour */
```

The palette above is the design anchor. Do not introduce new hues. All design expression — atmosphere, texture, depth — must come from **composition, layering, and typography**, not from new colours.

**`--yellow` is the primary accent colour** — it must appear on every card as the top border line (see Card System below), and on all key structural punctuation (rules, highlights, active nav dots on dark slides). It should feel like a signature, not decoration.

### Header & Footer Proximity — REQUIRED

**Headers and footers must always feel connected to the content, not isolated at the page edges.**

- Slide headers (section labels, chapter numbers) must have **tighter spacing below** (toward content) than above. Use `margin-bottom: clamp(8px, 1.2vh, 16px)` and `margin-top: clamp(0px, 2vh, 24px)`.
- Slide footers (event name, date, page context) must sit **close to the last content element**, not pinned to the absolute bottom edge. Use `margin-top: clamp(12px, 1.8vh, 24px)` rather than `position: absolute; bottom: 0`.
- Exception: the slide counter and dot nav are fixed UI chrome — they may remain at the edge. All *content* headers/footers must breathe toward the content, not away from it.

```css
/* Header proximity pattern */
.slide-header {
  margin-top: clamp(0px, 2vh, 20px);
  margin-bottom: clamp(8px, 1.5vh, 18px);  /* tighter — pulls toward content */
}

/* Footer proximity pattern */
.slide-footer {
  margin-top: clamp(12px, 1.8vh, 24px);    /* close to content above */
  padding-bottom: clamp(8px, 1.2vh, 16px); /* small buffer from edge */
}
```

### Typography — Professional & Distinctive

Choose fonts that are **refined, professional, and distinctive**. Never default to Inter, Roboto, Arial, Space Grotesk, or system-ui. **Equally important: avoid overtly artistic, display, or decorative fonts** — outputs are executive documents, not editorial magazines. The target aesthetic is a top-tier consulting firm or premium in-house design team.

**Primary pairing — clean humanist sans (use as default):**
- `'Nunito Sans'` (300/400/600/700, body + UI) + `'Lora'` (600/700, headings) + `'DM Mono'` (labels) — **preferred default** *(clean, approachable, corporate — humanist sans body with authoritative serif headings)*

**Approved professional pairings (rotate — don't always pick the same):**
- `'Nunito Sans'` + `'Lora'` + `'DM Mono'` — clean corporate *(financial services, insurance)*  ← **default**
- `'Source Sans 3'` + `'Lora'` + `'DM Mono'` — open humanist *(professional services, consulting)*
- `'Lato'` + `'DM Serif Display'` + `'DM Mono'` — warm professional *(enterprise, HR, training)*
- `'Libre Caslon Text'` + `'Nunito Sans'` + `'DM Mono'` — scholarly authority *(research, policy)*
- `'Spectral'` + `'Source Sans 3'` + `'DM Mono'` — technical editorial *(data, engineering)*

**Two-font-family rule for cover + closing slides:**
Use a `--font-display` variable distinct from `--font-serif` for the cover title and closing title only. This creates intentional typographic contrast between the high-impact bookend slides and the functional content slides.

```css
--font-display: 'Lora', Georgia, serif;           /* cover + closing titles only */
--font-serif:   'Lora', Georgia, serif;            /* chapter titles, headings */
--font-sans:    'Nunito Sans', sans-serif;         /* all body text */
--font-mono:    'DM Mono', monospace;              /* labels, badges, counters */
```

```css
/* Cover + closing: use display font at heavy weight with tight tracking */
.cover-title, .closing-title {
  font-family: var(--font-display);
  font-weight: 700;
  letter-spacing: -0.01em;
}
/* Italic second lines in yellow for visual rhythm */
.cover-title em, .closing-title em { font-style: italic; color: var(--yellow); }
```

**Ampersand rule — avoid the `&` glyph in visible text.** Ornate serif fonts render `&` decoratively, which looks out of place in executive documents. Use `+` for author name pairings (e.g. "Agrawal, Gans + Goldfarb") and "and" for prose. In HTML, use `+` or "and" rather than `&amp;` in displayed content.

**Weight rules — NON-NEGOTIABLE:**
- Serif display fonts must be loaded at weight **600 or 700 minimum** — never 400 or italic-only. Thin serifs read as weak on slides.
- Sans-serif body must be **400 as the floor** — never 300 or light. Nunito Sans 400 is the correct baseline.
- Always explicitly specify weights in the Google Fonts URL: `family=Nunito+Sans:opsz,wght@6..12,400;6..12,600;6..12,700`

**Fonts to avoid entirely** — too decorative, too thin, or too casual for executive contexts:
- `Fraunces`, `Syne`, `Josefin Sans`, `Abril Fatface`, `Bebas Neue`, `Raleway`, `Satisfy`, `Lobster`, `Playfair Display` (ornate ampersand), `Cormorant Garamond` (too editorial/literary for corporate contexts)
- Any font loaded at weight 300 or below for body text

Choose the pairing that fits the *content's tone and audience*. When in doubt, use `'Lora'` + `'Nunito Sans'`.

All font sizes and spacing **must use `clamp(min, preferred, max)`** — never hard-coded px/rem values. This is non-negotiable.

```css
/* Minimum clamp() sizing rules — v2.1 scale (increased ~15%) */
.cover-title    { font-size: clamp(48px, 7vw, 80px); }
.cover-subtitle { font-size: clamp(22px, 2.4vw, 28px); font-weight: 400; }   /* never weight 300 */
.cover-eyebrow  { font-size: clamp(14px, 1.5vw, 17px); }
.chapter-title  { font-size: clamp(40px, 5vw, 58px); }
.chapter-precis { font-size: clamp(20px, 2.1vw, 25px); }
.chapter-number { font-size: clamp(14px, 1.5vw, 17px); }
.body-text      { font-size: clamp(19px, 1.95vw, 23px); }
.resource-title { font-size: clamp(21px, 2.4vw, 27px); }
.resource-author{ font-size: clamp(16px, 1.7vw, 20px); }
.resource-precis{ font-size: clamp(16px, 1.7vw, 20px); }
.badge-text     { font-size: clamp(12px, 1.3vw, 14px); }
.link-btn       { font-size: clamp(12px, 1.3vw, 14px); padding: 5px 12px; }
.stat-value     { font-size: clamp(52px, 6.5vw, 74px); }
.stat-label     { font-size: clamp(18px, 1.85vw, 22px); }
.slide-footer   { font-size: clamp(12px, 1.3vw, 14px); }
.section-label  { font-size: clamp(12px, 1.3vw, 14px); }
.pull-quote     { font-size: clamp(36px, 4.8vw, 60px); }
.closing-title  { font-size: clamp(50px, 7vw, 80px); }
.closing-detail { font-size: clamp(20px, 2.1vw, 25px); }
```

**Never negate CSS functions directly.** Use `calc(-1 * clamp(...))` not `-clamp(...)`.

### Layout Principles
- Each slide: `height: 100vh; height: 100dvh; overflow: hidden;` — enforced without exception
- Max content width: `860px`, centred with padding
- **Full-width layout required** — content must span the full usable width of each slide. Never allow content to occupy only the left half or left column unless it is paired with a right-column element (image, chart, pull stat). Asymmetric layouts must be intentional and balanced.
- **Proportionate density** — the amount of content on a slide must feel balanced against the slide's available area. Slides with little content should use larger font, more generous spacing, or a visual accent to fill the space with intention — never feel empty. Slides with much content must split rather than shrink.
- Chapter sections: left border accent in `--teal`
- Resource cards: subtle background + border, link buttons at bottom
- **Card grids: use the full slide width** — 2-column or 3-column grids depending on card count. Single-column card layout is only acceptable on mobile or when cards have long précis text.
- All slides share the same navigation JS (arrow keys, PageUp/Down, Space, swipe)
- Print-safe: `@media print` renders each slide as a separate page

**Spacing system** — all padding and gap values must use `clamp()`:
```css
.slide          { padding: clamp(32px, 5vh, 72px) clamp(40px, 7vw, 100px); }
.card-grid      { gap: clamp(12px, 2vw, 24px); }
.section-gap    { margin-bottom: clamp(16px, 2.5vh, 36px); }
```

---

### Card System — Yellow Top Accent (NON-NEGOTIABLE)

**Every card — resource cards, key-point cards, stat cards — must have a yellow top accent line.** This is the design signature that gives the layout its professional consulting finish.

```css
/* Apply to ALL card types */
.resource-card,
.key-point,
.stat-card {
  border: 1px solid var(--light-grey);
  border-top: 3px solid var(--yellow);   /* Yellow accent — required on every card */
  border-radius: 8px;
}

/* On dark-background slides, adjust side/bottom border only — keep yellow top */
.slide-dark .stat-card {
  border-color: rgba(255,255,255,0.1);
  border-top-color: var(--yellow);       /* yellow top always survives dark treatment */
}
```

The yellow line must appear regardless of slide background colour. On light slides it reads against white cards. On dark slides it reads against semi-transparent cards. Never remove or replace it.

---

### Slide Colour Variation — Required for Consecutive Content Slides

Adjacent content slides must not share the same background. Apply this rotation to avoid visual monotony:

| Slide position | Background treatment | Card style |
|---------------|---------------------|------------|
| Light (default) | `linear-gradient(160deg, var(--offwhite), #eaeeeb)` | White cards, `--light-grey` border |
| Teal-tint | `linear-gradient(150deg, #e8f2f0, #d4eae7, #c2ddd9)` | Frosted white cards (`rgba(255,255,255,0.72)`) |
| Dark charcoal | `linear-gradient(140deg, var(--charcoal), #1e3446)` | Semi-transparent cards (`rgba(255,255,255,0.06)`), yellow stat values |

**Rule:** No two adjacent content slides should share the same treatment. Cover/quote/closing slides do not count toward this rotation.

```css
/* Teal-tint slide */
.slide-teal-tint {
  background: linear-gradient(150deg, #e8f2f0 0%, #d4eae7 60%, #c2ddd9 100%);
}
.slide-teal-tint .key-point {
  background: rgba(255,255,255,0.72);
  border-color: rgba(0,133,124,0.15);
  border-top-color: var(--yellow);
  backdrop-filter: blur(4px);
}

/* Dark charcoal slide */
.slide-dark {
  background: linear-gradient(140deg, var(--charcoal) 0%, #1e3446 100%);
  color: var(--offwhite);
}
.slide-dark .chapter-number { color: var(--yellow); }
.slide-dark .chapter-title  { color: var(--offwhite); }
.slide-dark .chapter-precis { color: rgba(245,245,240,0.6); }
.slide-dark .stat-value     { color: var(--yellow); }
.slide-dark .stat-label     { color: rgba(245,245,240,0.6); }
```

---

## SLIDE ENGINE — Required in Every Output

### CSS: Slide Container (NON-NEGOTIABLE)

```css
/* === SLIDE ENGINE === */
html, body {
  margin: 0; padding: 0;
  height: 100%; overflow: hidden;
}

.slide-container {
  height: 100vh;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
  scroll-behavior: smooth;
  -webkit-overflow-scrolling: touch;
}

.slide {
  height: 100vh;
  height: 100dvh;          /* dynamic viewport for mobile */
  overflow: hidden;         /* NEVER allow slide-level scroll */
  scroll-snap-align: start;
  scroll-snap-stop: always;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  box-sizing: border-box;
  padding: clamp(24px, 5vh, 64px) clamp(20px, 6vw, 80px);
}
```

### JS: Keyboard + Navigation (include verbatim)

```javascript
// === SLIDE NAVIGATION ===
// FIX: Use container.scrollTo(offsetTop) instead of scrollIntoView().
// scrollIntoView() conflicts with scroll-snap-type:mandatory and breaks
// navigation from slide 2 onward. scrollTo against the container is reliable.
const container = document.querySelector('.slide-container');
const slides = Array.from(document.querySelectorAll('.slide'));
let current = 0;
let isNavigating = false;

function goTo(index) {
  index = Math.max(0, Math.min(slides.length - 1, index));
  if (index === current && !isNavigating) return;
  current = index;
  isNavigating = true;
  container.scrollTo({ top: slides[index].offsetTop, behavior: 'smooth' });
  updateDots();
  updateCounter();
  setTimeout(() => { isNavigating = false; }, 600);
}

document.addEventListener('keydown', e => {
  if (e.key === 'ArrowDown' || e.key === 'ArrowRight' || e.key === 'PageDown' || e.key === ' ') {
    e.preventDefault(); goTo(current + 1);
  }
  if (e.key === 'ArrowUp' || e.key === 'ArrowLeft' || e.key === 'PageUp') {
    e.preventDefault(); goTo(current - 1);
  }
});

// Touch swipe support
let touchStartY = 0;
container.addEventListener('touchstart', e => { touchStartY = e.touches[0].clientY; }, { passive: true });
container.addEventListener('touchend', e => {
  const delta = touchStartY - e.changedTouches[0].clientY;
  if (Math.abs(delta) > 50) goTo(current + (delta > 0 ? 1 : -1));
});

// Scroll-snap sync — use scrollend (or fallback scroll) to keep index accurate
// after native scroll-snap settles, without fighting the snap engine mid-scroll.
function syncCurrent() {
  if (isNavigating) return;
  const mid = container.scrollTop + container.clientHeight / 2;
  const idx = slides.findIndex(s => s.offsetTop + s.offsetHeight > mid);
  if (idx !== -1 && idx !== current) { current = idx; updateDots(); updateCounter(); }
}
container.addEventListener('scrollend', syncCurrent);              // modern browsers
container.addEventListener('scroll', () => {                       // fallback
  clearTimeout(container._scrollTimer);
  container._scrollTimer = setTimeout(syncCurrent, 120);
}, { passive: true });

// Dot navigation: generate dots if not already in markup
const nav = document.querySelector('.slide-nav');
if (nav && nav.children.length === 0) {
  slides.forEach((s, i) => {
    const dot = document.createElement('button');
    dot.className = 'nav-dot' + (i === 0 ? ' active' : '');
    dot.setAttribute('aria-label', `Slide ${i + 1}`);
    dot.addEventListener('click', () => goTo(i));
    nav.appendChild(dot);
  });
}

function updateDots() {
  document.querySelectorAll('.nav-dot').forEach((d, i) => d.classList.toggle('active', i === current));
}
function updateCounter() {
  const el = document.getElementById('slide-counter');
  if (el) el.textContent = `${current + 1} / ${slides.length}`;
}

// Initialise
updateDots();
updateCounter();
```

### Navigation UI (include in every output)

```html
<!-- Dot navigation -->
<nav class="slide-nav" aria-label="Slide navigation">
  <!-- One .nav-dot per slide, generated by JS or hand-written -->
</nav>

<!-- Slide counter -->
<div id="slide-counter" style="
  position: fixed; bottom: 20px; right: 24px;
  font-size: clamp(10px, 1.2vw, 13px);
  color: var(--mid-grey); font-family: var(--font-mono, monospace);
  pointer-events: none; z-index: 100;
"></div>
```

```css
.slide-nav {
  position: fixed;
  right: 20px; top: 50%;
  transform: translateY(-50%);
  display: flex; flex-direction: column; gap: 8px;
  z-index: 100;
}
.nav-dot {
  width: 8px; height: 8px; border-radius: 50%;
  background: var(--mid-grey); opacity: 0.4;
  cursor: pointer; transition: opacity 0.2s, background 0.2s;
  border: none;
}
.nav-dot.active { background: var(--teal); opacity: 1; }
```

### Content Density Limits Per Slide

| Slide Type | Maximum Content |
|------------|----------------|
| Cover / Title | 1 heading + 1 subtitle + optional tagline |
| Chapter intro | 1 heading + 2–3 sentences + optional chapter number |
| Content slide | 1 heading + 4–6 bullet points OR 1 heading + 2 short paragraphs |
| Resource cards | 1 heading + 3–4 cards maximum (2×2 grid or single column) |
| Quote slide | 1 quote (max 3 lines) + attribution |
| Closing slide | 1 heading + contact/date/CTA |

**Content exceeds limits? Split into multiple slides. Never cram. Never scroll.**



---

## Step 3: Design Aesthetics — Atmosphere & Craft

This is where "AI slop" is avoided. Generic handouts have flat white backgrounds, timid colour use, and predictable layouts. **Do the opposite.**

### Backgrounds: Create Depth
Never use a flat background colour. Layer gradients, patterns, or texture:

```css
/* Option A: Subtle grain + gradient */
body {
  background:
    url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.03'/%3E%3C/svg%3E"),
    linear-gradient(160deg, var(--offwhite) 0%, #eef2ef 100%);
}

/* Option B: Geometric teal halo behind cover */
.cover::before {
  content: '';
  position: absolute;
  top: -60px; right: -60px;
  width: 300px; height: 300px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(0,133,124,0.10) 0%, transparent 70%);
  pointer-events: none;
}

/* Option C: Dark charcoal cover with teal diagonal */
.cover {
  background: linear-gradient(135deg, var(--charcoal) 60%, var(--dark-teal) 100%);
}
```

Choose the approach that fits the content's tone. Technical topics suit geometric precision; leadership/culture topics suit organic gradients.

### Colour Commitment: Dominant + Accent
Don't spread the palette evenly. Commit:
- **Light mode default**: `--offwhite` / `--light-grey` dominant → `--teal` structural accent → `--yellow` punctuation
- **Bold/dark contexts**: `--charcoal` dominant → `--teal` mid-tone → `--yellow` highlight
- The **cover block** should use the palette's most saturated expression; fade to lighter treatment through the body

### Motion: One Orchestrated Entrance
One well-orchestrated page load beats scattered micro-interactions everywhere.

```css
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(24px); }
  to   { opacity: 1; transform: translateY(0); }
}
@keyframes scaleX {
  from { transform: scaleX(0); transform-origin: left; }
  to   { transform: scaleX(1); transform-origin: left; }
}

/* Cover: staggered entrance */
.cover-title    { animation: fadeUp 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.1s both; }
.cover-subtitle { animation: fadeUp 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.2s both; }
.cover-rule     { animation: scaleX 0.5s ease 0.35s both; }

/* Sections: staggered reveals */
.chapter-section:nth-child(1) { animation: fadeUp 0.5s ease 0.0s both; }
.chapter-section:nth-child(2) { animation: fadeUp 0.5s ease 0.07s both; }
.chapter-section:nth-child(3) { animation: fadeUp 0.5s ease 0.14s both; }
.chapter-section:nth-child(4) { animation: fadeUp 0.5s ease 0.21s both; }
.chapter-section:nth-child(5) { animation: fadeUp 0.5s ease 0.28s both; }

/* ALWAYS include */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation: none !important;
    transition: none !important;
  }
}
```

### Quote & Closing Slides — Larger Type

Quote and closing slides carry fewer words and more visual weight. Use larger font sizes to fill the space with intention:

```css
/* Quote slide — go bigger than body slides */
.pull-quote {
  font-size: clamp(26px, 3.5vw, 42px);
  font-weight: 600;
  line-height: 1.32;
}
.quote-attribution {
  font-size: clamp(12px, 1.2vw, 15px);
}
.quote-mark {
  font-size: clamp(72px, 10vw, 120px);
}

/* Closing slide — largest type in the deck */
.closing-title {
  font-size: clamp(34px, 5vw, 58px);
  font-weight: 700;
  line-height: 1.1;
}
.closing-detail {
  font-size: clamp(14px, 1.5vw, 17px);
}
.cta-btn {
  font-size: clamp(11px, 1.1vw, 14px);
  padding: 12px 24px;
}
```

**Hover is a primary information layer.** Where content is dense or abbreviated on-screen, hover states must reveal additional detail — author bio context, resource précis (if truncated), link destination, chapter description, or metadata.

**Card hover — lift + tooltip:**
```css
.resource-card {
  position: relative;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}
.resource-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(28, 43, 57, 0.12);
}

/* Hover detail tooltip — shown on cards with truncated précis */
.resource-card[data-detail]:hover::after {
  content: attr(data-detail);
  position: absolute;
  bottom: calc(100% + 8px);
  left: 0; right: 0;
  background: var(--charcoal);
  color: var(--offwhite);
  font-size: clamp(11px, 1.1vw, 13px);
  line-height: 1.5;
  padding: clamp(8px, 1vh, 12px) clamp(10px, 1.2vw, 14px);
  border-radius: 6px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.2);
  z-index: 200;
  pointer-events: none;
  white-space: normal;
}
```

**Hover rules — apply across all interactive elements:**

| Element | Hover behaviour |
|---------|----------------|
| Resource card | Lift + shadow; `data-detail` tooltip with full précis if body is truncated |
| Link button | Lighten/darken 10%; show destination domain in `title` attribute |
| Chapter heading | Subtle underline in `--teal`; `title` attribute with chapter summary |
| Badge | `title` attribute explains resource type |
| Nav dot | `title` attribute with slide title |
| Key point bullet | `title` attribute with expanded context if available |

**Implementation pattern** — add `data-detail` and `title` attributes directly in HTML:
```html
<div class="resource-card" data-detail="Full précis text here for hover reveal">
  <div class="resource-badge badge-book" title="Book">BOOK</div>
  <div class="resource-title">Title</div>
  ...
</div>

<a href="..." class="link-btn link-amazon" title="Search Amazon Australia" target="_blank">Amazon AU</a>
```

---

## Step 4: HTML Architecture

Every output follows this shell. Content is divided into `.slide` blocks.

```html
<body>
  <div class="slide-container">

    <!-- Slide 1: Cover -->
    <section class="slide slide-cover">
      <div class="cover-content">
        <div class="cover-title">Presentation Title</div>
        <div class="cover-subtitle">Subtitle · Event · Date</div>
        <div class="cover-rule"></div>
      </div>
      <button class="print-btn" onclick="window.print()">Save PDF</button>
    </section>

    <!-- Slide 2: Chapter intro (one per theme) -->
    <section class="slide slide-chapter">
      <div class="chapter-number">01</div>
      <h2 class="chapter-title">Chapter Title</h2>
      <p class="chapter-precis">2–3 sentence overview of this section.</p>
      <ul class="key-points">
        <li>Point one</li>
        <li>Point two</li>
      </ul>
    </section>

    <!-- Slide 3+: Resource cards (max 3–4 per slide) -->
    <section class="slide slide-resources">
      <h3 class="section-label">Resources — Chapter Name</h3>
      <div class="card-grid">
        <!-- resource-card components here -->
      </div>
    </section>

    <!-- Optional: Pull quote slide -->
    <section class="slide slide-quote">
      <blockquote class="pull-quote">"Quote text here."</blockquote>
      <cite class="quote-attribution">— Author, Source</cite>
    </section>

    <!-- Closing slide -->
    <section class="slide slide-closing">
      <div class="closing-title">Thank you</div>
      <div class="closing-detail">Prepared for · Event · Date</div>
    </section>

  </div><!-- /.slide-container -->

  <!-- Navigation UI -->
  <nav class="slide-nav"></nav>
  <div id="slide-counter"></div>

  <script>/* slide navigation JS here */</script>
</body>
```


---

## Step 5: Resource Card Component

```html
<div class="resource-card">
  <div class="resource-badge badge-book">BOOK</div>
  <div class="resource-title">Title of Work</div>
  <div class="resource-author">Author Name</div>
  <div class="resource-precis">Précis in 1–2 sentences.</div>
  <div class="resource-links">
    <a href="..." class="link-btn link-amazon" target="_blank">Amazon AU</a>
    <a href="..." class="link-btn link-booktopia" target="_blank">Booktopia</a>
    <a href="..." class="link-btn link-audible" target="_blank">Audible</a>
  </div>
</div>
```

**Badge colours (all palette-safe, no red):**
- `badge-book`    → `--dark-teal` bg, white text
- `badge-film`    → `--charcoal` bg, `--yellow` text
- `badge-article` → `--teal` bg, white text
- `badge-podcast` → `--yellow` bg, `--charcoal` text
- `badge-tool`    → `--mid-grey` bg, white text
- `badge-quote`   → transparent bg, `--teal` border + text

**Link button CSS (NO red anywhere):**
```css
.link-amazon    { background: var(--charcoal); color: #fff; }
.link-booktopia { background: var(--teal); color: #fff; }
.link-audible   { background: var(--yellow); color: var(--charcoal); }
.link-netflix   { background: var(--charcoal); color: var(--yellow); }   /* NOT brand red */
.link-prime     { background: var(--teal); color: #fff; }                /* NOT brand blue */
.link-youtube   { background: var(--dark-teal); color: var(--offwhite); } /* NOT brand red */
.link-spotify   { background: var(--dark-teal); color: #fff; }
.link-apple     { background: var(--mid-grey); color: #fff; }
.link-web       { background: var(--light-grey); color: var(--charcoal); border: 1px solid var(--mid-grey); }
```

---

## Step 6: Print Styles

Each slide prints as one page.

```css
@media print {
  .slide-nav, #slide-counter, .print-btn { display: none !important; }
  html, body { overflow: visible; height: auto; }
  .slide-container { overflow: visible; scroll-snap-type: none; }
  .slide {
    height: 100vh;
    page-break-after: always;
    break-after: page;
    overflow: hidden;
  }
  .resource-links { display: none; }
  .slide-cover, .slide-chapter, .slide-quote {
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }
}
```

---

## Step 7: Interaction Clarification

**If content is provided directly:**
→ Extract, validate, structure, and build immediately.

**If content is vague:**
Ask ONE focused question to unblock.

**Colour palette:**
- User specifies → use exactly (still enforce no-red constraint)
- User says nothing → use default palette

**Tone:**
- Default: professional, warm, executive-grade
- Adjust if specified (academic / casual / editorial)

---

## Step 8: Output & Delivery

1. Build the `.html` file in `/home/claude/`
2. Validate: well-formed links, no red colours, no broken structure, clamp() on all font sizes
3. Copy to `/mnt/user-data/outputs/executive-presentation.html`
4. Use `present_files` to share
5. Briefly summarise: chapters, resources, link types

---

## Quality Checklist

- [ ] All resources from source content captured
- [ ] No fabricated titles, authors, or direct product URLs
- [ ] Every resource has at least 2 working search/stream links
- [ ] **Zero red or red-adjacent colours anywhere** (including Netflix/YouTube buttons)
- [ ] Typography is professional and distinctive — not Inter, Roboto, Arial, Space Grotesk, Cormorant Garamond, or any decorative/artistic font
- [ ] **All font sizes use `clamp()`** — no hard-coded px/rem
- [ ] Colour palette uses CSS variables throughout; no new hues introduced
- [ ] Background has depth — gradient, texture, or layered pattern (not flat colour)
- [ ] Cover slide uses the palette's most saturated expression
- [ ] **Every `.slide` has `height: 100vh; overflow: hidden;`** — no exceptions
- [ ] **No slide overflows** — content-heavy sections split across multiple slides
- [ ] **Every card has `border-top: 3px solid var(--yellow)`** — resource cards, key-point cards, stat cards, no exceptions
- [ ] **No two adjacent content slides share the same background** — rotate light / teal-tint / dark
- [ ] **Serif fonts loaded at weight 600+ minimum** — never thin/400 for display text
- [ ] **Body sans-serif at weight 400 minimum** — never 300 or light
- [ ] **Quote slide pull-quote** ≥ `clamp(26px, 3.5vw, 42px)` — sized to fill the slide with presence
- [ ] **Closing title** ≥ `clamp(34px, 5vw, 58px)` — the largest type in the deck
- [ ] **Layout uses full slide width** — no content stranded in left column without a paired right element
- [ ] **Header/footer spacing pulls toward content** — not isolated at page edges; `margin-bottom` tighter than `margin-top` on headers
- [ ] **Hover reveals detail** — all cards have `data-detail` tooltip; all link buttons have `title` attribute; nav dots have slide title
- [ ] Keyboard navigation works: ArrowDown/Up, PageDown/Up, Space
- [ ] Dot navigation rendered and synced to current slide
- [ ] Touch swipe navigation present
- [ ] Slide counter (e.g. "3 / 8") visible bottom-right
- [ ] Cover entrance animation + staggered section reveals
- [ ] `prefers-reduced-motion` support included
- [ ] `@media print` renders each slide as one page
- [ ] Save PDF button present on cover slide
- [ ] Single self-contained `.html` file
- [ ] Saved to `/mnt/user-data/outputs/`

---

## Reference: Resource Types & Link Sets

| Type | Primary Links | Secondary | Button Style |
|------|--------------|-----------|-------------|
| Book | Amazon AU, Booktopia | Audible | charcoal / teal / yellow |
| Film/Doco | Netflix (charcoal/yellow), Prime | YouTube (dark-teal) — NO red |
| Podcast | Spotify (dark-teal), Apple | — | dark-teal / mid-grey |
| Article | Direct URL or Google search | — | light-grey |
| Tool/App | Official website | Product Hunt | teal / mid-grey |
| Quote | Google search for source | — | light-grey |
| Academic Paper | Google Scholar | ResearchGate | mid-grey |

---

*Skill version 2.1 — Default typography updated to humanist sans-serif pairing: Lora (headings) + Nunito Sans (body). Brand-specific references removed; palette and design system retained as generic corporate standard. Cover + closing slides use a dedicated `--font-display` variable (Lora 700) distinct from `--font-serif` used on content slides. Font scale increased ~15% across all type roles for improved legibility at presentation scale.*
