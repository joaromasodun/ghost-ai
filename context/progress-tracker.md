# Progress Tracker

Update this file after every meaningful implementation
change.

## Current Phase

- In progress

## Current Goal

- Design system (`context/feature-specs/01-design-system.md`) is complete. Pick the next feature spec.

## Completed

- Design system: shadcn/ui installed (Radix base, Nova preset), `lucide-react` installed, `lib/utils.ts` exports `cn()`, and `app/globals.css` wires shadcn's semantic tokens (`--background`, `--primary`, `--border`, etc.) plus the Ghost AI semantic Tailwind tokens (`bg-base`, `text-copy-primary`, `border-surface-border`, `text-brand`, `bg-accent-dim`, etc.) to the dark palette defined in `context/ui-context.md`. Added components: Button, Card, Dialog, Input, Tabs, Textarea, ScrollArea (`components/ui/*`, untouched since generation). `<html>` carries a permanent `dark` class (no light mode).

## In Progress

- None yet.

## Next Up

- Pick the next feature unit from `context/feature-specs/`.

## Open Questions

- None currently.

## Architecture Decisions

- The installed `shadcn` CLI is v4.21.0, a materially different major version from the classic shadcn/ui CLI (component library choice is `-b radix|base|aria`; theming ships via a "preset"; `cn()` comes from the official `cn` npm package rather than a hand-written `clsx` + `tailwind-merge` helper). Used `-b radix` (Radix primitives, matching `architecture.md`'s implicit assumption of standard shadcn/Radix components) and the `nova` preset (Lucide icons + Geist fonts, matching `ui-context.md` and the fonts already wired in `app/layout.tsx`).
- `app/globals.css` had no theme tokens yet (just `@import "tailwindcss";`) despite `01-design-system.md` referring to an "existing dark theme" — the palette was fully specified in `ui-context.md` but not yet implemented in CSS, so it was added as part of this unit rather than raised as an open question.
- Because the product is dark-only, the shadcn `:root` and `.dark` blocks were both set to the same dark values (rather than relying solely on the `.dark` class on `<html>`) so the theme can't regress to shadcn's light defaults if that class is ever dropped.

## Session Notes

- Verified visually: ran `next dev`, rendered all 7 components on a temporary route, screenshotted with Playwright, confirmed dark palette (cyan primary, dark surfaces) with no light-mode flash, no console errors, then deleted the temporary route. `tsc --noEmit` and `next build` both pass.
