---
name: planning
description: Generates comprehensive, test-driven implementation plans broken into bite-sized, verifiable tasks from design specs or user requirements before touching code. Enforces exact file paths, signatures, TDD verification steps, and human review gates. Use when planning multi-step features, creating development roadmaps, or writing implementation plans.
---

# Planning & Implementation Plans

Transform approved design specifications or complex requirements into rigorous, bite-sized, test-driven implementation plans. The plan must allow an engineer unfamiliar with the project to implement the feature cleanly without guessing interfaces or edge behaviors.

## When to Use This Skill
- After completing a design specification via the `brainstorming` skill.
- When embarking on multi-step feature implementations or complex refactoring.
- Whenever structured task breakdown, architectural decomposition, and TDD verification are required.
- When the user asks to plan, write an implementation roadmap, or prepare a task checklist.

---

## Workflow Checklist
- [ ] **Step 1: Scope & Subsystem Decomposition** — Verify if scope should be split into independent plans.
- [ ] **Step 2: File Structure & Interface Mapping** — Define exact file responsibilities, imports, and contracts.
- [ ] **Step 3: Draft Header & Review Focus** — Establish global constraints and top 5 unhandled failure modes.
- [ ] **Step 4: Task Right-Sizing & Step Authoring** — Break work into atomic, test-driven tasks (TDD loop).
- [ ] **Step 5: 5-Point Self-Review** — Audit plan against the spec for coverage, consistency, and proportion.
- [ ] **Step 6: Handoff & Execution Strategy** — Save to `docs/plans/` and present execution options to the user.

---

## Plan Location & Naming
Implementation plans must be saved to:
`docs/plans/YYYY-MM-DD-<feature-name>.md`  
*(User preferences for plan location override this default).*

---

## 1. Architectural Decomposition & File Structure

Before authoring tasks, lock in boundaries:
- **Single Responsibility:** Each file must have one distinct, clear purpose. Small, focused files minimize agent context errors.
- **Locality of Change:** Files that change together should live together. Group by business domain rather than purely technical layer.
- **Respect Existing Patterns:** In established codebases, mirror prevailing conventions. If existing files are bloated, plan explicit extraction tasks.

---

## 2. Task Right-Sizing & Step Granularity

### Task Sizing
- A **Task** is the smallest cohesive unit of work that contains its own complete test cycle and is worthy of an independent review gate.
- Fold setup, scaffolding, and type definitions into the task whose deliverable consumes them.
- Each task must yield an independently testable deliverable.

### Step Granularity
Each step within a task represents **one atomic action with a verifiable result**:
1. **Write failing test** — Provide exact test name, inputs, and expected assertion.
2. **Run test to verify failure** — Provide exact CLI command and expected error output.
3. **Implement minimal code** — Provide file path, method signature, and approach.
4. **Run test to verify pass** — Provide exact CLI command and expected pass output.
5. **Commit** — Exact `git add` and conventional commit command.

---

## 3. Standard Plan Document Structure

Every plan document must adhere to the following schema:

```markdown
# [Feature Name] Implementation Plan

> **Goal:** [One sentence summarizing the user-facing outcome]  
> **Architecture:** [2-3 sentences explaining architectural approach]  
> **Tech Stack:** [Languages, libraries, key frameworks]  
> **Spec Reference:** [Path to design doc, e.g., docs/specs/YYYY-MM-DD-<topic>-design.md]  

## Global Constraints
- [Constraint 1: exact library version, language standard, or naming convention]
- [Constraint 2: performance ceiling, bundle size limit, or backward compatibility]

## Review Focus (Edge Cases & Failure Modes)
Identify the 5 critical failure modes or unhandled input classes most likely to break in production:
1. [Condition/Input 1: Expected graceful behavior]
2. [Condition/Input 2: Expected graceful behavior]
3. [Condition/Input 3: Expected graceful behavior]
4. [Condition/Input 4: Expected graceful behavior]
5. [Condition/Input 5: Expected graceful behavior]

---

## Task Decomposition

### Task 1: [Component / Unit Name]
**Files:**
- Create: `src/path/to/component.ts`
- Test: `tests/path/to/component.test.ts`

**Interfaces:**
- Consumes: [Signatures of utilities or types needed]
- Produces: `export function processData(input: InputSchema): ResultSchema`

- [ ] **Step 1: Write failing test**
```typescript
test('should validate and parse payload correctly', () => {
  const result = processData({ ... });
  expect(result.status).toBe('success');
});
```
- [ ] **Step 2: Run test to verify failure**
  Run: `npm test tests/path/to/component.test.ts`
  Expected: FAIL with `processData is not defined`
- [ ] **Step 3: Implement minimal code in `src/path/to/component.ts`**
  Implement signature and core parsing logic.
- [ ] **Step 4: Verify test passes**
  Run: `npm test tests/path/to/component.test.ts`
  Expected: PASS
- [ ] **Step 5: Commit**
  Run: `git add src/path/to/component.ts tests/path/to/component.test.ts && git commit -m "feat(scope): implement processData handler"`
```

*(Reference full starter template in [resources/plan-template.md](resources/plan-template.md))*

---

## 4. Rigorous 5-Point Self-Review

Before presenting the plan to the user, run this internal audit:

| Audit Check | Verification Criteria |
| :--- | :--- |
| **1. Spec Coverage** | Every requirement in the spec is traced to at least one task. No unmapped features. |
| **2. Step Unambiguity** | Every step leaves zero guesswork. Commands and assertions are literal and runnable. |
| **3. Type & Signature Consistency** | Function names and type contracts match identically across tasks (no mismatched naming). |
| **4. Review Focus Integration** | All 5 critical failure modes identified in Review Focus have dedicated tests in owning tasks. |
| **5. Proportion & Brevity** | The plan is not a full transcript of the code. Implementers write standard bodies; only novel algorithms, exact signatures, and edge assertions are prescribed. |

---

## 5. Execution Handoff & Strategy

Once the plan is drafted and validated, link the document and present execution options:

> *"Plan complete and saved to `docs/plans/YYYY-MM-DD-<feature-name>.md`. Please review. Which execution approach would you prefer?"*

- **Subagent-Driven Execution:** A fresh subagent is dispatched per task, reviewed independently before moving to the next. Best for critical, high-reliability features.
- **Native Inline Execution:** Implemented step-by-step within the current session, followed by an end-to-end verification. Fastest and most token-efficient.
