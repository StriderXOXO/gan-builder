# Evaluation Criteria: Four-Dimensional Scoring Rubric

The Evaluator agent scores every round across four dimensions. Each dimension is scored 1-10. All four must reach the pass threshold for a sprint to be considered complete.

**Pass threshold: ALL dimensions must score >= 7/10.**

---

## Dimension 1: Functionality (Weight: 30%)

> Does every feature work end-to-end?

**What to test:**
- Forms submit and data persists correctly
- Navigation works — all links/buttons lead where expected
- Error states are handled (empty inputs, network failures, edge cases)
- Data flows correctly between frontend and backend
- Authentication/authorization works if applicable
- CRUD operations complete without errors

**How to test:** Use Playwright to actually exercise every feature. Click every button. Submit every form. Navigate every route. Check the database state after mutations.

### Calibration Examples

**Score 5/10 — "It mostly works but has gaps":**
A todo app where adding items works, but deleting throws a console error. The list renders but doesn't update after deletion without a page refresh. Empty state shows a blank page instead of a helpful message. No loading indicators during API calls.

**Score 8/10 — "Solid and reliable":**
A todo app where all CRUD operations work smoothly. Items add, edit, delete, and reorder with immediate UI updates. Empty state shows "No todos yet — add one above!" Loading spinners appear during API calls. Form validation prevents empty submissions with clear error messages. Handles rapid clicking without duplicate submissions.

### Scoring Guide

| Score | Description |
|-------|-------------|
| 1-3 | App doesn't start, or core features are broken |
| 4-5 | Core happy path works, but significant bugs exist |
| 6 | Most features work, minor bugs remain |
| 7 | All specified features work, edge cases mostly handled |
| 8-9 | All features work reliably, excellent error handling |
| 10 | Bulletproof — handles adversarial inputs gracefully |

---

## Dimension 2: Design Quality (Weight: 25%)

> Does the UI feel cohesive and intentional?

**What to evaluate:**
- Consistent color palette (not random colors; a deliberate 3-5 color system)
- Typography hierarchy (clear distinction between headings, body, captions)
- Spacing rhythm (consistent padding/margins, not arbitrary values)
- Imagery and visual elements create a distinct mood/brand
- Layout feels intentional, not default

**Critical anti-patterns to penalize:**
- **Template defaults:** Unchanged boilerplate colors, fonts, or layouts from frameworks
- **AI slop patterns:** Generic hero sections with gradient backgrounds, stock placeholder text ("Welcome to our amazing platform!"), overuse of card grids with identical structure, meaningless decorative gradients
- **Inconsistency:** Mixed border-radius values, inconsistent button styles, color palette violations
- **No visual identity:** The app could be any app — nothing about the design reflects what it actually does

### Calibration Examples

**Score 5/10 — "It's styled but generic":**
A restaurant booking app using default Tailwind blue for everything. Cards have shadows but no consistent depth system. The hero section says "Book Your Table Today" over a generic gradient. Buttons are different sizes on different pages. Font sizes are arbitrary — no clear hierarchy.

**Score 8/10 — "Cohesive and distinctive":**
A restaurant booking app with a warm palette (deep burgundy, cream, gold accents) that evokes fine dining. Typography uses a serif for headings and clean sans-serif for body. Photos are styled with consistent overlays. The booking flow has subtle animations. Every page feels like it belongs to the same brand. You could identify the app from a screenshot without seeing the logo.

### Scoring Guide

| Score | Description |
|-------|-------------|
| 1-3 | No styling, or completely inconsistent visual language |
| 4-5 | Basic styling applied but feels generic/template-like |
| 6 | Decent visual coherence with some inconsistencies |
| 7 | Cohesive design system, minimal template feel |
| 8-9 | Distinctive, polished, feels professionally designed |
| 10 | Could win a design award — every pixel is intentional |

---

## Dimension 3: Craft (Weight: 25%)

> Are the details right?

**What to evaluate:**
- Typography: Clear hierarchy (H1 > H2 > H3 > body > caption), consistent line heights, appropriate font weights
- Spacing: Consistent grid (e.g., 4px or 8px base unit), balanced whitespace, aligned elements
- Color: Sufficient contrast ratios (WCAG AA minimum), harmonious palette, purposeful use of color
- Responsive behavior: Works on mobile, tablet, desktop — not just "doesn't break" but actually looks good
- Loading states: Skeleton screens or spinners during async operations
- Transitions: Smooth, purposeful animations (not gratuitous)
- Micro-interactions: Hover states, focus indicators, active states on interactive elements

### Calibration Examples

**Score 5/10 — "Functional but rough":**
Text is readable but line heights vary between sections. Spacing between elements is inconsistent — some cards have 16px gaps, others have 24px, with no pattern. On mobile, elements overflow horizontally. No hover states on buttons. Text against colored backgrounds sometimes fails contrast checks. No loading indicators.

**Score 8/10 — "Polished and professional":**
Consistent 8px spacing grid throughout. Typography scale follows a clear ratio (14/16/20/28/36). All interactive elements have hover, focus, and active states. Smooth 200ms transitions on state changes. Skeleton screens during data loading. Passes WCAG AA contrast everywhere. Mobile layout is thoughtfully reorganized, not just shrunk. Form inputs have clear focus rings.

### Scoring Guide

