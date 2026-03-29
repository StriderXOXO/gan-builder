# gan-builder

**Adversarial multi-agent skill for Claude Code** — build production-quality full-stack applications through a GAN-inspired Builder vs Quality Checker loop.

## The Problem

When you ask AI to build an app, it writes code, tells you "done!", and moves on. But when you actually click around — buttons don't work, forms don't submit, core features are broken.

Worse: when you ask it to check its own work, it confidently says everything looks great.

**Self-evaluation doesn't work.** Anthropic's engineering team [proved this](https://www.anthropic.com/engineering/harness-design-long-running-apps) — and found a solution inspired by GANs (Generative Adversarial Networks): make two separate AI agents compete. One builds, one breaks. They go back and forth until the quality is genuinely good.

This skill brings that architecture to Claude Code as a one-command experience.

## How It Works

```
You: "Build me a recipe management app"
     ↓
🎯 Guided Onboarding → collects your idea, tech stack, quality level
     ↓
📋 Planning → expands into full product spec, you confirm
     ↓
🔄 For each feature:
     ↓
   ┌─────────────────────────────────────┐
   │  🔨 Builder Agent writes the code    │
   │           ↓                          │
   │  🔎 Quality Checker Agent tests      │
   │     the RUNNING app via browser      │
   │           ↓                          │
   │  📊 Scores on 4 dimensions           │
   │     (Functionality/Design/Craft/UX)  │
   │           ↓                          │
   │  ❌ Failed? → feedback → rebuild     │
   │  ✅ Passed? → show you, continue     │
   └─────────────────────────────────────┘
     ↓
🎉 Delivery with quality report
```

The Quality Checker is a **separate agent** that opens a real browser (Playwright), clicks buttons, fills forms, and navigates pages — just like a real user would. It scores across 4 dimensions, and all dimensions must reach ≥ 7/10 to pass.

Every round is shown to you transparently:

```
⚔️ Round 1 — Build Phase 2: Recipe CRUD

  🔨 Builder:
     - Created recipe data model and API
     - Built create/edit form
     - Added recipe list page

  🔎 Quality Check:
     - 🔴 Form submits but page doesn't refresh
     - 🟡 No empty state when list is empty
     - ✅ API endpoints work correctly
     - ✅ Edit/delete functions work

  📊 Scores: Functionality 6/10 | Design 5/10 | Craft 5/10 | UX 4/10
  🎯 Quality bar: All ≥ 7/10
  📍 Result: FAIL → Round 2
```

## Install

```bash
# Clone this repo
git clone https://github.com/StriderXOXO/gan-builder.git

# Copy the skill folder to Claude Code
cp -r gan-builder/gan-builder ~/.claude/skills/

# Install Playwright (required for browser-based quality checking)
npm install -g playwright
npx playwright install chromium
```

Or manually: download the `gan-builder/` folder from this repo and place it in `~/.claude/skills/`.

## Usage

After installing, just describe what you want to build:

```
> Build me a fitness tracking dashboard

> /gan
```

The skill will guide you through:
1. **What** you want to build
2. **Tech stack** preference (default: React + FastAPI + SQLite)
3. **Quality level** you want (see below)
4. **Special requirements** (mobile support, AI features, style preferences, etc.)

Then it builds — with transparent adversarial quality checking at every step.

## Quality Tiers

| Tier | Time | Cost | QA Rounds |
|------|------|------|-----------|
| ⚡ Quick Prototype | ~30 min | ~$10 | 1 per phase |
| 🎯 Standard Build | ~2 hours | ~$50 | Full loop |
| 💎 Premium Polish | ~4-6 hours | ~$150-200 | Multiple rounds + design iteration |

## Key Features

- **Guided onboarding** — asks one question at a time, no technical knowledge needed
- **Adversarial quality loop** — Builder and Quality Checker are separate agents, never skip QA
- **Browser-based testing** — Quality Checker uses Playwright to interact with the real running app
- **Battle View** — every round's results shown transparently (what was built, what failed, scores)
- **Non-convergence recovery** — if quality stalls, pauses and asks you for direction instead of looping forever
- **Checkpoints** — pauses after each feature for you to review before continuing
- **Session recovery** — state persisted to `.harness/` files, recoverable after interruption

## Skill Structure

```
gan-builder/
├── SKILL.md                    # Core workflow + adversarial loop rules
├── references/
│   ├── harness-principles.md   # Design principles from Anthropic's research
│   ├── evaluation-criteria.md  # 4-dimension scoring rubric
│   ├── sprint-contract-guide.md
│   ├── planner-guidelines.md
│   └── ux-interaction-patterns.md
└── templates/
    ├── product-spec.md
    ├── sprint-contract.md
    ├── evaluation-report.md
    ├── dashboard.md
    └── final-report.md
```

## Inspired By

This skill is built on the architecture described in Anthropic's engineering blog post: [Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps).

The core insight: *"When asked to evaluate work they've produced, agents tend to respond by confidently praising the work — even when, to a human observer, the quality is obviously mediocre."* The solution: separate the builder from the evaluator, and let them compete.

## License

MIT
