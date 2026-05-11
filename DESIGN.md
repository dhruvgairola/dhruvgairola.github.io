# Design System — dhruvgairola.github.io

Register: **brand**. Personal writing site. Design serves the reading and carries the identity; no marketing ornamentation, no product chrome.

## Principles

1. **Words first, chrome last.** Every decoration earns its place or is removed.
2. **One accent, one family.** A single hyperlink blue and a single sans-serif stack. Hierarchy comes from weight, scale, and whitespace.
3. **Rules and air, not containers.** Horizontal rules and padding separate content. No cards, no shadows, no pills, no badges.
4. **Quiet on every screen.** Motion is limited to link hover. Nothing animates as you scroll.
5. **Category reflex is the enemy.** A "developer blog" is not dark-themed with neon accents. This is a paper-quiet writing surface.

## Color

Strategy: **Restrained**. Tinted neutrals on warm paper, one accent covering under 10% of any view (inline links and "READ MORE" micro-labels).

Neutrals carry a faint cool tint so they never read as clinical `#fff` / `#000`. The paper is faintly warm to soften long-form reading.

| Token | OKLCH | Hex (compiled) | Use |
|---|---|---|---|
| `--paper` | `oklch(0.995 0.002 85)` | `#fffefb` | Page background |
| `--ink-900` | `oklch(0.22 0.006 250)` | `#222222` | Headings, strongest text |
| `--ink-700` | `oklch(0.32 0.005 250)` | `#333333` | Body text |
| `--ink-500` | `oklch(0.52 0.004 250)` | `#666666` | Dates, captions, tags, muted meta |
| `--rule-200` | `oklch(0.93 0.003 250)` | `#eeeeee` | Horizontal rules, selection background, footer fill |
| `--accent-link` | `oklch(0.60 0.13 245)` | `#4183C4` | Inline links, READ MORE, inline refs |

Rules:
- Never introduce a second accent. If something needs emphasis, use weight or scale, not a new colour.
- Never tint backgrounds with the accent. It lives only in text.
- No gradient anywhere — text, background, or border.

## Typography

Single sans stack. Hierarchy through weight (300 / 400 / 700) and a ~1.3 scale step. Body stays humane at 18/1.55.

Stacks:
- **Sans (system)**: `"Helvetica Neue", Helvetica, Arial, sans-serif`
- **Serif (reserved)**: `Georgia, serif` — not currently used; held in reserve for pull quotes if ever needed.

| Role | Size | Weight | Line height | Transform | Notes |
|---|---|---|---|---|---|
| Site name | 28px | 300 | 1.2 | — | Letter-spacing +1px |
| Site tagline | 16px | 400 | 1.2 | — | `ink-500` |
| Nav link | 18px | 300 | 1.0 | — | Letter-spacing +1px; `ink-700` |
| H1 / post title | 30px | 700 | 1.25 | — | `ink-900` |
| H2 | 24px | 700 | 1.3 | — | |
| H3 | 20px | 700 | 1.3 | — | |
| H4 | 18px | 700 | 1.3 | — | `ink-500` (shifts to meta feel) |
| Body | 18px | 400 | 1.55 | — | `ink-700` |
| Date | 18px | 400 *italic* | 1.4 | — | `ink-500` |
| Blockquote | 18px | 400 *italic* | 1.5 | — | `ink-500` |
| Caption | 15px | 400 *italic* | 1.4 | — | `ink-500` |
| Read more | 15px | 400 | 1.0 | UPPERCASE | `accent-link` |

Measure: cap prose lines near **70ch** via the 740px container at 18px body.

Italics are reserved for dates, blockquotes, captions, and the `.hint` utility. Bold is reserved for headings and inline `<strong>`. Nothing else carries italic or bold styling.

## Spacing

Rhythm-based scale — not every power of two is used. These are the values that actually repeat in the site.

| Token | Value | Use |
|---|---|---|
| `--s-1` | 4px | Micro-nudges (avatar gap internals) |
| `--s-2` | 10px | Container side padding |
| `--s-3` | 15px | Paragraph top/bottom, heading → body |
| `--s-4` | 20px | Masthead vertical padding, footer vertical padding |
| `--s-5` | 32px | Between post entries in the list (`2em` at 16px base) |
| `--s-6` | 50px | Masthead → content, content → footer |

Paragraph rhythm is `--s-3` top/bottom. Headings get `1em` top / `--s-3` bottom. Don't introduce fresh one-off margins — if a new value appears three times, promote it to the scale.

## Layout