| Score | Description |
|-------|-------------|
| 1-3 | No attention to detail — broken layout, unreadable text |
| 4-5 | Basic layout works but inconsistent details |
| 6 | Good attention to detail with occasional misses |
| 7 | Consistent craft, few detail gaps |
| 8-9 | Meticulous attention to detail throughout |
| 10 | Pixel-perfect — indistinguishable from professional handoff |

---

## Dimension 4: User Experience (Weight: 20%)

> Can a real human use this without confusion?

**What to evaluate:**
- Can users understand what the app does within 5 seconds of landing?
- Are primary actions visible and obvious? (Not buried in menus or ambiguous icons)
- Can users complete core tasks without guessing?
- Is feedback clear? (Success messages, error messages, progress indicators)
- Is the information hierarchy logical? (Most important content is most prominent)
- Do user flows follow conventions? (Login in expected place, navigation is standard)

### Calibration Examples

**Score 5/10 — "Usable but confusing":**
A project management app where the "Create Project" button is in the bottom-right corner behind a floating action button. The dashboard shows 8 equal-sized cards with no hierarchy — users don't know where to start. After creating a task, there's no confirmation — users click again thinking it didn't work. Navigation labels use internal jargon ("Kanban View" instead of "Board").

**Score 8/10 — "Intuitive and guided":**
A project management app where the dashboard immediately shows your active projects with a prominent "New Project" button top-right. Creating a project gives a satisfying confirmation with a "next step" suggestion. The sidebar uses clear labels with icons. Empty states explain what to do. The most common action on each page is the most visually prominent element.

### Scoring Guide

| Score | Description |
|-------|-------------|
| 1-3 | Users cannot complete basic tasks |
| 4-5 | Core tasks possible but require hunting/guessing |
| 6 | Reasonable UX with some friction points |
| 7 | Smooth experience, clear information hierarchy |
| 8-9 | Delightful, anticipates user needs |
| 10 | So intuitive a first-time user completes all tasks effortlessly |

---

## Anti-Patterns in Evaluation

The Evaluator must guard against these failure modes:

### 1. Score Inflation
**Symptom:** Scores start at 7+ in round 1 and barely change.
**Cause:** Evaluator is being "nice" or anchoring to a generous baseline.
**Fix:** Calibrate against the examples above. A round-1 attempt almost never deserves 7+ on all dimensions.

### 2. Praising Own Work (Leaked Context)
**Symptom:** Evaluator uses phrases like "great job implementing..." or "I can see the effort in..."
**Cause:** Generator context leaked into evaluator, or evaluator is identifying with the generator.
**Fix:** Evaluator must evaluate purely based on the observable artifact (the running app), not on effort or intent.

### 3. Bug Acceptance Rationalization
**Symptom:** Evaluator notes a bug, then immediately explains why it's "not a big deal."
**Cause:** Evaluator is trying to move the process forward rather than maintain standards.
**Fix:** Enforce rule: if a bug is noted, it must be reflected in the score. No "noting but not penalizing."

### 4. Dimension Blindness
**Symptom:** Evaluator gives detailed feedback on functionality but one-line comments on design.
**Cause:** Evaluator defaults to code-review mode, ignoring visual/UX dimensions.
**Fix:** Require minimum 3 specific observations per dimension in every evaluation.

### 5. Vague Feedback
**Symptom:** "The design could be better." "UX needs improvement."
**Cause:** Evaluator isn't testing specifically enough.
**Fix:** Every piece of feedback must be actionable. Bad: "Improve the design." Good: "The card grid uses 3 different border-radius values (4px, 8px, 12px) — standardize to 8px."

---

## Convergence Monitoring

Track scores across rounds to detect problems:

### Healthy Convergence
Round 1: [4, 4, 3, 5] -> Round 2: [6, 5, 5, 6] -> Round 3: [7, 7, 7, 7] -> PASS

Scores increase by 1-3 points per round. Steady upward trajectory.

### Oscillation (STALLED)
**Detection rule:** If any dimension varies by +/- 1 or less across 3 consecutive rounds, the sprint is stalled.

Example: Functionality goes 6 -> 7 -> 6 -> 7. The generator is fixing one thing and breaking another.

**Recovery:** Stop the loop. Present the situation to the user with recovery options (see ux-interaction-patterns.md).

### Regression
**Detection rule:** If any dimension drops by 2+ points from its peak in the sprint.

Example: Design goes 6 -> 7 -> 5. The generator "fixed" design by breaking something that was working.

**Recovery:** Roll back to the version with the highest composite score and provide targeted feedback on only the failing dimensions.

### Diminishing Returns
**Detection rule:** Total score improvement < 2 points across the last 2 rounds, but not yet passing.

Example: Total goes 22 -> 23 -> 23. Generator is making micro-changes that don't move the needle.

**Recovery:** Escalate to user. Either lower the bar, provide a reference site, or manually guide the next change.

---

## Composite Score Calculation

```
composite = (functionality * 0.30) + (design * 0.25) + (craft * 0.25) + (ux * 0.20)
```

The composite score is used for tracking and reporting, but the pass/fail decision is based on the **minimum dimension rule**: ALL four must be >= 7.

This prevents a scenario where excellent functionality (10) masks terrible design (4) — composite could be 7.5 but the app still looks broken.
