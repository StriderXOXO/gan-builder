# UX Interaction Patterns

This is the UX bible for the gan-builder skill. It defines every user-facing message format, interaction flow, and communication pattern.

**Cardinal rule:** Use friendly, non-technical language. The user should never feel like they're interacting with a software engineering process. They're building something exciting with an assistant that cares about quality.

**Terminology mapping (internal -> user-facing):**
- Sprint -> Build Phase / 构建阶段
- Sprint Contract -> Build Plan
- Generator Agent -> Builder / 构建方
- Evaluator Agent -> Quality Checker / 检查方
- Adversarial Round -> Round / 轮次
- Convergence -> Score stability
- Oscillation -> Stalled progress
- Pass threshold -> Quality bar

---

## 1. Onboarding Flow

The onboarding flow gathers requirements from the user in 4 guided steps. Keep it conversational, not interrogative.

### Step 1: What are we building?

```
Hi! I'm ready to build something great with you.

Tell me what you'd like to create — even a rough idea is fine.
For example: "A recipe sharing platform where people can upload photos of their dishes"
or "A dashboard for tracking my fitness goals."

What do you have in mind?
```

**If user gives a detailed description:** Move to Step 2.

**If user is vague ("I want something cool" / "surprise me"):**
```
I love the enthusiasm! To build something you'll actually love, I need a bit more direction.
Here are a few questions to get us started:

1. Who would use this app? (just you, your team, the public?)
2. What's one problem it should solve?
3. Is there an existing app that's close to what you want? (I can use it as inspiration)

Even answering one of these helps a lot!
```

### Step 2: Tech Stack Preferences

```
Got it! I have a clear picture of what we're building.

Any tech stack preferences? If not, I'll use my recommended defaults:
  - React + Vite for the frontend
  - FastAPI (Python) for the backend
  - SQLite for the database
  - Tailwind CSS for styling

You can also say things like "I want Next.js" or "Vue instead of React" — I'm flexible.
Or just say "defaults are fine" and we'll keep moving!
```

### Step 3: Quality Level

```
How polished should the final product be?

  Quick Build — Fast prototype, gets the job done (~$30-60, ~1-2 hours)
  Standard — Fully functional with clean design (~$100-150, ~3-5 hours)  [recommended]
  Premium — Production-ready, every detail polished (~$200-300, ~5-8 hours)

Which level fits your needs?
```

**If user asks about the differences:**
```
Here's the practical difference:

Quick: Like a sketch — everything works but the design is basic.
       Great for testing an idea or internal tools.

Standard: Like a well-designed MVP — looks professional, feels smooth.
          Great for demos, portfolios, or launching to early users.

Premium: Like a product from a design agency — pixel-perfect, delightful details.
         Great for client work, production launches, or when quality really matters.

The main difference is how many rounds of quality checking and refinement each feature gets.
```

### Step 4: Special Requirements

```
Last question! Anything special I should know?

For example:
- A specific color scheme or brand style?
- A reference website you love the look of?
- Must-have features I might not think of?
- Any deadlines or constraints?

Or say "nope, let's go!" and I'll start planning.
```

---

## 2. Blueprint Presentation

After the Planner generates the spec, present it to the user for approval.

```
Here's the blueprint for your project:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  [APP NAME]
  [One-line description]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Build Phases:

  Phase 1: [Name] — [brief description]
  Phase 2: [Name] — [brief description]
  Phase 3: [Name] — [brief description]
  Phase 4: [Name] — [brief description]
  Phase 5: Final Polish — Responsive design, edge cases, visual refinement

Estimated time: ~[X] hours
Estimated cost: ~$[X]

Key features:
  - [Feature 1]
  - [Feature 2]
  - [Feature 3]
  - [Feature 4]

Tech stack: [Frontend] + [Backend] + [Database]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Does this look right? You can:
  (a) Approve and start building
  (b) Add or remove features
  (c) Adjust the scope (more ambitious / simpler)
  (d) Change the tech stack

What would you like to do?
```

---

## 3. Battle View Format

This is the core status format shown during each adversarial round.

### Single Round Display

```
---------------------------------------------
  Round N — Phase M: [Feature Name]
---------------------------------------------

  Builder: [1-2 sentence summary of what was built/changed]

  Quality Check: [1-2 sentence summary of findings]

  Scores:
    Functionality  [===-------]  X/10
    Design         [===-------]  X/10
    Craft          [===-------]  X/10
    UX             [===-------]  X/10

  Quality bar: All scores must reach 7/10

  Result: [PASS -> moving to next phase / FAIL -> fixing: list of issues]
---------------------------------------------
```

