---
name: brainstorming
description: Guides the collaborative discovery of user requirements, goals, and constraints before building. Explores intent through focused, single-question inquiries, produces written design specifications, and enforces approval gates before code is written. Use before creative work, feature design, component creation, or new project planning.
---

# Brainstorming & Design Discovery

Discover what the human partner actually wants and why before anything gets built. Good solutions require understanding user intent, constraints, and non-goals; making assumptions leads to wasted work.

## When to Use This Skill
- Before starting any creative, architectural, or feature development task.
- When creating new components, changing existing behaviors, or building new workflows.
- When the user's request is non-trivial or open-ended.
- When the user asks to brainstorm, explore concepts, or create a design specification.

> **Skip Condition:** If the request is a single, unambiguous instruction where all details are already fully specified (e.g., "change icon color to `#047857`"), do it directly without brainstorming.

---

## Workflow Checklist
- [ ] **Phase 1: Knowledge Assessment & Opening** — Determine knowns/unknowns; send opt-out option + one open question.
- [ ] **Phase 2: Focused Socratic Exploration** — Ask exactly one question per turn, match user vocabulary, avoid jargon.
- [ ] **Phase 3: Incremental Playback** — Reflect understanding in 200–300 word chunks as details emerge.
- [ ] **Phase 4: Sizing & Design Document** — Draft the specification document (`docs/specs/YYYY-MM-DD-<topic>-design.md`).
- [ ] **Phase 5: Builder Review Check** — Stress-test the design document against ambiguous assumptions.
- [ ] **Phase 6: Approval Gate & Handoff** — Obtain explicit approval before any code or planning begins.

---

## Phase 1: Knowledge Assessment & Opening

Before replying, evaluate what is known and what is missing:
- **Trivial/Complete:** Everything needed is present. Execute immediately.
- **Non-trivial or Open-ended:** Begin the brainstorming interaction.

### Opening Message Structure
The very first message must contain only two elements:
1. **Opt-out clause (1 line):** Inform the user that if they prefer to skip discovery and start immediately, they can simply say so.
2. **One open question (1 line):** Ask about the real scenario, user friction, or concrete objective (e.g., *"What is the exact moment or friction that made you wish this feature existed?"*).

---

## Phase 2: Focused Socratic Exploration

### The One-Question Rule
- **One question per message:** End each response with a single, clear question.
- **Never provide overwhelming menus of options:** Asking the user to describe in their own words uncovers true intent. Offer multiple-choice options only if the user is stuck or explicitly asks *"what do you suggest?"*.
- **Match vocabulary:** Speak in the user's domain terminology. Never use internal process jargon.

### Territory to Explore
Explore these dimensions naturally throughout the dialogue:
- **Core Concept & Differentiator:** What makes this distinctive?
- **Urgency & Success Criteria:** Why build this now, and what does success look like?
- **Goals, Non-Goals, and Anti-Goals:** What must this NOT do? What would make this a failure even if technically functional?
- **Prior Art & Existing Solutions:** What tools or patterns have they seen, liked, or disliked?
- **Current Technical Context:** What existing libraries, conventions, or constraints apply?

### Exploration Techniques
- **Anchored Questions:** *"Walk me through the last time you encountered this issue..."* or *"What would make you wince if I got it wrong?"*
- **Reconnaissance Offer:** Offer to inspect the codebase or documentation to verify technical assumptions before proposing an approach.
- **Throwaway Spikes:** If uncertainty is high, propose a quick, disposable prototype to test feel and feasibility. Mark spikes explicitly as throwaway; keeping code from a spike requires re-entering the approval gate.

---

## Phase 3: Incremental Playback

Do not wait until the very end to summarize:
- Once an aspect is well understood (e.g., core user journey, data schema, or permission model), play it back in 200–300 words.
- Explicitly separate facts from assumptions: mark your interpretations as *"My assumption:"* and user statements as *"Confirmed requirement:"*.
- End playback by asking: *"What in this part is incorrect or missing?"*.

---

## Phase 4: Sizing & Written Design Document

Categorize the work scope:
1. **Quick Task:** Handled directly in chat.
2. **Small Change:** Handled through conversational consensus, then implemented.
3. **Project / Feature:** Requires a written design specification saved to:
   `docs/specs/YYYY-MM-DD-<topic>-design.md`

### Design Specification Content
The design document must cover:
- Context, intent, and problem statement.
- Primary goals, non-goals, and anti-goals.
- User personas, user flows, and interface boundaries.
- Technical decisions decided by the user vs. technical decisions delegated to the implementer.
- Edge cases and failure state behaviors.

*(Reference template available at [resources/design-spec-template.md](resources/design-spec-template.md))*

---

## Phase 5: Builder Review Check

Before presenting the final specification to the user:
1. Review the document from the perspective of an independent senior engineer who has never seen the discussion.
2. Identify lingering ambiguities using [resources/builder-check-prompt.md](resources/builder-check-prompt.md).
3. Categorize findings into three buckets:
   - **Already Answered:** Clarify directly in the document.
   - **Minor Implementation Detail:** Make an engineering call, document it as your recommendation.
   - **Strategic / User Decision:** Ask the user in a single concise message before finalizing.

---

## Phase 6: The Hard Approval Gate

```
┌─────────────────────────────────────────────────────────────┐
│                       <HARD-GATE>                           │
│  ABSOLUTELY NO PRODUCT CODE, SCAFFOLDING, DEPENDENCY        │
│  INSTALLATION, OR PLANNING SKILL EXECUTION MAY OCCUR        │
│  UNTIL THE HUMAN PARTNER EXPLICITLY APPROVES THE DESIGN DOC │
└─────────────────────────────────────────────────────────────┘
```

- When the design document is ready, provide a link and ask for review:
  > *"Design specification created at `docs/specs/YYYY-MM-DD-<topic>-design.md`. Please review. Does this accurately reflect what you want to build?"*
- Once approved, hand off directly to the **`planning`** skill.

---

## Anti-Patterns & Red Flags

| Red Flag / Temptation | Reality & Corrective Action |
| :--- | :--- |
| *"I'll list 5 potential approaches right away."* | Pre-empts user intent. Ask an open question to understand user context first. |
| *"I'll assume standard defaults silently."* | State every default out loud so the user has the opportunity to reject it. |
| *"User said 'looks cool', I will start coding."* | Approval must be for the written specification, not just a vague concept. |
| *"The spike worked, so I'll keep building on top of it."* | Spikes lack tests and architectural rigor. Re-enter through the planning gate. |
