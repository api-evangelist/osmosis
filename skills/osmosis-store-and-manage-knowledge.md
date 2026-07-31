---
name: Store and manage agent interaction knowledge
description: Upload an agent's completed interaction turns to Osmosis, poll the ingestion job to completion, and remove stored knowledge by intent.
api: openapi/osmosis-agent-improvement-openapi.json
operations: [store_knowledge_store_knowledge_post, knowledge_status_knowledge_status__job_id__get, delete_by_intent_delete_by_intent_post]
---

# Store and manage agent knowledge

Feed completed agent interactions back into Osmosis so future `enhance_task` calls
improve, then manage that stored knowledge.

## Auth
All requests require an `x-api-key: <YOUR_API_KEY>` header.

## Store knowledge
1. Call `POST /store_knowledge` (`store_knowledge_store_knowledge_post`) with an
   `AgentKnowledgeUpload` body:
   - `tenant_id` (required), `query` (required — the intent), `turns` (required —
     array of `AgentTurn` `{ turn, inputs, decision, memory?, result? }`).
   - optional: `success`, `agent_type`, `timestamp`, `metadata`.
2. The call returns `202 Accepted` with a `job_id` (async ingestion).

## Poll ingestion
3. Call `GET /knowledge_status/{job_id}`
   (`knowledge_status_knowledge_status__job_id__get`) until the job reports complete.

## Delete by intent
4. To remove stored knowledge for an intent, call `POST /delete_by_intent`
   (`delete_by_intent_delete_by_intent_post`).

## Errors
- `422` Validation Error → FastAPI `{ detail: [...] }`; verify `tenant_id`, `query`,
  and non-empty `turns`. See `errors/osmosis-problem-types.yml`.

## Conventions
Async 202→poll pattern and per-`tenant_id` scoping are documented in
`conventions/osmosis-conventions.yml`.
