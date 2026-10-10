# DESIGN.md — Global Design Guideline

> **Owner**: UI/UX Designer agent (write). All other agents: read-only.
> **Status**: Starter template with daisyUI defaults. Replace the `<placeholder>` values with your brand before the first project.

---

## 1. Purpose & Precedence

This file is the **single source of truth for visual and interaction design** across every project the agency delivers.

| Document | Scope | Owner |
| :--- | :--- | :--- |
| `DESIGN.md` (this file) | Global rules: brand, themes, tokens, typography, spacing, components, accessibility | UI/UX Designer |
| `design/specs/design_spec_<feature>.md` | Per-feature screens, flows, and states | UI/UX Designer |
| `design/tokens/daisyui_theme_tokens.json` | Machine-readable token values | UI/UX Designer |

**Rules**
1. Every feature spec must comply with this file.
2. If a feature spec conflicts with this file, **this file wins**.
3. A deviation needs Tech Lead approval. If approved as a permanent rule, update this file and add an entry to the [Change Log](#15-change-log).
4. The Frontend Developer follows this file for anything a feature spec does not cover.

---

## 2. Brand Principles

| Attribute | Value |
| :--- | :--- |
| Product name | `<product name>` |
| Personality | `<e.g. trustworthy, calm, efficient>` |
| Voice | `<e.g. clear, direct, friendly — never jokey in errors>` |
| Design keywords | `<e.g. clean, data-dense, high-contrast>` |

---

## 3. Themes

| Mode | daisyUI theme | Notes |
| :--- | :--- | :--- |
| Light (default) | `emerald` | Default on first load |
| Dark | `business` | Applied when the user selects dark or the OS prefers dark |

- Switch themes via `document.documentElement.setAttribute('data-theme', '<theme>')`.
- Persist the user's choice (Zustand `useThemeStore` with `persist`).
- Every screen must be verified in **both** themes.

---

## 4. Color Tokens

Use **daisyUI semantic tokens only**. Raw hex / rgb values and Tailwind palette colors (`bg-green-500`, `text-gray-700`) are not allowed in components.

| Token | Use for |
| :--- | :--- |
| `primary` / `primary-content` | Main call-to-action, active nav item, key highlights |
| `secondary` / `secondary-content` | Secondary actions, supporting highlights |
| `accent` / `accent-content` | Sparing emphasis (badges, charts), never for primary CTA |
| `neutral` / `neutral-content` | Dark surfaces, tooltips, neutral buttons |
| `base-100` | Cards, modals, main content surface |
| `base-200` | Page background, sidebar |
| `base-300` | Borders, dividers, inactive states |
| `base-content` | Default text and icons |
| `info` | Informational messages |
| `success` | Completed / positive states |
| `warning` | Needs attention, non-blocking |
| `error` | Failures, destructive actions, validation errors |

Custom brand colors, if any, are defined **only** in the theme configuration and exposed through the tokens above.

---

## 5. Typography

| Item | Value |
| :--- | :--- |
| Font family | `<Inter>`, fallback `ui-sans-serif, system-ui, sans-serif` |
| Monospace | `ui-monospace, SFMono-Regular, Menlo, monospace` (code, IDs) |
| Base size | 16px (`text-base`) |

| Role | Tailwind class | Weight |
| :--- | :--- | :--- |
| Page title (h1) | `text-2xl lg:text-3xl` | `font-bold` |
| Section title (h2) | `text-xl` | `font-semibold` |
| Card title (h3) | `text-lg` | `font-semibold` |
| Body | `text-base` | `font-normal` |
| Secondary / helper text | `text-sm text-base-content/70` | `font-normal` |
| Caption / label | `text-xs` | `font-medium` |
| KPI value | `stat-value` | daisyUI default |

- Line length for body text: max ~75 characters (`max-w-prose`).
- Numbers in tables and KPIs use `tabular-nums`.

---

## 6. Spacing & Layout

- **Grid unit**: 4px (Tailwind spacing scale). Use `gap-*`, `p-*`, `m-*`; no arbitrary pixel values.
- Common spacing: `gap-2` (inside controls), `gap-4` (between related items), `gap-6` (between cards), `p-4 lg:p-6` (page padding).

| Breakpoint | Width | Layout |
| :--- | :--- | :--- |
| Mobile | < 640px | Single column, drawer hidden behind hamburger |
| Tablet | 640–1024px | 2-column grids, drawer still overlay |
| Desktop | > 1024px | `drawer lg:drawer-open`, 3–4 column grids |

- **App shell**: daisyUI `drawer lg:drawer-open` + `navbar bg-base-100 shadow-md`.
- **Max content width**: `max-w-7xl mx-auto` for content pages; dashboards may be full width.

---

## 7. Radius, Shadow & Borders

| Item | Class |
| :--- | :--- |
| Cards, modals | `rounded-box` |
| Buttons, inputs | daisyUI default (`rounded-btn`) |
| Badges | daisyUI default (`rounded-badge`) |
| Card shadow | `shadow-md` (raised: `shadow-xl`) |
| Borders | `border border-base-300` |

---

## 8. Iconography

- Library: **Lucide** (`lucide-react`).
- Sizes: 16px inline with text, 20px in buttons/nav, 24px in KPI `stat-figure`.
- Icons inherit `currentColor`. Icon-only buttons require `aria-label`.

---

## 9. Components (canonical daisyUI usage)

| Component | Canonical classes |
| :--- | :--- |
| Primary button | `btn btn-primary` |
| Secondary button | `btn btn-outline` / `btn btn-ghost` |
| Destructive button | `btn btn-error` |
| Card | `card bg-base-100 shadow-md border border-base-300` |
| KPI stats | `stats shadow bg-base-100 border border-base-300` |
| Table | `table table-zebra table-pin-rows` |
| Status badge | `badge badge-success` / `badge-warning` / `badge-error` / `badge-outline` |
| Text input | `input input-bordered w-full` |
| Select | `select select-bordered w-full` |
| Textarea | `textarea textarea-bordered w-full` |
| Modal | `modal` + `modal-box` (use `<dialog>`) |
| Alert | `alert alert-info` / `alert-success` / `alert-warning` / `alert-error` |
| Loading placeholder | `skeleton h-4 w-full` |
| Inline spinner (buttons only) | `loading loading-spinner loading-sm` |
| Navbar | `navbar bg-base-100 shadow-md` |

- One primary button per view or section.
- Destructive actions require a confirmation modal.

---

## 10. States

**Screen states** (every screen must define all five):

| State | Pattern |
| :--- | :--- |
| Empty | Icon + short message + primary action |
| Loading | `skeleton` placeholders matching final layout (no full-page spinners) |
| Populated | Normal content |
| Error | `alert alert-error` + clear message + Retry button |
| Disabled | `btn-disabled` / `disabled` attribute + reason tooltip where helpful |

**Interaction states** (every interactive element): default, hover, active, focus-visible, disabled, loading.

---

## 11. Accessibility (WCAG 2.1 AA)

- Text contrast >= 4.5:1 (large text >= 3:1) in **both** themes.
- Focus ring on all interactive elements: `focus-visible:ring-2 focus-visible:ring-primary`.
- Full keyboard operation; logical tab order; `Esc` closes modals and drawers.
- Semantic HTML: `<nav>`, `<main>`, `<header>`, `<button>`, `<dialog>`, `<table>`.
- ARIA: `aria-label` on icon-only buttons, `aria-expanded` on toggles, `aria-live="polite"` for async status messages.
- Touch targets >= 44x44px on mobile.
- Never convey meaning by color alone (pair badges with text/icons).

---

## 12. Motion

| Use | Duration |
| :--- | :--- |
| Hover / color change | 150ms |
| Dropdown, tooltip | 200ms |
| Modal, drawer | 300ms |

- Easing: `ease-out` for entering, `ease-in` for leaving.
- Respect `prefers-reduced-motion` (`motion-reduce:transition-none`).

---

## 13. Content & Copy

- Casing: Sentence case for titles, labels, and buttons.
- Buttons: verb-first (`Save changes`, `Create campaign`), not `OK` / `Submit`.
- Error messages: what happened + how to fix (`Amount must be greater than 0.`).
- Dates: `<e.g. 12 Oct 2026>`; numbers and currency use the user's locale.

---

## 14. Do / Don't

| Do | Don't |
| :--- | :--- |
| Use daisyUI semantic tokens | Use raw hex or Tailwind palette colors |
| Use `skeleton` for loading | Use full-page spinners |
| Verify in light and dark themes | Design for one theme only |
| Keep one primary action per view | Show several competing primary buttons |
| Label icon-only buttons with `aria-label` | Rely on icons or color alone |
| Use the 4px spacing scale | Use arbitrary pixel values |

---

## 15. Change Log

| Version | Date | Change | Approved by |
| :--- | :--- | :--- | :--- |
| 0.1.0 | 2026-10-11 | Initial starter template (daisyUI defaults) | — |
