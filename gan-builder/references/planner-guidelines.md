# Planner Agent Guidelines

The Planner is the first agent in the pipeline. It takes the user's brief (typically 1-4 sentences) and expands it into a comprehensive product specification that guides the entire build process.

---

## Planner's Role

**Input:** User's brief description of what they want built.
**Output:** A complete product specification with feature list, technical architecture, and sprint breakdown.

The Planner does NOT write code. It thinks about the product, its users, its features, and the best way to decompose it into buildable sprints.

---

## Expansion Process

### Step 1: Interpret the Brief

Take the user's description and identify:
- **Core value proposition:** What problem does this app solve?
- **Primary user persona:** Who will use this?
- **Must-have features:** What's explicitly requested?
- **Implied features:** What's necessary but not stated? (e.g., if user says "e-commerce store," authentication is implied)

### Step 2: Expand Ambitiously but Achievably

The Planner should push scope to be impressive while remaining buildable. The goal is a product that makes the user say "wow, I didn't think of that" — not "where's the feature I asked for?"

**Guidelines for expansion:**
- Include features the user would expect even if not stated (login, error pages, loading states)
- Add 2-3 "delight" features that elevate the product (animations, smart defaults, keyboard shortcuts)
- Look for opportunities to weave AI features into the product where they add genuine value:
  - Smart search / filtering
  - Content generation assistance
  - Personalized recommendations
  - Natural language input processing
- Do NOT add features that require external services the user hasn't mentioned (e.g., don't add Stripe payments unless user mentioned payments)

### Step 3: Structure the Output

The Planner's output must follow this structure:

```markdown
# Product Specification: [Product Name]

## Overview
[2-3 paragraph description of the product, its purpose, target user, and key differentiators]

## Feature List

### Core Features
1. **[Feature Name]** — [Description]
   - As a user, I can [user story]
   - As a user, I can [user story]

2. **[Feature Name]** — [Description]
   - As a user, I can [user story]

### Enhancement Features
3. **[Feature Name]** — [Description]
   - As a user, I can [user story]

### Polish Features
4. **[Feature Name]** — [Description]

## Technical Architecture

### Tech Stack
- **Frontend:** [framework + key libraries]
- **Backend:** [framework + key libraries]
- **Database:** [database + ORM if applicable]
- **Styling:** [CSS framework/approach]

### Data Model
[Key entities and their relationships]

### API Overview
[Key endpoints grouped by feature]

## Sprint Breakdown

### Sprint 1: [Feature Name] (~X rounds)
- [Key deliverables]

### Sprint 2: [Feature Name] (~X rounds)
- [Key deliverables]

...

### Sprint N: Polish & Responsive Design (~X rounds)
- [Key deliverables]

## Estimated Cost & Time
[Based on quality level selected by user]
```

---

## Sprint Decomposition Rules

### Number of Sprints

| App Complexity | Sprint Count | Examples |
|----------------|-------------|----------|
| Simple (1-2 pages, basic CRUD) | 3-4 sprints | Todo app, note-taking app, bookmark manager |
| Medium (3-5 pages, auth, multiple features) | 5-6 sprints | Blog platform, project tracker, recipe app |
| Complex (6+ pages, complex data, multiple user roles) | 6-7 sprints | E-commerce store, SaaS dashboard, social platform |

### Sprint Ordering Principles

1. **Foundation first:** Sprint 1 should always set up the project structure, core data model, and primary layout/navigation. This gives subsequent sprints a stable base.

2. **Core value early:** The feature that represents the app's core value proposition should be Sprint 2 or 3. Users should see what makes the app special as early as possible.

3. **Dependencies respected:** If Feature B depends on Feature A's data model, Feature A's sprint comes first.

4. **Authentication placement:** If the app needs auth, it should be Sprint 1 or 2. Many features depend on knowing who the user is.

5. **Polish last:** The final sprint is always dedicated to responsive design, edge cases, loading states, and visual polish. This sprint operates on the entire app, not a single feature.

### Sprint Independence

Each sprint should leave the app in a **usable state**. After Sprint 3 completes, the app should work with 3 features — it shouldn't be broken waiting for Sprint 4.

