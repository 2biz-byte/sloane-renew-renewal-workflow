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
- Performance: Run durations remain stable (1.1s - 2.1s), with the most recent run at 1.5s.
- Browser Lifecycle: Intermittent "null reference" errors during browser closure (reading 'close') continue to occur; these are non-critical and do not impact task success.
- Notification: Completion notifications are consistently dispatched at the end of the workflow.
- Reliability: Interactive step recovery is consistently disabled; internal workflow state management is required for error handling.

## Run History
- 3ab4a1c7 | 2026-08-31 | completed
- aee94de7 | 2026-08-31 | completed
- 0f52af01 | 2026-08-31 | completed
- 61574f5c | 2026-08-31 | completed
- a02a699d | 2026-08-31 | completed

## Resources

## Troubleshooting