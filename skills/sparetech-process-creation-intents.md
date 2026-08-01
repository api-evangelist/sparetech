---
name: Review and resolve Sparetech creation intents
description: List pending material creation intents from Sparetech and confirm or reject each one.
api: openapi/sparetech-sync-v1-openapi.json
operations: [authenticate, getCreationIntents, getCreationIntent, confirmCreationIntent, rejectCreationIntent]
---

# Review and resolve creation intents

Creation intents are proposed new material master records awaiting approval before
they sync into your ERP. Use this skill to work the queue.

## Auth
Get a JWT with `authenticate` (or `oAuthAuthenticate`) and send it as a bearer token.

## Steps
1. `getCreationIntents` (GET /creation-intents) — page the queue with `page` and
   `limit`; filter with `status`, `created.gt`, `created.lt`, `changes.only`.
2. For each item, `getCreationIntent` (GET /creation-intents/{id}) to inspect it.
3. Resolve it:
   - `confirmCreationIntent` (POST /creation-intents/{id}/confirm) to approve, which
     creates the material master record.
   - `rejectCreationIntent` (POST /creation-intents/{id}/reject) with a reject comment
     to decline.
4. A `404` means the intent id couldn't be found (already resolved or wrong id).

## Notes
- The same confirm/reject pattern applies to change intents
  (`applyChangeIntent`/`rejectChangeIntent`) and extension intents
  (`confirmExtensionIntent`/`rejectExtensionIntent`).
