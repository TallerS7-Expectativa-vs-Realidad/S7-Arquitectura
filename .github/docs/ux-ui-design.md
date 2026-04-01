# UX/UI Design System
> Source of truth for all frontend agents.
> Last updated: 2026-03-31
> Features: hu-01-consultar-estado-disponibilidad-libro, hu-02-registrar-prestamo-libro, hu-03-registrar-devolucion-en-plazo, hu-04-registrar-devolucion-tardia-generar-multa, hu-05-consultar-prestamos-vencidos-y-lector, hu-06-registrar-pago-total-multa-rehabilitar-lector
> Status: ACTIVE

---

## 1. Brand Identity

### 1.1 Color Palette

| Token | Hex (light) | Usage |
|-------|-------------|-------|
| `--color-brand-primary` | #C17F24 | Ocre dorado — primary action buttons, active nav state, focus ring, actionable links |
| `--color-brand-secondary` | #A0522D | Terracota suave — secondary accent, hover emphasis |
| `--color-surface-base` | #FAF7F2 | Crema cálida — page background |
| `--color-surface-raised` | #FFFDF9 | Blanco marfil — cards, modals, raised containers |
| `--color-surface-overlay` | #F2EDE4 | Beige medio — sidebar, lateral panels |
| `--color-text-primary` | #2C1A0E | Marrón oscuro — primary body text, headings |
| `--color-text-secondary` | #7A5C3E | Marrón medio — secondary text, labels, captions |
| `--color-text-disabled` | #C4B49A | Beige grisáceo — disabled text, placeholder |
| `--color-text-inverse` | #FAF7F2 | Crema clara — text on dark/primary backgrounds |
| `--color-border-default` | #DDD0BC | Arena — default borders on inputs, cards, dividers |
| `--color-border-focus` | #C17F24 | Ocre intenso — focus ring on interactive elements |
| `--color-feedback-success` | #5A7A3A | Verde oliva — success toasts, "Disponible"/"Habilitado" badges |
| `--color-feedback-warning` | #D4860B | Ámbar — warning toasts, "Deuda pendiente" badges, near-due loans |
| `--color-feedback-error` | #B83C2B | Rojo ladrillo — error states, validation messages, destructive actions |
| `--color-feedback-info` | #4A6FA5 | Azul pizarra — informational toasts, neutral badges |

**Semantic rules**:
- `--color-brand-primary` must ONLY be used for: primary action buttons, active navigation state, focus rings, actionable links
- Never use raw hex values in component code — always reference tokens via `var(--token-name)`
- Never create new color tokens; use only those listed here
- Dark mode: not implemented in MVP. Only `:root {}` block, no `.dark {}` block

### 1.2 Typography

| Token | Font family | Weight | Size | Line height | Usage |
|-------|-------------|--------|------|-------------|-------|
| `--text-display-lg` | Lora | 600 | 30px | 1.2 | Main page title |
| `--text-heading-md` | Lora | 600 | 24px | 1.3 | Section headings |
| `--text-heading-sm` | Lora | 600 | 20px | 1.3 | Card titles, panel headings |
| `--text-body-lg` | Inter | 400 | 18px | 1.6 | Emphasized body text |
| `--text-body-md` | Inter | 400 | 16px | 1.6 | Default body text |
| `--text-body-sm` | Inter | 400 | 14px | 1.6 | Secondary text, captions |
| `--text-label-md` | Inter | 500 | 14px | 1.4 | Form labels, button text, nav items |
| `--text-label-sm` | Inter | 500 | 12px | 1.4 | Badges, tags, timestamps |
| `--text-code` | JetBrains Mono | 400 | 14px | 1.6 | Numeric values in dense tables, IDs |

**Font loading rule**: Fonts must be loaded via `<link>` in `index.html` only. Never import fonts inside individual components.

### 1.3 Spacing Scale

Base unit: `4px`. Use only these tokens — never arbitrary pixel values.

| Token | Value | Usage |
|-------|-------|-------|
| `--space-1` | 4px | Tight inline spacing, icon-to-label gap |
| `--space-2` | 8px | Label-to-input gap, compact padding |
| `--space-4` | 16px | Intra-group spacing, default gap between fields in same group |
| `--space-6` | 24px | Card internal padding, section padding |
| `--space-8` | 32px | Gap between cards, between major sections |
| `--space-12` | 48px | Gap between form groups, page section dividers |
| `--space-16` | 64px | Major vertical rhythm between page-level sections |
| `--space-24` | 96px | Top/bottom page margin |

### 1.4 Border Radius

| Token | Value | Usage |
|-------|-------|-------|
| `--radius-sm` | 4px | Badges, tags, small elements |
| `--radius-md` | 6px | Inputs, buttons, small containers |
| `--radius-lg` | 8px | Cards, modals, raised surfaces |
| `--radius-xl` | 12px | Large panels, featured containers |
| `--radius-full` | 9999px | Avatars, circular indicators, pills |

### 1.5 Elevation & Shadow

| Token | Value | Usage |
|-------|-------|-------|
| `--shadow-sm` | 0 1px 3px rgba(44, 26, 14, 0.08) | Cards, raised surfaces |
| `--shadow-md` | 0 4px 12px rgba(44, 26, 14, 0.12) | Dropdowns, popovers |
| `--shadow-lg` | 0 8px 24px rgba(44, 26, 14, 0.16) | Modals, dialogs |
| `--shadow-focus` | 0 0 0 3px rgba(193, 127, 36, 0.35) | Focus ring for interactive elements |

---

## 2. Responsive Breakpoints

| Token | Min width | Columns | Max content width | Gutter |
|-------|-----------|---------|-------------------|--------|
| `--bp-lg` | 1024px | 12 | 1280px | 24px |
| `--bp-xl` | 1280px | 12 | 1280px | 24px |

**Rule**: This system is desktop-only (min 1024px). No mobile or tablet breakpoints are required. Do not implement media queries below 1024px.

**Between 1024px–1280px**: The sidebar may collapse automatically to 64px to reclaim space.

---

## 3. Design Tokens File

- **Path**: `src/index.css` (`:root {}` block with all CSS custom properties)
- **Format**: CSS custom properties (native CSS variables)
- **Import rule**: `index.css` is imported in `src/main.jsx` only. Components access tokens via `var(--token-name)` in their `.module.css` files.
- **Dark mode strategy**: None in MVP. Only one `:root {}` block.

---

## 4. Shared Components

> These components are defined ONCE. No agent may reimplement them.
> Each has a single owner path. Any agent needing it imports from that path only.

### 4.1 Navigation / Sidebar

**File**: `src/components/Navigation.jsx`
**Styles**: `src/components/Navigation.module.css`
**Conflict rule**: Only one Navigation component exists. No feature agent may create a Navbar, Header, Sidebar, or TopBar. To add navigation items, update `Navigation.jsx` nav items array only — never create a parallel navigation component.

**Structure**:
```
Navigation
├── NavList
│   └── NavItem (Link) × N
│       ├── Icon (from lucide-react)
│       └── Label text
```

**Behavior**:
- Active item detection: `useLocation().pathname` comparison via `isActive(path)` — never hardcode active states
- Position: sidebar left, fixed, z-index 100
- Width: 240px expanded / 64px collapsed (between 1024px–1280px)
- Active item: `--color-brand-primary` background tint + left border accent
- Content area must have `margin-left` equal to sidebar width

**Navigation items** (current):
```
{ label: 'Registrar Préstamo', href: '/loan', icon: BookMarked }
{ label: 'Registrar Devolución', href: '/return', icon: BookOpen }
{ label: 'Préstamos Vencidos', href: '/loans/outTime', icon: AlertTriangle }
{ label: 'Pagar Multa', href: '/payment', icon: CheckCircle }
```

### 4.2 Sidebar

**Applicable**: The Navigation component serves as the sidebar per requirements. No separate Sidebar component is needed. See 4.1.

### 4.3 Page Layout Wrapper

**File**: `[PENDING: architectural-builder must create src/components/PageLayout.jsx]`
**Rule**: Every page must use `<PageLayout>`. Never build page-level padding from scratch.

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `title` | string | yes | Sets page H1 heading using `--text-display-lg` |
| `actions` | ReactNode | no | Top-right header slot for page-level actions |
| `maxWidth` | `'sm' \| 'md' \| 'lg'` | no | Content width constraint: sm=640px, md=800px, lg=1280px |

### 4.4 Button

**File**: `[PENDING: architectural-builder must create src/components/Button.jsx]`
**Styles**: `[PENDING: src/components/Button.module.css]`

| Variant | Background | Text color | Border | Usage |
|---------|------------|------------|--------|-------|
| `primary` | `--color-brand-primary` | `--color-text-inverse` | none | Primary CTA: search, register, confirm |
| `secondary` | `--color-surface-raised` | `--color-text-primary` | 1px solid `--color-border-default` | Secondary actions: clear, cancel, back |
| `ghost` | transparent | `--color-brand-primary` | none | Tertiary actions, inline links |
| `danger` | `--color-feedback-error` | `--color-text-inverse` | none | Destructive actions: delete, revoke |
| `link` | transparent | `--color-brand-primary` | none | Inline text links, navigation aids |

| Size | Height | H. padding | Font token |
|------|--------|------------|------------|
| `sm` | 32px | `--space-4` | `--text-label-sm` |
| `md` | 40px | `--space-4` | `--text-label-md` |
| `lg` | 48px | `--space-6` | `--text-label-md` |

**States**: `default`, `hover`, `active`, `focus-visible`, `disabled`, `loading`
- `loading`: replace label with spinner, disable pointer events
- `focus-visible`: use `--shadow-focus`. Never `outline: none` without a replacement
- `disabled`: `--color-text-disabled` text, `opacity: 0.65`, `cursor: not-allowed`

### 4.5 Form Components

**File**: `[PENDING: architectural-builder must create src/components/FormField.jsx]`

**Field anatomy**:
```
FormField
├── Label (for={id}) — --text-label-md, --color-text-primary
├── {Input | Textarea | Select}
│   ├── default:  border --color-border-default
│   ├── focus:    border --color-border-focus + --shadow-focus
│   ├── error:    border --color-feedback-error
│   └── disabled: bg --color-surface-overlay, text --color-text-disabled
├── HelperText — --text-body-sm, --color-text-secondary
└── ErrorMessage — --text-body-sm, --color-feedback-error
```

**Validation rule**: Never show error state on untouched fields. Validate on blur after field is dirtied.

### 4.6 Toast / Notification

**File**: `[PENDING: architectural-builder must create src/components/Toast.jsx]`
**Provider location**: Root layout (`App.jsx`) only.
**Usage**: `import { useToast } from '../hooks/useToast'` — never render toast directly.

| Type | Background token | Duration |
|------|-----------------|----------|
| `success` | `--color-feedback-success` | 4000ms |
| `error` | `--color-feedback-error` | 8000ms (requires manual dismiss) |
| `warning` | `--color-feedback-warning` | 6000ms |
| `info` | `--color-feedback-info` | 4000ms |

**Position**: bottom-right. Max simultaneous toasts: 3.

### 4.7 Modal / Dialog

**File**: `[PENDING: architectural-builder must create src/components/Modal.jsx]`

**Rules**:
- Never use `window.alert()`, `window.confirm()`, or raw `<dialog>`
- Rendered into portal at `#modal-root` (add `<div id="modal-root">` in `index.html`)
- Closes on: Escape key, backdrop click, close button
- Focus trap: built-in (manual focus management)
- Scroll lock: `overflow: hidden` on `<body>` while open

| Size | Max width |
|------|-----------|
| `sm` | 400px |
| `md` | 560px |
| `lg` | 720px |

### 4.8 Loading States

| Pattern | Component | When to use |
|---------|-----------|-------------|
| Content load | Skeleton (`[PENDING: src/components/Skeleton.jsx]`) | Tables, lists, cards with known layout |
| Button action | Button `loading` prop | Async form submissions, search |
| Full section | Skeleton grid matching content layout | First page load |

**Rule**: Never use a spinner for content with a known layout. Use skeleton screens that replicate the exact shape of the content they replace.

### 4.9 Icon Library

- **Library**: `lucide-react`
- **Import**: `import { IconName } from 'lucide-react'`
- **Default size**: 20px. In context inline with text: 16px.
- **Color**: inherits from container text color, never hardcoded
- Always pass `size` and `aria-hidden={true}` (unless the icon is the sole content of a button, in which case use `aria-label`)
- **Never**: inline SVG paths, emoji as icons, multiple icon libraries

**Domain icons**:
| Entity | Icon |
|--------|------|
| Libro | `BookOpen` |
| Lector | `User` |
| Préstamo | `BookMarked` |
| Multa / Deuda | `AlertCircle` |
| Pago | `CheckCircle` |
| Historial | `Clock` |
| Vencido | `AlertTriangle` |
| Buscar | `Search` |
| Disponible | `CircleCheck` |
| No disponible | `CircleX` |

### 4.10 RadioGroup (Visual)

**File**: `[PENDING: architectural-builder must create src/components/RadioGroup.jsx]`
**Styles**: `[PENDING: src/components/RadioGroup.module.css]`

