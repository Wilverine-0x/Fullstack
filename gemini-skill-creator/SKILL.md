---
name: gemini-skill-creator
description: Guides the creation and validation of high-quality agent skills, defining folder structures, YAML frontmatter standards, progressive disclosure, and execution workflows. Use whenever creating, scaffolding, or refining new skills for Antigravity or Gemini agent environments.
---

# Gemini Skill Creator System Instructions

Use this skill whenever you are asked to create, design, scaffold, or refine a skill for Antigravity or Gemini agent environments. It defines the structural standards, frontmatter specifications, writing principles, and workflow patterns required for predictable and effective agent skills.

## 1. Skill Locations & Scope

Skills can be installed in two locations:
1. **Global Skills (Available across all projects)**:
   - Path: `<global_root>/skills/<skill-name>/`
   - Windows Default: `C:/Users/<Username>/.gemini/config/skills/<skill-name>/`
2. **Workspace / Project Skills (Available within the current repository)**:
   - Path: `<workspace_root>/.agents/skills/<skill-name>/`

Every skill must have its own directory containing at minimum a valid `SKILL.md` file.

---

## 2. Core Structural Hierarchy

Every skill must adhere to this folder structure:
```
<skill-name>/
├── SKILL.md                 # Required: Main instruction logic & frontmatter
├── scripts/                 # Optional: Helper scripts & automation utilities
├── examples/                # Optional: Reference implementations & templates
└── resources/               # Optional: Assets, schemas, or reference documents
```

---

## 3. YAML Frontmatter Standards

Every `SKILL.md` file **must** begin with a YAML frontmatter block adhering to these strict rules:

```yaml
---
name: [skill-name]
description: [Third-person description with concrete triggers and keywords]
---
```

- **`name`**:
  - Matches the skill directory name.
  - Lowercase alphanumeric characters and hyphens only (`[a-z0-9-]`).
  - Max 64 characters.
  - Recommended gerund or kebab-case format (e.g. `testing-code`, `gemini-skill-creator`, `anti-ai-slop-design`).
  - Do **not** use reserved vendor keywords like "claude" or "anthropic" in the name.
- **`description`**:
  - Written in **third person** (e.g., "Guides the creation of...", "Extracts text from...").
  - Must explicitly mention triggers, keywords, and scenarios for when the skill should be invoked.
  - Max 1024 characters.

---

## 4. Writing Principles

When writing the content of `SKILL.md`:

1. **Conciseness & High Information Density**:
   - Assume the AI agent is intelligent. Avoid basic tutorial explanations (e.g., do not explain what Git, JSON, or a REST API is). Focus exclusively on the domain logic, behavioral constraints, and actionable instructions.
2. **Progressive Disclosure**:
   - Keep `SKILL.md` clean, focused, and under 500 lines.
   - If extensive documentation or schemas are needed, link to auxiliary files in the skill directory (e.g. `[See ADVANCED.md](ADVANCED.md)` or `resources/schema.json`) at most one level deep.
3. **Forward Slashes**:
   - Always use forward slashes `/` for paths, never backslashes `\`, regardless of the host OS.
4. **Degrees of Freedom**:
   - **Bullet Points**: Use for high-freedom heuristics and conceptual guidelines.
   - **Code Blocks**: Use for medium-freedom templates and standardized patterns.
   - **Specific Commands**: Use for low-freedom, fragile operations (exact CLI invocations).

---

## 5. Workflow & Feedback Loops

For complex skills and multi-step tasks, incorporate:

1. **Checklists**:
   - Provide a clear Markdown checklist that the agent can track as execution progresses.
2. **Validation Loops (Plan-Validate-Execute)**:
   - Enforce verification steps before applying disruptive changes (e.g., validate syntax or check configurations before writing files).
3. **Black-Box Script Execution**:
   - Instruct the agent to run scripts with `--help` or `--dry-run` when discovering flags or arguments.
   - Include robust error-handling guidance.

---

## 6. Standard Output Template

When requested to create a new skill, produce the output following this standardized structure:

### Target Directory
- **Global**: `C:/Users/<Username>/.gemini/config/skills/<skill-name>/`
- **Workspace**: `.agents/skills/<skill-name>/`

### `SKILL.md` Template
```markdown
---
name: [skill-name]
description: [3rd-person description with specific keywords and triggers]
---

# [Skill Title]

[Brief 1-2 sentence overview of the skill purpose]

## When to Use This Skill
- Trigger scenario 1
- Trigger scenario 2
- User mentions keyword / task

## Workflow
- [ ] Step 1: Analyze requirements and environment
- [ ] Step 2: Validate preconditions
- [ ] Step 3: Execute core task
- [ ] Step 4: Verify results

## Instructions & Rules
[Domain-specific logic, code snippets, constraints, and patterns]

## Resources & Scripts (If applicable)
- `scripts/`: Helper automation
- `examples/`: Example patterns
```

---

## 7. Skill Creator Resources
- [skill-template.md](resources/skill-template.md): Reusable starter template for new skills.
- [testing-vitest-example.md](examples/testing-vitest-example.md): End-to-end reference implementation.

