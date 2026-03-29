# Sprint Contract Guide

A sprint contract defines "what done looks like" before any code is written. It's the agreement between the Generator (builder) and Evaluator (checker) that prevents scope creep, moving goalposts, and wasted rounds.

---

## Contract Template

```markdown
# Sprint N Contract: [Feature Name]

## Scope
- Exact feature 1 to implement
- Exact feature 2 to implement
- Exact feature 3 to implement

### Explicitly Out of Scope
- Thing that might seem related but is NOT part of this sprint
- Enhancement that will be handled in a later sprint

## Acceptance Criteria (testable)
- [ ] Criterion 1 (must be verifiable by observing the running app)
- [ ] Criterion 2
- [ ] Criterion 3
- [ ] Criterion 4
- [ ] Criterion 5

## Design Direction
- Style reference: [description or URL if provided by user]
- Layout approach: [e.g., "sidebar navigation with main content area"]
- Key UI components: [e.g., "data table with sortable columns, modal for editing"]
- Color/mood: [e.g., "professional and clean, blue/gray palette"]

## Technical Constraints
- Frontend: [e.g., React + Vite + Tailwind]
- Backend: [e.g., FastAPI + SQLite]
- API design: [e.g., "RESTful, /api/v1/ prefix"]
- Database: [e.g., "users table with id, email, name, created_at"]
- External dependencies: [e.g., "Stripe for payments"]

## Max Rounds
- Target: 3 rounds
- Hard limit: 5 rounds (escalate to user if not passing by round 5)
```

---

## Writing Testable Acceptance Criteria

The single biggest source of wasted rounds is vague acceptance criteria. If the Evaluator can't objectively test it, it's not a criterion — it's a wish.

### Bad Criteria (Untestable)

| Bad Criterion | Why It's Bad |
|---------------|-------------|
| "Good design" | Subjective — means different things to generator and evaluator |
| "Fast loading" | No measurable threshold |
| "User-friendly" | Vague — can't be tested with Playwright |
| "Clean code" | Not observable from the running app |
| "Responsive" | Unclear which breakpoints, which layouts |
| "Proper error handling" | Which errors? What's "proper"? |

### Good Criteria (Testable)

| Good Criterion | Why It's Good |
|----------------|---------------|
| "Consistent 8px spacing grid, max 3 font sizes used across all pages" | Observable, measurable, specific |
| "Page loads in under 2 seconds on simulated 3G" | Measurable with tooling |
| "Login form shows inline error when email format is invalid" | Specific behavior, testable via Playwright |
| "Dashboard shows skeleton loading states during data fetch" | Observable visual behavior |
| "Navigation works at 375px, 768px, and 1440px widths" | Specific breakpoints to test |
| "Submitting empty form shows validation errors on all required fields" | Exact behavior, testable |
| "After deleting an item, it disappears from the list without page refresh" | Observable, immediate verification |

### The Playwright Test Rule

**Every acceptance criterion should be completable as a sentence starting with:**
"Using a browser, I can verify that..."

If you can't complete that sentence, the criterion is not testable.

---

## Scoping Guidelines

### Too Large (Split It)

Signs a sprint is too large:
- More than 7 acceptance criteria
- Involves both frontend and backend work for multiple features
- Estimated at more than 5 adversarial rounds
- Contains the word "and" connecting unrelated features (e.g., "authentication AND product catalog")
- Requires multiple new database tables

**Fix:** Split into separate sprints. Each sprint should focus on one cohesive feature.

### Too Small (Combine It)

Signs a sprint is too small:
- Only 1-2 acceptance criteria
- Purely cosmetic with no functional component
- Could be completed in a single generation pass without evaluation
- Is a sub-task of a larger feature (e.g., "add border to cards" is not a sprint)

**Fix:** Combine with related work into a meaningful feature sprint.

### Just Right

A well-scoped sprint typically has:
- 4-6 acceptance criteria
- One primary user-facing feature
- Both functional and visual requirements
- Can be built and iterated on in 3-5 rounds
- Leaves the app in a usable state when complete (even if other features are missing)

---

## Sprint Scoping Examples

### Example 1: E-Commerce App — Product Listing