**Purpose**: Styled radio button group rendered as selectable cards/tiles. Used when the option set is small (≤5 items) and all options must be visible simultaneously.

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `name` | string | yes | Name attribute for the radio group (form submission) |
| `label` | string | yes | Visible group label rendered as `<legend>` inside `<fieldset>` |
| `options` | `{ value: string, label: string, description?: string }[]` | yes | Array of selectable options |
| `value` | string | yes | Currently selected value (controlled) |
| `onChange` | `(value: string) => void` | yes | Callback when selection changes |
| `disabled` | boolean | no | Disables all options |
| `error` | string | no | Error message displayed below the group |

**Visual anatomy**:
```
RadioGroup
├── <fieldset>
│   ├── <legend> — group label, --text-label-md, --color-text-primary
│   └── OptionRow (flex, gap: --space-2)
│       └── Option × N
│           ├── Radio indicator (circle, 20px)
│           ├── Label — --text-body-md, --color-text-primary
│           └── Description (optional) — --text-body-sm, --color-text-secondary
└── ErrorMessage — --text-body-sm, --color-feedback-error
```

**Option states**:
| State | Border | Background | Radio indicator |
|-------|--------|------------|----------------|
| default | 1px solid `--color-border-default` | `--color-surface-raised` | hollow circle |
| hover | 1px solid `--color-brand-primary` | `--color-surface-raised` | hollow circle |
| selected | 2px solid `--color-brand-primary` | tint of `--color-brand-primary` at 8% opacity | filled circle `--color-brand-primary` |
| focus-visible | `--shadow-focus` ring | unchanged | unchanged |
| disabled | 1px solid `--color-border-default` | `--color-surface-overlay` | `--color-text-disabled` |

**Rules**:
- Rendered as `<fieldset>` with `<legend>` for accessibility
- Each option is an `<input type="radio">` with associated `<label>` — visually hidden native radio, styled card overlay
- Options must be laid out horizontally when ≤3 items, vertically when >3
- Minimum option card size: 40px height (click area compliance)
- Never use this component when options exceed 5 — use `<select>` instead
- The `description` prop on options is optional for simple labels

---

## 5. Feature: HU-01 — Consultar Estado y Disponibilidad de un Libro

> Sections 1–4 are global. This section is added per feature and must not modify sections above.

### 5.1 Page Map

| Route | Component file | Layout | Auth required |
|-------|---------------|--------|---------------|
| `/loan` | `src/pages/LoanCombinedPage.jsx` | PageLayout (maxWidth="lg") | no |

The book availability search is integrated into the combined loan page at `/loan`. It does not introduce a new route.

### 5.2 Navigation Changes

None. The `/loan` route already exists in the navigation configuration. The `LoanSearch` component is rendered within `LoanCombinedPage`.

### 5.3 Screen Specifications

#### Screen: Consultar Disponibilidad de Libro (integrated in `/loan`)

**Purpose**: Allow the librarian to search a book by name and see the availability status of all copies.

**Layout**:
```
PageLayout (title="Sistema de Préstamos", maxWidth="lg")
└── Section: Consultar Disponibilidad
    ├── Heading "Consultar Disponibilidad de Libro" (--text-heading-md)
    ├── SearchForm (card: --color-surface-raised, --shadow-sm, --radius-lg)
    │   ├── FormField (label="Nombre del Libro", input type="text", required)
    │   └── ButtonGroup
    │       ├── Button (variant="primary", label="Buscar", icon=Search)
    │       └── Button (variant="secondary", label="Limpiar")
    └── ResultsSection
        ├── ResultsMessage (--text-body-md, contextual color)
        └── ResultsTable | EmptyState | ErrorState | SkeletonState
```

**States**:

- **Initial (no search performed)**:
  - The results section is hidden
  - Only the search form is visible
  - The search input has `placeholder="ej. Don Quijote"` as format example

- **Loading**:
  - Button "Buscar" shows loading state (spinner replacing label text, displays "Buscando...")
  - Input is disabled during search
  - `aria-live="polite"` region announces "Buscando libro..."

- **Results — copies found**:
  - A summary message appears above the table: "{N} copia(s) encontrada(s) para '{bookName}'"
  - Results table displays all copies:

    | Column | Alignment | Content | Token |
    |--------|-----------|---------|-------|
    | ID Copia | left | `id_book` value | `--text-code` (tabular-nums) |
    | ID Préstamo | right | `loan_id` or "—" if null | `--text-code` (tabular-nums) |
    | Estado | center | Status badge | See badge rules below |

  - Status badges:
    - `RETURNED` → Badge with `--color-feedback-success` background, text "Disponible", icon `CircleCheck` (16px)
    - `ON_LOAN` → Badge with `--color-feedback-error` background, text "Préstamo Activo", icon `CircleX` (16px)
    - No history → Badge with `--color-feedback-info` background, text "Sin historial — Disponible", icon `BookOpen` (16px)
  - Badges must always include text + icon. Never use color alone to convey status.

- **Results — no copies found (book not in system)**:
  - Icon: `BookOpen` (48px, `--color-text-secondary`)
  - Heading: "Sin resultados para '{bookName}'"
  - Supporting text: "No se encontraron libros con ese nombre. Verifica el nombre e intenta de nuevo."
  - No CTA button (user can simply search again)

- **Error (network/server)**:
  - Toast error: "No se pudo consultar la disponibilidad. Verifica tu conexión e intenta de nuevo."
  - The error message also appears inline below the form as fallback: `--color-feedback-error`, icon `AlertCircle`

- **Validation error (empty input)**:
  - Inline error below the input field: "Ingresa el nombre del libro para buscar."
  - `--color-feedback-error` text, `aria-describedby` linking error to input
  - Error shown only after form submit attempt or field blur, never on pristine field

**Responsive**:
- Desktop (≥ 1024px): default layout above. Search form card has `max-width: 800px`, centered in content area.
- No mobile/tablet layouts required.

### 5.4 Feature Components

#### `LoanSearch`

**File**: `src/components/LoanSearch.jsx`
**Styles**: `src/components/LoanSearch.module.css`

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| (none — self-contained) | — | — | Component manages its own state via `useLoan` hook |

**Visual states**: initial, loading, results-found, no-results, error, validation-error

**Rules**:
- Must use `useLoan` hook for all data fetching — never call services directly
- The search input must have `id="bookName"` with a `<label htmlFor="bookName">`
- Search is triggered on form submit (Enter key or button click), never on input change
- The "Limpiar" button must reset both the input and the results section
- The results table must use `--text-code` with `font-variant-numeric: tabular-nums` for `id_book` and `loan_id` columns
- Status badges must include both icon and text — never color alone
- All hex color references in CSS must use `var(--token-name)` — no raw hex values

### 5.5 Data Display

| Data type | Format | Null display |
|-----------|--------|--------------|
| id_book | Raw string, left-aligned, `--text-code` | — (never null in results) |
| loan_id | Integer, right-aligned, `--text-code` | "—" (em dash) |
| status | Badge with icon + text (see 5.3 badge rules) | "Sin historial — Disponible" badge |
| Book name (in summary) | Original casing from user input, wrapped in single quotes | — |

### 5.6 Interaction Patterns

| Trigger | Component | Behavior |
|---------|-----------|----------|
| Form submit (click "Buscar" or press Enter) | LoanSearch form | Validate input not empty → show loading → call `searchByName(name)` → display results or error |
| Click "Limpiar" | Secondary button | Reset input to empty, hide results section, remove any error state |
| Empty input submit | Form | Show inline validation error "Ingresa el nombre del libro para buscar." below input |
| Network error | Any async action | Toast error (bottom-right): "No se pudo consultar la disponibilidad. Verifica tu conexión e intenta de nuevo." + inline error fallback |
| Type in search input | Input field | Update local state only — no search triggered until submit |

### 5.7 Accessibility Checklist

- [x] Search input has visible `<label htmlFor="bookName">` with text "Nombre del Libro"
- [x] Search input has associated `<label>` via `htmlFor` / `id` pairing
- [x] Every interactive element has visible focus state using `--shadow-focus`
- [x] Status badges use icon + text, never color alone
- [x] Error messages use `aria-describedby` referencing the input field
- [x] Loading state uses `aria-live="polite"` region
- [x] Results table uses proper `<table>`, `<thead>`, `<th>`, `<tbody>` semantic markup
- [x] Color contrast meets WCAG AA (4.5:1 body, 3:1 large text) — verified against warm palette
- [x] "Buscar" and "Limpiar" buttons have minimum 40×40px click area
- [x] Form can be fully operated via keyboard (Tab, Enter, Escape)

### 5.8 Motion

| Element | Animation | Duration | Easing | Reduced motion |
|---------|-----------|----------|--------|----------------|
| Results table appear | fade-in | 150ms | ease-out `cubic-bezier(0, 0, 0.2, 1)` | none (instant) |
| Loading spinner (in button) | rotate 360° | 800ms linear infinite | linear | static icon |
| Error message appear | fade-in | 100ms | ease-out | none (instant) |
| Toast enter | slide-up + fade | 200ms | ease-out | none (instant) |
| Toast exit | fade-out | 150ms | ease-in | none (instant) |

**Rule**: All animations must be wrapped in `@media (prefers-reduced-motion: no-preference)`.

---

## 5. Feature: HU-02 — Registrar Préstamo de un Libro a un Lector Habilitado

> Sections 1–4 are global. This section is added per feature and must not modify sections above.

### 5.1 Page Map

| Route | Component file | Layout | Auth required |
|-------|---------------|--------|---------------|
| `/loan` | `src/pages/LoanCombinedPage.jsx` | PageLayout (maxWidth="lg") | no |

The loan registration form is integrated into the combined loan page at `/loan`, below the `LoanSearch` component. It does not introduce a new route.

### 5.2 Navigation Changes

None. The `/loan` route already exists in the navigation configuration. The `LoanForm` component is rendered within `LoanCombinedPage` below the search section.

### 5.3 Screen Specifications

#### Screen: Registrar Préstamo de Libro (integrated in `/loan`)

**Purpose**: Allow the librarian to register a loan for an available book to a reader without pending debt, selecting a return term and previewing the due date.

**Layout**:
```
PageLayout (title="Sistema de Préstamos", maxWidth="lg")
└── Section: Registrar Préstamo
    ├── Heading "Registrar Préstamo de Libro" (--text-heading-md)
    └── FormCard (card: --color-surface-raised, --shadow-sm, --radius-lg, max-width: 640px)
        ├── Fieldset: Información del Libro
        │   ├── FormField (label="ID del Libro", input type="text", required)
        │   └── FormField (label="Título del Libro", input type="text", required)
        ├── Fieldset: Información del Lector
        │   ├── FormField (label="Tipo de Identificación", select: CI|DNI, required)
        │   ├── FormField (label="Número de Identificación", input type="text", required)
        │   └── FormField (label="Nombre del Lector", input type="text", required)
        ├── Fieldset: Información del Préstamo
        │   ├── RadioGroup (label="Plazo del Préstamo", options: 7|14|21 días)
        │   └── DueDatePreview (read-only computed display)
        ├── BusinessErrorAlert (conditional, inline)
        └── ButtonGroup
            ├── Button (variant="primary", label="Registrar Préstamo", icon=BookMarked)
            └── Button (variant="secondary", label="Limpiar")
```

**Form fields specification**:

| Field | ID | Type | Placeholder | Validation | Required |
|-------|----|------|-------------|------------|----------|
| ID del Libro | `idBook` | text | "Ej: BOOK-001" | Non-empty, trimmed | yes |
| Título del Libro | `title` | text | "Ej: Cien años de soledad" | Non-empty, trimmed | yes |
| Tipo de Identificación | `typeIdReader` | select | — (default: DNI) | Must be CI or DNI | yes |
| Número de Identificación | `idReader` | text | "Ej: 1023456789" | Non-empty, trimmed | yes |
| Nombre del Lector | `nameReader` | text | "Ej: Juan García" | Non-empty, trimmed | yes |
| Plazo del Préstamo | `loanDays` | radio group | — (default: 7) | Must be 7, 14, or 21 | yes |

**Loan days — radio group visual (MANDATORY pattern)**:
- Must be rendered as a `RadioGroup` component (Section 4.10) with 3 options
- Never use a `<select>` dropdown for this field — this is a forbidden pattern per requirements
- Options:
  | Value | Label | Description |
  |-------|-------|-------------|
  | `"7"` | "7 días" | "1 semana" |
  | `"14"` | "14 días" | "2 semanas" |
  | `"21"` | "21 días" | "3 semanas" |
- Default selection: `"7"`
- Layout: horizontal (3 options, fits single row)

**Due date preview**:
- Displayed as a read-only computed value below the radio group
- Format: `DD/MM/YYYY` — using `--text-code` with `font-variant-numeric: tabular-nums`
- Label: "Fecha Límite de Devolución" (`--text-label-md`)
- Value container: `--color-surface-overlay` background, `--radius-md`, `--space-4` padding
- Helper text: "(Calculado automáticamente)" in `--text-body-sm`, `--color-text-secondary`
- Updates instantly when the user changes the loan days selection — no submit required
- Computed as: `today + loan_days` (local date, no timezone conversion)

**States**:

- **Initial (empty form)**:
  - All text fields are empty with their placeholder text visible
  - Tipo de Identificación defaults to "DNI"
  - Plazo del Préstamo defaults to "7 días" (first option pre-selected)
  - Due date preview shows the computed date for 7 days from today
  - No error messages visible
  - "Registrar Préstamo" button is enabled

- **Filling (user entering data)**:
  - Fields accept input normally
  - Due date preview updates reactively when loan days selection changes
  - No validation errors shown until field blur (for touched fields) or form submit

