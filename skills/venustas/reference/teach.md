# Teach Mode

A one-time setup that gathers design context for the project. Run when `/venustas teach` is invoked, or when any design skill needs context and none exists.

## Step 1: Explore the Codebase

Before asking questions, thoroughly scan the project to discover what you can:

- **README and docs**: Project purpose, target audience, any stated goals
- **Package.json / config files**: Tech stack, dependencies, existing design libraries
- **Existing components**: Current design patterns, spacing, typography in use
- **Brand assets**: Logos, favicons, color values already defined
- **Design tokens / CSS variables**: Existing color palettes, font stacks, spacing scales
- **Any style guides or brand documentation**

**Extract real tokens** — don't just note that they exist. Capture the actual values:
- Grep for CSS custom properties (`--color-*`, `--space-*`, `--font-*`, `--radius-*`)
- Read Tailwind config, theme files, or design token JSON if present
- Check for token packages (e.g., `@company/tokens`, `style-dictionary` output)
- For React Native / Flutter: read theme objects, `ThemeData`, or style constants
- Record the complete list of colors, spacing scale, font families, font sizes, border radii, shadows, and breakpoints actually in use

Note what you've learned and what remains unclear.

## Step 1b: Check for Figma Integration (Optional)

If a Figma MCP server is available in the environment:
1. Ask the user for the Figma file URL or key for the project's design source
2. Use the Figma MCP to pull the current design tokens, component inventory, and style definitions
3. Cross-reference Figma tokens with codebase tokens — flag any drift between design and code

If no Figma MCP is available, ask the user:
- "Do you have a Figma file, Zeplin, or other design source of truth? If so, I can't connect to it directly, but you can paste key specs (colors, spacing, fonts) or export a design tokens JSON and I'll incorporate them."

## Step 2: Ask UX-Focused Questions

STOP and call the AskUserQuestion tool to clarify. Focus only on what you couldn't infer from the codebase:

### Users & Purpose
- Who uses this? What's their context when using it?
- What job are they trying to get done?
- What emotions should the interface evoke? (confidence, delight, calm, urgency, etc.)

### Brand & Personality
- How would you describe the brand personality in 3 words?
- Any reference sites or apps that capture the right feel? What specifically about them?
- What should this explicitly NOT look like? Any anti-references?

### Aesthetic Preferences
- Any strong preferences for visual direction? (minimal, bold, elegant, playful, technical, organic, etc.)
- Light mode, dark mode, or both?
- Any colors that must be used or avoided?

### Brand Font & Anti-Pattern Overrides
- Does your brand explicitly use any fonts from the standard web font list (Inter, Roboto, etc.)? If so, those will be marked as approved brand fonts, exempt from the AI-default ban.
- Are there any design patterns the skill normally discourages (side-stripe borders, glassmorphism, etc.) that are intentional in your design system?

### Platform & Device Context
- Is this a web app, mobile app (React Native, Flutter, SwiftUI), or hybrid?
- What are the primary target devices and screen sizes?
- Any platform-specific design guidelines to follow (Material Design, Human Interface Guidelines)?

### Accessibility & Inclusion
- Specific accessibility requirements? (WCAG level, known user needs)
- Considerations for reduced motion, color blindness, or other accommodations?

Skip questions where the answer is already clear from the codebase exploration.

## Step 3: Write Design Context

Synthesize your findings and the user's answers into a structured `.venustas.md` with **both** human-readable context and machine-readable tokens:

```markdown
## Design Context

### Users
[Who they are, their context, the job to be done]

### Brand Personality
[Voice, tone, 3-word personality, emotional goals]

### Aesthetic Direction
[Visual tone, references, anti-references, theme]

### Platform
[Web / React Native / Flutter / SwiftUI / etc. Target devices and OS]

### Design Principles
[3-5 principles derived from the conversation that should guide all design decisions]

### Brand Overrides
[List any fonts, patterns, or anti-patterns that are explicitly approved for this project despite being on the default ban list. E.g., "Inter is our brand font — approved." or "Left-border accents are part of our card system — approved."]

## Design Tokens

### Colors
[Paste the actual token names and values extracted from the codebase]
- `--color-primary`: oklch(55% 0.15 250)
- `--color-surface`: oklch(98% 0.005 250)
- ...

### Typography
- **Display font**: [name] (loaded from [source])
- **Body font**: [name] (loaded from [source])
- **Scale**: [list the actual sizes used, e.g., 12/14/16/20/24/32/48]
- **Weights**: [list weights used]

### Spacing
[The actual spacing scale, e.g., 4/8/12/16/24/32/48/64/96]

### Border Radii
[e.g., --radius-sm: 4px, --radius-md: 8px, --radius-lg: 16px]

### Shadows
[List shadow tokens if they exist]

### Breakpoints
[List responsive breakpoints]

## Figma Source
[URL or "Not connected". If tokens were pulled from Figma, note the sync date]

## Change Log
[Track when this file was last updated and what changed, so drift is visible]
- [date]: Initial setup via /venustas teach
```

Write this to `.venustas.md` in the project root. If the file already exists, update sections in place — do not overwrite the entire file.

Then STOP and call the AskUserQuestion tool to ask whether they'd also like the Design Context appended to CLAUDE.md. If yes, append or update the section there as well.

Confirm completion and summarize the key design principles and token counts that will now guide all future work.