```markdown
# Sprint 2 Contract: Product Listing Page

## Scope
- Product grid/list view with product cards
- Search by product name
- Filter by category
- Sort by price (low-high, high-low) and name

### Explicitly Out of Scope
- Shopping cart functionality (Sprint 3)
- Product detail page (Sprint 4)
- User reviews on products (Sprint 6)

## Acceptance Criteria (testable)
- [ ] Products display in a responsive grid (4 cols desktop, 2 cols tablet, 1 col mobile)
- [ ] Each product card shows: image, name, price, category badge
- [ ] Typing in search bar filters products in real-time (debounced 300ms)
- [ ] Category filter dropdown shows all categories from database
- [ ] Sort dropdown offers: "Price: Low to High", "Price: High to Low", "Name: A-Z"
- [ ] Empty state shows "No products found" with suggestion to clear filters
- [ ] Products load from API endpoint GET /api/v1/products with query params

## Design Direction
- Clean, modern e-commerce aesthetic
- Product images should be prominent (at least 60% of card height)
- Subtle hover effect on product cards (slight lift + shadow)
- Filter/sort controls in a toolbar above the grid

## Technical Constraints
- Frontend: React + Vite + Tailwind
- Backend: FastAPI
- Database: SQLite with products table (id, name, price, category, image_url, description)
- API: GET /api/v1/products?search=&category=&sort=price_asc
- Seed database with 20+ realistic products

## Max Rounds
- Target: 3 rounds
- Hard limit: 5 rounds
```

### Example 2: SaaS Dashboard — User Settings

```markdown
# Sprint 4 Contract: User Settings Page

## Scope
- Profile editing (name, email, avatar)
- Password change
- Notification preferences
- Account deletion

### Explicitly Out of Scope
- Billing/subscription management (Sprint 5)
- Team settings (Sprint 6)
- API key management (future)

## Acceptance Criteria (testable)
- [ ] Settings page accessible from avatar dropdown in header
- [ ] Profile section: edit name and email with inline validation
- [ ] Avatar upload: click to upload, preview before save, max 2MB
- [ ] Password change: requires current password, new password with strength indicator
- [ ] Notification toggles: email notifications, browser notifications, weekly digest
- [ ] "Delete Account" requires typing "DELETE" to confirm, shows warning modal
- [ ] All changes show success/error toast notifications
- [ ] Settings persist after page refresh

## Design Direction
- Clean, organized sections with clear visual separation
- Use tabs or vertical nav for settings categories
- Destructive actions (delete) in red, clearly separated from other actions
- Form inputs should have clear labels, help text where needed

## Technical Constraints
- Frontend: React + Vite + Tailwind
- Backend: FastAPI with existing auth system from Sprint 1
- API: PATCH /api/v1/users/me, POST /api/v1/users/me/password, DELETE /api/v1/users/me
- File upload: multipart/form-data for avatar
- Notification preferences stored as JSON column on users table

## Max Rounds
- Target: 3 rounds
- Hard limit: 5 rounds
```

---

## Mid-Sprint Renegotiation

Sometimes, mid-sprint, it becomes clear the contract needs adjustment. This is OK, but it must be explicit.

### When to Renegotiate

1. **Blocker discovered:** A technical limitation makes a criterion impossible (e.g., "real-time search" requires WebSocket but the stack doesn't support it).
2. **Scope was underestimated:** After round 2, it's clear there are 8+ criteria worth of work remaining.
3. **User changes their mind:** User sees an intermediate result and wants to pivot direction.
4. **Dependency gap:** The sprint depends on work from a previous sprint that's incomplete.

### How to Renegotiate

1. **Document what changed and why** in the sprint feedback file.
2. **Propose a revised contract** with clear diff from original:
   - Criteria removed (and why)
   - Criteria added (and why)
   - Scope reduced or expanded
3. **Get user approval** if the change is significant (removing a feature they asked for).
4. **Reset the round counter** — don't penalize the generator for lost rounds due to scope change.

### What's NOT Renegotiation

- Lowering scores to force a pass — this is cheating, not renegotiation.
- Removing criteria because the generator "can't do it" — try at least 3 rounds first.
- Adding criteria mid-sprint because the evaluator thought of something new — save it for next sprint.

---

## Contract Negotiation Process

The contract is established before the first line of code is written:

1. **Planner outputs sprint breakdown** with high-level scope per sprint.
2. **Generator drafts the contract** based on the planner's sprint description, adding technical specifics and acceptance criteria.
3. **Evaluator reviews the contract** and pushes back on:
   - Criteria that aren't testable
   - Scope that's too large or too small
   - Missing edge cases that should be specified upfront
4. **Both agree** — contract is written to `sprint-N-contract.md`.
5. **Generation begins** — generator writes code, evaluator evaluates against the contract.

This negotiation typically takes 1 exchange and is part of the sprint setup, not counted toward the round limit.