- **Loading (form submitted)**:
  - "Registrar Préstamo" button shows loading state (spinner replacing label text, displays "Registrando...")
  - All form fields are disabled during submission
  - "Limpiar" button is disabled during submission
  - `aria-live="polite"` region announces "Registrando préstamo..."

- **Success**:
  - Toast notification (success, bottom-right, 4000ms auto-dismiss): "Préstamo registrado exitosamente"
  - If the API returns `loan_id`, the toast includes: "ID del préstamo: {loan_id}"
  - Form resets to initial state (all fields cleared, defaults restored)
  - Focus returns to the first field (`idBook`)

- **Business error — BOOK_NOT_AVAILABLE (409)**:
  - Inline alert inside the form (NOT a toast), between the last fieldset and the button group
  - Alert style: `--color-feedback-error` left border (4px), light error background, `--color-feedback-error` text
  - Icon: `CircleX` (20px) inline with message
  - Message: "El libro no está disponible para préstamo. Verifica que el libro haya sido devuelto antes de continuar."
  - Form fields remain enabled for correction
  - Alert is dismissed when user modifies the `idBook` field

- **Business error — READER_HAS_DEBT (409)**:
  - Inline alert inside the form (same position and style as BOOK_NOT_AVAILABLE)
  - Icon: `AlertCircle` (20px) inline with message
  - Message: "El lector tiene una deuda pendiente por multa. Registra el pago de la multa antes de continuar."
  - Form fields remain enabled for correction
  - Alert is dismissed when user modifies the `idReader` field

- **Validation error — INVALID_PAYLOAD (400)**:
  - Per-field inline errors below each invalid field
  - Error style: `--text-body-sm`, `--color-feedback-error`, linked via `aria-describedby`
  - Error messages:
    - Empty required field: "Este campo es obligatorio."
    - Invalid `id_book`: "Ingresa un ID de libro válido."
    - Invalid `id_reader`: "Ingresa un número de identificación válido."
  - Errors shown after blur on dirtied fields, or on form submit for all fields
  - Submit button remains enabled (user may fix and retry)

- **Validation error — INVALID_LOAN_DAYS (400)** (defensive — UI prevents this):
  - The RadioGroup constraint makes this error impossible from UI
  - If received from API (defensive), show inline alert: "El plazo seleccionado no es válido. Selecciona 7, 14 o 21 días."

- **System error (500 / network failure)**:
  - Toast error (bottom-right, 8000ms, requires manual dismiss): "No se pudo registrar el préstamo. Verifica tu conexión e intenta de nuevo."
  - Inline error fallback below the form as secondary indicator
  - Form fields remain enabled, user data is preserved

**Responsive**:
- Desktop (≥ 1024px): FormCard has `max-width: 640px`, left-aligned within content area
- Radio group options remain horizontal at all supported widths (3 compact options)
- No mobile/tablet layouts required

### 5.4 Feature Components

#### `LoanForm`

**File**: `src/components/LoanForm.jsx`
**Styles**: `src/components/LoanForm.module.css`

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| (none — self-contained) | — | — | Component manages its own state via `useLoan` hook |

**Visual states**: initial, filling, loading, success, business-error, validation-error, system-error

**Fieldset structure**:
- 3 `<fieldset>` elements, each with a styled `<legend>`
- Legend text uses `--text-label-md`, `--color-text-primary`, weight 600
- Fieldset border: 1px solid `--color-border-default`, `--radius-md`
- Fieldset padding: `--space-4`
- Gap between fieldsets: `--space-6`

**Rules**:
- Must use `useLoan` hook for form submission — never call services directly
- Every input must have `id` attribute matching the field ID in the table above, with a `<label htmlFor>` pair
- Loan days must use RadioGroup component (Section 4.10) — never `<select>` dropdown
- Due date preview must use `--text-code` token for the date value
- Business error alerts (BOOK_NOT_AVAILABLE, READER_HAS_DEBT) must be rendered inline within the form — never as toast
- Success confirmation must use toast notification — never inline
- System errors must use toast + inline fallback
- On success, form must reset to initial state and move focus to `idBook` input
- All hex color references in CSS must use `var(--token-name)` — no raw hex values
- All spacing must use `var(--space-*)` tokens — no arbitrary pixel values

### 5.5 Data Display

| Data type | Format | Null display |
|-----------|--------|-------------- |
| id_book | Raw string, left-aligned | — (required field, never null in form) |
| loan_days | Radio selection display ("7 días", "14 días", "21 días") | — (always has default) |
| date_limit | `DD/MM/YYYY`, `--text-code`, `tabular-nums` | — (always computed) |
| loan_id (in success toast) | Integer | — (may be absent; toast shows simpler message) |
| Error messages | Domain language, never technical codes | — |

### 5.6 Interaction Patterns

| Trigger | Component | Behavior |
|---------|-----------|----------|
| Form submit (click "Registrar Préstamo" or press Enter) | LoanForm | Validate all fields → show loading → call `createLoan(data)` → success toast + reset OR error inline/toast |
| Click "Limpiar" | Secondary button | Reset all fields to defaults, clear all error states, move focus to `idBook` |
| Change loan days radio | RadioGroup | Update `loanDays` state → due date preview recalculates instantly |
| Blur on required text field (after dirtied) | FormField | Validate non-empty → show/hide inline error |
| Modify `idBook` after BOOK_NOT_AVAILABLE error | Input field | Dismiss the business error alert |
| Modify `idReader` after READER_HAS_DEBT error | Input field | Dismiss the business error alert |
| Network error during submit | Any async action | Toast error (bottom-right): "No se pudo registrar el préstamo. Verifica tu conexión e intenta de nuevo." + inline fallback |
| Successful submit | Form | Toast success → form reset → focus to `idBook` |

### 5.7 Accessibility Checklist

- [x] Every text input has a visible `<label>` with `htmlFor`/`id` pairing
- [x] Loan days uses `<fieldset>` + `<legend>` wrapping `<input type="radio">` elements
- [x] Every interactive element has visible focus state using `--shadow-focus`
- [x] Business error alerts use `role="alert"` for screen reader announcement
- [x] Validation error messages use `aria-describedby` referencing the input field
- [x] Loading state uses `aria-live="polite"` region
- [x] Type of identification `<select>` has associated `<label>`
- [x] Color contrast meets WCAG AA (4.5:1 body, 3:1 large text)
- [x] All buttons have minimum 40×40px click area
- [x] Radio group options have minimum 40px height click area
- [x] Form can be fully operated via keyboard (Tab between fields, Space/Enter to select radio, Enter to submit)
- [x] Success toast is announced by screen reader via `aria-live` region
- [x] On success reset, focus is programmatically moved to first field for efficiency

### 5.8 Motion

| Element | Animation | Duration | Easing | Reduced motion |
|---------|-----------|----------|--------|----------------|
| Business error alert appear | fade-in + slide-down (8px) | 150ms | ease-out `cubic-bezier(0, 0, 0.2, 1)` | none (instant) |
| Business error alert dismiss | fade-out | 100ms | ease-in | none (instant) |
| Loading spinner (in button) | rotate 360° | 800ms linear infinite | linear | static icon |
| Due date value change | number cross-fade | 100ms | ease-out | none (instant) |
| Toast enter | slide-up + fade | 200ms | ease-out | none (instant) |
| Toast exit | fade-out | 150ms | ease-in | none (instant) |
| Radio option select | border color transition | 100ms | ease-out | none (instant) |

**Rule**: All animations must be wrapped in `@media (prefers-reduced-motion: no-preference)`.

---

## 5. Feature: HU-03 — Registrar Devolución de un Libro Dentro del Plazo

> Sections 1–4 are global. This section is added per feature and must not modify sections above.

### 5.1 Page Map

| Route | Component file | Layout | Auth required |
|-------|---------------|--------|---------------|
| `/return` | `src/pages/ReturnPage.jsx` | PageLayout (maxWidth="md") | no |

### 5.2 Navigation Changes

None. The `/return` route already exists in the navigation configuration:
```
{ label: 'Registrar Devolución', href: '/return', icon: BookOpen }
```

### 5.3 Screen Specifications

#### Screen: Registrar Devolución de Libro (`/return`)

**Purpose**: Allow the librarian to register the return of a book that was returned on or before the due date, closing the loan without generating a fine.

**Layout**:
```
PageLayout (title="Registrar Devolución", maxWidth="md")
└── Section: Formulario de Devolución
    ├── Heading "Registrar Devolución de Libro" (--text-heading-md)
    └── FormCard (card: --color-surface-raised, --shadow-sm, --radius-lg, max-width: 640px)
        ├── Fieldset: Identificación del Préstamo
        │   ├── FormField (label="ID del Libro", input type="text", optional*)
        │   ├── FormField (label="Título del Libro", input type="text", optional*, disabled if id_book empty)
        │   ├── FormField (label="Tipo de Identificación", select: CI|DNI, required)
        │   └── FormField (label="Número de Identificación del Lector", input type="text", optional*)
        ├── Fieldset: Información de Devolución
        │   └── FormField (label="Fecha de Devolución", input type="date", required)
        ├── BusinessErrorAlert (conditional, inline)
        └── ButtonGroup
            ├── Button (variant="primary", label="Registrar Devolución", icon=BookOpen)
            └── Button (variant="secondary", label="Limpiar")
```

*\* At least one of "ID del Libro" or "Número de Identificación del Lector" must be provided. This is a smart-search pattern — the system locates the active loan using whichever identifiers are supplied.*

**Form fields specification**:

| Field | ID | Type | Placeholder | Validation | Required |
|-------|----|------|-------------|------------|----------|
| ID del Libro | `idBook` | text | "Ej: BOOK-001" | Trimmed; at least one of `idBook` or `idReader` must be non-empty | conditional |
| Título del Libro | `title` | text | "Ej: Cien años de soledad" | Trimmed; disabled when `idBook` is empty | no |
| Tipo de Identificación | `typeIdReader` | select | — (default: CI) | Must be CI or DNI | yes |
| Número de Identificación | `idReader` | text | "Ej: 1023456789" | Trimmed; at least one of `idBook` or `idReader` must be non-empty | conditional |
| Fecha de Devolución | `dateReturn` | date | — (browser date picker) | Must be a valid date; must not be in the future | yes |

**Smart-search helper text**:
- Below the "Identificación del Préstamo" fieldset legend, display a helper text line:
  "Ingresa al menos el ID del libro o la identificación del lector para localizar el préstamo."
- Style: `--text-body-sm`, `--color-text-secondary`, icon `Search` (16px) inline

**Title field behavior**:
- When `idBook` is empty, the `title` field must be visually disabled (`--color-surface-overlay` background, `--color-text-disabled` text, `cursor: not-allowed`)
- When `idBook` has a value, the `title` field becomes enabled as an optional refinement
- Helper text below title: "Opcional. Refina la búsqueda si hay múltiples copias." (`--text-body-sm`, `--color-text-secondary`)

**Date field constraints**:
- Must use the native `<input type="date">` — never a text field for date entry (H5: Error Prevention)
- The `max` attribute must be set to today's date to prevent future dates
- Display format follows browser locale; stored as ISO 8601 (`YYYY-MM-DD`)

**States**:

- **Initial (empty form)**:
  - All text fields are empty with placeholder text visible
  - Tipo de Identificación defaults to "CI"
  - Title field is disabled (idBook is empty)
  - Date field is empty; max attribute set to today
  - No error messages visible
  - "Registrar Devolución" button is enabled

- **Filling (user entering data)**:
  - Fields accept input normally
  - Title field enables/disables reactively based on idBook value
  - No validation errors shown until field blur (for touched fields) or form submit

- **Loading (form submitted)**:
  - "Registrar Devolución" button shows loading state (spinner replacing label text, displays "Registrando...")
  - All form fields are disabled during submission
  - "Limpiar" button is disabled during submission
  - `aria-live="polite"` region announces "Registrando devolución..."

- **Success — return on time (200, no fine)**:
  - Toast notification (success, bottom-right, 4000ms auto-dismiss): "Devolución registrada exitosamente. No se generó multa."
  - Form resets to initial state (all fields cleared, defaults restored)
  - Focus returns to the first field (`idBook`)

- **Business error — LOAN_NOT_FOUND (404)**:
  - Inline alert inside the form (between the last fieldset and the button group)
  - Alert style: `--color-feedback-info` left border (4px), light info background, `--color-feedback-info` text
  - Icon: `Search` (20px) inline with message
  - Message: "No se encontró un préstamo activo con los datos proporcionados. Verifica el ID del libro o la identificación del lector."
  - Form fields remain enabled for correction
  - Alert is dismissed when user modifies `idBook` or `idReader` field

- **Business error — ALREADY_RETURNED (409)**:
  - Inline alert inside the form (same position as LOAN_NOT_FOUND)
  - Alert style: `--color-feedback-warning` left border (4px), light warning background, `--color-feedback-warning` text
  - Icon: `AlertCircle` (20px) inline with message
  - Message: "Este préstamo ya fue devuelto anteriormente. No es posible registrar la devolución nuevamente."
  - Form fields remain enabled for correction
  - Alert is dismissed when user modifies `idBook` or `idReader` field

