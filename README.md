# Executive Presentation Skill

> A Claude skill that transforms raw content into polished, executive-ready HTML presentations — single-file, zero-dependency, print-to-PDF ready.

---

## What It Does

Drop in any raw content — speaking notes, resource lists, research summaries, meeting materials, workshop packs — and this skill outputs a **single self-contained `.html` file** structured as full-viewport slides. Each slide fills exactly one screen. Navigation is by keyboard, scroll, or touch. It prints cleanly to PDF (one slide per page).

The aesthetic is deliberately elevated: layered gradients, distinctive typography pairings, and a locked Suncorp-aligned colour palette. No generic "AI slop." Every output should look like it came from a top-tier consulting or in-house design team.

---

## Trigger Phrases

Claude will invoke this skill when it sees:

- `"make a handout"`
- `"create a resource guide"`
- `"turn this into a presentation"`
- `"executive summary"` / `"leave-behind document"`
- `"summarise this for stakeholders"`
- `"build a slide deck from these notes"`

---

## Key Features

| Feature | Detail |
|---|---|
| **Single file** | One `.html` file — no external CSS, JS, or image dependencies |
| **Full-viewport slides** | Every slide is exactly `100vh` — no scrolling within slides |
| **Keyboard + touch nav** | Arrow keys, Page Up/Down, Space, swipe |
| **Dot navigation** | Synced nav dots with slide-title tooltips |
| **Slide counter** | "3 / 8" style counter, bottom-right |
| **Resource cards** | Books, films, podcasts, articles — each with live search/stream links |
| **Print to PDF** | Each slide prints as one page; resource links hidden in print |
| **Entrance animations** | Cover + staggered section reveals; `prefers-reduced-motion` respected |
| **Locked colour palette** | Suncorp brand: dark-teal, teal, yellow, offwhite, charcoal — no red, ever |
| **Distinctive typography** | Cormorant Garamond + DM Serif + DM Sans (and other approved pairings) |

---

## Colour Palette

```css
--dark-teal:  #1A6B5A;
--teal:       #00857C;
--yellow:     #FFD100;   /* primary accent */
--offwhite:   #F5F5F0;
--charcoal:   #1C2B39;
--mid-grey:   #6B7C8D;
--light-grey: #E8EAE6;
```

> ⚠️ Red is absolutely prohibited — including for Netflix, YouTube, or other red-branded platforms.

---

## Skill File

The full skill definition lives in [`SKILL.md`](./SKILL.md). It contains:

- Content extraction & validation rules
- Design system (palette, typography, card system, spacing)
- Slide architecture (cover → chapter → content → quote → closing)
- Resource card component spec
- Print styles
- Full quality checklist (30+ items)

---

## Example Output

See [`example/executive-presentation-example.html`](./example/executive-presentation-example.html) for a rendered sample deck.

---

## Installation

Place `SKILL.md` in your Claude skills directory:

```
/mnt/skills/user/executive-presentation/SKILL.md
```

Claude will automatically detect and apply this skill when the trigger conditions are met.

---

## Version

**v1.9** — Cover + closing slides now use a dedicated `--font-display` variable (Cormorant Garamond 700) distinct from `--font-serif` used on content slides. Italic second lines in yellow codified as a design pattern for cover/closing titles.

---

*Built for [Anthropic Claude](https://claude.ai) · Maintained by [@Superevh](https://github.com/Superevh)*
