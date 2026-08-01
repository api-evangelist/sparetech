---
name: Create a material master record via Sparetech Sync
description: Authenticate to the Sparetech Sync API and create a new material master record from an ERP/CMMS system.
api: openapi/sparetech-sync-v1-openapi.json
operations: [authenticate, createMaterial]
---

# Create a material master record

Use this skill to push a new spare-part material master record from your ERP/CMMS
into SPARETECH through the Sync API v1.

## Auth
1. Obtain the client credentials SPARETECH issued for your organisation and
   environment (sandbox vs production are separate credentials).
2. Exchange them for a JWT with `authenticate` (POST /auth) or `oAuthAuthenticate`
   (POST /oauth/token). If you get a `403 token quota exceeded`, reuse the current
   token until the quota window resets rather than minting new tokens.
3. Send the JWT as `Authorization: Bearer <token>` on every request.

## Steps
1. Call `createMaterial` (POST /material) with the material payload.
2. Handle errors from `errors/sparetech-problem-types.yml`:
   - `404 Material reference already exists` — the reference is already known; switch
     to `updateMaterial` instead of creating.
3. Point the base URL at `https://sync.sandbox.sparetech.io/v1` to test first, then
   `https://sync.sparetech.io/v1` for production.

## Notes
- No idempotency key is supported; guard against duplicate submits on your side.
- v2 (`upsertMaterial`) is the SAP-specific variant if you integrate S/4HANA.
