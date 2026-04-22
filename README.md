# Venustus

Design skills, anti-pattern detection, and quality auditing for AI coding agents.

Venustus helps AI agents produce distinctive, production-grade frontend interfaces instead of generic "AI slop." It ships a CLI that scans for 25 UI anti-patterns, a browser extension for live inspection, and a suite of design skills that guide agents through typography, color, layout, animation, accessibility, and more.

## Quick start

```bash
# Install
npm install venustus        # or: bun add venustus

# Scan a directory, a file, or a live URL
bunx venustas detect src/
bunx venustas detect https://example.com

# Drop design skills into your project
bunx venustas skills install
```

Requires Node.js >= 18. Exit codes: `0` = clean, `2` = findings.

## Skills

Venustus ships 21 skills registered as `agent.sh`-compatible skills. They work with any compatible AI harness — Claude Code, Cursor, Gemini, Codex, and more. After install, invoke them from your harness (e.g. `/venustas`, `/venustas-critique`).

| Skill | Purpose |
|-------|---------|
| **venustas** | Core skill — full shape-then-build flow and design context setup |
| **venustas-adapt** | Responsive design, cross-device, cross-platform adaptation |
| **venustas-animate** | Purposeful animations and micro-interactions |
| **venustas-arrange** | Layout, spacing, and visual rhythm |
| **venustas-audit** | Technical quality checks (a11y, performance, theming) |
| **venustas-bolder** | Amplify safe or boring designs |
| **venustas-clarify** | UX copy, error messages, and microcopy |
| **venustas-colorize** | Add strategic color to monochromatic interfaces |
| **venustas-critique** | UX evaluation with quantitative scoring |
| **venustas-delight** | Joy, personality, and unexpected touches |
| **venustas-distill** | Strip designs to their essence |
| **venustas-extract** | Extract reusable components and design tokens |
| **venustas-harden** | Error handling, i18n, edge cases |
| **venustas-normalize** | Realign UI to design system standards |
| **venustas-onboard** | Onboarding flows and first-run experiences |
| **venustas-optimize** | UI performance (loading, rendering, bundle size) |
| **venustas-overdrive** | Technically ambitious effects (shaders, WebGL, spring physics) |
| **venustas-polish** | Final quality pass before shipping |
| **venustas-quieter** | Tone down visually aggressive designs |
| **venustas-shape** | Plan UX/UI before writing code |
| **venustas-typeset** | Typography fixes and hierarchy |

```bash
bunx venustas skills install   # register under .agents/skills/
bunx venustas skills update    # upgrade to the latest version
```

## CLI

```bash
# Detection
bunx venustas detect [file-or-dir...]            # local scan (HTML, CSS, JSX, TSX, Vue, Svelte, Astro...)
bunx venustas detect https://example.com         # live URL scan (Puppeteer)
bunx venustas detect --fast src/                 # regex-only, skip jsdom
bunx venustas detect --json src/                 # machine-readable output

# Live overlay server
bunx venustas live [--port=PORT]                 # start
bunx venustas live stop                          # stop

# Skills
bunx venustas skills install
bunx venustas skills update
```

### Detection modes

The CLI picks the best detection method per input:

- **HTML files** — jsdom with computed styles (catches linked CSS, inline styles, Tailwind classes)
- **Non-HTML source** (CSS, JSX, TSX, Vue, Svelte, Astro) — regex pattern matching
- **URLs** — Puppeteer full browser rendering with live DOM inspection
- **`--fast`** — forces regex-only mode for all files

When scanning a directory, Venustus auto-detects running dev servers (Next.js, Vite, SvelteKit, Nuxt, Astro, Angular, Remix) and suggests scanning the URL for higher-fidelity results.

## Browser extension

A Chrome DevTools extension for live page inspection:

- Real-time overlay highlights anti-patterns directly on the page
- Page-level findings in a scrollable top banner
- Hover any highlighted element for detailed findings
- Spotlight mode dims the page and focuses one finding
- Toggle overlay visibility from the banner

## Programmatic API

```js
import { detectAntipatterns } from 'venustus';
```

### Browser build

Drop the browser detector onto any page:

```html
<script src="detect-antipatterns-browser.js"></script>
<script>
  // Auto-scans on load. Re-scan manually:
  window.venustasScan();
</script>
```

Or inject from a running live server:

```js
const s = document.createElement('script');
s.src = 'http://localhost:8400/detect.js';
document.head.appendChild(s);
```

## What it detects

### AI slop (tells that code was AI-generated)

| Pattern | What it catches |
|---------|-----------------|
| Side-tab accent border | Thick colored border on one side of a card |
| Border accent on rounded element | Accent border clashing with rounded corners |
| Overused font | Inter, Roboto, Open Sans, Lato, Montserrat, Arial |
| Single font for everything | Only one font family across the entire page |
| Flat type hierarchy | Font sizes too close together, no visual hierarchy |
| Gradient text | `background-clip: text` with a gradient |
| AI color palette | Purple/violet gradients and cyan-on-dark schemes |
| Nested cards | Cards inside cards creating visual noise |
| Monotonous spacing | Same padding/margin value used everywhere |
| Everything centered | Every text element center-aligned |
| Bounce/elastic easing | Bounce, elastic, wobble, spring animations |
| Dark mode with glowing accents | Colored box-shadow glows on dark backgrounds |
| Icon tile stacked above heading | Rounded-square icon container above every heading |

### Quality issues (design and accessibility problems)

| Pattern | What it catches |
|---------|-----------------|
| Pure black background | Harsh `#000000` backgrounds |
| Gray text on colored background | Low-contrast gray text on chromatic backgrounds |
| Low contrast text | WCAG AA failures (4.5:1 body, 3:1 large text) |
| Layout property animation | Animating `width`, `height`, `padding`, `margin` |
| Line length too long | Text lines wider than ~80 characters |
| Cramped padding | Text too close to container edges |
| Tight line height | Line height below 1.3× font size |
| Skipped heading level | Heading hierarchy gaps (e.g. h1 → h3) |
| Justified text | `text-align: justify` without `hyphens: auto` |
| Tiny body text | Body text below 12px |
| All-caps body text | Long uppercase passages |
| Wide letter spacing on body text | Tracking above 0.05em on body content |

## Supported file types

HTML, CSS, SCSS, Less, JSX, TSX, JS, TS, Vue, Svelte, Astro.

## License

Apache-2.0
