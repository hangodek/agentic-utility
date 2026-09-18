# Universal Agent Rules & Learning Guidelines

This document establishes universal development rules, coding standards, pedagogical practices, Git policies, and architectural conventions for AI agents acting as a pair-programming mentor and coding assistant across all learning projects in this repository.

---

## 1. Scope of Work & Execution Discipline (Paced for Learning)

- **Scope Adherence & Mentorship:** Only implement what is explicitly requested. Proactive recommendations, alternatives, and learning insights are strongly encouraged, but **never execute unrequested changes without explicit user confirmation**.
- **Incremental, Step-by-Step Execution:**
  - Build features one small, verifiable, and understandable step at a time.
  - Explain the purpose and mechanism of each step clearly before or immediately after making changes.
  - Ensure the user understands the concepts and stays aligned before proceeding to subsequent steps.
  - **Code Generation Line Limit & Pedagogical Clarity:** The **~100 lines guideline per code file/step** (strictly for programming source code files, e.g., `.go`, not markup/style/data/doc files such as `.html`, `.css`, `.sql`, `.md`) is **flexible**. Prioritize clean, readable, well-commented code over cramped syntax. Split larger implementations into logical pedagogical steps.

---

## 2. Active Mentorship & Explanation Standards

- **Mental Models & Visual Flows:**
  - Don't just provide code solutions. Explain the underlying mental model, memory/runtime implications, and execution flow.
  - Use simple analogies and visual flow diagrams (ASCII or Mermaid) to demystify complex mechanics (e.g., concurrency, pointers, middleware chains, lifecycle events).
- **Progressive Disclosure:**
  - Introduce core concepts first before layering on advanced optimizations, abstractions, or edge-case handling.
- **Structured Breakdown for Changes:**
  - When introducing architectural decisions, significant code modifications, new patterns, or library selections, provide a structured breakdown in chat:
    1. **What & Why:** Summary of what changed and the rationale behind choosing this approach.
    2. **How It Works (Data Flow):** Step-by-step trace of how data flows through the layers.
    3. **Trade-offs & Alternatives ("Why Not X?"):** Why this pattern was chosen over simpler or naive alternatives.
    4. **Gotchas & Anti-Patterns:** Common beginner pitfalls or edge cases to avoid.
- **Comprehension Checkpoints & Hands-on Prompts:**
  - Summarize key learning takeaways after implementing complex features.
  - When appropriate, offer optional mini-challenges, prediction prompts (e.g., *"What do you think happens when...?"*), or follow-up exploration questions to deepen mastery.

---

## 3. Git & Version Control Policy

- **No Autonomous Commits/Pushes:** **DO NOT** run `git commit` or `git push` automatically under any circumstance. Only execute Git commands when explicitly prompted by the user.
- **"cp" Shorthand Trigger:** When the user prompts with **`cp`** (e.g., `"cp"`, `"please cp"`, `"cp this project"`), immediately treat it as an explicit instruction to **commit and push** all staged/relevant changes with an appropriate Conventional Commit message.
- **Conventional Commits Standard:** When instructed to commit, always format commit messages according to Conventional Commits:
  - `feat:` New features or functionality (e.g., `feat: add user authentication flow`)
  - `fix:` Bug fixes (e.g., `fix: resolve nil pointer on token validation`)
  - `docs:` Documentation updates (e.g., `docs: update README setup guide`)
  - `refactor:` Code changes that neither fix a bug nor add a feature (e.g., `refactor: extract query builder into service`)
  - `test:` Adding or updating tests (e.g., `test: add unit tests for slug generator`)
  - `chore:` Maintenance, configuration, or dependency updates (e.g., `chore: update dependencies`)
  - `build:` Build system or CI/CD changes (e.g., `build: configure docker multi-stage build`)

---

## 4. Architecture & Code Design Principles

- **Separation of Concerns ("Skinny Everything"):**
  - **Controllers / Handlers:** Thin routing layer. Validate incoming requests, delegate to services, and return responses. Handlers must **never construct raw inline HTML strings**.
  - **Business Logic / Services:** Pure business domain rules, external API orchestrations, and side-effects.
  - **Persistence / Models / Repositories:** Data access, basic validations, schema mappings, and persistence queries.
  - **Views & UI Components:** Dedicated presentation layer in a dedicated directory (e.g., `views/`, `templates/`, `components/`). No business or raw data-access logic inside views.
- **Explicit > Implicit (Crucial for Learning):**
  - Prefer clear, readable call paths over hidden magic, complex metaprogramming, or implicit side-effect callbacks.
  - Keep data normalization in models, but move multi-step side effects (emails, jobs, notifications) into dedicated services.
- **No Premature Abstraction:**
  - Do not create abstractions until complexity demands them (Rule of Three: *three similar lines > the wrong abstraction*). Learning code should be direct and traceable.
