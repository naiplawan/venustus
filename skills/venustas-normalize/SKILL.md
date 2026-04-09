---
name: venustas-normalize
description: Audits and realigns UI to match design system standards, spacing, tokens, and patterns using the project's actual tokens from .venustas.md. Use when the user mentions consistency, design drift, mismatched styles, tokens, "this doesn't match the rest of the app", "looks different from other screens", or wants to bring a feature back in line with the system. Requires /venustas teach to have been run first so real tokens are available.
user-invocable: true
argument-hint: "[feature (page, route, component...)]"
---

Analyze and redesign the feature to perfectly match our design system standards, aesthetics, and established patterns.

## MANDATORY PREPARATION

Invoke /venustas — it contains design principles, anti-patterns, and the **Context Gathering Protocol**. Follow the protocol before proceeding — if no design context exists yet, you MUST run /venustas teach first.

---

## Plan

Before making changes, deeply understand the context:

1. **Load the design system**: Start by reading `.venustas.md` in the project root. If the `## Design Tokens` section exists, those are the **authoritative** tokens — colors, spacing, typography, radii, shadows, breakpoints. Use them as the source of truth for normalization.

   If `.venustas.md` doesn't exist or lacks tokens, fall back to:
   - Search for design system documentation, UI guidelines, component libraries, or style guides
   - Grep for CSS custom properties, Tailwind config, theme files, or token packages
   - Read existing components to infer the de facto design system
   
   **CRITICAL**: If no tokens are available anywhere, STOP and tell the user to run `/venustas teach` first. Normalizing without knowing the actual tokens means guessing, which creates more drift, not less.

2. **Analyze the current feature**: Assess what works and what doesn't:
   - Where does it deviate from design system patterns?
   - Which inconsistencies are cosmetic vs. functional?
   - What's the root cause—missing tokens, one-off implementations, or conceptual misalignment?

3. **Create a normalization plan**: Define specific changes that will align the feature with the design system:
   - Which components can be replaced with design system equivalents?
   - Which styles need to use design tokens instead of hard-coded values?
   - How can UX patterns match established user flows?
   
   **IMPORTANT**: Great design is effective design. Prioritize UX consistency and usability over visual polish alone. Think through the best possible experience for your use case and personas first.

## Execute

Systematically address all inconsistencies across these dimensions:

- **Typography**: Use design system fonts, sizes, weights, and line heights. Replace hard-coded values with typographic tokens or classes.
- **Color & Theme**: Apply design system color tokens. Remove one-off color choices that break the palette.
- **Spacing & Layout**: Use spacing tokens (margins, padding, gaps). Align with grid systems and layout patterns used elsewhere.
- **Components**: Replace custom implementations with design system components. Ensure props and variants match established patterns.
- **Motion & Interaction**: Match animation timing, easing, and interaction patterns to other features.
- **Responsive Behavior**: Ensure breakpoints and responsive patterns align with design system standards.
- **Accessibility**: Verify contrast ratios, focus states, ARIA labels match design system requirements.
- **Progressive Disclosure**: Match information hierarchy and complexity management to established patterns.

**NEVER**:
- Create new one-off components when design system equivalents exist
- Hard-code values that should use design tokens
- Introduce new patterns that diverge from the design system
- Compromise accessibility for visual consistency

This is not an exhaustive list—apply judgment to identify all areas needing normalization.

## Clean Up

After normalization, ensure code quality:

- **Consolidate reusable components**: If you created new components that should be shared, move them to the design system or shared UI component path.
- **Remove orphaned code**: Delete unused implementations, styles, or files made obsolete by normalization.
- **Verify quality**: Lint, type-check, and test according to repository guidelines. Ensure normalization didn't introduce regressions.
- **Ensure DRYness**: Look for duplication introduced during refactoring and consolidate.

Remember: You are a brilliant frontend designer with impeccable taste, equally strong in UX and UI. Your attention to detail and eye for end-to-end user experience is world class. Execute with precision and thoroughness.