---
description: Describe when these instructions should be loaded by the agent based on task context
# applyTo: 'Describe when these instructions should be loaded by the agent based on task context' # when provided, instructions will automatically be added to the request context when the pattern matches an attached file
---
# UX/UI Design Document Template
# .github/instructions/ux-ui-design-template.instructions.md
#
# Consumed by: ux-ui-designer agent only
# Purpose: Defines the exact structure and placeholders of the ux-ui-design.md output document.
#          The agent fills in every {placeholder} based on the requirements and spec files.
#          Update this file to change the output document structure without modifying the agent.
#
# Placeholder conventions:
#   {value}        → single value to be filled in
#   {list}         → one or more items, one per line
#   {yes/no}       → boolean choice
#   [PENDING: ...] → value that cannot yet be determined; must be resolved before frontend agents start

---

## Output Document

The agent must generate exactly this structure. No sections may be skipped.
If a value cannot be determined from available inputs, write `[PENDING: reason]` — never invent values.

---

```markdown
# UX/UI Design System
> Source of truth for all frontend agents.
> Last updated: {ISO date}
> Feature: {feature name}
> Status: ACTIVE

---

## 1. Brand Identity

### 1.1 Color Palette

| Token | Hex (light) | Hex (dark) | Usage |
|-------|-------------|------------|-------|
| `--color-brand-primary` | {hex} | {hex} | {usage} |
| `--color-brand-secondary` | {hex} | {hex} | {usage} |
| `--color-surface-base` | {hex} | {hex} | {usage} |
| `--color-surface-raised` | {hex} | {hex} | {usage} |
| `--color-surface-overlay` | {hex} | {hex} | {usage} |
| `--color-text-primary` | {hex} | {hex} | {usage} |
| `--color-text-secondary` | {hex} | {hex} | {usage} |
| `--color-text-disabled` | {hex} | {hex} | {usage} |
| `--color-text-inverse` | {hex} | {hex} | {usage} |
| `--color-border-default` | {hex} | {hex} | {usage} |
| `--color-border-focus` | {hex} | {hex} | {usage} |
| `--color-feedback-success` | {hex} | {hex} | {usage} |
| `--color-feedback-warning` | {hex} | {hex} | {usage} |
| `--color-feedback-error` | {hex} | {hex} | {usage} |
| `--color-feedback-info` | {hex} | {hex} | {usage} |

**Semantic rules**:
- `--color-brand-primary` must ONLY be used for: {list}
- Never use raw hex values in component code — always reference tokens
- Never create new color tokens; use only those listed here

### 1.2 Typography

| Token | Font family | Weight | Size | Line height | Usage |
|-------|-------------|--------|------|-------------|-------|
| `--text-display-xl` | {font} | {weight} | {size} | {lh} | {usage} |
| `--text-display-lg` | {font} | {weight} | {size} | {lh} | {usage} |
| `--text-heading-md` | {font} | {weight} | {size} | {lh} | {usage} |
| `--text-heading-sm` | {font} | {weight} | {size} | {lh} | {usage} |
| `--text-body-lg` | {font} | {weight} | {size} | {lh} | {usage} |
| `--text-body-md` | {font} | {weight} | {size} | {lh} | {usage} |
| `--text-body-sm` | {font} | {weight} | {size} | {lh} | {usage} |
| `--text-label-md` | {font} | {weight} | {size} | {lh} | {usage} |
| `--text-label-sm` | {font} | {weight} | {size} | {lh} | {usage} |
| `--text-code` | {font-mono} | {weight} | {size} | {lh} | {usage} |

**Font loading rule**: Fonts must be loaded in the root layout only. Never import fonts inside individual components.

### 1.3 Spacing Scale

Base unit: `{base}px`. Use only these tokens — never arbitrary pixel values.

| Token | Value | Usage |
|-------|-------|-------|
| `--space-1` | {value} | {usage} |
| `--space-2` | {value} | {usage} |
| `--space-4` | {value} | {usage} |
| `--space-6` | {value} | {usage} |
| `--space-8` | {value} | {usage} |
| `--space-12` | {value} | {usage} |
| `--space-16` | {value} | {usage} |
| `--space-24` | {value} | {usage} |

### 1.4 Border Radius

| Token | Value | Usage |
|-------|-------|-------|
| `--radius-sm` | {value} | {usage} |
| `--radius-md` | {value} | {usage} |
| `--radius-lg` | {value} | {usage} |
| `--radius-xl` | {value} | {usage} |
| `--radius-full` | 9999px | {usage} |

### 1.5 Elevation & Shadow

| Token | Value | Usage |
|-------|-------|-------|
| `--shadow-sm` | {value} | {usage} |
| `--shadow-md` | {value} | {usage} |
| `--shadow-lg` | {value} | {usage} |
| `--shadow-focus` | {value} | {usage} |

---

## 2. Responsive Breakpoints

| Token | Min width | Columns | Max content width | Gutter |
|-------|-----------|---------|-------------------|--------|
| `--bp-xs` | 0px | {n} | {value} | {value} |
| `--bp-sm` | 480px | {n} | {value} | {value} |
| `--bp-md` | 768px | {n} | {value} | {value} |
| `--bp-lg` | 1024px | {n} | {value} | {value} |
| `--bp-xl` | 1280px | {n} | {value} | {value} |

**Rule**: All components must be built mobile-first using `min-width` queries only.

---

## 3. Design Tokens File

- **Path**: `{path}`
- **Format**: `{CSS custom properties | Tailwind config | TS object}`
- **Import rule**: Import globally in `{root layout file}` only
- **Dark mode strategy**: `{class-based (.dark) | media query (prefers-color-scheme) | none}`

---

## 4. Shared Components

> These components are defined ONCE. No agent may reimplement them.
> Each has a single owner path. Any agent needing it imports from that path only.

### 4.1 Navigation / Navbar

**File**: `{path}`
**Conflict rule**: Only one Navbar exists. No feature agent may create a Navbar, Header, or TopBar.
To add navigation items, update `{navItems config file}` only — never the component itself.

**Structure**:
```
Navbar
├── Logo (links to {route})
├── NavItems (rendered from {config file})
│   └── NavItem (active state via {router method})
├── {UserMenu | AuthButtons} (condition: {condition})
└── {MobileMenuButton} (visible below {breakpoint})
```

**Behavior**:
- Active item detection: `{method}` — never hardcode active states
- Mobile menu: `{behavior}` triggered below `{breakpoint}`
- Position: `{sticky top-0 | fixed | static}`, z-index `{value}`
- Height: `{value}px` desktop / `{value}px` mobile
- Content below navbar must have `padding-top: {value}px`

### 4.2 Sidebar

**File**: `{path}`
**Applicable**: `{yes | no — reason}`
**Conflict rule**: Same as Navbar. Add items via `{config file}` only.

**Structure**:
```
Sidebar
├── SidebarHeader (logo + collapse toggle)
├── SidebarNav (items from {config file})
│   ├── SidebarSection (grouped items)
│   └── SidebarItem (icon + label + badge)
└── SidebarFooter ({content})
```

**Behavior**:
- Default state: `{expanded | collapsed}`
- Collapsed width: `{value}px` / Expanded width: `{value}px`
- Mobile: `{drawer overlay | bottom sheet | hidden}`
- Collapse state persisted in: `{localStorage key | none}`

### 4.3 Page Layout Wrapper

**File**: `{path}`
**Rule**: Every page must use `<PageLayout>`. Never build page-level padding from scratch.

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `title` | string | yes | Sets `<title>` and H1 |
| `breadcrumb` | BreadcrumbItem[] | no | Breadcrumb trail |
| `actions` | ReactNode | no | Top-right header slot |
| `maxWidth` | {union type} | no | Content width constraint |

### 4.4 Button

**File**: `{path}`

| Variant | Background | Text color | Border | Usage |
|---------|------------|------------|--------|-------|
| `primary` | `{token}` | `{token}` | {value} | {usage} |
| `secondary` | `{token}` | `{token}` | {value} | {usage} |
| `ghost` | `{token}` | `{token}` | {value} | {usage} |
| `danger` | `{token}` | `{token}` | {value} | {usage} |
| `link` | `{token}` | `{token}` | {value} | {usage} |

| Size | Height | H. padding | Font token |
|------|--------|------------|------------|
| `sm` | {value} | {token} | {token} |
| `md` | {value} | {token} | {token} |
| `lg` | {value} | {token} | {token} |

**States**: `default`, `hover`, `active`, `focus-visible`, `disabled`, `loading`
- `loading`: replace label with `{spinner path}`, disable pointer events
- `focus-visible`: use `{token}`. Never `outline: none` without a replacement

### 4.5 Form Components

**File**: `{path}`

**Field anatomy**:
```
FormField
├── Label (for={id}) — {token}
├── {Input | Textarea | Select}
│   ├── default:  border {token}
│   ├── focus:    border {token} + {token}
│   ├── error:    border {token}
│   └── disabled: bg {token}, text {token}
├── HelperText — {token}, {token}
└── ErrorMessage — {token}, {token}
```

**Validation rule**: Never show error state on untouched fields. Use `{form library}`.

### 4.6 Toast / Notification

**File**: `{path}`
**Provider location**: Root layout only.
**Usage**: `import { useToast } from '{path}'` — never render toast directly.

| Type | Background token | Duration |
|------|-----------------|----------|
| `success` | {token} | {ms} |
| `error` | {token} | {ms} |
| `warning` | {token} | {ms} |
| `info` | {token} | {ms} |

**Position**: `{position}`. Max simultaneous toasts: `{n}`.

### 4.7 Modal / Dialog

**File**: `{path}`

**Rules**:
- Never use `window.alert()`, `window.confirm()`, or raw `<dialog>`
- Rendered into portal at `{container id}`
- Closes on: Escape key, backdrop click, close button
- Focus trap: `{library | built-in}`
- Scroll lock: `overflow: hidden` on `<body>` while open

| Size | Max width |
|------|-----------|
| `sm` | {value} |
| `md` | {value} |
| `lg` | {value} |
| `xl` | {value} |

### 4.8 Loading States

| Pattern | Component | When to use |
|---------|-----------|-------------|
| Page load | `{component}` | {usage} |
| Data fetch | `{component}` | {usage} |
| Button action | `{component}` | {usage} |
| Full screen | `{component}` | {usage} |

**Rule**: Never use a spinner for content with a known layout. Use skeleton screens.

### 4.9 Icon Library

- **Library**: `{library}`
- **Import**: `import { IconName } from '{library}'`
- **Default size**: `{n}px`. Always pass `size` and `aria-hidden={true}`
- **Never**: inline SVG paths, emoji as icons, multiple icon libraries

---

## 5. Feature: {Feature Name}

> Sections 1–4 are global. This section is added per feature and must not modify sections above.

### 5.1 Page Map

| Route | Component file | Layout | Auth required |
|-------|---------------|--------|---------------|
| `/{route}` | `{path}` | {layout} | {yes/no} |

### 5.2 Navigation Changes

> Items to add to `{navItems config file}`. If none, write: None.

```ts
{ label: '{label}', href: '/{route}', icon: {IconName}, section: '{section}' }
```

### 5.3 Screen Specifications

> Repeat this block for each screen introduced by this feature.

#### Screen: {Screen Name} (`{route}`)

**Purpose**: {one sentence}

**Layout**:
```
PageLayout (title="{title}", maxWidth="{size}")
├── {section}
│   └── {component}
└── {section}
    └── {component}
