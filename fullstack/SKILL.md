---
name: fullstack
description: Master full-stack orchestrator that guides users from a raw product idea to live production deployment through 5 deterministic phases (PRD Discovery, BLAST Architecture, TDD Task Planning, Autonomous Build & Test, Cloud Deployment). Activates on /fullstack, "projek baru", "bikin aplikasi", "buat web", "buat saas", or full-stack engineering requests.
---

# FullStack Orchestrator (State Machine Protocol)

Lead Full Stack Engineer protocol designed for Antigravity. Enforces strict human-in-the-loop product ownership paired with deterministic agentic engineering.

```
[Phase 1: PRD Discovery] ➔ [GATE 1: PRD Approval] 
      │
[Phase 2: BLAST Architecture] ➔ [GATE 2: Schema Approval]
      │
[Phase 3: TDD Task Planning] ➔ Generates task_plan.md
      │
[Phase 4: Autonomous Build & Test] ➔ Self-Auditing Playwright Loop
      │
[Phase 5: Cloud Deployment] ➔ GitHub Push & Vercel Live Deploy
```

---

## 0. Anti-Amnesia & Context Hygiene Invariant (Mandatory)

To prevent context window degradation and token exhaustion across large sessions:
1. **File-Based State Persistence:** Never rely on chat memory for system state. Every single decision, schema, and progress metric must be persisted immediately to disk in the project directory:
   - `docs/PRD.md`: The single source of product truth.
   - `gemini.md`: The supreme project law (Data schemas, architectural constraints, API contracts).
   - `task_plan.md`: The atomic TDD task checklist.
   - `progress.md`: Execution logs, test runs, and completed milestone history.
2. **Context Recovery Protocol:** If the session is compacted or interrupted, the agent must run `read_file` on `task_plan.md` and `progress.md` to immediately resume execution at the exact pending task without asking the user to repeat instructions.
3. **Compacting Recommendation:** At the end of Phase 2 and Phase 4, proactively advise the user to run `/compact` to purge raw tool output while preserving physical file state.

---

## Phase 1: PRD Discovery (The Socratic Interview)

**Objective:** Transform an ambiguous user idea into an actionable, AI-ready Product Requirements Document.

### Rules of Engagement:
1. **The One-Question Rule:** Ask exactly **one** focused question per response. Never bombard the user with a questionnaire.
2. **Key Dimensions to Cover:**
   - **North Star:** What is the single core problem being solved?
   - **Target Persona:** Who is using this, and on what device (Desktop/Mobile)?
   - **Scope Boundaries (Critical):** Explicitly define **In-Scope** vs **Out-of-Scope** to prevent feature creep.
   - **Key User Flows:** Walk through the step-by-step user journey.
3. **Artifact Generation:** Compile findings into `docs/PRD.md` following the Lean Technical PRD standard.

```
┌─────────────────────────────────────────────────────────────┐
│                    <HARD-GATE-1: PRD>                       │
│  DO NOT WRITE ARCHITECTURE SOPS OR TOUCH SYSTEM SCHEMAS     │
│  UNTIL THE HUMAN PARTNER EXPLICITLY APPROVES docs/PRD.md   │
└─────────────────────────────────────────────────────────────┘
```

---

## Phase 2: BLAST Architecture & Schema Lock

**Objective:** Translate the approved PRD into deterministic data models, directory structure, and technical SOPs using the B.L.A.S.T. Framework.

### Rules of Engagement:
1. **Data-First Invariant:** Explicitly define PostgreSQL/Supabase table schemas, TypeScript interfaces, and enum types in `gemini.md` before touching frontend UI.
2. **Connectivity & Handshake Probe:** Verify Supabase MCP connectivity and required environment variables (`.env.local`).
3. **Tech Stack Standards:**
   - Frontend: Next.js (App Router), Tailwind CSS, TypeScript.
   - Backend & Database: Supabase (PostgreSQL, Auth, RLS policies).
   - Validation & State: Zod, Server Actions / Route Handlers.
   - Testing: Playwright (E2E) + Vitest.

```
┌─────────────────────────────────────────────────────────────┐
│                   <HARD-GATE-2: SCHEMA>                     │
│  DO NOT WRITE APPLICATION CODE OR RUN SCAFFOLDING SCRIPTS   │
│  UNTIL THE USER APPROVES THE DATA SCHEMA IN gemini.md       │
└─────────────────────────────────────────────────────────────┘
```

---

## Phase 3: TDD Task Decomposition

**Objective:** Decompose the architecture into atomic, bite-sized tasks saved to `task_plan.md`.

### Rules of Engagement:
1. Every task must be self-contained and test-driven:
   - Files to create/modify.
   - Expected failing test / verification step.
   - Implementation step.
   - Verification command (`npm run build`, `tsc --noEmit`, or Playwright test).
2. Format checklist tasks using standard `- [ ]` Markdown checkboxes.

---

## Phase 4: Autonomous Build & Playwright Verification Loop

**Objective:** Execute the checklist autonomously with closed-loop verification.

### Rules of Engagement:
1. **The Autonomous Loop:**
   - Pick the first pending `- [ ]` task from `task_plan.md`.
   - Write code (Front End / Back End).
   - Run type-check and linter: `npm run build` or `npx tsc --noEmit`.
   - Run Playwright headless browser smoke test to verify UI renders without console errors and forms submit properly.
   - On error: Analyze stack trace, patch code, re-test (Self-Annealing Loop).
   - On success: Mark `- [x]` in `task_plan.md` and log to `progress.md`.
2. **Human Handoff:** Only pause when blocked by external credentials or upon 100% completion of the task plan.

---

## Phase 5: Cloud Deployment & Launch

**Objective:** Commit code and launch the verified application to the public internet.

### Rules of Engagement:
1. **Git Synchronization:** Create a clean Git commit and push to the connected GitHub repository via GitHub MCP.
2. **Vercel Live Deployment:** Trigger deployment via Vercel MCP / CLI. Verify build logs and ensure production environment variables are properly wired.
3. **Final Delivery:** Deliver the live production URL and GitHub repository link to the user.
