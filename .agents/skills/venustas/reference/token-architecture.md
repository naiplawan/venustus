# 3-Tier Token Architecture

Design tokens should follow a three-tier hierarchy. This prevents hardcoded values, enables theming (dark mode, white-label), and makes the system maintainable at scale.

## The Three Tiers

```
┌─────────────────────────────────────────────┐
│ COMPONENT TOKENS (use in code)              │
│ button-bg-primary → {semantic.action.primary}│
├─────────────────────────────────────────────┤
│ SEMANTIC TOKENS (use in design decisions)   │
│ action.primary → {primitive.blue.600}       │
├─────────────────────────────────────────────┤
│ PRIMITIVE TOKENS (raw palette, never ref)   │
│ blue.600 → oklch(55% 0.2 260)              │
└─────────────────────────────────────────────┘
```

### Primitive Tokens
Raw color/size values. The palette. **Never referenced directly in components.**

- Colors: 6+ hues × 11 shades (50–950), generated in OKLCH for perceptual uniformity
- Spacing: The raw numeric scale (4, 8, 12, 16, 24, 32, 48, 64, 96)
- Radii: The raw radius values (2, 4, 6, 8, 12, 16, 9999)
- Shadows: Raw shadow definitions

### Semantic Tokens
Purpose-based aliases. **Used in design decisions and general styling.**

```
Naming: {category}.{property}.{variant}
```

| Category | Examples |
|----------|---------|
| `action` | `action.primary`, `action.destructive`, `action.ghost` |
| `text` | `text.primary`, `text.secondary`, `text.link`, `text.on-action` |
| `surface` | `surface.page`, `surface.card`, `surface.sunken`, `surface.elevated` |
| `border` | `border.default`, `border.strong`, `border.focus`, `border.error` |
| `feedback` | `feedback.success`, `feedback.error`, `feedback.warning`, `feedback.info` |
| `space` | `space.page-padding`, `space.card-padding`, `space.stack-sm`, `space.stack-lg` |

### Component Tokens
Scoped to specific components. **Used in component implementations.**

```
Naming: {component}.{element}-{property}-{state}
```

Examples:
- `button.primary-bg` → `{semantic.action.primary}`
- `button.primary-bg-hover` → `{semantic.action.primary-hover}`
- `input.border-focus` → `{semantic.border.focus}`
- `card.padding` → `{semantic.space.card-padding}`

## Dark Mode Strategy

Dark mode works by swapping at the **semantic** level. Primitives stay the same.

```css
/* Light (default) */
:root {
  --text-primary: var(--gray-900);    /* dark text */
  --surface-page: var(--gray-50);     /* light bg */
  --surface-card: var(--white);
  --border-default: var(--gray-200);
}

/* Dark */
[data-theme="dark"], .dark {
  --text-primary: var(--gray-50);     /* light text */
  --surface-page: var(--gray-950);    /* dark bg */
  --surface-card: var(--gray-900);
  --border-default: var(--gray-800);
}
```

For React Native / SwiftUI, use platform theming:
- **RN**: `useColorScheme()` + conditional token objects
- **SwiftUI**: Asset Catalog with "Any/Dark" appearances, or `@Environment(\.colorScheme)`
- **Flutter**: `ThemeData` + `ThemeData.dark()`

## DTCG Format

When generating token files, use the Design Tokens Community Group standard:

```json
{
  "semantic": {
    "action": {
      "primary": {
        "$type": "color",
        "$value": "{primitive.blue.600}",
        "$description": "Primary action color for buttons and interactive elements"
      }
    }
  }
}
```

This format is consumed by tools like Style Dictionary, Figma Tokens, and Tailwind v4.

## Token Capture Checklist

When running `/venustas teach`, capture tokens at all three tiers:

1. **Primitives**: What raw colors, sizes, radii exist? (grep CSS variables, Tailwind config, theme files)
2. **Semantic**: Are there purpose-based aliases? (surface, text, action, feedback)
3. **Component**: Are tokens scoped to specific components? (button-bg, input-border)
4. **Dark mode**: How does theming work? (CSS variables swap? Separate token set? Platform API?)
5. **Gaps**: Which tiers are missing? (Most projects have primitives but lack semantic/component layers)

If a project only has primitives, recommend building the semantic layer as the first design system investment — it's the highest ROI tier.
