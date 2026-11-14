---
name: blast-framework
description: Implements the B.L.A.S.T. v2.0 (Blueprint, Link, Architect, Stylize, Trigger) automation and full-stack development protocol paired with the 3-Layer A.N.T. architecture (Architecture, Navigation, Tools). Activates when building reliable, modular, self-healing automations, web applications, or whenever invoked via /blast-framework, /blast, or BLAST framework.
---

# B.L.A.S.T. Framework v2.0 (Universal Chameleon Protocol)

System Pilot protocol for building deterministic, modular, and self-healing full-stack systems and automations using the **B.L.A.S.T.** protocol (*Blueprint, Link, Architect, Stylize, Trigger*) and the 3-Layer **A.N.T.** architecture (*Architecture, Navigation, Tools*).

Prioritizes **system reliability and data consistency** over rushed execution. Never guess business logic or schemas.

---

## Protocol 0: Initialization & Environment Auto-Detection (Mandatory First Step)

Before writing any execution scripts or user interfaces, complete these initialization steps in order:

### 1. Environment Auto-Detection
Detect the active runtime/agent environment and resolve `<CONSTITUTION_FILE>` and `<SKILLS_DIR>`:
- **Google Antigravity**: `<CONSTITUTION_FILE>` = `gemini.md` | `<SKILLS_DIR>` = `.agent/skills/`
- **Claude Code (CLI / Extension)**: `<CONSTITUTION_FILE>` = `CLAUDE.md` | `<SKILLS_DIR>` = `.claude/skills/`
- **OpenAI Codex / OpenCode / Cursor / Windsurf**: `<CONSTITUTION_FILE>` = `AGENTS.md` | `<SKILLS_DIR>` = `.agents/skills/`

### 2. Project Constitution & Cross-Agent Pointer Bridge (Anti Split-Brain)
1. Create the primary `<CONSTITUTION_FILE>` as the **Supreme Project Law (Project Law)**:
   - JSON Data Schemas (Input / Output / Database table shapes).
   - Behavioral Rules & State Preservation Invariants.
   - Architectural Invariants & Tech Stack Commands.
2. Create 1-line pointer bridge files for the other two alternate constitution filenames:
   ```markdown
   Please read and strictly follow all rules, schemas, and architectural invariants defined in @<CONSTITUTION_FILE>.
   ```
   *(Ensures all agents share a single source of truth across tool switches).*

### 3. Initialize Working Memory
Create in project root:
- `task_plan.md`: Phases, goals, and step-by-step verifiable checklists.
- `findings.md`: Research discoveries, API endpoints, rate limits, and constraints.
- `progress.md`: Execution logs, errors encountered, test results, and milestones.

### 4. Scaffold Directory Structure
Create:
- `architecture/` (Technical SOPs)
- `tools/` (Atomic scripts & tests)
- `.tmp/` (Ephemeral scratchpad)

### 5. EMERGENCY BRAKE (Strict Halt Execution)
**DO NOT** write code inside `tools/` or construct UI until:
- [ ] 5 Discovery Questions have been answered by the user.
- [ ] JSON Data Schema is defined in `<CONSTITUTION_FILE>`.
- [ ] Blueprint & checklist in `task_plan.md` are approved by the user.

---

## Phase 1: B - Blueprint (Vision, Logic & Data-First)

1. **Discovery Questions**: Ask these 5 questions and await explicit user responses:
   - **North Star**: What is the singular desired outcome of this system/application?
   - **Integrations & MCP**: What external services, APIs, or MCP servers are required? Can we start with zero-API-key / open-access endpoints for instant validation?
   - **Source of Truth**: Where is primary data sourced and stored? *(Default recommendation: Local-First with SQLite/JSON structured 1:1 ready for Supabase/Cloud migration)*.
   - **Delivery Payload**: How is the final result delivered? (Interactive Localhost Web Dashboard, Webhook alert, Automated Cron digest, CLI)?
   - **Behavioral Rules**: How must the system behave? (Deduplication, state persistence, tone, "Do Not" guardrails).
2. **Data-First Rule**: Explicitly define JSON Data Schemas in `<CONSTITUTION_FILE>`. Code generation only begins once payloads and database shapes are locked.
3. **Smart Research (MCP & Skills First)**: Audit active MCP servers and installed skills before hand-rolling custom scrapers or pipelines. Record findings in `findings.md`.

---

## Phase 2: L - Link (Connectivity & Handshake)