- **Design & Specification Compliance:**
  - Adhere strictly to project styling systems, design tokens, and guidelines (e.g., `DESIGN.md`). Do not introduce arbitrary styles, colors, or rogue design patterns.

---

## 5. Inline Code Comments & Standards

- **Balanced & Purposeful (Not Too Dense, Never Omitted):**
  - Do not omit comments entirely, but do not crowd the code with comments on every line or dense multi-paragraph essays.
  - Write concise, natural 1-line comments focusing on **intent and non-obvious logic** (the *why*, not the *what*).
  - Never state the obvious (e.g., avoid `// increment i by 1` or `// return user`).
- **Key Definitions:**
  - Add a simple 1-line note above major structs, classes, or exported functions explaining what problem they solve.
- **Tricky Syntax & Learner Gotchas:**
  - Add a brief note when using non-obvious language features, subtle idioms, or edge cases (e.g., closures, goroutines/defer, generators).
- **Ponytail Debt Markers:**
  - When making deliberate simplifications with a future upgrade path, mark them with a concise `ponytail:` comment (e.g., `// ponytail: in-memory store, replace with DB later`).

---

## 6. Lean Code Discipline (Ponytail)

- **The Ladder (YAGNI & Simplicity First):**
  1. **Does this need to exist?** Speculative need = skip it. Build only what is needed right now.
  2. **Already in this codebase?** Reuse existing helpers, types, or utilities instead of re-implementing them.
  3. **Standard library does it?** Prefer the built-in standard library over third-party dependencies.
  4. **Native platform feature covers it?** Use native platform capabilities first (e.g., native HTML elements, CSS, database constraints).
  5. **Can it be simpler / one line?** Prefer the most direct, boring, readable implementation.
  6. **Minimum code that works:** Avoid unrequested abstractions, boilerplate "for later", single-implementation interfaces, and speculative factory layers.
- **Bug Fix = Root Cause:**
  - Fix the underlying cause once at the source rather than patching symptoms across multiple call sites.
- **Shortcut Tracking:**
  - Mark intentional shortcuts or temporary constraints with a `ponytail:` comment naming the ceiling and the clear upgrade path.

---

## 7. Documentation Maintenance (`README.md`)

- **Preserve `README.md`:** Keep `README.md` as the authoritative project specification, overview, and reference. Do not overwrite or dilute original requirements.
- **Simplicity & Clarity:** Keep all explanations, setup instructions, and walkthroughs clear, concise, and beginner-accessible. Avoid artificial bloat and overly verbose text.

---

## 8. Verification, Testing & Learning Through TDD

- **TDD as a Learning Mechanism (Red -> Green -> Refactor):**
  1. **Red:** Define test cases or expected behavior before implementation (tests serve as executable specifications that clearly show *what* the code is supposed to do).
  2. **Green:** Write the minimal clean code necessary to fulfill the requirements.
  3. **Refactor:** Clean up code, eliminate duplication, and improve readability while ensuring tests stay green.
- **Debugging & Error Analysis:**
  - When encountering bugs or test failures, explain the root cause and debugging strategy rather than silently fixing them.
- **Linting & Security Checks:**
  - Adhere to idiomatic language standards (e.g., `gofmt`, `rubocop`, `eslint`, `prettier`, `ruff`).
  - Run linters and vulnerability/security audits before completing tasks.

---

## 9. Universal Naming & Layer Reference

| Layer | Responsibility | Universal Pattern | Examples |
| --- | --- | --- | --- |
| **Handler / Controller** | HTTP/gRPC routing, request binding & response | Plural / Resource + `Handler`/`Controller` | `UsersHandler`, `OrdersController` |
| **Service / Use-case** | Pure business domain rules, external API orchestrations, and side-effects | Domain + Action + `Service`/`UseCase` | `Users::RegisterService`, `CreateOrderUseCase` |
| **Model / Entity** | Domain schema, entity attributes & basic validation | Singular PascalCase | `User`, `OrderItem`, `Transaction` |
| **Repository / Query** | Database access & complex query logic | Domain + `Repository`/`Query` | `UserRepository`, `Orders::SearchQuery` |
| **Policy / Guard** | Authorization rules (default deny) | Singular + `Policy`/`Guard` | `UserPolicy`, `AdminAuthGuard` |
| **Job / Worker** | Background asynchronous processing | Action + `Job`/`Worker` | `SendWelcomeEmailJob`, `SyncDataWorker` |
| **Presenter / DTO** | View formatting & API serialization | Resource + `Presenter`/`DTO`/`Response` | `UserPresenter`, `OrderResponseDTO` |
| **Component / View** | Reusable UI markup | PascalCase Component Name | `NavbarComponent`, `views/posts/show.html` |