```

**States**:
- Empty: `{icon}` + "{heading}" + "{supporting text}" + `{CTA button | none}`
- Loading: `{skeleton component}` at `{location}`
- Error: `{inline | toast | redirect}` — "{exact message}"

**Responsive**:
- Mobile (< {bp}): {changes}
- Tablet ({bp}–{bp}): {changes}
- Desktop (> {bp}): default layout above

### 5.4 Feature Components

> Components exclusive to this feature.
> Location: `src/components/features/{feature-name}/` or `src/app/{route}/_components/`
> Repeat this block for each component.

#### `{ComponentName}`

**File**: `{path}`

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `{prop}` | `{type}` | {yes/no} | {description} |

**Visual states**: {list}

**Rules**:
- {rule}

### 5.5 Data Display

| Data type | Format | Null display |
|-----------|--------|--------------|
| Date | {format} | — |
| Timestamp | {format} | — |
| Currency | {format} | — |
| Percentage | {format} | — |
| Large number | {format} | — |
| Truncated text | {max chars, strategy} | — |

### 5.6 Interaction Patterns

| Trigger | Component | Behavior |
|---------|-----------|----------|
| {trigger} | {component} | {behavior} |
| Form submit with errors | Form | Scroll to first error, focus first invalid field |
| Delete action | Button danger | Confirmation modal: "{title}" / "{confirm label}" / "Cancel" |
| Network error | Any async action | Toast error: "{exact message}" |
| Session expired | Any protected route | Redirect to `/login?returnTo={current path}` |

### 5.7 Accessibility Checklist

> The agent must verify and document each item for this feature.

- [ ] Every interactive element has visible focus state using `{token}`
- [ ] All images have descriptive `alt`. Decorative images use `alt=""`
- [ ] Color contrast meets WCAG AA (4.5:1 body, 3:1 large text)
- [ ] All inputs have associated `<label>` via `htmlFor` / `id`
- [ ] Error messages use `aria-describedby` referencing the field
- [ ] Modal uses `role="dialog"`, `aria-labelledby`, and focus trap
- [ ] Loading states use `aria-live="polite"` or `role="status"`
- [ ] Icon-only buttons have `aria-label`

### 5.8 Motion

| Element | Animation | Duration | Easing | Reduced motion |
|---------|-----------|----------|--------|----------------|
| {element} | {type} | {ms} | {easing} | none |

**Rule**: All animations must be wrapped in `@media (prefers-reduced-motion: no-preference)`.

---

## 6. File & Folder Conventions

### 6.1 Directory Structure

```
src/
├── app/
│   ├── layout.tsx              # Root layout — owned by architectural-builder. Do not modify.
│   ├── globals.css             # Global styles — owned by architectural-builder. Do not modify.
│   └── {feature}/
│       ├── page.tsx
│       └── _components/        # Private to this route
├── components/
│   ├── shared/                 # Section 4 components — never override
│   ├── ui/                     # Primitive components — never override
│   └── features/
│       └── {feature-name}/     # Section 5.4 components
├── styles/
│   └── {tokens file}           # Design tokens — owned by architectural-builder. Do not modify.
├── hooks/
├── lib/
└── types/
```

### 6.2 Naming Conventions

| Item | Convention |
|------|-----------|
| Component files | PascalCase `.tsx` |
| Hook files | camelCase, `use` prefix, `.ts` |
| Utility files | camelCase `.ts` |
| Page files | lowercase-hyphen `/page.tsx` |
| Test files | same name + `.test.tsx` |

### 6.3 CSS Strategy

**Method**: `{method}` — agents must not introduce any other styling approach.

{If Tailwind}: Custom values must be defined in `tailwind.config.ts` under `theme.extend`. Never use arbitrary values (e.g., `w-[347px]`).

{If CSS Modules}: One `.module.css` per component. No global class names. No `!important`.

---

## 7. Conflict Prevention Rules

| Rule | Forbidden pattern | Correct pattern |
|------|------------------|-----------------|
| No duplicate Navbar / Header / TopBar | Creating `FeatureHeader.tsx` with a sticky bar | Use `PageLayout` `actions` prop |
| No global CSS additions | Adding classes to `globals.css` | CSS modules or Tailwind classes |
| No shared component edits | Editing `Button.tsx` for a feature style | Add variant via props |
| No cross-feature imports | `import X from '../other-feature/_components/X'` | Move to `src/components/features/` |
| No hardcoded breakpoints | `@media (max-width: 768px)` | `var(--bp-md)` or `@screen md` |
| No hardcoded colors | `color: {hex}` | `color: var(--color-{token})` |
| No custom spinners | New spinner SVG | `<Skeleton />` or `<Button loading>` |
| No custom modals | Raw `<div>` overlay | `<Modal>` from shared |
| {additional rule} | {forbidden} | {correct} |

---

## 8. Third-Party Library Policy

> Agents must not introduce libraries outside this list without architectural-builder approval.

| Purpose | Library | Version |
|---------|---------|---------|
| Framework | {library} | {version} |
| Styling | {library} | {version} |
| Forms | {library} | {version} |
| Validation | {library} | {version} |
| Icons | {library} | {version} |
| Dates | {library} | {version} |
| Animation | {library} | {version} |
| Notifications | {library} | {version} |

---

## Appendix: Design Rationale

> Document non-obvious decisions so future agents understand the constraints.

| Decision | Rationale |
|----------|-----------|
| {decision} | {reason} |
```