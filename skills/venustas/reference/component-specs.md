# Component Specification Template

When designing or extracting components, every component must meet this quality bar. Use this template to ensure completeness — missing specs cause rework.

---

## Component Quality Bar

Every component needs these 6 sections. Skip none.

### 1. Anatomy
Visual structure breakdown showing the parts of the component.

```
[icon?] [label] [trailing-icon?]
└──────── pressable container ────────┘
```

Name each part. Mark optional parts with `?`. This tells developers what to build and designers what to spec.

### 2. Variants
All visual variants with their use case and token mapping.

| Variant | Use Case | Token Pattern |
|---------|----------|---------------|
| Primary | Main CTA — one per visible area | `component.{name}.primary-*` |
| Secondary | Supporting actions | `component.{name}.secondary-*` |
| Ghost | Tertiary, inline actions | `component.{name}.ghost-*` |
| Destructive | Irreversible actions | `component.{name}.destructive-*` |

**Rule**: If you can't describe when to use a variant in one sentence, it shouldn't exist.

### 3. Sizes
Exact dimensions for each size. No "auto" or "responsive" — give numbers.

| Size | Height | Padding (H × V) | Font | Icon Size |
|------|--------|-----------------|------|-----------|
| sm | 32px | 12px × 8px | 14px | 16px |
| md | 40px | 16px × 10px | 16px | 20px |
| lg | 48px | 24px × 12px | 18px | 24px |

Touch targets: sm is 32px (below 44px minimum for mobile primary actions). Use md or lg for touch-primary interfaces.

### 4. States (The 8-State Matrix)

Every interactive component must define these states. Not all apply to every component — but you must consciously decide which do.

| # | State | Required? | Visual Treatment | Token Suffix |
|---|-------|-----------|-----------------|--------------|
| 1 | **Default** | Always | Base appearance | (base) |
| 2 | **Hover** | Always (web) | Background shift +1 shade | `-hover` |
| 3 | **Focus** | Always | Focus ring (2px offset, 3:1 contrast) | `-focus` |
| 4 | **Active/Pressed** | Always | Background shift +2 shades | `-active` |
| 5 | **Disabled** | Always | opacity: 0.5, no pointer events | `-disabled` |
| 6 | **Loading** | If async | Spinner replaces icon, `aria-busy` | `-loading` |
| 7 | **Error** | If input | Red border + error message | `-error` |
| 8 | **Selected** | If selectable | Accent background, checkmark | `-selected` |

**State Matrix Example** (fill this out for each component):

| State | Background | Border | Text | Shadow | Additional |
|-------|-----------|--------|------|--------|-----------|
| Default | surface.card | border.default | text.primary | shadow.sm | — |
| Hover | surface.card | border.strong | text.primary | shadow.md | cursor: pointer |
| Focus | surface.card | border.focus | text.primary | focus-ring | 2px offset ring |
| Active | action.primary-light | border.focus | text.primary | none | — |
| Disabled | surface.sunken | border.default | text.tertiary | none | opacity: 0.5 |

### 5. Token Mapping
Every visual value must trace to a design token. Zero hardcoded values.

```
Background:  var(--surface-card)        ← semantic.surface.card
Border:      var(--border-default)      ← semantic.border.default
Text:        var(--text-primary)        ← semantic.text.primary
Padding:     var(--space-card-padding)  ← semantic.space.card-padding
Radius:      var(--radius-card)         ← semantic.radius.card
Shadow:      var(--shadow-sm)           ← elevation.sm
```

If a value doesn't match any token: discuss with design, potentially add a new token. Never hardcode.

### 6. Accessibility
Consult [aria-patterns.md](aria-patterns.md) for the full ARIA pattern.

Minimum documentation per component:
- **Semantic element**: What HTML element? (`<button>`, `<input>`, `<dialog>`, etc.)
- **ARIA role**: If custom element, what role?
- **ARIA properties**: What states/properties? (`aria-expanded`, `aria-pressed`, etc.)
- **Keyboard model**: What keys do what?
- **Screen reader announcement**: What does it announce? (e.g., "Save, button" or "Dark mode, switch, off")
- **Focus management**: Where does focus go on open/close/action?

---

## Atomic Design Hierarchy

Components belong to one of four levels:

### Atoms
Smallest, indivisible elements. Single responsibility.
- Button, Input, Label, Icon, Badge, Avatar, Checkbox, Radio, Toggle, Tooltip
- **Test**: Can you break this into smaller meaningful parts? If no → atom.

### Molecules
Combine 2-3 atoms for a single task.
- Form Field (Label + Input + Error), Search Bar, Card, Nav Item, Alert, Dropdown
- **Test**: Does this combine atoms to do one job? If yes → molecule.

### Organisms
Complex sections composed of molecules and atoms.
- Header, Sidebar, Form, Data Table, Modal, Drawer, Bottom Sheet
- **Test**: Is this a distinct section of a page? If yes → organism.

### Templates
Page-level layout compositions.
- Dashboard, Auth (Login/Register), Settings, List/Detail, Onboarding
- **Test**: Does this define the layout structure for a page type? If yes → template.

---

## Definition of Done

A component is done when:

- [ ] **Functional**: All variants render, all states work, all edge cases handled
- [ ] **Visual**: Pixel-accurate, all tokens applied, responsive, dark mode
- [ ] **Accessible**: Keyboard nav, screen reader tested, contrast verified, target sizes met
- [ ] **Code quality**: Typed (no `any`), composable (accepts refs, className), tested
- [ ] **Documented**: Anatomy, variants, states, tokens, a11y notes all written
