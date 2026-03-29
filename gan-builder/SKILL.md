---
name: gan-builder
description: >
  Build high-quality full-stack applications through guided, adversarial
  multi-agent workflow. Onboarding collects your idea, then a Builder and
  Quality Checker compete in iterative rounds until quality thresholds are met.
  Real-time progress with transparent build-vs-check battle views.
  Use when user says "build me an app", "create a web application",
  "make a game", "build a dashboard", or invokes /gan.
  Do NOT use for simple code edits, debugging, documentation, or single-file tasks.
user-invocable: true
compatibility: Requires Playwright for browser-based quality checking. Works on macOS and Linux.
metadata:
  version: 1.0.0
---

# GAN Builder

Build high-quality applications using an adversarial Builder vs Quality Checker loop.

## CRITICAL: Adversarial Loop Rules

These rules are **non-negotiable**. Never skip or weaken them.

1. **Every build phase MUST go through independent quality checking.** Even if the Builder reports "everything works", the Quality Checker MUST independently verify by testing the running app via Playwright. No exceptions.
2. **Builder and Quality Checker MUST be separate Agent instances.** Never let the Builder evaluate its own work. Spawn a separate Agent for quality checking. Self-evaluation produces inflated scores.
3. **Convergence prerequisites MUST be verified before entering the loop.** Before starting any build-QA round, verify that testable acceptance criteria exist for all four dimensions (functionality, design, craft, UX). If any dimension lacks clear criteria, stop and ask the user.
4. **Every round's results MUST be shown to the user in Battle View format.** After each round, output: what the Builder did, what the Quality Checker found, scores across 4 dimensions, and pass/fail verdict. Never run rounds silently.
5. **Non-convergence MUST trigger user intervention.** If scores don't improve by ≥1 point in any dimension across 3 consecutive rounds, pause and present the user with recovery options. Never loop indefinitely.

## Performance Notes

- Take your time to do this thoroughly
- Quality is more important than speed
- Do not skip validation steps or quality checking rounds
- Always test the running application, never just review code

---

## Instructions

### Step 1: Guided Onboarding

Collect requirements through conversational, step-by-step interaction. Ask one question at a time, wait for response before proceeding.

1. **What to build** — Ask the user to describe their idea (even rough is fine)
2. **Tech stack** — Ask if they have preferences. Default: React + Vite, FastAPI, SQLite
3. **Quality level** — Present three tiers:
   - Quick prototype (~30min, ~$10) — basic functionality, 1 QA round per phase
   - Standard build (~2h, ~$50) — complete features, full QA loop
   - Premium polish (~4-6h, ~$150-200) — multiple QA rounds, design iteration
4. **Special requirements** — Any constraints, style preferences, must-have features

Consult `references/ux-interaction-patterns.md` for exact message templates and how to handle vague answers.

### Step 2: Planning

Launch a **Planner Agent** to expand the user's brief into a full product spec.

Agent prompt must include:
- The user's requirements from Step 1
- Instructions from `references/planner-guidelines.md`
- Output format from `templates/product-spec.md`

After the Planner returns, present the blueprint to the user:
- Show feature list organized by build phases
- Show estimated time and cost
- Ask: "Does this plan look good? Reply 'continue' or tell me what to adjust."

**Do NOT proceed until user confirms.** Save the spec to `.harness/product-spec.md`.

### Step 3: Convergence Pre-flight

Before entering the adversarial loop for each build phase, verify ALL of the following:

- [ ] **Functional criteria exist**: Specific, testable behaviors (e.g., "form submits and data persists")
- [ ] **Design direction exists**: Style reference, color scheme, or layout approach
- [ ] **Scope is bounded**: Clear in-scope and out-of-scope items
- [ ] **Success threshold is clear**: All four dimensions must score ≥ 7/10

If any item is missing, ask the user targeted questions to fill the gap. Save the contract to `.harness/sprint-N-contract.md`.

Consult `references/sprint-contract-guide.md` for contract format and `references/evaluation-criteria.md` for scoring dimensions.

### Step 4: Build-QA Adversarial Loop

This is the core mechanism. For each build phase, execute this loop:

#### 4a. Build Round

Launch a **Builder Agent** with:
- The sprint contract from Step 3
- The current codebase state
- Any feedback from previous QA rounds (if not first round)
- Instructions to implement the features, start the dev server, and ensure it runs

The Builder Agent writes code, installs dependencies, and starts the dev server.

#### 4b. Quality Check Round

