---
name: Extend a Sparetech material to another plant
description: Authenticate and extend an existing material master record to an additional plant.
api: openapi/sparetech-sync-v1-openapi.json
operations: [authenticate, updateMaterial, extendMaterial]
---

# Extend a material to another plant

Use this skill to update an existing material or extend it to a new plant in the
SPARETECH material master via the Sync API v1.

## Auth
Obtain a JWT with `authenticate` / `oAuthAuthenticate` and send it as a bearer token.

## Steps
1. To change existing fields, call `updateMaterial` (PUT /material/{reference}).
2. To add a plant, call `extendMaterial` (POST /material/{reference}/extend) with the
   plant-level fields only.
3. Handle the documented errors (`errors/sparetech-problem-types.yml`):
   - `404` — material reference and/or plant couldn't be found.
   - `409 Invalid material reference` (update) or `409 Plant is already included`
     (extend) — the plant is already on the material; nothing to do.
   - `422 Material fields should not be part of an extension` — strip material-level
     fields from an extend request; send only plant-level data.

## Notes
- Test against `https://sync.sandbox.sparetech.io/v1` before production.
