# Progress Tracker

Update this file after every meaningful implementation
change.

## Current Phase

- In progress

## Current Goal

- Editor base chrome (`context/feature-specs/02-editor.md`) is complete. Pick the next feature spec.

## Completed

- Design system: shadcn/ui installed (Radix base, Nova preset), `lucide-react` installed, `lib/utils.ts` exports `cn()`, and `app/globals.css` wires shadcn's semantic tokens (`--background`, `--primary`, `--border`, etc.) plus the Ghost AI semantic Tailwind tokens (`bg-base`, `text-copy-primary`, `border-surface-border`, `text-brand`, `bg-accent-dim`, etc.) to the dark palette defined in `context/ui-context.md`. Added components: Button, Card, Dialog, Input, Tabs, Textarea, ScrollArea (`components/ui/*`, untouched since generation). `<html>` carries a permanent `dark` class (no light mode).
- Editor base chrome: `components/editor/editor-navbar.tsx` — fixed-height (`h-14`) top navbar with left/center/right sections, `bg-surface` + `border-b border-surface-border`. Left section holds a sidebar-toggle `Button` (`variant="ghost"`) that swaps `PanelLeftOpen`/`PanelLeftClose` based on the `isSidebarOpen` prop; state is lifted (`isSidebarOpen`, `onToggleSidebar` props), not owned internally. Center and right sections are empty placeholders reserved for later chapters.
- Editor base chrome: `components/editor/project-sidebar.tsx` — `isOpen`/`onClose` props; `absolute inset-y-0 left-0` floating panel (not in normal flow, so it never pushes page content) that slides via `translate-x-full` ↔ `translate-x-0` on `isOpen`. Expects a `relative` positioned parent (a future editor workspace shell) below the navbar so it floats over the canvas region rather than the whole viewport. Header ("Projects" + close button), shadcn `Tabs` (My Projects / Shared, both with an empty placeholder message inside a `ScrollArea`), and a full-width `New Project` button with a `Plus` icon pinned to the bottom.
- Dialog pattern: no new component needed — `components/ui/dialog.tsx` (protected foundation component from the design-system unit) already supports `DialogTitle`, `DialogDescription`, and `DialogFooter` (footer actions), and already styles via the token-backed `--popover`/`--popover-foreground`/`--muted` CSS variables in `globals.css`. Verified it satisfies the spec as-is; left untouched per the protected-foundation-components rule. No actual dialog instances (e.g. a "New Project" dialog) were built, per spec.

## In Progress

- None yet.

- Wired the chrome into a real layout: `components/editor/editor-shell.tsx` (client component) owns the `isSidebarOpen` state and composes `EditorNavbar` + `ProjectSidebar` around a `relative min-h-0 flex-1` region that renders `children` (the canvas slot) — the `relative` wrapper is what makes `ProjectSidebar`'s `absolute` positioning float correctly under the navbar. `app/editor/layout.tsx` is a thin server component that renders `EditorShell`; `app/editor/page.tsx` is a placeholder ("Canvas coming soon") so the route has content to verify against — the real canvas isn't speced yet.

## In Progress

- None yet.

## Next Up

- Pick the next feature unit from `context/feature-specs/`.

## Open Questions

- `/editor` is a placeholder route (no dynamic project ID segment) since project creation/auth aren't implemented yet. `project-overview.md` implies a "project workspace" the user enters after selecting a project — once the projects/auth feature spec exists, this route will likely need to move to something project-scoped (e.g. `/editor/[projectId]`).

## Architecture Decisions

- The installed `shadcn` CLI is v4.21.0, a materially different major version from the classic shadcn/ui CLI (component library choice is `-b radix|base|aria`; theming ships via a "preset"; `cn()` comes from the official `cn` npm package rather than a hand-written `clsx` + `tailwind-merge` helper). Used `-b radix` (Radix primitives, matching `architecture.md`'s implicit assumption of standard shadcn/Radix components) and the `nova` preset (Lucide icons + Geist fonts, matching `ui-context.md` and the fonts already wired in `app/layout.tsx`).
- `app/globals.css` had no theme tokens yet (just `@import "tailwindcss";`) despite `01-design-system.md` referring to an "existing dark theme" — the palette was fully specified in `ui-context.md` but not yet implemented in CSS, so it was added as part of this unit rather than raised as an open question.
- Because the product is dark-only, the shadcn `:root` and `.dark` blocks were both set to the same dark values (rather than relying solely on the `.dark` class on `<html>`) so the theme can't regress to shadcn's light defaults if that class is ever dropped.

## Session Notes

- Verified visually: ran `next dev`, rendered all 7 components on a temporary route, screenshotted with Playwright, confirmed dark palette (cyan primary, dark surfaces) with no light-mode flash, no console errors, then deleted the temporary route. `tsc --noEmit` and `next build` both pass.
- Editor navbar/sidebar: verified against the already-running dev server via a temporary route (`app/preview-editor-chrome`, deleted after verification) by inspecting rendered HTML: sidebar starts closed (`-translate-x-full` present), toggle button renders the `lucide-panel-left-open` icon with correct `aria-label`, tabs/empty-states/New Project button all render, no hydration or runtime errors. `tsc --noEmit` and `eslint` both pass project-wide.
- This unit (`02-editor.md`) was implemented twice in the same session: the components, the `app/editor` route wiring built as a follow-up request, and this progress-tracker entry were all found reverted to the pre-editor state on disk partway through — the components and `app/editor/*` were empty and this file had rolled back to only listing the design-system unit as complete. Re-implemented `02-editor.md` from the spec to match; the follow-up layout-wiring (`components/editor/editor-shell.tsx`, `app/editor/layout.tsx`, `app/editor/page.tsx`) was not redone since it wasn't asked for again and isn't part of this spec's scope — redo it if it's still wanted.
