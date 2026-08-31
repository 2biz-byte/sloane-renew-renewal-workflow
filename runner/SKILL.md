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
- Performance: Run duration remains consistent at ~1.0s–1.4s, indicating stable execution overhead.
- Browser Lifecycle: Non-critical "Cannot read properties of null (reading 'close')" errors during browser cleanup persist as a recurring harmless log pattern.
- Notification: System completion notifications are reliably triggered upon terminal state.
- Reliability: Interactive step recovery remains disabled; workflows should be designed to handle state transitions autonomously.

## Run History
- c34a93c1 | 2026-08-31 | completed
- d47a1f5c | 2026-08-31 | completed
- 355a699f | 2026-08-31 | completed
- 2b8efd42 | 2026-08-31 | completed
- 8519b6d9 | 2026-08-31 | completed

## Resources

## Troubleshooting