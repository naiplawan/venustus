# Venustas

Design skills, anti-pattern detection, and quality auditing for AI coding agents.

Venustas helps AI agents produce distinctive, production-grade frontend interfaces instead of generic "AI slop." It provides a CLI for detecting UI anti-patterns and a suite of 21 design skills that guide agents through typography, color, layout, animation, accessibility, and more.

## Installation

```bash
npm install venustus
```

Requires Node.js >= 18.

## CLI Usage

```bash
# Scan local files or directories for UI anti-patterns
npx venustas detect [file-or-dir...]

# Scan a live URL (uses Puppeteer)
npx venustas detect https://example.com

# Regex-only mode (skip jsdom)
npx venustas detect --fast src/

# JSON output
npx venustas detect --json src/

# Start a browser detection overlay server
npx venustas live [--port=PORT]

# Stop the live server
npx venustas live stop
```

Exit codes: `0` = clean, `2` = findings.

## Skills

Venustas ships 21 skills for Claude Code (or any compatible agent). Each skill targets a specific design concern:

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

## Programmatic API

```js
import { detectAntipatterns } from 'venustus';
```

A browser build is also available:

```html
<script src="detect-antipatterns-browser.js"></script>
<script>window.venustasScan();</script>
```

## License

Apache-2.0