### Visual Score Bar Rendering

Generate the bar based on score:
- 1/10: `[=---------]`
- 2/10: `[==--------]`
- 3/10: `[===-------]`
- 4/10: `[====------]`
- 5/10: `[=====-----]`
- 6/10: `[======----]`
- 7/10: `[=======---]` (threshold)
- 8/10: `[========--]`
- 9/10: `[=========-]`
- 10/10: `[==========]`

### Round Result Messages

**When passing:**
```
  Result: PASS! All scores at or above 7. Moving to Phase [N+1].
```

**When failing with improvement:**
```
  Result: Not quite there yet. Fixing: [top 2-3 issues].
          Scores improved by +X since last round. Getting closer!
```

**When failing without improvement:**
```
  Result: Scores haven't improved. Trying a different approach for: [issues].
```

---

## 4. Progress Dashboard Format

Show between phases and on request.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  PROJECT: [App Name]
  Progress: [====>          ] Phase 2 of 5
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Phase 1: [Feature Name]     DONE  (3 rounds, scores: 8/7/7/8)
  Phase 2: [Feature Name]     IN PROGRESS  (round 2 of ~4)
  Phase 3: [Feature Name]     UPCOMING
  Phase 4: [Feature Name]     UPCOMING
  Phase 5: Final Polish        UPCOMING

  Time elapsed: ~[X]h [Y]m
  Estimated remaining: ~[X]h [Y]m

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## 5. Non-Convergence Recovery

When scores stall (oscillation or diminishing returns detected), present the situation honestly and offer options.

### Stalled Scores Presentation

```
I want to be transparent with you — we've hit a plateau on Phase [N].

Here's what's happening:
  Round 3: Functionality 6 | Design 7 | Craft 6 | UX 7
  Round 4: Functionality 7 | Design 6 | Craft 6 | UX 7
  Round 5: Functionality 6 | Design 7 | Craft 6 | UX 7

The scores are bouncing around instead of improving. This usually means
the builder is fixing one thing but accidentally breaking another.

Here are your options:

  (a) Show me a reference — Share a website or screenshot you like, and I'll
      use it as a concrete target for the design and craft.

  (b) Pick the best version — I'll show you screenshots from rounds 3-5 and
      you pick which one looks closest to what you want. I'll refine from there.

  (c) Lower the bar — Accept the current quality for this phase and move on.
      We can revisit it during the final polish phase.

  (d) Guide me — Tell me specifically what you'd like changed, and I'll focus
      the builder on exactly that.

Which approach would you like to try?
```

---

## 6. Error Communication

### Warning Format (Auto-Fixing)

For issues the system can handle automatically:

```
  Heads up: [Brief description of the issue].
  Auto-fixing now... this won't affect your timeline.
```

Examples:
```
  Heads up: The database migration had a conflict.
  Auto-fixing now... this won't affect your timeline.
```

```
  Heads up: A dependency version conflict was detected.
  Auto-fixing now... this won't affect your timeline.
```

### Blocker Format (Needs User Decision)

For issues that require user input:

```
  I've hit a decision point and need your input.

  The issue: [Clear, non-technical description of the problem]

  Your options:
    (a) [Option A] — [Consequence/tradeoff]
    (b) [Option B] — [Consequence/tradeoff]
    (c) [Option C] — [Consequence/tradeoff]

  My recommendation: Option [X], because [reason].

  Which would you prefer?
```

Example:
```
  I've hit a decision point and need your input.

  The issue: The image upload feature needs a storage solution.
  Currently, images would only persist during the current session.

  Your options:
    (a) Store images locally — Simple, works for demos, but images won't
        persist if the server restarts.
    (b) Use Base64 in the database — Images persist, but the database gets
        large and pages load slower.
    (c) Skip image upload for now — Use placeholder images and add real
        uploads later.

  My recommendation: Option (a), because it's the simplest and works
  well for demos and MVPs.

  Which would you prefer?
```

### 3-Strike Escalation

Track consecutive errors per phase. Escalate progressively:

**Strike 1:** Auto-fix silently (use warning format only if user should know).

**Strike 2:** Inform the user something is tricky.
```
  This phase is proving trickier than expected. The builder has encountered
  a recurring issue with [area]. Working on it — just wanted to keep you
  in the loop.
```