This means:
- Each sprint includes both backend and frontend work for its feature
- Navigation should accommodate all completed features
- The app should gracefully handle "not yet built" areas (don't show broken links to features from future sprints)

---

## Default Tech Stack

When the user doesn't specify a tech stack, use:

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| Frontend | React + Vite | Fast development, large ecosystem, most models trained extensively on React |
| Styling | Tailwind CSS | Utility-first enables rapid UI iteration without CSS file management |
| Backend | FastAPI (Python) | Clean API definition, automatic docs, fast to build |
| Database | SQLite | Zero configuration, file-based, sufficient for demos/MVPs |
| ORM | SQLAlchemy (if needed) | Only if data model is complex enough to warrant it |

### When to Deviate from Defaults

- **User specifies stack:** Always respect user's preference
- **Static site:** Drop the backend, use Vite for static generation
- **Complex real-time features:** Consider adding WebSocket support or switching to a framework that supports it natively
- **Mobile-first:** Consider React Native or a PWA approach
- **Data-heavy app:** Consider PostgreSQL instead of SQLite

---

## AI Feature Integration

When the product could benefit from AI features, the Planner should suggest them. But they must add genuine value — not be gimmicks.

### Good AI Integration Examples

| Product | AI Feature | Value |
|---------|-----------|-------|
| Recipe app | "Generate recipe from ingredients I have" | Solves real user problem |
| Project tracker | "Summarize weekly progress from task updates" | Saves time |
| E-commerce | "Smart product recommendations based on browsing" | Increases engagement |
| Blog platform | "AI writing assistant for drafts" | Aids content creation |

### Bad AI Integration Examples (Avoid)

| Product | AI Feature | Why It's Bad |
|---------|-----------|-------------|
| Todo app | "AI suggests tasks for you" | Doesn't solve a real problem |
| Calculator | "AI explains the math" | Overengineered |
| Static portfolio | "AI chatbot on every page" | Adds complexity, no user value |

### Implementation Note

AI features should use a simple API integration pattern:
- Backend endpoint receives user input
- Backend calls Claude API (or equivalent)
- Backend returns structured response to frontend
- Frontend displays result with appropriate loading state

The Planner should specify AI features but note them as "enhancement" — the core app should work without them.

---

## Cost and Time Estimation

Provide estimates based on the user's selected quality level:

### Quick Build (Score target: 5-6/10)
- **Approach:** Reduced adversarial rounds (1-2 per sprint), relaxed evaluation
- **Cost:** ~$30-60
- **Time:** ~1-2 hours
- **Result:** Functional prototype, generic styling, minimal error handling
- **Good for:** Quick demos, proof of concept, internal tools

### Standard Build (Score target: 7/10)
- **Approach:** Full adversarial loop (3-4 rounds per sprint), standard evaluation
- **Cost:** ~$100-150
- **Time:** ~3-5 hours
- **Result:** Fully functional, cohesive design, good UX
- **Good for:** MVPs, client demos, portfolio projects

### Premium Build (Score target: 8-9/10)
- **Approach:** Extended adversarial loop (4-6 rounds per sprint), strict evaluation
- **Cost:** ~$200-300
- **Time:** ~5-8 hours
- **Result:** Production-quality, polished design, excellent UX, comprehensive error handling
- **Good for:** Launch-ready products, high-stakes demos

### Planner's Estimation Formula

```
estimated_cost = num_sprints * avg_rounds_per_sprint * cost_per_round
estimated_time = num_sprints * avg_rounds_per_sprint * time_per_round

Where:
  cost_per_round ~= $8-12 (depends on code complexity)
  time_per_round ~= 10-20 minutes
  avg_rounds_per_sprint = 3 (standard), 2 (quick), 5 (premium)
```

---

## Output Quality Checklist

Before the Planner's spec is finalized, verify:

- [ ] Every user story is concrete and testable
- [ ] Sprint breakdown covers all features in the feature list
- [ ] No sprint depends on a later sprint
- [ ] The final sprint is dedicated to polish
- [ ] Tech stack is justified and consistent
- [ ] Data model covers all features
- [ ] API endpoints cover all frontend needs
- [ ] Cost/time estimates are provided
- [ ] AI features (if any) add genuine value
- [ ] The spec is ambitious enough to impress, but achievable in the sprint count
