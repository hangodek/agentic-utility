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
  - `docs:` Documentation updates (e.g., `docs: update FLOW.md architecture diagram`)
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

- **Top-Level Function & Type Block Comments:**
  - For every significant struct, type, constructor, and top-level function, include a concise comment block explaining:
    - **Why:** The purpose, mental model, and rationale of the component.
    - **How & Correlation:** How it interacts with surrounding layers (e.g., database, templates, callers).
    - **Trade-offs:** Any notable constraints, memory characteristics, or edge cases.
- **Helper Function Location References & Intent Comments:**
  - Whenever calling an internal helper function, include a brief inline comment indicating:
    1. Its definition location: `[defined below: functionName]` or `[path/to/file.go: functionName]`.
    2. A 1-line plain-language summary of what it does.
    - *Example:*

      ```go
      // [defined below: h.validate] Validates name, email format, and min 8-char password
      if errMsg := h.validate(name, email, password); errMsg != "" {
          ...
      }
      ```

- **Standard Library & Built-in Intent Comments:**
  - When calling standard library functions or language idioms whose behavior may not be immediately obvious (e.g., Go's `r.ParseForm()`, `http.MaxBytesReader(...)`, `defer`, channel operations; Python's `yield`, decorators; JS closures, `Promise.all`), include a short 1-line comment directly above describing what it achieves under the hood.
  - *Keep all comments concise, pedagogical, readable, and practical without fluff.*

---

## 6. Documentation Maintenance (`README.md`, `SIMPLE_FLOW.md`, `FLOW.md`)

- **Preserve `README.md`:** Keep `README.md` as the authoritative project specification, overview, and reference. Do not overwrite or dilute original requirements.
- **Ultra-Simple Linear Execution Flow (`SIMPLE_FLOW.md`):**
  - **Purpose & Core Philosophy:** Maintain a `SIMPLE_FLOW.md` at the project root. Programming is passing data through a chain of functions. `SIMPLE_FLOW.md` must provide a crystal-clear, step-by-step trace showing **how every piece of code connects and operates, leaving zero functions or helpers undocumented**.
  - **100% Exhaustive Function Coverage (No Skipped Functions):**
    - **Every single function, helper, event listener, parser, and setup utility in the codebase must be explicitly documented in the flow.**
    - Setup routines, input listeners (e.g., `setupInputHandler`, `cleanup` callbacks), validation helpers, formatters, and state initializers cannot be skipped.
  - **Dual-Mode Structure for Every Flow:**
    `SIMPLE_FLOW.md` must include two complementary sections for each major workflow/feature:
    1. **Part 1: The Step-by-Step Technical Chain (Code-Level):**
       - Strict markdown bullet list (`- **Step X [Function/Layer]:**`) so each step renders on its own line on the web.
       - Clean ASCII indicators: `-->` for next step, `[v]` for success/passes, `[x]` for error/exit early.
       - Shows exact function names, file paths, what data is checked/passed, and error branches.
    2. **Part 2: The Plain-Language Human Walkthrough (Story-Level):**
       - Located directly underneath the technical steps.
       - Explains how the entire program works in natural, conversational human language (like a senior engineer explaining to a beginner), describing user actions, what functions get triggered, what variables change, and how everything connects.
  - **Continuous Updates:** Whenever a new route, feature, function, or helper is added or modified, immediately update `SIMPLE_FLOW.md`.
  - **Standard Template & Example:**

    ```markdown
    ### Feature Flow: Terminal Dungeon Game (`node main.js <skill>`)

    #### Part 1: Step-by-Step Technical Flow
    - **Step 1 [Entrypoint]:** `main()` `[main.js]`
      - **Does:** Starts the program and orchestrates game initialization.
      -->
    - **Step 2 [CLI Argument Parsing]:** `parseSkillArg()` `[main.js]`
      - **Does:** Reads `process.argv[2]` and validates against allowed skills (`ulti`, `regen`).
      - **[x] If Invalid / Missing:** Logs error message and exits program (`return null`).
      - **[v] If Valid:** Returns `selectedSkill` (e.g., `"ulti"`).
      -->
    - **Step 3 [State Initialization]:** `createInitialState(selectedSkill)` `[main.js]`
      - **Does:** Builds initial `gameState` object `{heroHp: 100, currentStage: 0, skill, status: "PREPARATION", monsters: []}`.
      - **Helper Triggered:** Calls `generateMonster()` 10 times to pre-populate stage monsters (30% chance for `BIG` monster 'M' with 2 HP, 70% standard 'm' with 1 HP).
      - **[v] Passes:** `gameState`
      -->
    - **Step 4 [Initial Screen Render]:** `renderScreen(gameState)` `[main.js]`
      - **Does:** Clears console screen with `console.clear()`.
      - **Helper Triggered:** Calls `renderDungeonTrack(gameState)` to format the 10-slot dungeon track `|   |   | ... |`.
      - **[v] Output:** Prints header `"Here Ready to Go"`, HP indicator, and empty preparation track to terminal.
      -->
    - **Step 5 [Input Listener Setup]:** `setupInputHandler(onKeyPress)` `[main.js]`
      - **Does:** Configures `process.stdin` (sets UTF-8 encoding, enables raw mode if TTY, resumes stream) and attaches `data` event listener `handleInput`.
      - **[v] Returns:** `cleanupInput` function closure to allow restoring terminal state.
      -->
    - **Step 6 [Interactive Event Loop / Key Press]:** User presses keyboard keys
      - **Branch A [Key is SPACE (' ')]:**
        - Calls `handleSpacebarAction(gameState)`:
          - If status is `PREPARATION`: Advances status to `BATTLE` and sets `currentStage = 1`.
          - If status is `BATTLE`: Deducts 7 HP (`heroHp - 7`). If `heroHp == 0` or `currentStage >= 10`, sets status to `RESULT`; otherwise advances `currentStage += 1`.
        - Calls `renderScreen(gameState)`: Redraws terminal with updated HP, stage counter, and active battle slot `| H vs m |`.
      - **Branch B [Key is 'q']:**
        - Calls `cleanupInput()`: Sets `isListening = false`, detaches `stdin.off("data")`, disables raw mode, pauses stream, and exits.
      -->
    - **Step 7 [Final Outcome]:**
      - **Outcome:** Player either defeats all 10 monsters and reaches `Final Result` screen, dies at `HP: 0`, or cleanly quits with `q`.

    #### Part 2: Plain Human-Language Walkthrough
    In simple terms, here is what happens when you run and play the game:
    1. When you run `node main.js ulti`, the game first runs `parseSkillArg` to make sure you passed a valid skill name. If you forgot the skill, it prints an error and stops immediately.
    2. Once verified, `createInitialState` sets up the starting game world. Behind the scenes, it calls `generateMonster` 10 times to randomly generate monsters for all 10 stages (with a 30% chance for a strong 2 HP monster 'M').
    3. Next, `renderScreen` clears your terminal and uses `renderDungeonTrack` to draw the starting UI showing your 100 HP and an empty 10-slot track.
    4. To listen to your keypresses in real-time without needing to press ENTER, `setupInputHandler` turns on raw mode on your terminal and begins listening for incoming keystrokes.
    5. When you press **SPACEBAR**, `handleSpacebarAction` moves you from preparation into Stage 1. On every subsequent SPACEBAR hit, your hero attacks the monster, takes 7 damage, and steps forward into the next stage. After each hit, `renderScreen` immediately redraws the screen to show your new HP and the updated battle position (`| H vs m |`).
    6. If your HP drops to 0 or you clear stage 10, the game switches to the final result screen. If you press **'q'**, the `cleanupInput` function safely turns off raw mode on your terminal and exits cleanly.
    ```