- **Validation error — INVALID_PAYLOAD (400)**:
  - Per-field inline errors below each invalid field
  - Error style: `--text-body-sm`, `--color-feedback-error`, linked via `aria-describedby`
  - Error messages:
    - Neither idBook nor idReader provided: "Ingresa al menos el ID del libro o la identificación del lector." (shown below both fields)
    - Empty date_return: "Este campo es obligatorio."
    - Future date_return: "La fecha de devolución no puede ser posterior a hoy."
    - Invalid type_id_reader: "Selecciona un tipo de identificación válido."
  - Errors shown after blur on dirtied fields, or on form submit for all fields
  - Submit button remains enabled (user may fix and retry)

- **System error (500 / network failure)**:
  - Toast error (bottom-right, 8000ms, requires manual dismiss): "No se pudo registrar la devolución. Verifica tu conexión e intenta de nuevo."
  - Inline error fallback below the form as secondary indicator
  - Form fields remain enabled, user data is preserved

**Responsive**:
- Desktop (≥ 1024px): FormCard has `max-width: 640px`, left-aligned within content area
- No mobile/tablet layouts required

### 5.4 Feature Components

#### `ReturnForm`

**File**: `src/components/ReturnForm.jsx`
**Styles**: `src/components/ReturnForm.module.css`

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| (none — self-contained) | — | — | Component manages its own state via `useLoan` hook |

**Visual states**: initial, filling, loading, success, loan-not-found, already-returned, validation-error, system-error

**Fieldset structure**:
- 2 `<fieldset>` elements, each with a styled `<legend>`
- Legend text uses `--text-label-md`, `--color-text-primary`, weight 600
- Fieldset border: 1px solid `--color-border-default`, `--radius-md`
- Fieldset padding: `--space-4`
- Gap between fieldsets: `--space-6`

**Rules**:
- Must use `useLoan` hook for form submission via `returnLoan(data)` — never call services directly
- Every input must have `id` attribute matching the field ID in the table above, with a `<label htmlFor>` pair
- The `title` field must be disabled when `idBook` is empty — use `disabled` attribute, never hide the field
- The `dateReturn` field must use `<input type="date">` with `max` set to today — never a text input
- Business error alerts (LOAN_NOT_FOUND, ALREADY_RETURNED) must be rendered inline within the form — never as toast
- Success confirmation must use toast notification — never inline
- System errors must use toast + inline fallback
- On success, form must reset to initial state and move focus to `idBook` input
- The smart-search validation (at least one of idBook or idReader) must run on submit, not on individual field blur
- All hex color references in CSS must use `var(--token-name)` — no raw hex values
- All spacing must use `var(--space-*)` tokens — no arbitrary pixel values

### 5.5 Data Display

| Data type | Format | Null display |
|-----------|--------|--------------|
| id_book | Raw string, left-aligned | — (conditional field) |
| title | Raw string, left-aligned | — (optional field) |
| id_reader | Raw string, left-aligned | — (conditional field) |
| type_id_reader | "CI" or "DNI" from select | — (always has default) |
| date_return | `YYYY-MM-DD` (ISO 8601 via native date input) | — (required field) |
| Error messages | Domain language, never technical codes | — |

### 5.6 Interaction Patterns

| Trigger | Component | Behavior |
|---------|-----------|----------|
| Form submit (click "Registrar Devolución" or press Enter) | ReturnForm | Validate smart-search + date → show loading → call `returnLoan(data)` → success toast + reset OR error inline/toast |
| Click "Limpiar" | Secondary button | Reset all fields to defaults, clear all error states, move focus to `idBook` |
| Type in `idBook` field | Input field | If non-empty: enable `title` field. If empty: disable `title` field and clear its value |
| Blur on `dateReturn` (after dirtied) | FormField | Validate date is not empty and not in the future → show/hide inline error |
| Modify `idBook` or `idReader` after LOAN_NOT_FOUND or ALREADY_RETURNED error | Input field | Dismiss the business error alert |
| Submit with neither `idBook` nor `idReader` | Form | Show validation error: "Ingresa al menos el ID del libro o la identificación del lector." below both conditional fields |
| Submit with future `dateReturn` | Form | Show inline error below date field: "La fecha de devolución no puede ser posterior a hoy." |
| Network error during submit | Any async action | Toast error: "No se pudo registrar la devolución. Verifica tu conexión e intenta de nuevo." + inline fallback |
| Successful submit | Form | Toast success → form reset → focus to `idBook` |

### 5.7 Accessibility Checklist

- [x] Every text input has a visible `<label>` with `htmlFor`/`id` pairing
- [x] Search criteria fieldset uses `<fieldset>` + `<legend>` wrapping inputs
- [x] Date field uses native `<input type="date">` for built-in accessibility
- [x] Every interactive element has visible focus state using `--shadow-focus`
- [x] Disabled `title` field communicates disabled state via `--color-text-disabled` + `cursor: not-allowed` + native `disabled` attribute
- [x] Business error alerts use `role="alert"` for screen reader announcement
- [x] Validation error messages use `aria-describedby` referencing the input field
- [x] Loading state uses `aria-live="polite"` region
- [x] Type of identification `<select>` has associated `<label>`
- [x] Color contrast meets WCAG AA (4.5:1 body, 3:1 large text)
- [x] All buttons have minimum 40×40px click area
- [x] Form can be fully operated via keyboard (Tab between fields, Enter to submit)
- [x] Success toast is announced by screen reader via `aria-live` region
- [x] On success reset, focus is programmatically moved to first field for efficiency
- [x] Smart-search helper text is associated with the fieldset via `aria-describedby` on the `<fieldset>`

### 5.8 Motion

| Element | Animation | Duration | Easing | Reduced motion |
|---------|-----------|----------|--------|----------------|
| Business error alert appear | fade-in + slide-down (8px) | 150ms | ease-out `cubic-bezier(0, 0, 0.2, 1)` | none (instant) |
| Business error alert dismiss | fade-out | 100ms | ease-in | none (instant) |
| Loading spinner (in button) | rotate 360° | 800ms linear infinite | linear | static icon |
| Title field enable/disable transition | opacity + background-color | 100ms | ease-out | none (instant) |
| Toast enter | slide-up + fade | 200ms | ease-out | none (instant) |
| Toast exit | fade-out | 150ms | ease-in | none (instant) |

**Rule**: All animations must be wrapped in `@media (prefers-reduced-motion: no-preference)`.

---

## 5. Feature: HU-04 — Registrar Devolución Tardía y Generar Multa Fibonacci

> Sections 1–4 are global. This section is added per feature and must not modify sections above.

### 5.1 Page Map

| Route | Component file | Layout | Auth required |
|-------|---------------|--------|---------------|
| `/return` | `src/pages/ReturnPage.jsx` | PageLayout (maxWidth="md") | no |

This feature does not introduce a new route. It extends the existing `/return` page (HU-03) to handle late returns. When the backend detects `date_return > date_limit`, it calculates the Fibonacci fine and creates a `debt_reader` record. The frontend displays the fine breakdown via `DebtSummary`.

### 5.2 Navigation Changes

None. The `/return` route already exists in the navigation configuration:
```
{ label: 'Registrar Devolución', href: '/return', icon: BookOpen }
```

### 5.3 Screen Specifications

#### Screen: Registrar Devolución Tardía con Multa (`/return`)

**Purpose**: Allow the librarian to register the return of a book that was returned after the due date, automatically generating a Fibonacci-scaled fine and creating a debt record for the reader.

**Layout**:
```
PageLayout (title="Registrar Devolución", maxWidth="md")
└── Section: Formulario de Devolución
    ├── Heading "Registrar Devolución de Libro" (--text-heading-md)
    └── FormCard (card: --color-surface-raised, --shadow-sm, --radius-lg, max-width: 640px)
        ├── Fieldset: Identificación del Préstamo
        │   ├── FormField (label="ID del Libro", input type="text", conditional*)
        │   ├── FormField (label="Título del Libro", input type="text", optional, disabled if id_book empty)
        │   ├── FormField (label="Tipo de Identificación", select: CI|DNI, required)
        │   └── FormField (label="Número de Identificación del Lector", input type="text", conditional*)
        ├── Fieldset: Información de Devolución
        │   └── FormField (label="Fecha de Devolución", input type="date", required)
        ├── Fieldset: Configuración de Multa
        │   └── FormField (label="Base de Multa Fibonacci (unidad monetaria)", input type="number", required)
        ├── BusinessErrorAlert (conditional, inline)
        ├── ButtonGroup
        │   ├── Button (variant="primary", label="Registrar Devolución", icon=BookOpen)
        │   └── Button (variant="secondary", label="Limpiar")
        ├── SuccessAlert (conditional, inline, below buttons)
        └── DebtSummary (conditional, only on late return success)
```

*\* At least one of "ID del Libro" or "Número de Identificación del Lector" must be provided (same smart-search pattern as HU-03).*

**Form fields specification (additions and overrides vs HU-03)**:

All fields from HU-03 apply unchanged. This feature adds:

| Field | ID | Type | Placeholder | Validation | Required |
|-------|----|------|-------------|------------|----------|
| Base de Multa Fibonacci | `baseFibAmount` | number | "Ej: 1.00" | Must be a valid number ≥ 0.01; validated on blur and submit | yes |

**baseFibAmount field behavior**:
- Rendered inside its own `<fieldset>` with legend "Configuración de Multa"
- Default value: `"1"` (pre-populated, not empty)
- `min="0.01"`, `step="0.01"` attributes on the `<input type="number">`
- On blur: if empty → no error (cleared state); if non-empty → validate ≥ 0.01 and format to 2 decimal places
- On submit: field must be non-empty and ≥ 0.01; if invalid → inline error below the field
- Helper text (below input, always visible when no error): "Este valor se multiplica por las unidades Fibonacci para calcular la multa total." (`--text-body-sm`, `--color-text-secondary`)
- Error messages:
  - Empty on submit: "La multa base es obligatoria."
  - Non-numeric: "Debe ser un número válido."
  - Below threshold: "El valor debe ser igual o mayor a 0.01."
- Error style: `--text-body-sm`, `--color-feedback-error`, linked via `aria-describedby`
- The value is sent to the backend as `base_fib_amount` in the payload and multiplied by Fibonacci units to compute the total fine

**States (extending HU-03 states)**:

All HU-03 states apply. This feature adds and overrides:

- **Success — late return with fine (200, debt generated)**:
  - Inline success alert inside the form (below buttons): 
    - Alert style: `--color-feedback-success` left border (4px), light success background, `--color-feedback-success` text
    - Icon: `CheckCircle` (20px) inline with message
    - Message line 1: "Devolución registrada exitosamente."
    - Message line 2: "ID del préstamo: {loan_id}" (if returned by API)
  - `DebtSummary` component rendered below the success alert
  - Form does NOT reset automatically on late return — the librarian must see the fine details before clearing
  - "Limpiar" button resets the form and hides the DebtSummary

- **Success — on-time return (200, no fine)**: Same as HU-03 — toast success + form reset. DebtSummary is not shown.

- **Business error — DEBT_CREATION_ERROR (500)**:
  - Toast error (bottom-right, 8000ms, requires manual dismiss): "Se registró la devolución pero no se pudo crear la multa. Contacta al administrador."
  - Inline error fallback: `--color-feedback-error` left border, icon `AlertCircle` (20px)
  - Message: "Error al crear el registro de deuda. El préstamo fue devuelto pero la multa no se generó correctamente."
  - This is a partial-success scenario — the loan was closed but the debt record failed

- **Validation error — baseFibAmount invalid**:
  - Per-field inline error below the `baseFibAmount` input
  - Error shown on blur (after dirtied) and on submit
  - Submit button must not be disabled by baseFibAmount validation alone — the user can fix and retry

**Responsive**:
- Desktop (≥ 1024px): FormCard has `max-width: 640px`, left-aligned within content area
- No mobile/tablet layouts required

### 5.4 Feature Components

#### `DebtSummary`

**File**: `src/components/DebtSummary.jsx`
**Styles**: `src/components/DebtSummary.module.css`

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| `debt` | `{ days_late: number, units_fib: number, amount_debt: number, id_debt?: string }` | yes | Debt details object from backend response |

**Visual states**:
- **Visible**: When `debt` prop is provided and contains valid data
- **Hidden**: When `debt` is `null` or `undefined` (component returns `null`)

**Layout**:
```
DebtSummary (role="region", aria-label="Resumen de multa")
├── DebtHeader
│   ├── Icon: AlertCircle (24px, --color-feedback-warning)
│   ├── Heading "Resumen de Multa" (--text-heading-sm)
│   └── DescriptionText (--text-body-md, --color-text-primary)
│       "El libro fue devuelto {days_late} día(s) después de la fecha límite
│        (equivalente a {weeks} semana(s) completa(s))."
├── DebtDetails (card: --color-surface-raised, --radius-md, --space-4 padding)
│   ├── Row: "Días de retraso" → "{days_late} día(s)" (--text-body-md)
│   ├── Row: "Semanas completas" → "{weeks} semana(s)" (--text-body-md)
│   ├── Row: "Unidades Fibonacci acumuladas" → "{units_fib} unidad(es)" (--text-body-md)
│   └── Row (highlighted): "Monto de la multa" → "${amount_debt}" (--text-heading-sm, --color-feedback-error)
├── DebtReference (conditional, if id_debt exists)
│   └── "ID de deuda: {id_debt}" (--text-label-sm, --color-text-secondary)
└── DebtWarning
    ├── Icon: AlertTriangle (16px, --color-feedback-warning)
    └── Text: "Esta multa debe ser pagada antes de poder solicitar nuevos préstamos."
        (--text-body-sm, --color-feedback-warning)
```

