---
name: Enhance an agent task with Osmosis memory
description: Send an agent's current input to Osmosis to get an improved response that draws on previously stored interaction knowledge for the tenant.
api: openapi/osmosis-agent-improvement-openapi.json
operations: [process_agent_input_enhance_task_post]
---

# Enhance an agent task

Use the Osmosis Agent Improvement API to improve an agent's response by learning
from past interactions stored for your tenant.

## Auth
All requests require an `x-api-key: <YOUR_API_KEY>` header (securityScheme `ApiKeyAuth`).

## Steps
1. (Optional) Confirm the service is up with `GET /` (`health_check__get`).
2. Call `POST /enhance_task` (`process_agent_input_enhance_task_post`) with an
   `AgentInput` body:
   - `tenant_id` (required) — your tenant identifier.
   - `input_text` (required) — the agent's current query/task text.
   - `context` (optional object) — any structured context for the task.
   - `agent_type` (optional) — the agent type, used to scope stored knowledge.
3. Read the `EnhancedResponse`: `response` (the improved text) and optional `metadata`.

## Errors
- `422` Validation Error → FastAPI envelope `{ detail: [ { loc, msg, type } ] }`.
  Check that `tenant_id` and `input_text` are present. See `errors/osmosis-problem-types.yml`.

## Conventions
See `conventions/osmosis-conventions.yml`: api-key auth, per-`tenant_id` isolation,
no idempotency key, FastAPI validation error envelope.
