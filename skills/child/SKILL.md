---
name: renew-a-household-service-personal-child
description: Child execution skill for Renew a household service (personal). 5 steps, mode: browserless.
type: child_skill
---

# Renew a household service (personal) — Child Skill

## Purpose
Execute the "Renew a household service (personal)" automation workflow.

## Operator Details
- **Mode:** browserless
- **Steps:** 5
- **Groups:** 0

## Step Summary

| # | Action | Intent |
|---|--------|--------|
| 1 | navigate | Start Canvas service renewal review |
| 2 | persona_capability | Gmail â€” read_contract |
| 3 | persona_capability | Sheets â€” summarize_usage |
| 4 | persona_capability | Gmail â€” draft_email |
| 5 | persona_capability | Mark draft ready |

## Execution Notes
- Steps execute sequentially by step_number
- Template variables ({{stepId.var}}) resolve from prior step exports
- Browser steps use selectorPrompts as vision AI fallback if selectors fail
