---
description: Describe when these instructions should be loaded by the agent based on task context
# applyTo: 'Describe when these instructions should be loaded by the agent based on task context' # when provided, instructions will automatically be added to the request context when the pattern matches an attached file
---
# UX/UI Knowledge Base
# .github/instructions/ux-ui-knowledge.instructions.md
#
# Consumed by: ux-ui-designer agent only
# Purpose: All UX/UI theory, heuristics, patterns and standards the agent applies
#          when producing the ux-ui-design.md document.
# Update this file to evolve design standards without modifying the agent.

---

## 1. Foundational Principles

### 1.1 Hierarchy of Design Decisions

Apply decisions in this order. Lower levels must not contradict higher ones:

1. **Accessibility** — if a decision creates a WCAG violation, it is rejected regardless of aesthetics
2. **Usability** — if a pattern confuses users, it is wrong regardless of visual appeal
3. **Consistency** — if a pattern breaks the established system, it must be justified
4. **Brand** — visual identity is applied within the constraints above, not before them
5. **Aesthetics** — visual refinement is applied last, never at the expense of the above

### 1.2 The Three Laws of Interface Design

**Law 1 — Don't make users think**
Every interface element must communicate its purpose immediately. If a user needs to read a label twice or hover to understand what a button does, the design has failed. Labels must be action-oriented verbs ("Save project", not "OK"). Icons must always be paired with labels unless the icon is universally understood (close X, search magnifier).

