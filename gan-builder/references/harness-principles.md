# Harness Design Principles for GAN-Builder

Distilled from Anthropic's research on long-running application development harnesses.

---

## 1. Separation of Concerns: Generator and Evaluator Must Be Separate

The single most important principle. An agent cannot reliably evaluate its own work.

**Why it matters:**
- Self-evaluation produces score inflation. The generator rationalizes its own shortcuts.
- A fresh evaluator context sees the app as a user would — with no knowledge of implementation compromises.
- Separation creates genuine adversarial pressure. The generator must actually fix problems, not explain them away.

**Implementation rule:** The generator agent and evaluator agent must operate in completely separate conversation contexts. They communicate exclusively through the filesystem (code files, score files, feedback files) — never through shared conversation history.

---

## 2. Start Simple, Add Complexity Only When Needed

Every component in a harness encodes an assumption about what the model cannot do on its own. Harnesses are workarounds for model limitations.

**Practical guidance:**
- Begin with the simplest possible loop: generate code, evaluate it, iterate.
- Add planning phases only after observing that unplanned generation fails.
- Add sprint decomposition only after observing that single-pass full-app generation fails.
- Add few-shot calibration examples only after observing score drift.

**Key question before adding any component:** "Would removing this cause the output quality to drop?" If you're not sure, remove it and find out.

---

## 3. File-Based Communication Between Agents

Agents communicate through the filesystem, not through conversation threading.

**Why files, not conversation:**
- Files persist across context window resets. Conversation history is volatile.
- Files can be selectively loaded. Conversation history is all-or-nothing.
- Files create a clear audit trail. Conversation history is opaque.

**Communication files:**
- `plan.md` — Planner writes, Generator reads
- `sprint-N-contract.md` — Negotiated scope for each sprint
- `sprint-N-feedback.md` — Evaluator writes detailed feedback, Generator reads
- `sprint-N-scores.json` — Structured scores for tracking convergence
- `progress.md` — Running status dashboard

**Rule:** If an agent needs information from another agent, it reads a file. If an agent produces information for another agent, it writes a file.

---

## 4. Sprint-Based Decomposition

Build one feature at a time, with clear contracts defining "done."

**Why sprints:**
- A full application exceeds what a single generation pass can produce reliably.
- Sprints create natural checkpoints for user feedback.
- Sprints limit blast radius — a failed sprint doesn't destroy previous work.

**Sprint sizing:**
- Each sprint = one focused feature (e.g., "user authentication," "product listing page," "checkout flow").
- A sprint should be completable in 2-5 adversarial rounds.
- If a sprint takes more than 5 rounds, it's scoped too large — split it.

**Sprint ordering:**
- Core data model and API first.
- Primary user-facing features next.
- Polish and edge cases last.

---

## 5. Context Management: Filesystem as Persistent Memory

Context window = RAM (volatile, limited). Filesystem = disk (persistent, unlimited).

**The problem:** Long-running tasks exceed context window limits. Important decisions, feedback, and architectural context get lost when context rotates.

**The solution:**
- Write all decisions, feedback, and state to files immediately.
- At the start of each agent invocation, load only the files relevant to the current task.
- Never rely on an agent "remembering" something from a previous invocation.

**Critical state to persist:**
- Product specification (from Planner)
- Sprint contracts (scope agreements)
- Evaluation scores per round (for convergence tracking)
- Cumulative feedback (what's been tried and failed)
- Architecture decisions (tech stack, database schema, API routes)

---

## 6. Stress-Test Assumptions Regularly

Harness components become obsolete as models improve. Periodically remove components to test if they're still needed.

**Process:**
1. List all harness components (planner, sprint decomposition, few-shot examples, evaluation rubric, etc.).
2. Remove one component at a time.
3. Run the same task with and without the component.
4. If output quality is unchanged, the component is dead weight — remove it permanently.

**Components most likely to become unnecessary first:**
- Detailed few-shot calibration examples (as model judgment improves).
- Explicit anti-pattern lists (as models learn common pitfalls).
- Sprint decomposition for simple apps (as context windows grow).

**Components most likely to remain necessary longest:**
- Generator/Evaluator separation (self-evaluation is a fundamental limitation).
- File-based state persistence (context windows will always have limits).

---

## 7. Cost-Quality Tradeoff

There is a direct relationship between compute investment and output quality.

| Approach | Cost | Time | Typical Result |
|----------|------|------|----------------|
| Single agent, one shot | ~$10 | ~20 min | Usually broken. Missing features, template defaults, no error handling. |
| Single agent, iterative | ~$30-50 | ~1-2h | Partially functional. Core features work, UI is generic. |
| Full harness (Planner + Generator + Evaluator loop) | ~$150-200 | ~4-6h | Fully functional. Polished UI, error handling, responsive design. |

**Guidance for users:**
- Quick prototype / proof of concept: Use simpler approach.
- Production-quality demo / MVP: Full harness is worth the investment.
- The harness pays for itself by avoiding the "90% done, 90% to go" trap where the last 10% of quality takes unbounded manual effort.

---

## Summary of Non-Negotiable Rules

1. **Never let an agent evaluate its own output.** Separate generator and evaluator.
2. **Never rely on conversation history for state.** Write to files.
3. **Never scope a sprint larger than 5 adversarial rounds can handle.** Split it.
4. **Never skip the evaluation step.** Even if the generator "feels confident."
5. **Never add complexity without evidence it's needed.** Measure first.
