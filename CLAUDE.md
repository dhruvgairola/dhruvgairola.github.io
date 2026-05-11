# CLAUDE.md

Personal writing site (Jekyll). Before any UI, CSS, or markup change, read [DESIGN.md](./DESIGN.md) and conform to it.

## UI change checklist

For any visual change, verify against DESIGN.md:

1. **One accent.** Only `$blue` / `#4183C4`, and it lives in **text only**, never as a background or border fill on interactive surfaces. No second color.
2. **No cards, shadows, gradients, pills, badges, or tinted containers.** Separation is horizontal rules (`$lightGray`) and whitespace.
3. **No icons in nav or post entries.** Words label themselves.
4. **Type hierarchy through weight and scale**, not decoration. Use the tokens in DESIGN.md's Typography table.
5. **No em dashes** in prose or UI copy. No `--` either. Use commas, colons, semicolons, periods.
6. **Motion:** only a 120ms opacity fade for script-toggled visibility. No scroll/entrance/hover animations.
7. **AI-slop test:** could someone say "AI made that"? If yes (card, pill, shadow, second color, decorative icon, em dash), remove it.

## Tokens

SCSS vars live in `_scss/_variables.scss`. Map to DESIGN.md's semantic tokens:

- `$blue` → accent link
- `$darkerGray` / `$darkGray` / `$gray` → ink 900 / 700 / 500
- `$lightGray` → rules, selection, footer fill
- `$white` → paper (warm-shift to `#fffefb` recommended when next touched)

## Layout

Container is `max-width: 740px`, centered, 10px side padding. Single column. Mobile breakpoint: `max-width: 640px` (see `@mixin mobile`).

## Build

Jekyll site. `style.scss` compiles from the front-matter at its top; edits to `_scss/` partials and `style.scss` flow through `_site/` on build. No separate asset pipeline.

## Content

Posts live in `_posts/` as Markdown. Don't rewrite post copy unless asked. Don't create new documentation files unless asked.