After the Builder completes, launch a **separate Quality Checker Agent** with:
- The sprint contract (acceptance criteria)
- Scoring rubric from `references/evaluation-criteria.md`
- Instructions to test the RUNNING application using Playwright (navigate pages, click buttons, fill forms, take screenshots)
- The evaluation report template from `templates/evaluation-report.md`

CRITICAL: The Quality Checker must:
- Actually interact with the running app via browser, not just read code
- Score each of the 4 dimensions independently (1-10)
- List specific issues found with severity (critical/minor)
- Write full report to `.harness/sprint-N-round-M.md`

#### 4c. Battle View Output

After each round, output the results to the user in this format:

```
⚔️ Round [M] — Build Phase [N]: [Feature Name]

  🔨 Builder: [bullet list of what was built/fixed]

  🔎 Quality Check: [bullet list of findings]
     - 🔴 Critical: [issues]
     - 🟡 Minor: [issues]
     - ✅ Pass: [what works]

  📊 Scores: Functionality X/10 | Design X/10 | Craft X/10 | UX X/10
  🎯 Quality bar: All dimensions ≥ 7/10
  📍 Result: [PASS → proceed | FAIL → round M+1]
```

#### 4d. Loop Control

- **PASS**: All 4 dimensions score ≥ 7/10 → proceed to checkpoint (Step 6)
- **FAIL**: Any dimension < 7 → feed QA report back to Builder, start round M+1
- **Max rounds**: 5 per build phase. If not passed after 5, trigger Non-Convergence Recovery

Update `.harness/dashboard.md` after every round using the format from `templates/dashboard.md`.

### Step 5: Non-Convergence Recovery

Triggered when scores stall (no ≥1 point improvement across 3 rounds) or max rounds reached.

1. Show the user the score trend across rounds
2. Diagnose the likely cause (vague design direction? technical blocker? scope too large?)
3. Present recovery options:
   - **A)** Provide a reference website/screenshot to align style standards
   - **B)** Pick the best version so far, refine from there
   - **C)** Lower the bar on the stalled dimension, focus on what's close to passing
   - **D)** Give specific feedback on what's wrong, adjust the quality criteria

After user responds, update the sprint contract and resume the loop.

Consult `references/ux-interaction-patterns.md` for the exact message format.

### Step 6: Checkpoint & Delivery

#### After Each Build Phase

Pause and present to the user:
- Quality scores from the final passing round
- List of features implemented
- URL to test the running app
- "Satisfied? Reply 'continue' for next phase, or tell me what to adjust."

**Do NOT proceed to the next phase without user confirmation.**

#### Final Delivery

After all phases complete:
- Generate final report using `templates/final-report.md` format
- Save to `.harness/final-report.md`
- Present summary: final scores, access URL, known issues, project file location

---

## Runtime Files

All runtime files are created in the project's `.harness/` directory:

| File | Purpose |
|------|---------|
| `dashboard.md` | Real-time progress with per-round scores |
| `product-spec.md` | Planner output (user-confirmed) |
| `sprint-N-contract.md` | Acceptance criteria for phase N |
| `sprint-N-round-M.md` | Full QA report for phase N, round M |
| `convergence-log.md` | Score trend tracking |
| `session-state.md` | State for session recovery |
| `final-report.md` | Final delivery report |

## Session Recovery

If conversation is cleared or interrupted:
1. Check if `.harness/session-state.md` exists in the project directory
2. If yes, read it to determine: which phase, which round, last scores
3. Resume from where it left off, informing the user what was recovered

Update `session-state.md` after every significant state change (phase transition, round completion, user checkpoint).

## Common Issues

### Dev server won't start
- Check if port is already in use: `lsof -i :[port]`
- Kill stale processes and retry
- If persistent, ask user if they have a preferred port

### Quality Checker can't access the app
- Verify dev server is running before launching QC agent
- Ensure the URL is correct (check terminal output for actual port)
- Try waiting 5 seconds for server startup before testing

### Scores oscillating between rounds
- This usually means the design direction is unclear
- Trigger Non-Convergence Recovery (Step 5)
- The user providing a reference website usually resolves this quickly

### Build phase is too large
- If a single phase takes >45 minutes to build, it's too large
- Split into smaller phases with the user's agreement
- Update the product spec and sprint contracts accordingly

## Language Guidelines

- Use friendly, non-technical language with the user
- Never say "sprint contract", "evaluator agent", "generator" — use "build phase", "quality check", "builder", "checker"
- Always explain what's happening and why
- When presenting options, keep them to 3-4 maximum
- Celebrate successes, be honest about problems
