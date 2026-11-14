# Skill Template Reference

Use this template as the standard scaffold when generating a new `SKILL.md`.

```markdown
---
name: [skill-name-in-gerund-or-kebab-case]
description: [Third-person description summarizing capabilities and specifying exact triggers/keywords. Max 1024 chars.]
---

# [Skill Title]

[Concise 1-2 sentence description explaining the purpose of this skill]

## When to Use This Skill
- [Trigger 1 / keyword]
- [Trigger 2 / user intent]
- [Scenario where this workflow must be followed]

## Workflow
Copy and track this execution checklist:
- [ ] 1. Analyze requirements, environment, and file targets
- [ ] 2. Plan changes and validate preconditions
- [ ] 3. Execute implementation using prescribed patterns
- [ ] 4. Run automated checks, lints, or test scripts
- [ ] 5. Confirm deliverables match acceptance criteria

## Instructions & Core Logic

### Heuristics & Standards (High Freedom)
- Guideline 1
- Guideline 2

### Code & Pattern Templates (Medium Freedom)
```[language]
// Standard template pattern
```

### Critical Commands & Operations (Low Freedom)
```bash
# Exact commands to run
npm test -- --run
```

## Error Handling & Feedback Loops
- If validation fails, revert changes and inspect diagnostic outputs.
- When calling CLI tools, pass `--help` or `--dry-run` if option parameters are unclear.

## Resources & Supporting Files
- `scripts/`: Helper scripts and automated runners.
- `examples/`: Reference implementations.
- `resources/`: Configuration files and schemas.
```
