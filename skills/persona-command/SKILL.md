---
name: persona-command-editor
description: Explain, extend, or modify a persona slash command across its parent chat configuration and connected Operator workflow.
metadata:
  version: "1.1"
  pageId: "9beee179-c29d-4d19-9899-8964f57827a1"
  commandId: "cmd_02997294-7ccb-47fd-975a-31612f3e4a7f"
  actionId: "6a90198a9334d064acf7c5de"
---

# Persona Slash Command Editor

Use this skill whenever the author asks what /renew-a-household-service-personal does, wants to add a feature, or wants to modify its existing behavior.

## Connected implementation

- Command: `/renew-a-household-service-personal`
- Operator action id: `6a90198a9334d064acf7c5de`
- Current description: Review a household service renewal (e.g. home internet) and draft a negotiation email before the contract renews. Drafts only, never sends.
- Parent persona repository: `https://github.com/2biz-byte/sloane-persona.git`
- Parent registration source: `assets/chat-config.json` (runtime registry only)
- Command workflow repository: `https://github.com/2biz-byte/sloane-renew-renewal-workflow.git`
- Executable source of truth: `assets/workflow.json`
- Slash-command debug graph: `assets/slash-connections.json`
- Relationship manifest: `assets/persona-command.json`

The parent persona configuration owns **registration only**: id, trigger, label, description, enabled state, presentation, and the Operator action reference. This workflow repository owns executable steps, prompts, runtime configuration, inputs, outputs, generated child skills, and the slash-connection **debug/docs** graph.

## First-response protocol

Before changing files:

1. Read `assets/persona-command.json`, this skill, the root `SKILL.md`, `runner/SKILL.md`, `assets/workflow.json`, and `assets/slash-connections.json`.
2. Open the parent persona repository only to verify registration: read its `SKILL.md` and locate command id `cmd_02997294-7ccb-47fd-975a-31612f3e4a7f` in `assets/chat-config.json` (confirm Operator action linkage).
3. Do **not** expect or edit `assets/slash-connections/` in the parent persona repository — debug graphs live here as `assets/slash-connections.json`.
4. Summarize the current feature in specific terms: invocation inputs, routing, major workflow stages, outputs, approvals, and user-visible behavior.
5. Tell the author that you can explain the implementation, extend it with new features, or modify existing behavior, then ask what they want to add or change.

If the initial request already describes a concrete change, summarize your understanding and identify which repository or repositories need edits before implementing. If a required repository or file is unavailable, report exactly what is missing instead of guessing.

## Editing rules

1. Decide file ownership before editing:
   - Edit parent `assets/chat-config.json` for registration, trigger, label, description, enabled state, presentation, or action linkage only.
   - Edit command `assets/workflow.json` for executable behavior, stages, prompts, runtime configuration, inputs, outputs, approvals, or artifacts.
   - Treat `assets/slash-connections.json` as server-owned debug documentation in this workflow repo (do not reinvent it in chat-config).
   - Edit both repositories when a feature crosses the routing/execution boundary.
2. Preserve `cmd_02997294-7ccb-47fd-975a-31612f3e4a7f` and its Operator action linkage unless the author explicitly requests replacement.
3. Follow the root workflow-builder skill and action-specific child skills. Preserve the locked runtime-start step and keep all step fields flat.
4. Update generated child skills whenever executable behavior changes.
5. Validate `assets/workflow.json` with the repository validator and keep `assets/chat-config.json` valid JSON without credentials or secrets.
6. Do not duplicate executable steps in chat config, and do not store slash-command debug graphs or docs dumps in the parent chat-config repository.
7. Summarize changes by repository, commit each affected repository, and push each default branch.