**Law 2 — Reduce cognitive load**
The human working memory holds 4±1 chunks at a time (Cowan, 2001 — updated from Miller's 7±2). Design implications:
- Navigation menus must not exceed 7 items at the same level
- Forms must group related fields and show no more than 5–7 fields per section
- Tables must not display more than 6–8 columns without progressive disclosure
- Dashboards must not display more than 5 primary metrics above the fold

**Law 3 — Prevent and recover from errors**
Design must first prevent errors (constraints, clear affordances, confirmation dialogs for destructive actions), then make recovery easy (undo, clear error messages with actionable instructions, autosave).

---

## 2. Nielsen's 10 Usability Heuristics

Apply these as a checklist when specifying every screen and interaction pattern.

### H1 — Visibility of system status
The system must always keep users informed about what is going on through appropriate feedback within a reasonable time.
- Every async action must show a loading state within 100ms
- Progress indicators must show completion percentage when the total duration is estimable (> 3s)
- Success and error states must be shown immediately after action completion
- Background processes must be surfaced via non-intrusive indicators (progress badge on nav item, status bar)

### H2 — Match between system and real world
Use words, phrases, and concepts familiar to the user, not system-oriented terms. Follow real-world conventions.
- Use domain language from the requirements file, not technical terms
- Date formats must follow user locale conventions
- Sort orders must match user mental models (alphabetical for names, chronological for events, numerical for quantities)
- Icons must map to universally understood metaphors (trash = delete, pencil = edit, eye = view)

### H3 — User control and freedom
Users often perform actions by mistake. Provide clearly marked "emergency exits" without requiring extended dialogue.
- Destructive actions (delete, archive, revoke) must have a confirmation step
- Forms with multiple steps must allow going back without data loss
- Modals must always be dismissible via Escape key and a visible close button
- Bulk actions must have an undo mechanism or a confirmation step
- Navigation must never trap the user (back button must always work as expected)

### H4 — Consistency and standards
Users should not have to wonder whether different words, situations, or actions mean the same thing. Follow platform conventions.
- The same action must always use the same label (never "Save" in one place and "Submit" in another for equivalent actions)
- The same component must always behave the same way regardless of where it appears
- Error messages must follow the same format: what went wrong + why + how to fix it
- Position of recurring elements (primary action, cancel, close) must be consistent across all screens

### H5 — Error prevention
Design carefully to prevent problems from occurring in the first place.
- Destructive actions must use the `danger` button variant and require explicit confirmation
- Forms must validate in real time after a field is dirtied (not before the user has touched it)
- Irreversible actions must clearly state they are irreversible in the confirmation dialog
- Inputs must constrain invalid input where possible (date pickers instead of free text for dates, numeric keyboards for number fields)
- Disable submit buttons while a form has unresolved validation errors

### H6 — Recognition over recall
Minimize the user's memory load by making objects, actions, and options visible.
- Navigation must always show the current location (active state, breadcrumb)
- Recently accessed items should be surfaced in relevant contexts
- Form fields must show the expected format as placeholder or helper text (not only on error)
- Search must show recent queries and suggestions
- Action menus must show all available actions, not require the user to remember keyboard shortcuts

### H7 — Flexibility and efficiency of use
Allow users to tailor frequent actions. Accelerators for expert users must be invisible to novice users.
- Keyboard shortcuts may be added for frequent actions but must never be required
- Tables must support column sorting and filtering without requiring knowledge of their existence
- Search must support advanced filters progressively (basic search always visible, advanced filter panel behind a toggle)
- Bulk actions must be available for power users without cluttering the default view

### H8 — Aesthetic and minimalist design
Dialogues must not contain irrelevant or rarely needed information. Extra information competes with relevant information.
- Every element on a screen must serve a purpose. If you cannot state the purpose, remove the element.
- Empty states must not show blank space — they are an opportunity for guidance
- Error messages must be specific, never generic ("Something went wrong" is forbidden)
- Form helper text must be shown only when it adds information not already obvious from the label
- Tooltips must only appear when an element's purpose is genuinely ambiguous

### H9 — Help users recognize, diagnose, and recover from errors
Error messages must be expressed in plain language, precisely indicate the problem, and constructively suggest a solution.
- Format: "{What happened}. {Why it happened}. {How to fix it}."
- Example (good): "Email already in use. An account with this email exists. Sign in instead, or use a different email."
- Example (bad): "Error 409. Conflict."
- Never show raw error codes, stack traces, or technical identifiers to end users
- Validation errors must appear inline next to the offending field, not only as a page-level alert
- Network errors must distinguish between "no connection" and "server error" and provide different recovery paths

### H10 — Help and documentation
Even though it is better if the system can be used without documentation, it may be necessary to provide help.
- Onboarding flows must be designed for first-time users without requiring a separate help page
- Complex features must include inline contextual help (helper text, info icons with tooltips, guided tours)
- Empty states are the primary onboarding moment — they must explain what the section does and how to get started
- Error states in empty data scenarios must distinguish between "no data yet" and "failed to load"

---

## 3. Gestalt Principles for Layout

Apply these when defining the layout structure of every screen and component.

### 3.1 Proximity
Elements close to each other are perceived as related. Elements far apart are perceived as separate.
- Group related form fields visually (fieldset or card container) with `--space-4` between fields within a group and `--space-12` between groups
- Label and its input must be separated by no more than `--space-2`
- Action buttons must be proximate to the content they act upon (inline table row actions, not a remote action bar)

### 3.2 Similarity
Elements that look alike are perceived as belonging together.
- All primary actions must share the same button variant (`primary`) across the entire product
- All destructive actions must share the `danger` variant
- All read-only data must use consistent typography and color tokens — never make one number bold and another not without semantic reason

### 3.3 Continuity
The eye follows lines, curves, and aligned elements naturally.
- Lists, tables, and card grids must use consistent alignment — never mix left-aligned and center-aligned content in the same column
- Vertical rhythm must be maintained using the spacing scale — content should feel like it flows along invisible grid lines
- Multi-step flows must show progression in a consistent direction (left-to-right or top-to-bottom, never both)

### 3.4 Closure
The mind completes incomplete shapes. Use this to create clean layouts without heavy borders.
- Cards can be separated by whitespace alone (`--space-8` gap) without requiring a border between them
- Modal overlays use background dimming to imply containment without an explicit border
- Navigation sections can be delineated by whitespace instead of divider lines

### 3.5 Figure and Ground
Users perceive elements as either foreground (figure) or background (ground). Use contrast to establish hierarchy.
- Modals, dropdowns, and tooltips must use `--color-surface-raised` against `--color-surface-base` background
- The currently active navigation item must be visually distinct from inactive items (background + text color change, not just bold)
- Disabled elements must be clearly subordinate (`--color-text-disabled`) — they exist in the background, not the foreground

### 3.6 Common Region
Elements within the same bounded region are perceived as related.
- Use cards (`--color-surface-raised`, `--shadow-sm`, `--radius-lg`) to group conceptually related information
- Use `--color-surface-overlay` background for sidebar and drawer regions to separate them from the main content
- Form sections with their own heading must use a visual container (card or section with a top border accent) to establish region boundaries

---

## 4. Visual Hierarchy

### 4.1 The F-Pattern and Z-Pattern

Users scan interfaces before reading. Design for scanning first, reading second.

**F-Pattern** (content-heavy pages: dashboards, lists, detail views):
- The most important information must be in the first two rows (top horizontal bar scanned first)
- Primary labels and key data must be left-aligned — the left vertical strip is scanned third
- Secondary information belongs in the right column or below the fold

**Z-Pattern** (simple pages: landing pages, login, confirmation):
- Top-left: logo or identity element
- Top-right: primary action (Sign in, Get started)
- Bottom-left: supporting information
- Bottom-right: secondary action (Learn more, Contact)

### 4.2 Visual Weight

Heavier elements attract the eye first. Use weight intentionally:

| Weight level | How to achieve | Use for |
|-------------|----------------|---------|
| Heaviest | Large size + `--color-brand-primary` fill + `font-weight: 600` | Primary CTA, page title |
| Heavy | Medium size + `--color-text-primary` + `font-weight: 600` | Section headings, card titles |
| Medium | Normal size + `--color-text-primary` + `font-weight: 400` | Body text, table data |
| Light | Small size + `--color-text-secondary` | Captions, timestamps, helper text |
| Lightest | Small size + `--color-text-disabled` | Placeholder text, disabled labels |

### 4.3 Contrast and Emphasis

- Use **size** as the primary differentiator between hierarchy levels (not color)
- Use **color** as the secondary differentiator (brand color for CTAs, muted for secondary text)
- Use **weight** (bold) sparingly — if everything is bold, nothing is bold
- Never use more than 3 levels of visual hierarchy within a single card or section
- Avoid using both bold and a brand color simultaneously on the same element — one emphasis signal is enough

---

## 5. Typography System

### 5.1 Type Scale Construction

Build the scale using a modular ratio. Recommended ratios:

| Ratio | Name | Factor | Best for |
|-------|------|--------|----------|
| 1.067 | Minor second | ×1.067 | Dense UIs, data-heavy apps |
| 1.125 | Major second | ×1.125 | Standard B2B apps |
| 1.200 | Minor third | ×1.200 | Marketing-adjacent apps |
| 1.250 | Major third | ×1.250 | Bold, expressive UIs |

For B2B SaaS, use **Major second (1.125)** as the default. Round to nearest even number.

Base: 16px
- Label SM: 12px
- Label MD / Body SM: 14px
- Body MD (base): 16px
- Body LG: 18px
- Heading SM: 20px
- Heading MD: 24px (≈ 16 × 1.125³)
- Display LG: 30px
- Display XL: 36px

### 5.2 Line Height Rules

| Usage | Line height | Reason |
|-------|-------------|--------|
| Display / headings | 1.2 – 1.3 | Large text needs tighter leading |
| Body text | 1.6 – 1.7 | Optimal readability for paragraph text |
| Labels, captions | 1.4 | Short text, no paragraph rhythm needed |
| Code | 1.6 | Code benefits from generous leading |

### 5.3 Line Length (Measure)

Optimal: **60–80 characters** per line for body text.
Maximum: **90 characters** for wide containers.
Never let body text span a full-width container on desktop without a `max-width` constraint.

### 5.4 Font Pairing Principles

- One typeface for UI (sans-serif): used for all interface text — labels, body, headings
- One typeface for code (monospace): used exclusively for code blocks and inline code
- Never use more than two typefaces in a product UI
- Decorative or display fonts are allowed only for marketing/hero sections, never for functional UI

### 5.5 Font Weight Usage

| Weight | Usage |
|--------|-------|
| 400 (regular) | Body text, table data, helper text |
| 500 (medium) | Labels, button text, navigation items |
| 600 (semibold) | Headings, card titles, emphasis |
| 700 (bold) | Display headings, hero text only |

Never use 700 (bold) for body text or labels. Never use 400 for headings below Display level.

### 5.6 Numeric Typography in Data UIs

- Use **tabular figures** (font-variant-numeric: tabular-nums) for all numeric data in tables and dashboards — prevents column width jumping during updates
- Align numbers **right** in table columns for easy comparison
- Large numbers (> 999): always use locale-appropriate thousand separators
- Currency: symbol before the number, 2 decimal places, right-aligned
- Percentages: one decimal place, `%` symbol immediately after the number (no space)

---

## 6. Color Theory for UI

### 6.1 The 60-30-10 Rule

- **60%**: Neutral (background surfaces) — creates breathing room
- **30%**: Secondary (text, borders, secondary surfaces) — defines structure
- **10%**: Brand/accent (CTAs, active states, links) — draws attention

The brand primary color must never exceed 10% of screen real estate. Violations create visual fatigue and dilute the attention-drawing power of CTAs.

### 6.2 Color Semantics

Semantic colors carry meaning the user learns once and applies everywhere:

| Color | Semantic meaning | Allowed uses | Forbidden uses |
|-------|-----------------|--------------|----------------|
| Brand primary (blue) | Brand identity, action, trust | CTAs, links, active states, focus rings | Backgrounds, decorative elements |
| Green | Success, completion, positive | Success toasts, completed status badges, positive metrics | Branding, decorative |
| Red | Error, danger, destruction | Error states, validation, destructive actions, negative metrics | Branding, decorative, warnings |
| Amber/Yellow | Warning, caution, pending | Warning toasts, caution badges, pending status | Errors, success states |
| Blue (info) | Informational, neutral | Info toasts, informational badges | Same as brand primary to avoid confusion — use a distinct shade if needed |

**Critical rule**: Once a color carries a semantic meaning in a product, it must never be used decoratively. Using red as a decorative element in a section header will cause users to assume something is wrong.

### 6.3 Contrast Ratios (WCAG 2.1)

| Text size | Minimum contrast (AA) | Enhanced contrast (AAA) |
|-----------|----------------------|------------------------|
| Normal text (< 18pt / < 14pt bold) | 4.5:1 | 7:1 |
| Large text (≥ 18pt / ≥ 14pt bold) | 3:1 | 4.5:1 |
| UI components and graphical objects | 3:1 | — |
| Placeholder text | 4.5:1 | — |
| Disabled text | No requirement | — |

Check every foreground/background combination defined in the color palette against these ratios.

### 6.4 Color Blindness Considerations

- ~8% of males have red-green color blindness (deuteranopia/protanopia)
- Never use color as the **only** differentiator between states — always pair color with an icon, label, or pattern
- Specific pairs to avoid as sole differentiators: red/green, red/black, green/brown, blue/purple
- Ensure success/error states are distinguishable by icon + text, not color alone

### 6.5 Dark Mode Token Strategy

When dark mode is required, define tokens in two layers:

```css
:root {
  /* Surfaces: light-to-dark scale */
  --color-surface-base:    #F9FAFB;  /* lightest — page bg */
  --color-surface-raised:  #FFFFFF;  /* cards, modals */
  --color-surface-overlay: #F3F4F6;  /* sidebar */
}

.dark {
  /* Surfaces: inverted — dark-to-darker scale */
  --color-surface-base:    #111827;
  --color-surface-raised:  #1F2937;  /* slightly lighter than base */
  --color-surface-overlay: #1F2937;
}
```

**Dark mode elevation principle**: In light mode, surfaces become lighter as they elevate (card is whiter than page). In dark mode, surfaces become lighter as they elevate (card is slightly lighter gray than page). Never use pure black (#000000) as a surface in dark mode — it is too harsh.

---

## 7. Spacing and Layout Systems

### 7.1 The 4px Grid

All spacing values must be multiples of 4px. This creates visual consistency and aligns with most display pixel densities.

Values: 4, 8, 12, 16, 20, 24, 32, 40, 48, 64, 80, 96, 128px

### 7.2 Component Padding Rules

| Component size | Internal padding (horizontal) | Internal padding (vertical) |
|----------------|------------------------------|----------------------------|
| Small (32px height) | 12px | — (height is fixed) |
| Medium (40px height) | 16px | — (height is fixed) |
| Large (48px height) | 20px | — (height is fixed) |
| Card | 24px | 24px |
| Modal | 32px | 32px |
| Page section | 24–40px | 40–64px |

### 7.3 Touch Target Sizes

| Platform | Minimum touch target | Recommended |
|----------|---------------------|-------------|
| Mobile (iOS) | 44×44px | 48×48px |
| Mobile (Android) | 48×48px | 48×48px |
| Desktop | 32×32px | 40×40px |

Interactive elements smaller than these minimums must use padding to expand the hit area without changing visual size.

### 7.4 Layout Grid Principles

**Column grid**: Always design within a column grid. Content and components must align to the grid.

**Gutters**: Space between columns. Must remain consistent across all components on the same page.

**Margins**: The space between the viewport edge and the grid. On mobile, margins of 16px minimum.

**Nesting**: Grids can be nested within a column span. Inner grids use the same base unit.

**White space as a design element**: Generous white space between sections signals information boundaries without requiring visual dividers. The minimum vertical rhythm between major page sections is `--space-16`.

---

## 8. Component Design Patterns

### 8.1 Form Design

**Input field anatomy**:
1. Label (always above the field, never inside as placeholder-only)
2. Input (with placeholder as example, not instruction)
3. Helper text (optional, shown always when present — not only on error)
4. Error message (shown only after field is dirtied and invalid)

**Label rules**:
- Labels must use sentence case ("First name", not "First Name" or "FIRST NAME")
- Labels must be concise (2–4 words maximum)
- Required fields: mark with an asterisk (*) with a legend at the top of the form ("* Required")
- Optional fields: mark with "(optional)" after the label — never mark required fields with "(required)"

**Validation timing**:
- Never validate before the user has interacted with the field (pristine state)
- Validate on blur (when focus leaves the field) for most fields
- Validate on change (real-time) only for fields with strict format requirements (password strength, username availability)
- Validate the entire form on submit attempt

**Multi-step forms**:
- Show a progress indicator (step X of N)
- Preserve entered data when navigating between steps
- Allow going back without data loss
- Show a summary/review step before final submission if the action is irreversible

**Form layout**:
- Single-column forms are easier to complete than multi-column forms (NNGroup research)
- Use two columns only for closely related short fields (First name / Last name, City / ZIP)
- Never put unrelated fields in the same row to save space

### 8.2 Data Tables

**Column rules**:
- Left-align text columns
- Right-align numeric columns
- Center-align status badges and icon columns
- The first column (usually name/identifier) should be sticky on horizontal scroll
- Default sort column must be indicated with a directional arrow icon

**Row density options** (if the product supports user preference):
- Compact: 36px row height
- Default: 48px row height
- Comfortable: 56px row height

**Pagination vs infinite scroll**:
- Use **pagination** for: content where users need to find specific items, data that changes infrequently, tables with bulk-selection actions
- Use **infinite scroll** for: feeds, activity logs, search results where users browse sequentially
- Never use infinite scroll for tables with bulk actions (users cannot select across page boundaries)

**Empty table states**:
- No data at all: illustration + heading ("No {items} yet") + CTA ("Create your first {item}")
- No results from filter/search: icon + "No results for '{query}'" + "Clear filters" link
- Error loading data: error icon + message + "Retry" button

**Responsive tables**:
- Never let a table overflow its container silently — always use horizontal scroll with a visual affordance (shadow on the right edge indicating scrollable content)
- On mobile, consider card-based layout as an alternative to horizontal scrolling for tables with many columns

### 8.3 Navigation Patterns

**Top navigation bar** (global):
- Maximum 7 items
- Active state must be clearly distinct (not just bold — use background color change)
- User menu must be in the top-right corner (universal convention)
- Logo must link to the home/dashboard route

**Sidebar navigation** (application):
- Group items into sections with labels (max 7 items per section)
- Show item count badges for items with actionable counts (unread, pending)
- Indicate currently active item with background + brand color accent on left border
- Collapsible state must preserve the active item visibility (show icon even when collapsed)
- Never use hover-only dropdowns in sidebar — use accordion expand or nested navigation

**Breadcrumb**:
- Show when: the user is 2+ levels deep in a hierarchy
- Format: `Home > Section > Current page` (current page is not a link)
- On mobile: show only the immediate parent as "← Back to {parent}"

**Mobile navigation**:
- Bottom navigation bar: maximum 5 items (4 is optimal). Items must have icons + short labels.
- Hamburger/drawer: acceptable for apps with > 5 primary destinations, but bottom nav preferred
- Never use hamburger menu as the primary navigation pattern for apps used frequently

### 8.4 Empty States

Empty states are the most underdesigned part of most products. They are critical onboarding and recovery moments.

**Anatomy**:
1. Illustration (optional, 80–120px, simple and friendly)
2. Heading: "No {items} yet" or "{Action} your first {item}"
3. Supporting text: one sentence explaining what this section does
4. CTA button (optional): primary action to get started

**Types**:
- **First time**: user has never created any content. Tone: encouraging, helpful. CTA: create first item.
- **Cleared state**: user deleted all items. Tone: neutral, clean. CTA: create new item.
- **No results**: search or filter returned nothing. Tone: helpful. Show query, offer to clear filters.
- **Error state**: data failed to load. Tone: apologetic, actionable. Show retry button.
- **No permission**: user lacks access. Tone: informative. Explain how to get access.

**Rules**:
- Never leave a blank white space as an empty state — always design it
- Empty state illustrations must use only brand-approved colors (--color-brand-primary-light, --color-surface-raised)
- CTA in empty states must match the exact action that will populate the section

### 8.5 Loading and Skeleton Screens

**Skeleton screen rules**:
- Skeleton elements must mimic the exact shape and layout of the content they replace
- Use animated shimmer (opacity pulse) to indicate active loading
- Show skeletons for a minimum of 300ms to avoid flicker on fast loads
- Never mix skeleton screens and spinners on the same page

**Skeleton construction**:
- Text lines: `height: 1em`, `border-radius: --radius-sm`, width varies (60–90% of container)
- Images/avatars: same dimensions as real content, `border-radius` matches real component
- Buttons in skeleton state: same dimensions, `--color-surface-overlay` fill

**When to use spinner vs skeleton**:
- Spinner: for actions (button loading state, full-page overlay during auth)
- Skeleton: for content loading (page load, list fetch, table data)

### 8.6 Modals and Dialogs

**When to use a modal**:
- Requires user decision before proceeding (confirmation, selection)
- Short-form input that creates/edits a record
- Critical information that must be acknowledged

**When NOT to use a modal**:
- Complex multi-step workflows (use a dedicated page or drawer instead)
- Content the user will read for a long time
- Anything that requires navigation within it

**Modal header**:
- Always include a title (the action, e.g., "Delete project" not "Confirm")
- Always include a close (×) button in the top-right corner

**Modal footer**:
- Primary action button on the right
- Cancel / secondary action on the left
- For destructive actions: danger button for confirm, secondary button for cancel
- Never include more than 2 action buttons in a modal footer

**Drawer vs modal**:
- Use **modal** for: confirmations, short forms, alerts
- Use **drawer** for: detailed forms, preview panels, settings panels, anything that benefits from seeing the underlying content simultaneously

### 8.7 Tooltips and Popovers

**Tooltip** (simple text, no interaction):
- Trigger: hover on desktop, tap on mobile (with tap-again-to-dismiss)
- Delay: 300ms on hover (prevents accidental triggers)
- Max width: 240px
- Content: one sentence maximum, no interactive elements
- Always positioned to avoid viewport edge clipping

**Popover** (rich content, may have interaction):
- Trigger: click (never hover-only)
- Must be dismissible via Escape, outside click, and explicit close
- Can contain interactive elements (links, buttons)
- Max width: 360px

**When to use neither**:
- If the information is important for all users, show it inline (helper text, not tooltip)
- If the interaction is complex, use a modal or drawer
- On mobile, avoid tooltips entirely — use inline helper text

---

## 9. Motion and Animation

### 9.1 The Purpose of Motion

Motion in UI serves exactly four purposes. Any animation that does not serve one of these must be removed:

1. **Orientation**: helps users understand where they are in space (page transitions, expanding panels)
2. **Feedback**: confirms that an action was registered (button press, form submit)
3. **Causality**: shows the relationship between cause and effect (item deleted from list, item added to cart)
4. **Hierarchy**: reveals structure (accordion expanding, nested navigation opening)

Decorative motion (animation that merely looks nice) is forbidden in B2B/productivity applications.

### 9.2 Duration Scale

| Duration | Use |
|----------|-----|
| 50–100ms | Micro-interactions (button press, checkbox check) |
| 100–200ms | Small UI changes (dropdown open, tooltip appear) |
| 200–300ms | Medium transitions (modal open, panel expand) |
| 300–500ms | Page transitions, large layout changes |
| > 500ms | Only for intentional dramatic moments (onboarding, success celebration) — use sparingly |

**Rule**: Default to shorter durations. Animations that feel "slow" are universally more annoying than animations that feel "fast". When in doubt, use the shorter end of the range.

### 9.3 Easing Functions

| Easing | CSS value | Use |
|--------|-----------|-----|
| Ease out | `cubic-bezier(0, 0, 0.2, 1)` | Elements entering the screen (natural deceleration) |
| Ease in | `cubic-bezier(0.4, 0, 1, 1)` | Elements leaving the screen (natural acceleration) |
| Ease in-out | `cubic-bezier(0.4, 0, 0.2, 1)` | Elements moving within the screen |
| Linear | `linear` | Spinners, progress bars, continuous motion |

Never use default CSS `ease` for UI transitions — it has an unnatural deceleration curve that feels sluggish.

### 9.4 Reduced Motion

All animations must respect `prefers-reduced-motion`. This setting is used by people with vestibular disorders and motion sensitivity.

```css
@media (prefers-reduced-motion: no-preference) {
  .element { transition: transform 200ms cubic-bezier(0, 0, 0.2, 1); }
}
/* Without the media query: element shows no transition */
```

For skeleton screens: replace animated shimmer with a static gray fill when reduced motion is preferred.

### 9.5 Animation Anti-patterns

- **Bouncing**: spring/bounce easings feel playful and childish in B2B contexts — avoid
- **Spinning logos**: never animate the brand logo
- **Parallax scrolling**: causes motion sickness in users with vestibular disorders
- **Auto-playing content**: never auto-play video, animated content, or carousels without user initiation
- **Infinite decorative loops**: backgrounds, hero sections with perpetual animation — forbidden
- **Staggered list animations**: animating list items in sequence one-by-one on every load feels repetitive and annoying after the first visit

---

## 10. Accessibility (WCAG 2.1 AA)

### 10.1 Perceivable

**Text alternatives**:
- All non-text content must have a text alternative
- Images: `alt="descriptive text"` (or `alt=""` for decorative)
- Icon-only buttons: `aria-label="Action name"`
- Charts and graphs: provide a text summary or data table alternative

**Color independence**:
- Never use color as the only means of conveying information
- Error states: red border + error icon + error text (not red border alone)
- Success states: green + checkmark icon + success text
- Required fields: asterisk + legend (not red label color alone)

**Contrast ratios**: See Section 6.3.

### 10.2 Operable

**Keyboard navigation**:
- All interactive elements must be reachable via Tab key
- Tab order must follow visual reading order (top-to-bottom, left-to-right)
- No keyboard traps (except modals, which must trap focus intentionally and release on close)
- Custom components must implement keyboard patterns from ARIA Authoring Practices Guide:
  - Dropdown menus: arrow keys to navigate, Enter to select, Escape to close
  - Modals: Tab cycles within, Shift+Tab reverses, Escape closes
  - Tabs: arrow keys switch tabs (not Tab key)
  - Checkboxes/radios in a group: arrow keys to navigate within the group

**Focus management**:
- Focus must be visible at all times (`outline: none` is forbidden without a custom focus style)
- When a modal opens: move focus to the first interactive element inside the modal
- When a modal closes: return focus to the element that triggered it
- When a page navigates: move focus to the main heading of the new page

**Skip links**:
- Provide a "Skip to main content" link as the first focusable element on every page
- It may be visually hidden until focused

**Timing**:
- No time limits on user actions unless absolutely necessary
- If time limits exist, warn users before expiry and provide a way to extend

### 10.3 Understandable

**Consistent navigation**: Navigation must appear in the same location and use the same labels on every page.

**Error identification**:
- If a form error is detected, identify the field in error and describe the error in text
- Never indicate errors only with color or icon — always include text

**Labels and instructions**:
- Every form input must have a visible label (not only placeholder)
- Instructions that are critical for completing the form must be shown before the form, not only as error messages

### 10.4 Robust

**ARIA usage**:
- Prefer native HTML semantics over ARIA roles when possible (`<button>` over `<div role="button">`)
- Use ARIA roles, properties, and states only when HTML semantics are insufficient
- Never use ARIA to override the semantics of a native element incorrectly

**Required ARIA patterns**:

| Component | Required ARIA |
|-----------|--------------|
| Modal | `role="dialog"`, `aria-modal="true"`, `aria-labelledby="{title-id}"` |
| Alert/Toast | `role="alert"` (for errors) or `aria-live="polite"` (for info/success) |
| Loading state | `role="status"`, `aria-live="polite"`, `aria-label="Loading"` |
| Icon button | `aria-label="Action name"`, `aria-hidden="true"` on the icon |
| Expandable section | `aria-expanded="true|false"` on the trigger |
| Dropdown menu | `role="menu"`, `role="menuitem"` on items, `aria-haspopup="true"` on trigger |
| Form error | `aria-describedby="{error-id}"` on input, `role="alert"` on error message |
| Required field | `aria-required="true"` on input |
| Disabled element | `aria-disabled="true"` (in addition to visual disabled state) |
| Progress bar | `role="progressbar"`, `aria-valuenow`, `aria-valuemin`, `aria-valuemax` |

---

## 11. Responsive Design Principles

### 11.1 Mobile-First Methodology

Write base styles for mobile, then add complexity via `min-width` breakpoints.

```css
/* Mobile first: base styles */
.card { padding: var(--space-4); }

/* Tablet and up */
@media (min-width: 768px) {
  .card { padding: var(--space-6); }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .card { padding: var(--space-8); }
}
```

Never use `max-width` queries in component styles — it leads to override specificity conflicts and makes components harder to reason about.

### 11.2 Content Priority on Mobile

Not all content is equally important. On mobile, define what stays and what transforms:

| Priority | Content | Mobile treatment |
|----------|---------|-----------------|
| P1 — Must show | Primary data, key actions | Full display |
| P2 — Should show | Secondary data, supporting actions | Condensed or behind "Show more" |
| P3 — Can hide | Metadata, timestamps, tertiary actions | Hidden behind interaction (tap to expand, overflow menu) |

### 11.3 Navigation Transformation Rules

| Pattern | Desktop | Mobile |
|---------|---------|--------|
| Sidebar | Visible, collapsible | Drawer (swipe or hamburger trigger) |
| Top navigation | Full label + icon | Icon only, or bottom bar |
| Secondary navigation | Horizontal tabs | Horizontal scroll tabs or dropdown |
| Breadcrumb | Full path | Parent link only ("← Back to {parent}") |
| Data table | Full columns | Horizontal scroll or card layout |
| Action buttons | Inline | Floating action button or bottom action bar |

### 11.4 Typography Adjustments Across Breakpoints

Scale down display and heading sizes on mobile to prevent overflow:

| Token | Desktop | Tablet | Mobile |
|-------|---------|--------|--------|
| Display XL | 36px | 30px | 24px |
| Display LG | 30px | 24px | 22px |
| Heading MD | 24px | 22px | 20px |
| Body text | unchanged | unchanged | unchanged |

---

## 12. Design System Governance

### 12.1 Token Naming Convention

Tokens must follow a three-level naming convention:
`--{category}-{subcategory}-{variant}`

| Category | Examples |
|----------|---------|
| color | `--color-brand-primary`, `--color-surface-base`, `--color-text-primary` |
| text | `--text-body-md`, `--text-heading-sm`, `--text-label-lg` |
| space | `--space-4`, `--space-8`, `--space-16` |
| radius | `--radius-sm`, `--radius-md`, `--radius-full` |
| shadow | `--shadow-sm`, `--shadow-md`, `--shadow-focus` |
| bp | `--bp-md`, `--bp-lg`, `--bp-xl` |

Never create tokens with product-specific or feature-specific names:
- Forbidden: `--color-dashboard-header-background`
- Correct: `--color-surface-overlay` (semantic, reusable)

### 12.2 Component Variant Principles

When adding variants to a component, apply these rules:

- A new variant is justified if it appears in 3+ distinct use cases
- A new variant must not create visual inconsistency with existing variants
- A new variant must be documented with its exact use case (not "sometimes needed")
- Variants that only differ in color are usually wrong — they signal that the color should come from the context, not the variant

### 12.3 When to Create a New Shared Component

Create a new shared component when:
- The same UI pattern appears in 2+ features
- The pattern requires consistent behavior across instances (not just consistent appearance)
- The pattern is complex enough that independent reimplementations would diverge

Do not create a shared component for:
- A pattern that appears only once
- Simple styled wrappers with no logic (use a CSS class instead)
- Feature-specific layouts

### 12.4 Deprecation Process

When a design decision in this knowledge file is superseded:
1. Update this file with the new guidance
2. Mark the old guidance as deprecated with `[DEPRECATED as of {date}: {reason}]`
3. Add a migration note explaining how existing implementations should be updated
4. The ux-ui-designer agent will apply new guidance to new features automatically
5. Existing features are updated in the next dedicated design-debt sprint

---

## 13. UX Writing Principles

### 13.1 Voice and Tone

**Voice** (consistent always):
- Clear: use the simplest word that accurately conveys the meaning
- Concise: remove every word that doesn't carry meaning
- Direct: use active voice, action verbs, second person ("You", not "The user")
- Human: write as a helpful colleague, not a legal document

**Tone** (adapts to context):
- For errors: calm, constructive, non-blaming ("Something went wrong" → "We couldn't save your changes. Check your connection and try again.")
- For destructive actions: serious, clear about consequences
- For empty states: encouraging, helpful
- For success states: brief, positive (don't over-celebrate routine actions)

### 13.2 Microcopy Rules

**Button labels**:
- Use action verbs: "Save", "Delete", "Create project", "Send invitation"
- Never: "OK", "Yes", "No", "Submit" (these require reading the surrounding context)
- Confirmation modals: the confirm button label must restate the action ("Delete project", not "Confirm")
- Loading state: use gerund form ("Saving...", "Deleting...", "Creating...")

**Placeholder text**:
- Use as an example of valid input, not as a label
- Correct: `placeholder="e.g., john@example.com"`
- Forbidden: `placeholder="Enter your email address"` (this is a label — use the actual label)

**Error messages**:
- Format: what happened + why + how to fix
- Never blame the user: "Invalid email" → "This email address doesn't look right. Check for typos."
- Be specific: "An error occurred" → "We couldn't connect to the server. Check your internet connection."

**Empty state copy**:
- Heading: noun-first ("No projects yet") or action-first ("Start your first project")
- Supporting text: one sentence describing what this section does
- CTA: verb + object ("Create project", "Invite team member")

**Confirmation dialog copy**:
- Title: restate the action ("Delete project?")
- Body: describe the consequence ("This will permanently delete all tasks and files in this project.")
- Confirm button: restate the action ("Delete project")
- Cancel button: "Cancel" (not "No", not "Go back")

### 13.3 Capitalization

| Context | Rule | Example |
|---------|------|---------|
| Page titles | Title Case | "Project Settings" |
| Section headings | Title Case | "Team Members" |
| Button labels | Sentence case | "Save changes" |
| Form labels | Sentence case | "Email address" |
| Navigation items | Title Case | "My Projects" |
| Error messages | Sentence case | "Something went wrong" |
| Placeholder text | Sentence case | "e.g., enter a name" |
| Tooltips | Sentence case | "Click to expand this section" |

### 13.4 Numbers and Units

- Spell out numbers zero through nine; use numerals for 10 and above
- Exception: always use numerals for: measurements (5px, 10MB), percentages (5%), currency ($5), counts in UI elements ("3 items selected")
- Time: use 12-hour format with am/pm for consumer apps, 24-hour for developer/ops tools
- File sizes: always include unit (KB, MB, GB) — never show raw bytes to end users