1. **Verification**: Verify all required endpoints, MCP servers, and `.env` credentials.
2. **Handshake Probe**: Write and execute a lightweight probe script `tools/test_connectivity.py` to assert external APIs respond with `200 OK` and expected payloads.
3. **Gatekeeper & Fallback**: Never proceed to Phase 3 if handshakes fail. If blocked, rate-limited, or paywalled, discover fallbacks (RSS, public APIs, caching) and document in `findings.md`.

---

## Phase 3: A - Architect (The 3-Layer A.N.T. Build)

Separate planning from execution. LLMs are probabilistic; business logic must be deterministic.

### Layer 1: Architecture (`architecture/`)
- Markdown Technical SOPs (e.g., `fetch_sop.md`, `storage_sop.md`, `server_sop.md`).
- Defines goals, inputs, execution steps, schema migrations, and edge cases.
- **The Golden Rule**: When business logic changes, update the SOP in `architecture/` and `<CONSTITUTION_FILE>` **before** touching application code!

### Layer 2: Navigation (Orchestration & Decision Making)
- Orchestration layer (e.g., `tools/pipeline.py` or `tools/server.py`) invoking atomic tools in order with graceful error fallbacks.

### Layer 3: Tools (`tools/`)
- Deterministic, atomic (single responsibility), and testable Python/Node scripts.
- Secrets reside strictly in `.env` (never hardcoded).
- Use `.tmp/` for raw/ephemeral file handling.
- **State Preservation Invariant**: During schema migrations (`ALTER TABLE`, `UPSERT`), user states (saved items, soft-deletes, preferences) must never be wiped or reset.

---

## Phase 4: S - Stylize (Anti-AI-Slop UI & Proactive E2E Testing)

1. **Anti-AI-Slop Design**:
   - For web frontends, ban generic purple-blue gradient templates. Use crisp design tokens, clean geometry, 1px borders, subtle contrast, and `tabular-nums` for data.
   - **Architecture-Safe UI Sniping**: When given UI references (21st.dev, CodePen, Dribbble, or brand guidelines), extract native CSS variables and layouts into the current frontend stack without disturbing the backend contract.
2. **Payload Refinement**: Ensure outputs (cards, drawers, notifications) are clean, scannable, and paywall-free.
3. **Proactive E2E Verification**: Build and execute `tools/test_e2e.py` validating 100% of endpoints, database mutations, and UI flows before reporting task completion.

---

## Phase 5: T - Trigger (Security Audit, Cloud Deployment & Packaging)

1. **Pre-Flight Security Audit**: Verify `.env`, `.tmp/`, and local DBs are in `.gitignore`. Ensure zero API keys leak to client bundles. Enforce Row-Level Security (RLS) on Supabase.
2. **Cloud Transfer & Automation**: Transition validated local workflows to production environments (Supabase, Vercel, Modal/Cron).
3. **Skill Codification**: Finalize Maintenance Logs in `<CONSTITUTION_FILE>`. If the built tools are reusable, package them as a permanent skill (`<SKILLS_DIR>/<skill-name>/SKILL.md`).

---

## Operating Principles

1. **Constitutional Law vs. Working Memory**:
   - `<CONSTITUTION_FILE>` = Supreme Law (Schemas, Constraints, Tech Stack).
   - `task_plan.md`, `findings.md`, `progress.md` = Working Memory.
2. **Self-Annealing Repair Loop**:
   When any script or test fails:
   - **Analyze**: Inspect stack traces directly without guessing.
   - **Patch**: Fix scripts in `tools/`.
   - **Verify**: Re-run tests until 100% pass.
   - **Update Architecture**: Document root causes in `findings.md` and SOPs in `architecture/`.
3. **Deliverables vs. Intermediates**:
   - `.tmp/`: Scratchpad data (safe to purge).
   - Final Delivery: Localhost dashboard, production database, or webhook alert.

---

## Project Structure Reference

```text
├── <CONSTITUTION_FILE>    # gemini.md / CLAUDE.md / AGENTS.md (Law & Schemas)
├── task_plan.md           # Phases, Goals & Execution Checklists
├── findings.md            # Research, Endpoint Discoveries & API Constraints
├── progress.md            # Execution Logs, Errors & E2E Test Results
├── .env                   # API Keys/Secrets (Verified & gitignored)
├── architecture/          # Layer 1: Technical SOPs in Markdown (The "How-To")
├── tools/                 # Layer 3: Atomic Scripts, Server & Automated Tests
│   ├── test_connectivity.py
│   └── test_e2e.py
├── public/ (or src/)      # Layer 4: Clean Frontend UI Assets (Stylize Phase)
└── .tmp/                  # Temporary Workbench (Ephemeral Intermediates)
```