- **Container**: `max-width: 740px`, centred, `--s-2` side padding. No nested containers.
- **Mobile breakpoint**: `max-width: 640px`. Above, the masthead is horizontal; below, it stacks centered and nav turns accent-blue.
- **Grid**: single column. No sidebars, no split-screen, no sticky elements.
- **Dividers**: 1px `--rule-200` horizontal rules. Used between post entries, under the masthead, and as the footer's top/bottom edges. No vertical rules anywhere.

## Components

### Masthead
70×70 avatar with 5px radius on the left, site info next to it (name + tagline stacked), nav right-aligned at roughly the tagline's vertical centre. 1px `--rule-200` below.
- **Mobile**: avatar centers, info centers, nav collapses to a centered single row and adopts `--accent-link`.

### Post list entry
Plain stacked text on paper — no card, no box, no background shift.
1. Post title (H1 scale, weight 700, linked, inherits `ink-900`)
2. Date (italic, `ink-500`)
3. Excerpt (~3 lines of body)
4. `READ MORE` micro-label in `--accent-link`, uppercase, 15px
5. Bottom 1px `--rule-200`

Entries are separated by `--s-5` padding-bottom and the rule. The **last entry omits the rule**; use `:last-child` to suppress it.

### Prose (post body)
Flows top-down inside the container. Embedded images respect `max-width: 100%` and sit on their own line. A caption follows directly below as `--s-3`-spaced italic `ink-500` text at 15px.

### Blockquote
2px solid `ink-500` left border, `1em` horizontal padding, italic `ink-500` text at body size. This is the **one permitted side-stripe** on the site — it's the established typographic convention for quotations. Never apply this pattern to cards, callouts, or alerts.

### Footer
Full-bleed band filled with `--rule-200`, bracketed by 1px rules top and bottom, `--s-4` vertical padding, centered text in `ink-500`. Separated from content by `--s-6`.

### Inline link
`--accent-link`, no underline by default. Colour holds steady on `:hover` and `:active` — the hover affordance is the cursor, not a colour shift. Inside nav, links use `ink-700` (weight 300) so they read as chrome, not content.

### Hint / utility text
`.hint` — italic `ink-500` at body size. Used sparingly for editorial asides.

## Selection

Text selection: background `--rule-200`, foreground `--ink-900`. Keeps the "quiet paper" feel even when highlighted.

## Motion

- Hover on links: no colour change; cursor only.
- No entrance animations, no scroll reveals, no parallax, no easing curves on layout properties.
- The one acceptable transition is a 120ms opacity fade if a script toggles visibility (e.g. a comments widget loading). Nothing else moves.

## Anti-patterns on this site

Match-and-refuse list, specific to this project:

- **No cards.** Posts, nav items, footer — all text on paper, separated by rules or whitespace.
- **No shadows, no glassmorphism, no gradients.** Anywhere. Including on avatars.
- **No second accent colour.** Blue stays blue; do not introduce green success / red error surfaces unless a form demands them, and then use ink weight to convey state, not colour fills.
- **No icons in nav or on post entries.** Words label themselves.
- **No coloured tag pills.** Tags render as plain text ("Tags: Technology, Ai"), comma-separated, in `ink-500`.
- **No "featured" post card.** The list is uniform; recency is the only hierarchy.
- **No em dashes in prose or UI copy.** Use commas, colons, semicolons, or periods. Also no `--`.
- **No side-stripe borders except on `<blockquote>`.** See the blockquote entry for the only allowed use.
- **No hero-metric templates, no modal-as-first-thought.** Neither applies here; listed so they never creep in.

## AI-slop test

Before shipping any change, ask: could someone look at this and say "AI made that"? Specifically for this site:
- Did we add a card, a pill, or a shadow? Remove it.
- Did we introduce a second colour? Remove it.
- Did we replace typographic hierarchy with a graphic flourish (icon, illustration, divider art)? Remove it.
- Did we use an em dash? Rewrite.

If the change can't survive these questions, the change isn't ready.

## Token map (for `_scss/_variables.scss`)

Current variables map cleanly onto the semantic tokens above. Keep the existing names for backwards compatibility; add semantic aliases only if future work benefits from them.

| Current SCSS var | Semantic token |
|---|---|
| `$blue` `#4183C4` | `--accent-link` |
| `$darkerGray` `#222` | `--ink-900` |
| `$darkGray` `#333` | `--ink-700` |
| `$gray` `#666` | `--ink-500` |
| `$lightGray` `#eee` | `--rule-200` |
| `$white` `#fff` | `--paper` *(warm-shift to `#fffefb` recommended when next touched)* |
| `$black` `#000` | — *(unused in current UI; if reintroduced, use `--ink-900` instead)* |

When the stylesheet is next opened for meaningful work, introduce OKLCH custom properties on `:root` and reference them from the SCSS vars. Until then, the hex column above is the authoritative compiled output.