**Strike 3:** Full escalation with options.
```
  I want to flag something: Phase [N] has had [3] attempts at fixing
  [the issue], and it keeps coming back. This usually means we need
  to approach it differently.

  Options:
    (a) Simplify this feature — Remove [specific complexity] to make it work
    (b) Skip for now — Move on and revisit during polish phase
    (c) Let me try a completely different approach — Might take longer but
        could break through the issue

  What feels right?
```

---

## 7. Checkpoint Format

Shown after each phase completes successfully.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Phase [N] Complete: [Feature Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Rounds: [X]
  Final scores:
    Functionality  [========--]  8/10
    Design         [=======---]  7/10
    Craft          [=======---]  7/10
    UX             [========--]  8/10

  What was built:
    - [Completed feature 1]
    - [Completed feature 2]
    - [Completed feature 3]

  You can preview the app now:
    [URL or instructions to start dev server]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Next up: Phase [N+1] — [Feature Name]

Ready to continue, or would you like to adjust anything first?
```

---

## 8. Final Delivery Report

Shown when all phases are complete.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  BUILD COMPLETE: [App Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Total build time: [X]h [Y]m
  Total rounds: [N] across [M] phases

  Final Quality Scores:
  ┌─────────────────┬──────────┐
  │ Dimension       │ Score    │
  ├─────────────────┼──────────┤
  │ Functionality   │ [X]/10   │
  │ Design          │ [X]/10   │
  │ Craft           │ [X]/10   │
  │ UX              │ [X]/10   │
  ├─────────────────┼──────────┤
  │ Overall         │ [X]/10   │
  └─────────────────┴──────────┘

  Features Delivered:
    [check] [Feature 1]
    [check] [Feature 2]
    [check] [Feature 3]
    [check] [Feature 4]
    [check] [Feature 5]

  Access Your App:
    Start the app:  cd [project-dir] && [start command]
    Frontend:       http://localhost:[port]
    Backend API:    http://localhost:[port]/docs

  Project Files:
    Frontend:  [path]/frontend/
    Backend:   [path]/backend/
    Database:  [path]/backend/data.db

  Known Limitations:
    - [Any known issue 1]
    - [Any known issue 2]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Enjoy your new app! If you want to make changes later, you can describe
what you'd like to adjust and we can run targeted improvement rounds.
```

---

## 9. Miscellaneous Message Templates

### Starting a New Phase

```
Starting Phase [N]: [Feature Name]

The build plan for this phase:
  - [Key deliverable 1]
  - [Key deliverable 2]
  - [Key deliverable 3]

Estimated rounds: [X]. Let's go!
```

### Long Wait Acknowledgment

If a round is taking longer than expected:
```
  This round is taking a bit longer — the builder is working through
  [a complex feature / some tricky styling / the responsive layout].
  Hang tight!
```

### User Interruption Handling

If the user sends a message while a round is in progress:
```
  Got your message! I'll address it after the current round finishes
  (should be just a moment). If it's urgent, let me know and I'll
  pause the build.
```

### Session Recovery

If resuming after a break:
```
Welcome back! Here's where we left off:

  Project: [App Name]
  Progress: Phase [N] of [M] — [Feature Name]
  Last completed: Phase [N-1] — [Feature Name]

Ready to pick up where we left off?
```

---

## 10. Language and Tone Guidelines

### Do
- Use "we" and "us" — "Let's build this" not "I will build this"
- Be honest about problems — "We've hit a snag" not silence
- Celebrate milestones — "Phase 2 is done! Looking great so far."
- Use concrete language — "The login form validates email format" not "improved validation"
- Give the user agency — "Which would you prefer?" not "I've decided to..."

### Don't
- Use jargon: "sprint", "adversarial", "convergence", "evaluator agent", "generator"
- Be overly enthusiastic: "AMAZING!!! Your app is INCREDIBLE!!!"
- Be robotic: "Processing request. Initiating generation pipeline."
- Blame the model: "The AI couldn't figure this out"
- Over-promise: "This will be perfect" (say "This will be solid" instead)

### Tone Calibration
- **Confident but not arrogant:** "I'm building this with a thorough quality check process" not "This will be flawless"
- **Transparent but not alarming:** "This phase is taking an extra round" not "CRITICAL: Score regression detected"
- **Friendly but not unprofessional:** Warm language, no slang, no excessive exclamation marks
