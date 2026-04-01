---
name: ux-ui-agent-execute
description: >
  Executes the ux-ui-designer agent for a given feature spec. Reads the UX/UI
  requirements and the approved spec, applies the knowledge base and template,
  and writes or extends .github/docs/ux-ui-design.md. Must be called after
  spec-generator and before architectural-builder.
argument-hint: "<feature-name>"
---

# UX-UI Agent Execute

## Pre-conditions
Before executing, verify:
- `.github/specs/{feature}.spec.md` exists and status is `APPROVED`
- `.github/requirements/requirements.md` exists
- `.github/instructions/ux-ui-knowledge.instructions.md` exists
- `.github/instructions/ux-ui-design-template.instructions.md` exists

If any pre-condition fails, stop and report which file is missing or blocked.

## Execution
1. Read `.github/agents/ux-ui-designer.agent.md`
2. Follow its execution protocol exactly for the feature `{argument}`

## Post-conditions
The skill is complete when:
- `.github/docs/ux-ui-design.md` exists and contains a Section 5 block for `{argument}`
- The agent has reported `UX-UI DESIGN COMPLETE` with no unresolved `[PENDING]` items
  that would block the architectural-builder

## Output to orchestrator
Report the agent summary block verbatim so the orchestrator can log it.