---
name: venustas-shape
description: Plan the UX and UI for a feature before writing code. Runs a structured discovery interview, then produces a design brief and optional text wireframe that guide implementation. Use when the user says "let's plan this feature", "how should this look", "I need to design", "what's the UX for", "wireframe this", "sketch the layout", or wants to think through a screen/flow/component before building it. Also use when a feature is complex enough that jumping straight to code would produce a generic result.
user-invocable: true
argument-hint: "[feature to shape]"
---

## MANDATORY PREPARATION

Invoke /venustas, which contains design principles, anti-patterns, and the **Context Gathering Protocol**. Follow the protocol before proceeding. If no design context exists yet, you MUST run /venustas teach first.

---

Shape the UX and UI for a feature before any code is written. This skill produces a **design brief**: a structured artifact that guides implementation through discovery, not guesswork.

**Scope**: Design planning only. This skill does NOT write code. It produces the thinking that makes code good.

**Output**: A design brief that can be handed off to /venustas craft, /venustas, or any other implementation skill.

## Philosophy

Most AI-generated UIs fail not because of bad code, but because of skipped thinking. They jump to "here's a card grid" without asking "what is the user trying to accomplish?" This skill inverts that: understand deeply first, so implementation is precise.

## Fidelity Ladder

Design confidence should build incrementally. Don't jump to high-fidelity before validating the concept. This skill operates at **Level 1-2** of this ladder:

| Level | Output | When to Use |
|-------|--------|------------|
| **1. Content-first** | Text outline, information hierarchy | Always start here — validate what info is needed |
| **2. Design brief** | Layout strategy, interaction model, states | This skill's output → hand off to /venustas |
| **3. Low-fi prototype** | Rough clickable flow | When the user wants to test a flow before polishing |
| **4. High-fi mockup** | Polished design with tokens applied | /venustas craft |
| **5. Production code** | Working implementation | /venustas or direct coding |

The design brief from this skill is the bridge between "what are we building?" (Level 1) and "build it beautifully" (Levels 4-5). Skipping it produces generic output.

## Phase 1: Discovery Interview

**Do NOT write any code or make any design decisions during this phase.** Your only job is to understand the feature deeply enough to make excellent design decisions later.

Ask these questions in conversation, adapting based on answers. Don't dump them all at once; have a natural dialogue. STOP and call the AskUserQuestion tool to clarify.

### Purpose & Context
- What is this feature for? What problem does it solve?
- Who specifically will use it? (Not "users"; be specific: role, context, frequency)
- What does success look like? How will you know this feature is working?
- What's the user's state of mind when they reach this feature? (Rushed? Exploring? Anxious? Focused?)

### Content & Data
- What content or data does this feature display or collect?
- What are the realistic ranges? (Minimum, typical, maximum, e.g., 0 items, 5 items, 500 items)
- What are the edge cases? (Empty state, error state, first-time use, power user)
- Is any content dynamic? What changes and how often?

### Design Goals
- What's the single most important thing a user should do or understand here?
- What should this feel like? (Fast/efficient? Calm/trustworthy? Fun/playful? Premium/refined?)
- Are there existing patterns in the product this should be consistent with?
- Are there specific examples (inside or outside the product) that capture what you're going for?

### Constraints
- Are there technical constraints? (Framework, performance budget, browser support)
- Are there content constraints? (Localization, dynamic text length, user-generated content)
- Mobile/responsive requirements?
- Accessibility requirements beyond WCAG AA?

### Anti-Goals
- What should this NOT be? What would be a wrong direction?
- What's the biggest risk of getting this wrong?

## Phase 2: Design Brief

After the interview, synthesize everything into a structured design brief. Present it to the user for confirmation before considering this skill complete.

### Brief Structure

**1. Feature Summary** (2-3 sentences)
What this is, who it's for, what it needs to accomplish.

**2. Primary User Action**
The single most important thing a user should do or understand here.

**3. Design Direction**
How this should feel. What aesthetic approach fits. Reference the project's design context from `.venustas.md` and explain how this feature should express it.

**4. Layout Strategy**
High-level spatial approach: what gets emphasis, what's secondary, how information flows. Describe the visual hierarchy and rhythm, not specific CSS.

**5. Key States**
List every state the feature needs: default, empty, loading, error, success, edge cases. For each, note what the user needs to see and feel.

**6. Interaction Model**
How users interact with this feature. What happens on click, hover, scroll? What feedback do they get? What's the flow from entry to completion?

**7. Content Requirements**
What copy, labels, empty state messages, error messages, and microcopy are needed. Note any dynamic content and its realistic ranges.

**8. Recommended References**
Based on the brief, list which venustas reference files would be most valuable during implementation (e.g., spatial-design.md for complex layouts, motion-design.md for animated features, interaction-design.md for form-heavy features).

**9. Open Questions**
Anything unresolved that the implementer should resolve during build.

---

STOP and call the AskUserQuestion tool to clarify. Get explicit confirmation of the brief before finishing. If the user disagrees with any part, revisit the relevant discovery questions.

Once confirmed, the brief is complete. The user can now hand it to /venustas craft to build the feature, or use it to guide any other implementation approach.

---

## Phase 3: Text Wireframe (Optional)

If the user asks for a wireframe, or the feature is complex enough that a visual layout sketch would help before coding, generate a **text-based wireframe** — an ASCII/markdown representation of the screen layout.

This is Fidelity Level 2 on the ladder. It validates layout and information hierarchy before investing in code.

### Format

```
┌─────────────────────────────────────────┐
│ [←] Title                    [...] [🔔] │  ← Navigation bar
├─────────────────────────────────────────┤
│                                         │
│  ┌───────────────────────────────────┐  │
│  │        [ Hero Image/Carousel ]    │  │  ← Primary content zone
│  └───────────────────────────────────┘  │
│                                         │
│  Product Title                          │
│  $XX.XX  ~~$XX.XX~~  -20%              │  ← Price + discount
│  ★★★★☆ (123 reviews)                   │
│                                         │
│  ┌──┐ ┌──┐ ┌──┐ ┌──┐                  │
│  │ S│ │ M│ │ L│ │XL│   Size selector  │  ← Interactive elements
│  └──┘ └──┘ └──┘ └──┘                  │
│                                         │
│  Description text goes here...          │  ← Secondary content
│  [Read more]                            │
│                                         │
├─────────────────────────────────────────┤
│  $XX.XX        [ Add to Cart 🛒 ]      │  ← Sticky bottom bar
└─────────────────────────────────────────┘
```

### Rules for Text Wireframes
- Show **content zones** with correct proportions, not pixel-perfect layouts
- Label each zone with its purpose (← annotations)
- Mark interactive elements clearly (`[ Button ]`, `[Link]`, `( ) Radio`, `[x] Checkbox`)
- Show the **information hierarchy** — what's biggest/first vs smallest/last
- Include all key states in separate wireframes if needed (empty state, error state, loading)
- For mobile: show one viewport. For responsive: show mobile + desktop side by side
- For multi-screen flows: number each screen and show arrows between them

### When to Generate
- User says "wireframe this", "sketch the layout", "show me the structure"
- Feature has 5+ content zones or complex navigation
- Multiple stakeholders need to align on layout before coding
- You want to validate the design brief's Layout Strategy visually

After presenting the wireframe, ask: "Does this layout match what you had in mind? Any zones to add, remove, or rearrange?" Then update the design brief if needed.