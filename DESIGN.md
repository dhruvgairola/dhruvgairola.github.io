# DESIGN.md — dhruvgairola.com

## Register

Brand. Light theme only. Modern Minimal aesthetic, committed warm palette.

## Color — strategy: Restrained

Warm-tinted neutrals (hue 85) + one committed accent (terracotta, hue 32). All OKLCH. Never `#000` or `#fff`.

```css
--paper:        oklch(98.5% 0.006 85);   /* page background */
--paper-sunk:   oklch(96%   0.008 85);   /* code blocks, wells */
--ink:          oklch(18%   0.012 85);   /* body text */
--ink-muted:    oklch(45%   0.010 85);   /* meta, dates */
--ink-faint:    oklch(62%   0.008 85);   /* captions */
--rule:         oklch(91%   0.008 85);   /* 1px borders */
--accent:       oklch(56%   0.15  32);   /* terracotta — links, emphasis */
--accent-hover: oklch(48%   0.16  32);
--accent-weak:  oklch(94%   0.04  32);   /* tag pills bg */
--selection:    oklch(88%   0.10  85);
```

Contrast: `--ink` on `--paper` ≥ 7:1. `--accent` on `--paper` ≥ 4.5:1.

## Typography

Single display/UI family + monospace for code. No serif display faces — unified, modern, understated.

| Role | Family | Notes |
|---|---|---|
| Everything except code | **Inter** (variable) | 400 / 500 / 600 / 700 |
| Code | **JetBrains Mono** | 400 / 500 |

Loaded via Google Fonts with preconnect + `font-display: swap`. Hierarchy comes from weight (400 → 700) and size, not family contrast.

### Scale

```css
--step--1: 0.875rem;   /* 14px — meta, captions */
--step-0:  1.0625rem;  /* 17px — body */
--step-1:  1.25rem;    /* 20px — h4, lead */
--step-2:  1.5rem;     /* 24px — h3 */
--step-3:  1.875rem;   /* 30px — h2 */
--step-4:  2.5rem;     /* 40px — post H1 */
--step-5:  3.5rem;     /* 56px — homepage display */
--step-6:  clamp(3rem, 7vw, 4.5rem);  /* 48–72px — hero */
```

Body: 17px / line-height 1.65 / tracking −0.005em. Headings: line-height 1.15 / tracking −0.02em. Column: 68ch for prose.

## Spacing — 4px base

```css
--space-1: 0.25rem;  /* 4 */
--space-2: 0.5rem;   /* 8 */
--space-3: 0.75rem;  /* 12 */
--space-4: 1rem;     /* 16 */
--space-5: 1.5rem;   /* 24 */
--space-6: 2rem;     /* 32 */
--space-7: 3rem;     /* 48 */
--space-8: 4rem;     /* 64 */
--space-9: 6rem;     /* 96 */
--space-10: 8rem;    /* 128 */
--space-11: 12rem;   /* 192 */
```

## Layout

- Prose column: `min(68ch, 100%)`
- Page frame: `min(100% - 2rem, 1080px)`
- Post grid (named): `[gutter-l] 1fr [body-start] min(68ch, 100%) [body-end] 1fr [gutter-r]` — figures and code can `grid-column: body-start / body-end` (tight) or `grid-column: 1 / -1` (full bleed).
- Breakpoints: `sm 560px`, `md 820px`, `lg 1080px`.

## Motion

```css
--dur-fast: 140ms;
--dur:      220ms;
--dur-slow: 360ms;
--ease-out: cubic-bezier(0.22, 1, 0.36, 1);   /* ease-out-quart */
```

Animate only: opacity, transform, color, background, border-color. Never layout properties. Respect `prefers-reduced-motion: reduce`.

## Components

- **Masthead** — static (not sticky). Wordmark (bold Inter, period in accent) + tagline on left; nav (Inter) right. Single 1px `--rule` below.
- **Post list row** — typographic, no card. Grid: `[date] auto [title] 1fr` (wraps on narrow). Date `--ink-muted`, title `--ink` on `--paper`, hover title `--accent`. Year groups separated by small-cap Inter year labels.
- **Featured post (homepage)** — display-scale bold Inter title + dek + meta. Takes full page-frame width.
- **Post page** — bold Inter H1, meta row (date · reading time · tag pills), prose column. No drop cap.
- **Tag pill** — 12px Inter uppercase-tracked, 2px/10px padding, 999px radius, `--accent-weak` bg, `--accent` text. Used on post meta and post list.
- **Code block** — `--paper-sunk` bg, `--rule` top + bottom only (no side stripes), 16px/20px padding, JetBrains Mono 14px, bleed to wider grid line.
- **Inline code** — JetBrains Mono 0.92em, `--accent-weak` bg, 2px/5px padding, 3px radius.
- **Snippet (collapsible)** — Slack-style. Header bar (`--paper` bg, filename in mono + Show more/less toggle) over a syntax-highlighted body clipped to N lines (default 5) with a fade to `--paper-sunk`. Expands via the toggle or by clicking the faded preview; no-JS falls back to fully expanded. Paste raw files into `_includes/snippets/`, reference with `{% raw %}{% include snippet.html file="x.sh" lang="bash" %}{% endraw %}` (optional `lines`, `title`). See the header comment in `_includes/snippet.html`.
- **Blockquote** — Inter 500-weight at 1.25em on `--paper-sunk` background, padded. No side border, no italic.
- **Newsletter** — single-line: prompt text + email input with underline + submit button. Minimal.
- **Footer** — thin rule, small meta, social icons.

## Craft details

- `text-wrap: balance` on h1/h2
- `text-wrap: pretty` on body paragraphs
- `text-underline-offset: 3px; text-decoration-thickness: 1px` on all links
- Selection uses `--selection`
- `scroll-margin-top` on in-post headings
- Hanging punctuation on display headings where browser supports
- Print stylesheet: no nav, no footer, no newsletter, no dark backgrounds, full-width prose.
- Reduced motion: `transition: none` globally.

## Absolute bans

- No side-stripe borders on cards/callouts/quotes
- No gradient text
- No glassmorphism as default
- No identical card grids
- No hero-metric SaaS templates
- No em dashes in prose; parentheses or commas instead