**Visual specification**:
- Container: `--color-surface-raised` background, `--shadow-sm`, `--radius-lg`, `--space-6` padding
- Left border: 4px solid `--color-feedback-warning` (amber — signals pending debt, not error)
- Header border-bottom: 1px solid `--color-border-default`
- Detail rows: flex layout, `justify-content: space-between`, `--space-2` vertical gap
- Detail row separator: 1px solid `--color-border-default` (except last row)
- Amount row: highlighted with `--color-feedback-error` background at 8% opacity, `--radius-md`, `--space-4` padding
- Amount value: `--text-heading-sm`, `--color-feedback-error`, `font-variant-numeric: tabular-nums`
- Warning block: `--color-feedback-warning` left border (4px), `--color-feedback-warning` background at 8% opacity, `--radius-md`, `--space-4` padding
- All monetary values must use `toLocaleString('es-AR', { minimumFractionDigits: 2, maximumFractionDigits: 2 })` with `$` prefix
- All numeric labels must use proper singular/plural: "1 día" vs "2 días", "1 semana" vs "3 semanas", "1 unidad" vs "7 unidades"

**Rules**:
- Must never be used outside the return form context — it is a result display, not a standalone component
- Must not make any API calls — receives all data via props
- Must not use emoji in production (current implementation uses emoji — must replace with `lucide-react` icons)
- All hex color references in CSS must use `var(--token-name)` — no raw hex values
- All spacing must use `var(--space-*)` tokens — no arbitrary pixel values
- Heading must use `--text-heading-sm` token (Lora 600), never free-form `font-size`
- Labels must use `--text-label-md` token, values must use `--text-body-md` token
- Amount value must use `--text-heading-sm` for visual emphasis
- `role="region"` with `aria-label="Resumen de multa"` for screen reader landmark

#### `ReturnForm` (extensions for HU-04)

**File**: `src/components/ReturnForm.jsx` (same file as HU-03)

HU-04 extends the existing `ReturnForm` with:
1. A new `<fieldset>` "Configuración de Multa" containing the `baseFibAmount` field
2. Conditional rendering of `DebtSummary` after successful late return
3. Conditional success message distinguishing on-time vs late returns

**Additional rules for HU-04 behavior**:
- The `baseFibAmount` field must always be visible (not conditionally shown) — the librarian sets the base amount before submitting, regardless of whether the return will be late
- On late return success: render success alert + DebtSummary, do NOT reset form automatically
- On on-time return success: render toast success, reset form, do NOT show DebtSummary
- The `loanData` response from the hook determines which path: if `loanData.days_late > 0` → late return path; if `!loanData.days_late` or `days_late === 0` → on-time path
- Import `DebtSummary` only from `'./DebtSummary.jsx'` — never recreate the component inline

### 5.5 Data Display

| Data type | Format | Null display |
|-----------|--------|--------------|
| days_late | Integer, right-aligned, `--text-body-md` | — (never null when debt exists) |
| weeks (derived) | Integer, computed as `Math.floor((days_late - 1) / 7) + 1`, right-aligned | — (always computed) |
| units_fib | Integer, right-aligned, `--text-body-md` | — (never null when debt exists) |
| amount_debt | Currency: `$` prefix, 2 decimal places, right-aligned, `--text-heading-sm`, `tabular-nums` | — (never null when debt exists) |
| base_fib_amount (input) | Number, 2 decimal places after blur, `--text-body-md` | Default: "1" |
| id_debt | Raw string, `--text-label-sm`, `--color-text-secondary` | Hidden (no id_debt row shown) |
| Fibonacci formula explanation | Not displayed in UI — calculation is backend-only | — |

### 5.6 Interaction Patterns

| Trigger | Component | Behavior |
|---------|-----------|----------|
| Form submit with late return detected by backend | ReturnForm | Show inline success alert + render DebtSummary with fine breakdown; do NOT reset form |
| Form submit with on-time return detected by backend | ReturnForm | Toast success: "Devolución registrada exitosamente. No se generó multa." + reset form (same as HU-03) |
| Blur on `baseFibAmount` (after dirtied) | FormField | Validate: if empty → clear error; if non-empty and < 0.01 → error; if valid → format to 2 decimals |
| Submit with empty `baseFibAmount` | Form | Inline error below field: "La multa base es obligatoria." |
| Submit with `baseFibAmount` < 0.01 | Form | Inline error below field: "El valor debe ser igual o mayor a 0.01." |
| Click "Limpiar" after late return success | Secondary button | Reset all fields to defaults, hide DebtSummary, hide success alert, move focus to `idBook` |
| Backend returns DEBT_CREATION_ERROR (500) | Any async action | Toast error: "Se registró la devolución pero no se pudo crear la multa. Contacta al administrador." + inline error fallback |
| Form submit with errors (validation) | Form | Scroll to first error, focus first invalid field |

### 5.7 Accessibility Checklist

