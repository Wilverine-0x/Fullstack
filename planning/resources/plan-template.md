# [Feature Name] Implementation Plan

> **Goal:** [Concise statement describing what this builds and its user impact]  
> **Architecture:** [2-3 sentences explaining architectural patterns and design approach]  
> **Tech Stack:** [Languages, frameworks, testing libraries]  
> **Spec Reference:** [Path to design specification, e.g. docs/specs/YYYY-MM-DD-<feature>-design.md]  

---

## Global Constraints
- [Floor versions, e.g. Node >= 20.0, Python >= 3.11]
- [Bundle size limits, styling restrictions, or conventions]
- [Forbidden libraries or deprecated patterns]

---

## Review Focus (Top 5 Production Failure Modes)
1. **[Edge Case / Input Condition 1]:** [Expected graceful response]
2. **[Edge Case / Input Condition 2]:** [Expected graceful response]
3. **[Edge Case / Input Condition 3]:** [Expected graceful response]
4. **[Edge Case / Input Condition 4]:** [Expected graceful response]
5. **[Edge Case / Input Condition 5]:** [Expected graceful response]

---

## Tasks

### Task 1: [Module or Component Name]
**Files:**
- Create: `src/path/to/module.ts`
- Test: `tests/path/to/module.test.ts`

**Interfaces:**
- Consumes: `[Any external signatures or dependencies]`
- Produces: `export function handleAction(arg: ArgType): ReturnType`

- [ ] **Step 1: Write failing test**
```typescript
// Test code with assertions
```

- [ ] **Step 2: Run test to verify failure**
  Run: `[test runner command]`
  Expected: FAIL with `[expected error string]`

- [ ] **Step 3: Implement minimal code**
  Write minimal implementation in `src/path/to/module.ts`.

- [ ] **Step 4: Verify test passes**
  Run: `[test runner command]`
  Expected: PASS

- [ ] **Step 5: Commit**
  Run: `git add <files> && git commit -m "feat(scope): <message>"`