- **Continuous System Documentation (`FLOW.md`):** Update or create `FLOW.md` whenever a feature is introduced or modified:
  - **How to Use:** Clear setup instructions, environment configurations, and run commands.
  - **Architecture & Layer Map:** System layers, directory structure, and component boundaries.
  - **Schema & Data Models:** Database tables, relations, and data structures (with entity diagrams where helpful).
  - **Flow & Sequence (Learning Lifecycles):** End-to-end user journeys, request-response lifecycles, and data transformations.
- **Simplicity & Clarity:** Keep all documentation, markdown files, and explanations ultra-simple, clear, and beginner-accessible (simple enough for anyone to grasp immediately). Avoid bloat and overly verbose text.

---

## 7. Living Code Dictionary (`DICTIONARY.md`)

- **Purpose & Scope:** Maintain a living, beginner-friendly dictionary/glossary file named `DICTIONARY.md` at the project root. It serves as a quick-lookup reference explaining every unfamiliar syntax, built-in function, standard library utility, framework method, and design pattern used in the codebase (e.g., Go's `r.Context()`, `defer`, `sync.WaitGroup`, channels, pointers; Rails' `has_secure_password`, `before_action`, `delegate`; Python's decorators, generators; JS/TS closures, `async/await`).
- **Continuous Updating:** Whenever new concepts, functions, library utilities, or patterns are introduced to the project:
  - Automatically add or update the entry in `DICTIONARY.md`.
  - Keep definitions ultra-simple, intuitive, and easy to understand for learners without unnecessary academic jargon.
- **Standard Dictionary Entry Format:**
  - **Term / Function / Syntax:** The exact name of the concept (e.g., `r.Context()`, `context.Context`).
  - **What It Is (Simple Terms):** Plain-language explanation of what it is and what it does.
  - **Why We Use It Here:** Why it was chosen and its specific role in this codebase.
  - **Quick Example / Analogy:** A tiny code snippet or simple analogy to make it immediately intuitive.
  - **Common Pitfall:** One key mistake to avoid when using this concept.

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
