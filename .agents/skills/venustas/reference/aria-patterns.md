# ARIA Patterns Reference

Concrete implementation patterns for accessible interactive components. Each pattern shows the HTML structure, ARIA roles/properties, and keyboard interactions per the WAI-ARIA Authoring Practices Guide.

Consult this reference when building interactive components. Getting ARIA wrong is worse than omitting it — incorrect roles confuse screen readers more than missing ones.

---

## Quick Reference: When to Use What

| Component | Role | Key ARIA | Keyboard |
|-----------|------|----------|----------|
| Accordion | button + region | `aria-expanded`, `aria-controls` | Enter/Space toggle |
| Alert | `role="alert"` | implicit `aria-live="assertive"` | None (announced) |
| Dialog/Modal | `role="dialog"` | `aria-modal`, `aria-labelledby` | Esc closes, focus trap |
| Dropdown Menu | `role="menu"` + `menuitem` | `aria-expanded`, `aria-haspopup` | Arrow keys navigate |
| Tabs | `role="tablist"` + `tab` + `tabpanel` | `aria-selected`, `aria-controls` | Arrow keys switch |
| Tooltip | `role="tooltip"` | `aria-describedby` | Esc dismisses |
| Combobox | `role="combobox"` | `aria-expanded`, `aria-activedescendant` | Arrow keys + typing |
| Toggle | button | `aria-pressed` | Enter/Space toggle |
| Switch | `role="switch"` | `aria-checked` | Enter/Space toggle |
| Toast | `role="status"` | `aria-live="polite"` | None (announced) |

---

## Patterns

### Dialog / Modal

```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
  <p>This action cannot be undone.</p>
  <button>Cancel</button>
  <button>Delete</button>
</div>
```

**Keyboard**: Esc closes. Tab cycles within dialog (focus trap). Focus moves to first focusable element on open, returns to trigger on close.

**Focus trap pattern** (JS):
```javascript
// On open: save trigger, move focus into dialog
const trigger = document.activeElement;
dialog.querySelector('[autofocus], button, [href], input').focus();

// On close: restore focus
trigger.focus();

// Tab trap: redirect Tab from last→first and Shift+Tab from first→last
```

### Tabs

```html
<div role="tablist" aria-label="Settings">
  <button role="tab" aria-selected="true" aria-controls="panel-1" id="tab-1">
    Profile
  </button>
  <button role="tab" aria-selected="false" aria-controls="panel-2" id="tab-2" tabindex="-1">
    Security
  </button>
</div>
<div role="tabpanel" id="panel-1" aria-labelledby="tab-1">
  Profile content...
</div>
<div role="tabpanel" id="panel-2" aria-labelledby="tab-2" hidden>
  Security content...
</div>
```

**Keyboard**: Left/Right arrow moves between tabs. Home/End jump to first/last. Only the active tab is in the tab order (`tabindex="-1"` on inactive).

### Accordion

```html
<h3>
  <button aria-expanded="false" aria-controls="panel-1">Section Title</button>
</h3>
<div id="panel-1" role="region" aria-labelledby="header-1" hidden>
  Panel content...
</div>
```

**Keyboard**: Enter/Space toggles. Optionally Up/Down arrows move between headers.

### Dropdown Menu

```html
<button aria-expanded="false" aria-haspopup="menu" aria-controls="menu-1">
  Actions
</button>
<ul role="menu" id="menu-1" hidden>
  <li role="menuitem">Edit</li>
  <li role="menuitem">Duplicate</li>
  <li role="separator"></li>
  <li role="menuitem">Delete</li>
</ul>
```

**Keyboard**: Enter/Space opens. Arrow keys navigate items. Esc closes. Home/End jump. Type-ahead (typing "D" jumps to "Delete").

### Combobox (Autocomplete)

```html
<label for="search">Search</label>
<input role="combobox" id="search"
  aria-expanded="false"
  aria-autocomplete="list"
  aria-controls="results"
  aria-activedescendant="">
<ul role="listbox" id="results" hidden>
  <li role="option" id="opt-1">Result 1</li>
  <li role="option" id="opt-2">Result 2</li>
</ul>
```

**Keyboard**: Type to filter. Down arrow opens/navigates list. Enter selects. Esc closes. `aria-activedescendant` points to highlighted option (no actual focus move).

### Toast / Notification

```html
<!-- Container exists in DOM, content injected dynamically -->
<div role="status" aria-live="polite" aria-atomic="true">
  <!-- Injected: "File saved successfully" → announced by screen reader -->
</div>
```

Use `role="status"` + `aria-live="polite"` for non-urgent notifications. Use `role="alert"` for urgent messages (errors, warnings).

### Switch / Toggle

```html
<button role="switch" aria-checked="false" aria-label="Dark mode">
  <span class="switch-track"><span class="switch-thumb"></span></span>
</button>
```

**Keyboard**: Enter/Space toggles. Announces "Dark mode, switch, off" → "Dark mode, switch, on".

**Note**: Use `role="switch"` + `aria-checked` for on/off toggles. Use `aria-pressed` for toggle buttons (bold, italic). They announce differently.

---

## Common Mistakes

1. **Using `role="button"` on a `<div>`** — Use `<button>` instead. You get keyboard, focus, and activation for free.
2. **`aria-label` on non-interactive elements** — Only use on interactive elements or landmarks. For descriptive text, use visible text or `aria-describedby`.
3. **Hiding focus indicators** — `outline: none` without a replacement is an accessibility violation. Always provide a visible focus style.
4. **Missing live regions** — Dynamic content changes (toasts, form validation, loading states) need `aria-live` regions to be announced.
5. **`tabindex > 0`** — Never use positive tabindex values. They break the natural tab order. Use 0 (in order) or -1 (programmatically focusable).

---

## React Native Accessibility

React Native maps to platform a11y APIs differently:

```jsx
<Pressable
  accessibilityRole="button"
  accessibilityLabel="Add to cart"
  accessibilityState={{ disabled: false }}
  accessibilityHint="Adds this item to your shopping cart"
>
  <Text>Add to Cart</Text>
</Pressable>
```

| Web ARIA | React Native Prop |
|----------|------------------|
| `role` | `accessibilityRole` |
| `aria-label` | `accessibilityLabel` |
| `aria-checked/disabled/selected/expanded` | `accessibilityState` |
| `aria-valuenow/min/max` | `accessibilityValue` |
| `aria-live` | `accessibilityLiveRegion` ("polite"/"assertive") |
| `aria-hidden` | `importantForAccessibility="no-hide-descendants"` |

## Flutter Accessibility

```dart
Semantics(
  button: true,
  label: 'Add to cart',
  hint: 'Adds this item to your shopping cart',
  enabled: true,
  child: GestureDetector(
    onTap: _addToCart,
    child: Container(/* button UI */),
  ),
)
```

Use `Semantics` widget to wrap custom interactive elements. Built-in widgets (`ElevatedButton`, `TextField`) handle semantics automatically.