- [x] `baseFibAmount` input has visible `<label htmlFor="baseFibAmount">` with text "Base de Multa Fibonacci (unidad monetaria)"
- [x] `baseFibAmount` error messages use `aria-describedby` referencing the input field
- [x] `baseFibAmount` fieldset uses `<fieldset>` + `<legend>` wrapping the input
- [x] `DebtSummary` uses `role="region"` with `aria-label="Resumen de multa"` for screen reader landmark
- [x] Monetary amount in DebtSummary uses `font-variant-numeric: tabular-nums` for proper numeric rendering
- [x] Warning message in DebtSummary uses icon + text, never color alone
- [x] DebtSummary heading uses proper `<h3>` semantic heading (within the form's heading hierarchy)
- [x] Late return success alert uses `role="alert"` for screen reader announcement
- [x] All colors in DebtSummary reference tokens — no raw hex values
- [x] All interactive elements have visible focus state using `--shadow-focus`
- [x] Color contrast meets WCAG AA (4.5:1 body, 3:1 large text) — verified against warm palette
- [x] Form can be fully operated via keyboard (Tab between fields, Enter to submit)

### 5.8 Motion

| Element | Animation | Duration | Easing | Reduced motion |
|---------|-----------|----------|--------|----------------|
| DebtSummary appear (after late return success) | fade-in + slide-down (12px) | 200ms | ease-out `cubic-bezier(0, 0, 0.2, 1)` | none (instant) |
| Success alert appear | fade-in + slide-down (8px) | 150ms | ease-out `cubic-bezier(0, 0, 0.2, 1)` | none (instant) |
| baseFibAmount error appear | fade-in | 100ms | ease-out | none (instant) |
| Loading spinner (in button) | rotate 360° | 800ms linear infinite | linear | static icon |
| Toast enter | slide-up + fade | 200ms | ease-out | none (instant) |
| Toast exit | fade-out | 150ms | ease-in | none (instant) |

**Rule**: All animations must be wrapped in `@media (prefers-reduced-motion: no-preference)`.

---

## 5. Feature: HU-05 — Consultar Préstamos Vencidos y Lector Responsable

> Sections 1–4 are global. This section is added per feature and must not modify sections above.

### 5.1 Page Map

| Route | Component file | Layout | Auth required |
|-------|---------------|--------|---------------|
| `/loans/overdue` | `src/pages/OverduePage.jsx` | PageLayout (maxWidth="lg") | no |

**Route inconsistency**: The current navigation in `Navigation.jsx` links to `/loans/outTime`. The spec defines `/loans/overdue` as the canonical route. The frontend agent must update the navigation item `href` and the `App.jsx` route definition to `/loans/overdue`. This design spec uses the spec route `/loans/overdue` as source of truth.

### 5.2 Navigation Changes

Update existing entry in `Navigation.jsx`:

```js
// Before:
{ label: 'Préstamos Vencidos', href: '/loans/outTime', icon: AlertTriangle }

// After:
{ label: 'Préstamos Vencidos', href: '/loans/overdue', icon: AlertTriangle }
```

No new navigation items are required. The icon `AlertTriangle` and label remain unchanged.

### 5.3 Screen Specifications

#### Screen: Préstamos Vencidos (`/loans/overdue`)

**Purpose**: List all overdue loans (not yet returned, past due date) showing book and responsible reader information for operational debt management.

**Layout**:
```
PageLayout (title="Préstamos Vencidos", maxWidth="lg")
├── HeaderSection
│   ├── Heading "Préstamos Vencidos" (--text-display-lg)
│   └── Subtitle "Libros fuera de plazo y lector responsable" (--text-body-md, --color-text-secondary)
├── SummaryBar (conditional, visible only when data exists)
│   └── Text: "Total de préstamos vencidos: {count}" (--text-label-md, --color-text-secondary)
└── ContentSection
    └── OverdueLoansTable | EmptyState | SkeletonState | ErrorState
```

**Table specification**:

The table must display overdue loans using the following columns. Column count is 6, within the recommended ≤ 8 guideline.

| Column Header | Data Field(s) | Alignment | Typography | Content |
|---------------|--------------|-----------|------------|---------|
| ID Préstamo | `loan_id` | right | `--text-code`, `tabular-nums` | Integer ID |
| Libro | `id_book` + `title` | left | ID: `--text-label-sm`, `--color-text-secondary`; Title: `--text-body-md` | Two-line composite cell: title first line, `id_book` second line |
| Lector Responsable | `name_reader` + `type_id_reader` + `id_reader` | left | Name: `--text-body-md`; ID: `--text-label-sm`, `--color-text-secondary` | Two-line composite cell: name first line, `type_id_reader id_reader` second line |
| Estado | `state` (badge) | center | `--text-label-sm` | Always "Vencido" badge (see badge rules below) |
| Fecha Límite | `date_limit` | left | `--text-code`, `tabular-nums` | Formatted date `DD/MM/YYYY` |
| Días de Atraso | computed: `today - date_limit` | right | `--text-body-md`, `--color-feedback-error`, weight 600 | Integer with "día(s)" suffix |

**Status badge**:
- All records in this view are overdue by definition (`state=ON_LOAN` and `date_limit < today`)
- Badge: `--color-feedback-error` background at 12% opacity, `--color-feedback-error` text, `--radius-sm`
- Icon: `AlertTriangle` (14px) inline left of text
- Text: "Vencido"
- Badge must always include icon + text — never color alone

**"Días de Atraso" computed column**:
- Computed on the frontend: `Math.ceil((new Date() - new Date(date_limit)) / (1000 * 60 * 60 * 24))`
- Displayed as integer with singular/plural suffix: "1 día" vs "N días"
- Highlighted in `--color-feedback-error` with weight 600 for operational urgency
- Right-aligned per numeric column convention

**Table row styling**:
- Default row height: 56px (comfortable density for composite cells)
- Row separator: 1px solid `--color-border-default`
- Hover: `--color-surface-overlay` background
- No row click action (read-only consultation)
- Proper semantic markup: `<table>`, `<thead>`, `<th scope="col">`, `<tbody>`, `<tr>`, `<td>`

**States**:

- **Loading (initial page load)**:
  - Skeleton screen replicating the table layout: 5 skeleton rows
  - Each skeleton row: 6 cells matching column widths, using `--color-surface-overlay` fill with animated shimmer
  - `aria-live="polite"` region with `aria-label="Cargando préstamos vencidos"`
  - Summary bar hidden during loading

- **Data — overdue loans found**:
  - Summary bar visible: "Total de préstamos vencidos: {count}" (`--text-label-md`, `--color-text-secondary`)
  - Count value uses `--text-label-md` weight 600 for emphasis
  - Table rendered with all overdue records
  - No sorting, filtering, or pagination controls (excluded from MVP per spec)

- **Empty — no overdue loans**:
  - Icon: `BookOpen` (48px, `--color-text-secondary`)
  - Heading: "No hay préstamos vencidos" (`--text-heading-sm`, `--color-text-primary`)
  - Supporting text: "Todos los préstamos están dentro del plazo o ya fueron devueltos." (`--text-body-md`, `--color-text-secondary`)
  - No CTA button (this is a read-only consultation page)
  - Container: centered vertically and horizontally within the content area, `--space-16` vertical padding

- **Error (network/server)**:
  - Icon: `AlertCircle` (48px, `--color-feedback-error`)
  - Heading: "No se pudieron cargar los préstamos vencidos" (`--text-heading-sm`, `--color-text-primary`)
  - Supporting text: "Verifica tu conexión e intenta de nuevo." (`--text-body-md`, `--color-text-secondary`)
  - CTA: Button (variant="secondary", label="Reintentar", icon=`RefreshCw`)
  - Toast error (bottom-right, 8000ms, requires manual dismiss): "Error al consultar préstamos vencidos. Verifica tu conexión."
  - Container: same centering as empty state

**Responsive**:
- Desktop (≥ 1024px): Table at full content width within `maxWidth="lg"` (1280px). Composite cells use two-line layout for readable information density.
- No mobile/tablet layouts required.

### 5.4 Feature Components

#### `OverdueLoansTable`

**File**: `src/components/OverdueLoansTable.jsx`
**Styles**: `src/components/OverdueLoansTable.module.css`

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| (none — self-contained) | — | — | Component manages its own data fetch via `useLoan` hook |

**Visual states**: loading, data, empty, error

**Rules**:
- Must use `useLoan` hook with `getOverdue()` for data fetching — never call services directly
- Data fetch must trigger on component mount via `useEffect`
- Must use proper `<table>` semantic HTML with `<thead>`, `<th scope="col">`, `<tbody>`
- Composite cells (Libro, Lector) must use a flex container with two lines — never concatenate into a single string
- "Días de Atraso" column must be computed on the frontend from `date_limit` — never depend on a backend field
- Status badges must use `lucide-react` `AlertTriangle` icon (14px) + text "Vencido" — never raw `state` value
- All hex color references in CSS must use `var(--token-name)` — no raw hex values
- All spacing must use `var(--space-*)` tokens — no arbitrary pixel values
- Summary count must update to reflect the actual number of rows rendered
- Empty state must use `lucide-react` icons, not emoji
- Error state must include a "Reintentar" button that re-triggers `getOverdue()`
- Loading skeleton must replicate the exact table layout with 5 placeholder rows

#### `OverduePage`

**File**: `src/pages/OverduePage.jsx`
**Styles**: `src/pages/OverduePage.module.css`

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| (none — route-level page) | — | — | Renders OverdueLoansTable within PageLayout |

**Rules**:
- Must wrap content in `PageLayout` with `title="Préstamos Vencidos"` and `maxWidth="lg"`
- Must not contain any data-fetching logic — delegates entirely to `OverdueLoansTable`
- Page heading uses `--text-display-lg` (Lora 600, 30px)
- Subtitle uses `--text-body-md`, `--color-text-secondary`

### 5.5 Data Display

| Data type | Format | Null display |
|-----------|--------|--------------|
| loan_id | Integer, right-aligned, `--text-code`, `tabular-nums` | — (never null in overdue results) |
| id_book | Raw string, `--text-label-sm`, `--color-text-secondary` | — (never null) |
| title | Raw string, `--text-body-md`, left-aligned, truncated at 40 chars with `text-overflow: ellipsis` | — (never null) |
| name_reader | Raw string, `--text-body-md`, left-aligned | — (never null) |
| type_id_reader | "CI" or "DNI", `--text-label-sm`, `--color-text-secondary` | — (never null) |
| id_reader | Raw string, `--text-label-sm`, `--color-text-secondary` | — (never null) |
| state | Badge: "Vencido" with `AlertTriangle` icon + `--color-feedback-error` | — (always ON_LOAN) |
| date_limit | `DD/MM/YYYY` via `toLocaleDateString('es-ES')`, `--text-code`, `tabular-nums` | — (never null) |
| date_return | Not displayed as column — always null for overdue loans; absence implied by presence in list | "—" (em dash) if shown |
| days_overdue (computed) | Integer + " día(s)" suffix, right-aligned, `--color-feedback-error`, weight 600 | — (always computable) |
| count (summary) | Integer, `--text-label-md`, weight 600 | "0" |

### 5.6 Interaction Patterns

| Trigger | Component | Behavior |
|---------|-----------|----------|
| Page mount | OverdueLoansTable | Trigger `getOverdue()` → show skeleton → render table or empty/error state |
| Click "Reintentar" (error state) | Error CTA button | Re-trigger `getOverdue()` → show skeleton → attempt data load again |
| Network error during load | OverdueLoansTable | Render error state with icon + heading + supporting text + "Reintentar" button. Show toast error: "Error al consultar préstamos vencidos. Verifica tu conexión." |
| Navigate to `/loans/overdue` | Router | Mount OverduePage → mount OverdueLoansTable → fetch data |

**No user-initiated data mutations exist on this page.** This is a read-only consultation view per the spec.

### 5.7 Accessibility Checklist

- [x] Table uses semantic `<table>`, `<thead>`, `<th scope="col">`, `<tbody>` markup
- [x] Every `<th>` has `scope="col"` for screen reader column association
- [x] Status badge uses icon (`AlertTriangle`) + text ("Vencido") — never color alone
- [x] Every interactive element (Reintentar button) has visible focus state using `--shadow-focus`
- [x] Loading state uses `aria-live="polite"` with `aria-label="Cargando préstamos vencidos"`
- [x] Empty state heading uses proper `<h3>` semantic heading within page hierarchy
- [x] Error state heading uses proper `<h3>` semantic heading within page hierarchy
- [x] Color contrast meets WCAG AA (4.5:1 body, 3:1 large text) — verified against warm palette
- [x] "Reintentar" button has minimum 40×40px click area
- [x] Table row hover uses `--color-surface-overlay` — sufficient contrast against text
- [x] Composite cells use visually distinct lines but remain within a single `<td>` for correct screen reader cell reading
- [x] Summary count text is not conveyed by color alone
- [x] "Días de Atraso" column heading and values are accessible (right-aligned numerics with suffix text)
- [x] Page title "Préstamos Vencidos" is the first heading (`<h1>` or equivalent via PageLayout)

### 5.8 Motion

| Element | Animation | Duration | Easing | Reduced motion |
|---------|-----------|----------|--------|----------------|
| Skeleton shimmer | opacity 0.5→1.0 | 1200ms ease-in-out infinite | ease-in-out | static fill |
| Table rows appear (after load) | fade-in | 150ms | ease-out `cubic-bezier(0, 0, 0.2, 1)` | none (instant) |
| Error state appear | fade-in | 150ms | ease-out | none (instant) |
| Empty state appear | fade-in | 150ms | ease-out | none (instant) |
| Toast enter | slide-up + fade | 200ms | ease-out | none (instant) |
| Toast exit | fade-out | 150ms | ease-in | none (instant) |
| Row hover background | background-color transition | 100ms | ease-out | none (instant) |

**Rule**: All animations must be wrapped in `@media (prefers-reduced-motion: no-preference)`.

---

## 5. Feature: HU-06 — Registrar Pago Total de Multa y Rehabilitar Lector

> Sections 1–4 are global. This section is added per feature and must not modify sections above.

### 5.1 Page Map

| Route | Component file | Layout | Auth required |
|-------|---------------|--------|---------------|
| `/payment` | `src/pages/DebtPaymentPage.jsx` | PageLayout (maxWidth="md") | no |

### 5.2 Navigation Changes

None. The `/payment` route already exists in the navigation configuration:
```
{ label: 'Pagar Multa', href: '/payment', icon: CheckCircle }
```

### 5.3 Screen Specifications

#### Screen: Registrar Pago Total de Multa (`/payment`)

**Purpose**: Allow the librarian to search a reader by identification, view pending debt details, and confirm total payment to rehabilitate the reader for new loans.

**Layout**:
```
PageLayout (title="Pagar Multa", maxWidth="md")
└── Section: Pago de Multa
    ├── Heading "Registrar Pago de Multa" (--text-heading-md)
    └── FormCard (card: --color-surface-raised, --shadow-sm, --radius-lg, max-width: 640px)
        ├── Fieldset: Búsqueda de Lector
        │   ├── FormField (label="Tipo de Identificación", select: CI|DNI, required)
        │   ├── FormField (label="Número de Identificación", input type="text", required)
        │   └── FormField (label="Nombre del Lector (Opcional)", input type="text", optional)
        ├── ButtonGroup: Search
        │   ├── Button (variant="primary", label="Buscar Deuda", icon=Search)
        │   └── Button (variant="secondary", label="Limpiar")
        ├── BusinessErrorAlert (conditional, inline — NO_DEBT_FOUND / system error)
        └── DebtPaymentCard (conditional, only when pending debt found)
            ├── DebtDetailsSection
            │   ├── SectionHeading "Detalles de la Deuda" (--text-heading-sm)
            │   ├── ReaderStatusBadge (--color-feedback-warning, text: "Deuda pendiente", icon: AlertCircle)
            │   ├── Row: "Lector" → "{name_reader}" (--text-body-md)
            │   ├── Row: "Identificación" → "{type_id_reader} {id_reader}" (--text-body-sm, --color-text-secondary)
            │   ├── Row: "ID Préstamo" → "{loan_id}" (--text-code, tabular-nums)
            │   ├── Row: "ID Deuda" → "{id_debt}" (--text-code, tabular-nums)
            │   ├── Row: "Estado" → badge "PENDING" (--color-feedback-warning)
            │   └── Row (highlighted): "Monto Total" → "${amount_debt}" (--text-heading-sm, --color-feedback-error, tabular-nums)
            └── PaymentActionSection
                ├── ConfirmationText "¿Confirmas el pago total de esta multa?" (--text-body-md, --color-text-primary)
                └── Button (variant="primary", label="Confirmar Pago", icon=CheckCircle)
```

**Form fields specification**:

| Field | ID | Type | Placeholder | Validation | Required |
|-------|----|------|-------------|------------|----------|
| Tipo de Identificación | `typeIdReader` | select | — (default: DNI) | Must be CI or DNI | yes |
| Número de Identificación | `idReader` | text | "Ej: 1023456789" | Non-empty, trimmed | yes |
| Nombre del Lector | `nameReader` | text | "Ej: Juan Pérez" | Trimmed, no validation | no |

**States**:

- **Initial (empty form, no search performed)**:
  - Search fields visible with placeholder text
  - Tipo de Identificación defaults to "DNI"
  - No DebtPaymentCard visible
  - No error messages visible
  - "Buscar Deuda" button is enabled
  - "Limpiar" button is enabled

- **Loading — search in progress**:
  - "Buscar Deuda" button shows loading state (spinner replacing label text, displays "Buscando...")
  - All search fields are disabled during search
  - "Limpiar" button is disabled during search
  - `aria-live="polite"` region announces "Buscando deuda del lector..."

- **Results — pending debt found (200, data returned)**:
  - DebtPaymentCard appears below the search section
  - Reader status badge: `--color-feedback-warning` background at 12% opacity, `--color-feedback-warning` text, icon `AlertCircle` (16px), text "Deuda pendiente"
  - Debt details displayed in labeled rows with consistent typography
  - Amount highlighted: `--color-feedback-error` background at 8% opacity, `--radius-md`, `--space-4` padding
  - Amount value: `$` prefix, 2 decimal places, `--text-heading-sm`, `--color-feedback-error`, `font-variant-numeric: tabular-nums`
  - "Confirmar Pago" button enabled below the details
  - Confirmation text visible: "¿Confirmas el pago total de esta multa?"
  - Search form remains visible and enabled above the debt card — the librarian may search again

- **No debt found (reader has no pending debt)**:
  - Inline alert inside the form (below the search button group)
  - Alert style: `--color-feedback-info` left border (4px), light info background, `--color-feedback-info` text
  - Icon: `Search` (20px) inline with message
  - Message: "El lector no tiene deuda pendiente. Está habilitado para solicitar préstamos."
  - DebtPaymentCard is NOT shown
  - Form fields remain enabled for a new search
  - Alert is dismissed when user modifies `idReader` field

- **Loading — payment in progress**:
  - Confirmation modal is open (see modal spec below)
  - Modal "Confirmar Pago" button shows loading state (spinner replacing label, displays "Procesando...")
  - Modal "Cancelar" button is disabled during processing
  - Backdrop is non-dismissible during processing (no close on backdrop click or Escape)
  - `aria-live="polite"` region announces "Procesando pago..."

- **Payment confirmation modal** (MANDATORY per requirements — irreversible action):
  - Triggered when "Confirmar Pago" button is clicked in the DebtPaymentCard
  - Modal size: `sm` (400px max-width)
  - Modal header:
    - Title: "Confirmar Pago de Multa" (`--text-heading-sm`)
    - Close (×) button in top-right corner (disabled during processing)
  - Modal body:
    - Icon: `AlertCircle` (32px, `--color-feedback-warning`, centered)
    - Text: "Estás a punto de registrar el pago total de la multa por **${amount_debt}** del lector **{name_reader}**." (`--text-body-md`)
    - Subtext: "Esta acción es irreversible. El lector quedará habilitado para nuevos préstamos." (`--text-body-sm`, `--color-text-secondary`)
  - Modal footer:
    - Right: Button (variant="primary", label="Confirmar Pago", icon=CheckCircle) — NEVER "Aceptar", "Sí", or "OK"
    - Left: Button (variant="secondary", label="Cancelar")
  - Closes on: Escape key (when not processing), backdrop click (when not processing), Cancel button, close (×) button
  - Focus trap: built-in manual focus management
  - On open: focus moves to "Cancelar" button (safest default for destructive confirmation)

- **Success — payment registered (200)**:
  - Modal closes automatically
  - Toast notification (success, bottom-right, 4000ms auto-dismiss): "Pago registrado exitosamente. El lector ha sido rehabilitado."
  - DebtPaymentCard updates to show:
    - Reader status badge changes: `--color-feedback-success` background at 12% opacity, `--color-feedback-success` text, icon `CheckCircle` (16px), text "Habilitado"
    - State row updates: badge "PAID" (`--color-feedback-success`)
    - Amount row: strikethrough style on the amount value (`text-decoration: line-through`, `--color-text-disabled`)
    - "Confirmar Pago" button is hidden
    - Confirmation text replaced with: "Pago registrado. El lector puede solicitar nuevos préstamos." (`--text-body-md`, `--color-feedback-success`, icon `CheckCircle` 16px)
  - Form does NOT auto-reset — the librarian sees the confirmation before clearing
  - "Limpiar" button resets the entire form to initial state

- **Business error — DEBT_NOT_FOUND (404)**:
  - Modal closes if open
  - Inline alert (same position as no-debt-found alert)
  - Alert style: `--color-feedback-info` left border (4px), light info background
  - Icon: `Search` (20px)
  - Message: "No se encontró la deuda. Es posible que ya haya sido pagada."
  - DebtPaymentCard is hidden
  - Alert dismissed when user modifies `idReader`

- **Business error — DEBT_ALREADY_PAID (409)**:
  - Modal closes if open
  - Inline alert inside the form
  - Alert style: `--color-feedback-warning` left border (4px), light warning background, `--color-feedback-warning` text
  - Icon: `AlertCircle` (20px)
  - Message: "Esta deuda ya fue pagada anteriormente. El lector ya está habilitado."
  - DebtPaymentCard updates to show PAID state (same visual as success)
  - Alert dismissed when user modifies `idReader`

- **Validation error — empty identification**:
  - Per-field inline error below the `idReader` input
  - Error style: `--text-body-sm`, `--color-feedback-error`, linked via `aria-describedby`
  - Message: "Ingresa el número de identificación del lector."
  - Error shown on form submit or field blur after dirtied — never on pristine field
  - Submit button remains enabled

- **System error (500 / network failure)**:
  - Modal closes if open
  - Toast error (bottom-right, 8000ms, requires manual dismiss): "No se pudo procesar la operación. Verifica tu conexión e intenta de nuevo."
  - Inline error fallback below the form as secondary indicator
  - Form fields and DebtPaymentCard remain in their current state — user data preserved

**Responsive**:
- Desktop (≥ 1024px): FormCard has `max-width: 640px`, left-aligned within content area
- No mobile/tablet layouts required

### 5.4 Feature Components

#### `DebtPaymentForm`

**File**: `src/components/DebtPaymentForm.jsx`
**Styles**: `src/components/DebtPaymentForm.module.css`

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| (none — self-contained) | — | — | Component manages its own state via `useDebt` hook |

**Visual states**: initial, loading-search, debt-found, no-debt, loading-payment, modal-open, success, debt-not-found, debt-already-paid, validation-error, system-error

**Fieldset structure**:
- 1 `<fieldset>` for search inputs with styled `<legend>` "Búsqueda de Lector"
- Legend text uses `--text-label-md`, `--color-text-primary`, weight 600
- Fieldset border: 1px solid `--color-border-default`, `--radius-md`
- Fieldset padding: `--space-4`

**DebtPaymentCard structure** (conditional section, not a separate component):
- Container: `--color-surface-raised` background, `--shadow-sm`, `--radius-lg`, `--space-6` padding
- Left border: 4px solid `--color-feedback-warning` (pending) or `--color-feedback-success` (paid)
- Section heading: "Detalles de la Deuda" using `--text-heading-sm` (Lora 600)
- Detail rows: flex layout, `justify-content: space-between`, `--space-2` vertical gap
- Row separator: 1px solid `--color-border-default` (except last row before amount)
- Amount row: highlighted with `--color-feedback-error` background at 8% opacity, `--radius-md`, `--space-4` padding
- All monetary values must use `toLocaleString('es-AR', { minimumFractionDigits: 2, maximumFractionDigits: 2 })` with `$` prefix
- Reader status badge at the top of the card: icon + text, never color alone

**Rules**:
- Must use `useDebt` hook for all data fetching and payment — never call services directly
- Every input must have `id` attribute matching the field ID in the table above, with a `<label htmlFor>` pair
- Search is triggered on form submit (Enter key or button click), never on input change
- "Limpiar" button must reset the search form, hide DebtPaymentCard, clear all error states, move focus to `typeIdReader`
- Payment action must open a confirmation modal — never execute payment directly on button click
- The confirmation modal must use the shared `<Modal>` component (Section 4.7) — never `window.confirm()`
- Modal confirm button label must be exactly "Confirmar Pago" — never "Aceptar", "Sí", or "OK"
- Modal cancel button label must be exactly "Cancelar"
- On success: show toast + update DebtPaymentCard to paid state; do NOT auto-reset form
- On success: reader status badge must transition from warning (amber) to success (green) with text "Habilitado"
- Business error alerts (no debt, already paid) must be rendered inline — never as toast
- System errors must use toast + inline fallback
- Must NOT use emoji in any output — use `lucide-react` icons (`CheckCircle`, `AlertCircle`, `Search`)
- All hex color references in CSS must use `var(--token-name)` — no raw hex values
- All spacing must use `var(--space-*)` tokens — no arbitrary pixel values

#### `DebtPaymentPage`

**File**: `src/pages/DebtPaymentPage.jsx`
**Styles**: `src/pages/DebtPaymentPage.module.css`

**Props**:
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| (none — route-level page) | — | — | Renders DebtPaymentForm within PageLayout |

**Rules**:
- Must wrap content in `PageLayout` with `title="Pagar Multa"` and `maxWidth="md"`
- Must not contain any data-fetching logic — delegates entirely to `DebtPaymentForm`

### 5.5 Data Display

| Data type | Format | Null display |
|-----------|--------|--------------|
| type_id_reader | "CI" or "DNI" from select | — (always has default) |
| id_reader | Raw string, left-aligned | — (required field) |
| name_reader | Raw string, `--text-body-md`, left-aligned | "—" (em dash, if not returned by API) |
| loan_id | Integer, right-aligned, `--text-code`, `tabular-nums` | "—" (em dash) |
| id_debt | Raw string, `--text-code`, `tabular-nums` | — (never null when debt exists) |
| state_debt | Badge: "PENDING" with `--color-feedback-warning` OR "PAID" with `--color-feedback-success` | — (always present) |
| amount_debt | Currency: `$` prefix, 2 decimal places, right-aligned, `--text-heading-sm`, `tabular-nums` | — (never null when debt exists) |
| Reader status | Badge: "Deuda pendiente" (amber + `AlertCircle`) OR "Habilitado" (green + `CheckCircle`) | — (derived from state_debt) |
| Error messages | Domain language, never technical codes | — |

### 5.6 Interaction Patterns

| Trigger | Component | Behavior |
|---------|-----------|----------|
| Form submit (click "Buscar Deuda" or press Enter) | DebtPaymentForm search | Validate `idReader` not empty → show loading → call `getReaderDebt(filters)` → display DebtPaymentCard or inline alert |
| Click "Limpiar" | Secondary button | Reset search fields to defaults, hide DebtPaymentCard, clear all error/success states, move focus to `typeIdReader` |
| Empty `idReader` submit | Form | Show inline validation error: "Ingresa el número de identificación del lector." below `idReader` field |
| Click "Confirmar Pago" in DebtPaymentCard | Button | Open confirmation modal — do NOT execute payment yet |
| Click "Confirmar Pago" in modal | Modal primary button | Show loading in modal → call `payDebt(id_debt)` → on success: close modal, show toast, update card to paid state |
| Click "Cancelar" in modal | Modal secondary button | Close modal, no action taken, DebtPaymentCard remains visible |
| Press Escape while modal open (not processing) | Modal | Close modal, return focus to "Confirmar Pago" button in DebtPaymentCard |
| Modify `idReader` after no-debt or error alert | Input field | Dismiss the inline alert |
| Search returns no debt (null/empty response) | DebtPaymentForm | Show info alert: "El lector no tiene deuda pendiente. Está habilitado para solicitar préstamos." |
| Backend returns DEBT_ALREADY_PAID (409) | Payment response | Close modal, show warning alert inline, update card to paid visual state |
| Backend returns DEBT_NOT_FOUND (404) on payment | Payment response | Close modal, show info alert, hide DebtPaymentCard |
| Network error during search or payment | Any async action | Toast error: "No se pudo procesar la operación. Verifica tu conexión e intenta de nuevo." + inline fallback |
| Successful payment | Modal + Form | Close modal → toast success: "Pago registrado exitosamente. El lector ha sido rehabilitado." → update card to paid state → do NOT auto-reset form |
| Click "Limpiar" after successful payment | Secondary button | Reset entire form to initial state |
| Form submit with errors (validation) | Form | Focus first invalid field |

### 5.7 Accessibility Checklist

- [x] Every text input has a visible `<label>` with `htmlFor`/`id` pairing
- [x] Search fieldset uses `<fieldset>` + `<legend>` wrapping inputs
- [x] Type of identification `<select>` has associated `<label>`
- [x] Every interactive element has visible focus state using `--shadow-focus`
- [x] Reader status badges use icon + text — never color alone
- [x] State badges ("PENDING"/"PAID") use icon + text — never color alone
- [x] Monetary amount uses `font-variant-numeric: tabular-nums` for proper numeric rendering
- [x] Confirmation modal uses `role="dialog"`, `aria-labelledby` (title), and focus trap
- [x] Modal opens with focus on "Cancelar" button (safe default for irreversible action)
- [x] Modal is dismissible via Escape key (when not processing) and close button
- [x] Business error alerts use `role="alert"` for screen reader announcement
- [x] Validation error messages use `aria-describedby` referencing the input field
- [x] Loading states use `aria-live="polite"` region
- [x] Success toast is announced by screen reader via `aria-live` region
- [x] Color contrast meets WCAG AA (4.5:1 body, 3:1 large text) — verified against warm palette
- [x] All buttons have minimum 40×40px click area
- [x] Form can be fully operated via keyboard (Tab between fields, Enter to submit, Escape to close modal)
- [x] On "Limpiar", focus is programmatically moved to `typeIdReader` for efficiency
- [x] No emoji used in component output — `lucide-react` icons only

### 5.8 Motion

| Element | Animation | Duration | Easing | Reduced motion |
|---------|-----------|----------|--------|----------------|
| DebtPaymentCard appear (after search success) | fade-in + slide-down (12px) | 200ms | ease-out `cubic-bezier(0, 0, 0.2, 1)` | none (instant) |
| Inline alert appear | fade-in + slide-down (8px) | 150ms | ease-out `cubic-bezier(0, 0, 0.2, 1)` | none (instant) |
| Inline alert dismiss | fade-out | 100ms | ease-in | none (instant) |
| Modal open | fade + scale (0.95→1.0) | 150ms | ease-out | none (instant) |
| Modal close | fade-out | 100ms | ease-in | none (instant) |
| Loading spinner (in buttons) | rotate 360° | 800ms linear infinite | linear | static icon |
| Reader status badge transition (pending→habilitado) | cross-fade (color + icon) | 200ms | ease-out | none (instant) |
| Amount strikethrough on payment success | opacity 1.0→0.6 + line-through | 150ms | ease-out | none (instant) |
| Toast enter | slide-up + fade | 200ms | ease-out | none (instant) |
| Toast exit | fade-out | 150ms | ease-in | none (instant) |

**Rule**: All animations must be wrapped in `@media (prefers-reduced-motion: no-preference)`.

---

## 6. File & Folder Conventions

### 6.1 Directory Structure

```
src/
├── main.jsx                    # Entry point — imports index.css, renders App
├── App.jsx                     # Root component — defines routes with react-router-dom
├── App.module.css              # Root component styles
├── index.css                   # Global styles + design tokens (:root {} block)
├── components/                 # Reusable UI components
│   ├── [Component].jsx         # React component (PascalCase)
│   └── [Component].module.css  # Co-located CSS Module
├── hooks/                      # Custom hooks
│   └── use[Feature].js         # One hook per domain (useLoan, useDebt, etc.)
├── pages/                      # Page components — one per route
│   ├── [Feature]Page.jsx       # Page component (PascalCase + Page suffix)
│   └── [Feature]Page.module.css
├── services/                   # HTTP layer (axios)
│   └── [feature]Service.js     # One service per domain
└── __tests__/                  # Tests (Vitest + Testing Library)
    ├── setup.js
    ├── components/
    ├── hooks/
    └── pages/
```

### 6.2 Naming Conventions

| Item | Convention |
|------|-----------|
| Component files | PascalCase `.jsx` |
| Hook files | camelCase, `use` prefix, `.js` |
| Service files | camelCase `.js` |
| Page files | PascalCase + `Page` suffix `.jsx` |
| Test files | same name + `.test.jsx` (or `.test.js` for hooks) |
| CSS Modules | same name + `.module.css` |
| Styles import | `import styles from './Component.module.css'` |

### 6.3 CSS Strategy

**Method**: CSS Modules — agents must not introduce any other styling approach.

- One `.module.css` per component, co-located in the same directory
- No global class names outside `index.css`
- No `!important`
- All design values reference tokens via `var(--token-name)` — never raw hex, px, or font values
- Custom properties (tokens) are defined exclusively in `src/index.css` inside `:root {}`

---

## 7. Conflict Prevention Rules

| Rule | Forbidden pattern | Correct pattern |
|------|------------------|-----------------|
| No duplicate Navigation | Creating `FeatureNav.jsx`, `Header.jsx`, or `TopBar.jsx` | Update `Navigation.jsx` items array |
| No global CSS additions | Adding classes to `index.css` beyond tokens | CSS Modules in component `.module.css` |
| No shared component edits | Editing `Button.jsx` to add feature-specific style | Add variant via existing props |
| No cross-feature imports | `import X from '../other-feature/X'` | Move shared component to `src/components/` |
| No hardcoded colors | `color: #C17F24` | `color: var(--color-brand-primary)` |
| No hardcoded spacing | `padding: 16px` | `padding: var(--space-4)` |
| No hardcoded fonts | `font-family: Inter` | Use token `var(--text-body-md)` properties |
| No custom spinners | New spinner SVG or component | Use Button `loading` prop or Skeleton component |
| No custom modals | Raw `<div>` overlay, `window.alert()`, `window.confirm()` | Use shared `<Modal>` component |
| No multiple icon libraries | Importing from `react-icons`, `heroicons`, etc. | Use `lucide-react` exclusively |
| No dropdown for loan days | `<select>` for plazo del préstamo | Use `RadioGroup` component (Section 4.10) |
| No toast for business errors | Toast for BOOK_NOT_AVAILABLE or READER_HAS_DEBT | Inline alert within the form |
| No text input for date_return | `<input type="text">` for date of return | Use `<input type="date">` with `max` attribute |
| No toast for LOAN_NOT_FOUND or ALREADY_RETURNED | Toast for 404 or 409 in return form | Inline alert within the ReturnForm |
| No hiding conditional fields in return form | Removing `title` field when `idBook` is empty | Disable the field with native `disabled` attribute |
| No emoji in component output | Using emoji characters (📋, ⚠️, ✓) as status indicators | Use `lucide-react` icons (`AlertCircle`, `AlertTriangle`, `CheckCircle`) |
| No auto-reset on late return success | Resetting form after late return before user reads fine details | Keep form + DebtSummary visible; reset only via "Limpiar" button |
| No raw CSS in DebtSummary | Hardcoded hex, px, or font values in DebtSummary styles | Use `var(--token-name)` for all colors, spacing, and typography |
| No sorting/filtering on overdue table | Adding sort headers, filter inputs, or search on OverduePage | Table is read-only with no interactive data controls per MVP spec |
| No pagination on overdue table | Adding pagination controls to OverdueLoansTable | Render all results in a single table per MVP spec |
| No row actions on overdue table | Adding click handlers, action buttons, or links on table rows | Table is a read-only consultation view — no mutations |
| No route `/loans/outTime` for overdue | Using legacy `/loans/outTime` route | Use `/loans/overdue` per spec SPEC-005 |
| No raw state value display in overdue | Displaying raw `ON_LOAN` as state text in overdue table | Display "Vencido" badge with `AlertTriangle` icon |
| No direct payment without modal (HU-06) | Executing `payDebt()` on button click without confirmation | Open confirmation modal first; execute only after explicit "Confirmar Pago" |
| No `window.confirm()` for payment (HU-06) | Using `window.confirm()` for payment confirmation | Use shared `<Modal>` component (Section 4.7) |
| No emoji in payment status (HU-06) | Using ✓ or ✗ characters for status indicators | Use `lucide-react` icons (`CheckCircle`, `AlertCircle`) |
| No toast for no-debt-found (HU-06) | Using toast for "no debt found" response | Inline info alert within the form |
| No auto-reset on payment success (HU-06) | Resetting form after successful payment before user reviews confirmation | Keep DebtPaymentCard in paid state; reset only via "Limpiar" |
| No "Aceptar" or "Sí" in payment modal (HU-06) | Using generic labels for confirmation button | Use exactly "Confirmar Pago" and "Cancelar" per requirements |

---

## 8. Third-Party Library Policy

> Agents must not introduce libraries outside this list without architectural-builder approval.

| Purpose | Library | Version |
|---------|---------|---------|
| Framework | React (with Vite) | React 18+ / Vite 5+ |
| Styling | CSS Modules (native) | — |
| Forms | Native React state | — |
| Validation | zod | 3+ |
| Icons | lucide-react | latest |
| Dates | date-fns | 3+ |
| Client HTTP | axios | 1.6+ |
| Routing | react-router-dom | 6+ |
| Animation | CSS native | — |
| Notifications | Custom Toast component | — |
| Testing | Vitest + Testing Library | Vitest 1+ |

---

## Appendix: Design Rationale

| Decision | Rationale |
|----------|-----------|
| Desktop-only (min 1024px) | System is exclusively used by library staff on desktop/laptop workstations. No mobile/tablet use case per requirements. |
| Warm color palette with Lora serif headings | Requirements define a library-inspired brand: paper, wood, leather, ink. Lora provides warmth and approachability while Inter ensures readability for operational text. |
| No dark mode in MVP | Explicitly excluded from requirements scope. Single `:root` token block. |
| Status badges always use icon + text | WCAG AA compliance and ~8% male color blindness prevalence require information not conveyed by color alone. |
| Search on form submit only (not on input change) | Prevents excessive API calls, gives user control over when to trigger the search, and aligns with traditional form interaction patterns familiar to library staff. |
| Tabular-nums for IDs in results table | Prevents column width jumping when numeric values change, per typography knowledge base guidelines for data UIs. |
| No separate route for book search | Per spec, `LoanSearch` is integrated into the combined loan page at `/loan`. This reduces navigation complexity for the librarian's primary workflow. |
| Sidebar navigation pattern | Requirements specify "sidebar izquierdo fijo + área de contenido principal" as the navigation structure. Current implementation uses top nav — architectural-builder must migrate. |
| Radio group for loan days (not dropdown) | Requirements explicitly forbid dropdowns for loan term selection: "No usar dropdowns para la selección de plazo de préstamo — usar radio group visual". All 3 options (7/14/21) must be simultaneously visible. |
| Inline alerts for business errors (not toast) | Requirements state: "Errores de validación de negocio (lector bloqueado, libro no disponible): mensaje inline dentro del formulario, nunca como toast." Toasts are reserved for system errors and success confirmations. |
| Due date preview is read-only computed display | The due date is derived from `today + loan_days` and displayed as a read-only formatted value. The user never types a date — this prevents date format errors and aligns with H5 (Error Prevention). |
| Focus return to first field on success | After successful submission, programmatic focus to `idBook` reduces friction for consecutive loan registrations — a common librarian workflow (Nielsen H7: Flexibility and efficiency). |
| Current LoanForm uses `<select>` for loan days | This violates requirements. The design spec mandates RadioGroup. Frontend agent must replace `<select>` with RadioGroup during implementation. |
| Smart-search flexible identification (HU-03) | The return form allows identifying a loan by book ID, reader ID, or both. This matches the spec's "búsqueda inteligente" pattern and accommodates the librarian's variable workflow — sometimes they know the book, sometimes the reader, sometimes both. Reduces form friction per Nielsen H7 (Flexibility and efficiency). |
| Native date input for date_return (HU-03) | Using `<input type="date">` with `max` attribute prevents format errors and future-date entry at the HTML constraint level, aligning with H5 (Error Prevention). The browser provides locale-appropriate formatting without custom date-picker libraries. |
| Info-style alert for LOAN_NOT_FOUND (HU-03) | A 404 is not an error the user caused — it means the search criteria did not match an active loan. Using `--color-feedback-info` instead of `--color-feedback-error` avoids alarming the user and accurately conveys "not found, try different criteria" (H9: constructive error recovery). |
| Warning-style alert for ALREADY_RETURNED (HU-03) | A 409 indicates a conflict state, not a user input error. Using `--color-feedback-warning` signals caution: "this loan exists but was already processed." This differentiates it from validation errors (red) and not-found states (blue). |
| Title field disabled when idBook is empty (HU-03) | The title refines the book search but is meaningless without a book ID. Disabling it prevents the user from entering orphaned data, reducing cognitive load (Law 2) and preventing errors (H5). The field remains visible to communicate its existence for the case when idBook is provided. |
| Warning color (amber) for DebtSummary border, not error (red) (HU-04) | The fine is a consequence of a valid late return, not an error. Using `--color-feedback-warning` (amber) signals "pending action required" rather than "something went wrong". Red is reserved for the amount value itself to draw attention to the total owed. This follows color semantics (Section 1.1): warning for pending states, error for destructive actions. |
| No form auto-reset on late return (HU-04) | When a late return generates a fine, the librarian must review the breakdown (days late, weeks, Fibonacci units, total amount) before proceeding. Auto-resetting would force the user to remember or screenshot the fine details. Keeping the form + DebtSummary visible until manual "Limpiar" aligns with Nielsen H3 (User control and freedom). |
| baseFibAmount always visible (HU-04) | The base Fibonacci amount input is always shown, not conditionally displayed. The librarian sets a policy value before submitting — they cannot know in advance if the return is late. Hiding it until late detection would require a two-step flow, adding friction. Showing it upfront as part of the standard workflow is consistent (H4) and efficient (H7). |
| Separate fieldset for fine configuration (HU-04) | The `baseFibAmount` field is logically distinct from loan identification and return date. Placing it in its own fieldset creates a clear Gestalt grouping (proximity + common region) and communicates that it is a policy parameter, not loan data. |
| DebtSummary uses lucide-react icons, not emoji (HU-04) | Emoji render inconsistently across platforms and do not support `aria-hidden` or sizing tokens. Using `lucide-react` icons ensures visual consistency, accessibility compliance, and adherence to the single icon library policy (Section 4.9). |
| Confirmation modal for payment (HU-06) | Requirements explicitly state: "Registrar pago de multa requiere confirmación modal: es una acción irreversible." The current implementation uses an inline button without modal — this violates requirements. The modal provides a clear friction point before an irreversible financial action (Nielsen H3: User control and freedom, H5: Error prevention). |
| Modal focus on "Cancelar" by default (HU-06) | For destructive/irreversible confirmations, focusing the cancel button as default prevents accidental confirmation via Enter key. The user must actively Tab to and press the confirm button, adding intentional friction (H5: Error prevention). |
| "Confirmar Pago" exact label (HU-06) | Requirements specify: "El botón de confirmar en el modal debe replicar exactamente la acción: 'Confirmar pago' (no 'Aceptar', no 'Sí')." This follows H4 (Consistency) — the button label must match the action being performed, eliminating ambiguity. |
| Info-style alert for no-debt-found (HU-06) | When a reader has no pending debt, this is not an error — it is a positive outcome. Using `--color-feedback-info` (blue) signals neutral information, not failure. The message "Está habilitado para solicitar préstamos" turns the result into actionable guidance (H9: constructive recovery). |
| Warning badge for pending debt, success badge for habilitado (HU-06) | Requirements define explicit badge patterns: "Lector con deuda pendiente: badge ámbar con texto 'Deuda pendiente'" and "Lector habilitado: badge verde con texto 'Habilitado'". Following these ensures cross-feature consistency (H4) and accessibility (color + icon + text per Section 1.1 rules). |
| No form auto-reset on payment success (HU-06) | After paying a fine, the librarian must confirm the reader is now rehabilitated. Auto-resetting would remove this visual confirmation. The paid state (green badge, strikethrough amount) serves as receipt-like confirmation. Manual "Limpiar" provides user control (H3). |
| DebtPaymentCard as section, not separate component (HU-06) | The debt display and payment action are tightly coupled to the search flow and share state with `DebtPaymentForm`. Extracting it as a separate component would require prop-drilling the payment handler, modal state, and success state. Keeping it as a conditional section within `DebtPaymentForm` reduces complexity and keeps the single-responsibility within the hook boundary. |
| Non-dismissible modal during payment processing (HU-06) | While the payment API call is in-flight, the modal must not be closable via Escape or backdrop click. Closing mid-request could leave the user uncertain about whether the payment was registered. Blocking dismissal during processing ensures the user waits for the definitive result (H1: Visibility of system status). |
| Read-only table with no sorting/filtering/pagination (HU-05) | The spec explicitly states "Esta consulta no incluye filtros avanzados, ordenamiento configurable ni paginación en el MVP." Adding interactive controls would exceed MVP scope and add untested complexity. The full list is expected to remain manageable for a single-library operation. |
| Composite cells for Libro and Lector columns (HU-05) | The spec response includes 9 data fields. Showing each as a separate column would exceed the 6-8 guideline (Nielsen H8: Aesthetic and minimalist design). Grouping related fields (book ID + title, reader name + ID) into composite two-line cells reduces column count to 6 while preserving all required data. Each cell uses visual hierarchy (primary line = `--text-body-md`, secondary line = `--text-label-sm` + `--color-text-secondary`) per Gestalt similarity principle. |
| "Días de Atraso" computed column instead of date_return (HU-05) | For overdue loans, `date_return` is always `null` — displaying a null column wastes space and provides no operational value. "Días de Atraso" is computed on the frontend (`today - date_limit`) and gives the librarian immediate actionable context: how urgently to pursue each overdue loan. This aligns with the feature's stated goal of "gestionar deudas atrasadas y seguimiento." |
| "Vencido" badge instead of raw ON_LOAN state (HU-05) | All records in the overdue view have `state=ON_LOAN` by definition. Displaying "En préstamo" would be technically correct but operationally misleading — the librarian needs to know these are _overdue_, not just active. "Vencido" with `--color-feedback-error` + `AlertTriangle` icon immediately conveys urgency per requirements badge rules (Nielsen H2: Match between system and real world). |
| Route `/loans/overdue` over `/loans/outTime` (HU-05) | The spec SPEC-005 defines the endpoint as `/api/v1/loans/overdue` and the frontend route as `/loans/overdue`. The existing navigation uses `/loans/outTime`, which is a legacy naming inconsistency. The spec is the source of truth per ASDD rules. Frontend agent must align the route to `/loans/overdue`. |
| Retry button on error state (HU-05) | Unlike form pages where the user can re-submit, this page auto-fetches on mount. If the fetch fails, the user has no mechanism to retry without a browser refresh. The "Reintentar" button provides an explicit recovery path (Nielsen H9: Help users recover from errors). |
| Summary bar visible only with data (HU-05) | Showing "Total de préstamos vencidos: 0" adds no value and clutters the empty state. The empty state already communicates the absence of data with a dedicated design. The summary bar appears only when there are results to count, reducing noise (Nielsen H8: Aesthetic and minimalist design). |
