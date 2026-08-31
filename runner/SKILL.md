---
name: renew-a-household-service-personal-runner
description: Run the "Renew a household service (personal)" action through the automation API and poll run status.
---

# Renew a household service (personal) Runner

## Goal
Run an existing action and return the `runId` so the caller can track progress.

## Authentication
- Use an API token in `Authorization: Bearer <token>`.
- Recommended scope: `api:access` (legacy: `automation:run` + `automation:read`).

## Required Input Keys
This action has no required runtime inputs.

## Start Run
- Endpoint: `POST https://gabrieloperator.com/api/automation/run/9beee179-c29d-4d19-9899-8964f57827a1/6a90198a9334d064acf7c5de`
- JSON body:

```json
{
  "parameters": {},
  "runContext": {},
  "dynamicLoopItems": [],
  "selectedLoopGroupId": null,
  "connectorOverrides": [],
  "variableOverrides": {},
  "liveBrowserMode": false,
  "liveBrowserProviderId": "auto",
  "name": "API Run"
}
```

## Poll Status
- Endpoint: `GET https://gabrieloperator.com/api/automation/status/{runId}`
- Continue polling until status is terminal (`COMPLETED`, `FAILED`, or `CANCELLED`).

## Expected Response Format

When this connector completes, write your primary output using a `set_api_output` step
so it appears in the status response as `data.output`.

The calling supervisor reads `data.output` from `GET https://gabrieloperator.com/api/automation/status/{runId}` once the run
reaches a terminal status (`COMPLETED`, `FAILED`, or `CANCELLED`).

### set_api_output contract
The output must be a JSON object. Recommended structure:

```json
{
  "result": "<primary answer or extracted data>",
  "summary": "<one-sentence human-readable summary>",
  "metadata": {}
}
```

### Take-control behaviour
If a step requires human interaction, the run transitions to `paused` status.
The status endpoint returns:

```json
{
  "status": "paused",
  "pauseContext": {
    "message": "Human input required",
    "url": "https://...",
    "stepNumber": 5
  },
  "liveDebugUrl": "https://..."
}
```

Resume the run with:
```
POST https://gabrieloperator.com/api/automation/resume/{runId}
{ "agentId": "9beee179-c29d-4d19-9899-8964f57827a1", "actionId": "6a90198a9334d064acf7c5de" }
```

## Key Learnings
- Performance: Execution duration remains stable, showing a slight improvement to 1.5s in the most recent cycle.
- Browser Lifecycle: The "Cannot read properties of null (reading 'close')" error remains a consistent, non-breaking artifact during cleanup.
- Notification: System-level completion notifications are consistently triggered and delivered.
- Reliability: The workflow maintains high autonomy with interactive step recovery explicitly disabled.
- Stability: The integration maintains full functional stability with zero step failures across the 5 most recent executions.

## Run History
- 0d683dd6 | 2026-08-31 | completed
- db75f3f3 | 2026-08-31 | completed
- 091a343b | 2026-08-31 | completed
- 4d0f2092 | 2026-08-31 | completed
- db8a812c | 2026-08-31 | completed

## Resources

## Troubleshooting