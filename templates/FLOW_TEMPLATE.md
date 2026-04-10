# Flow Template

Stable structure exemplar for feature-owned Flow docs under `doc/flow/**`.

This template must mirror the current canonical feature-owned Flow format represented by `doc/flow/PREFERENCES.md`.
Use placeholders, but keep the same section order and document shape.

Authoring priorities:
- Keep one strict canonical section order. Engineers should know where to find endpoint matrix, examples, errors, migration impact, and implementation ownership without hunting.
- Make the endpoint contract matrix the primary index. Keep it compact and complete: endpoint ID, method/path, auth, request params, success, errors, and notes.
- Always include request/response examples for every endpoint family. Keep them compact: one good `curl`, one representative success example, and one canonical error example per endpoint family.
- When an example illustrates behavior introduced or materially changed by a specific task, include the task in the heading as ``(ENDPOINT_ID, Task: `TASK_ID`)``.
- Keep migration impact mandatory. `Before / After / Integrator action / Breaking` is more useful than long prose when behavior changes across requirement versions.

````text
# frozen_string_literal: true
---
title: <Feature> Flow
description: Authenticated <feature> APIs for <capability summary>.
date: YYYY-MM-DD
---

**Requirement source:** `doc/requirements/...`
**Canonical flow doc:** `doc/flow/...`
**Latest implemented requirement version:** `Version ...`
**Last updated by task:** `TASK_ID`
**Related changelog fragments:** `changelogs/unreleased/TASK_ID.md` (optional)
**Supersedes:** `doc/flow/OLD_TASK_OWNED_FILENAME.md` (optional; use when replacing a legacy Flow doc)

**Base URL:** `http://localhost:3000/api/v1`
**Auth header:** `Authorization: Bearer <TOKEN>`
**JSON header:** `Content-Type: application/json`

> Short auth/behavior note for integrators.

## Table of Contents
- [Recent Implemented History](#recent-implemented-history)
- [Task Traceability](#task-traceability)
- [General description](#general-description)
- [Validation use cases](#validation-use-cases)
- [Pundit Policy](#pundit-policy)
- [Primary Phone](#primary-phone)
- [Primary Email](#primary-email)
- [Additional Location Verification](#additional-location-verification)
- [Implementation Notes](#implementation-notes)
- [Common Headers](#common-headers)
- [Status Codes](#status-codes)
- [Flow (minimum)](#flow-minimum)
- [Responsible for Implementation Files](#responsible-for-implementation-files)
- [Drift Delta](#drift-delta)

## Recent Implemented History
- `Version ...` implemented by `TASK_ID`: short requirement-aligned summary.
- `Version ...` implemented by `TASK_ID`: short requirement-aligned summary.
- `Version ...` implemented by `legacy task ID not recorded`: short requirement-aligned summary.

## Task Traceability
| Task ID | Requirement version | Date | Summary | Endpoints |
| --- | --- | --- | --- | --- |
| `TASK_ID` | `Version ...` | `YYYY-MM-DD` | short summary | `GET /api/v1/...` |

## General description
- Short bullet describing persistence and payload ownership.
- Short bullet describing the latest requirement version behavior.
- Short bullet describing cumulative compatibility when applicable.

## Validation use cases
- New authenticated user receives a valid default payload shape.
- Invalid request examples and resulting canonical errors.
- Authorization mismatch behavior.
- Edge-case/default behavior.

## Pundit Policy
- Policy class: `<PolicyClass>`
- Allowed actors:
  - owner
  - admin
- Restricted actions:
  - `show?`
  - `create?`
  - `update?`
  - `destroy?`

- Unauthorized callers receive:

```json
{
  "success": false,
  "message": "Forbidden",
  "error": "FORBIDDEN"
}
```

## Primary Phone/Primary Email
### Primary Phone
> This section documents endpoint-level request contracts and concrete call examples.

### Endpoint Contract Matrix
> This matrix is the primary integration index. Keep every row compact and complete.

| Endpoint ID | Method | Path | Auth | Request Params | Success | Error | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FEAT-001 | GET | `/api/v1/...` | Required bearer | Path/query/body summary | `200` | `401`, `403`, `404` | Short note |

### Request Contracts (curl)
> Keep examples compact. Include one good `curl` per endpoint family rather than oversized request dumps.

### 1) Example request title (`FEAT-001`, Task: `TASK_ID`)
```bash
curl -X GET "http://localhost:3000/api/v1/..." \
  -H "Authorization: Bearer <TOKEN>" \
  -H "Content-Type: application/json"
```

### Primary Email
> This section captures response contracts, blueprint mapping, and integration-safe examples.

### JSON/Blueprint Contract Map
| Endpoint ID | `data` keys | Blueprint | Notes |
| --- | --- | --- | --- |
| FEAT-001 | `field_a`, `field_b` | `ExampleBlueprint` | Short note |

### Canonical Success Envelope
```json
{
  "success": true,
  "message": "string",
  "data": {}
}
```

### Canonical Error Envelope
```json
{
  "success": false,
  "message": "string",
  "error": "BAD_REQUEST"
}
```

### 1) Example success response (`FEAT-001`, Task: `TASK_ID`)
```json
{
  "success": true,
  "message": "string",
  "data": {}
}
```

### 2) Canonical bad-request example (`FEAT-001`, Task: `TASK_ID`)
```json
{
  "success": false,
  "message": "Invalid request",
  "error": "BAD_REQUEST",
  "details": {}
}
```

## Additional Location Verification
> This section captures retry guidance, compatibility notes, and client migration impact.

### Error Taxonomy & Client Actions
| Endpoint family | Status | `error` | When | Retry | Client action |
| --- | --- | --- | --- | --- | --- |
| `feature` | `400` | `BAD_REQUEST` | Invalid payload | No | Correct request before retry |

### Migration Impact
> This section is mandatory when behavior changes across requirement versions.

| Change type | Before | After | Integrator action | Breaking |
| --- | --- | --- | --- | --- |
| Response contract | old shape | new shape | update client parser | Yes |

## Implementation Notes
- Route namespace / route shape
- Controller
- Services / queries
- Blueprints
- Config / environment hooks
- Internal fallback behavior if applicable

## Common Headers
| Header | Value | Notes |
| --- | --- | --- |
| `Authorization` | `Bearer <TOKEN>` | Required or optional note |
| `Content-Type` | `application/json` | Supported methods note |

## Status Codes
| Endpoint | Success | Error |
| --- | --- | --- |
| `GET /api/v1/...` | `200` | `401`, `403`, `404` |

## Flow (minimum)

### 1. Example flow step
```bash
curl -X GET "http://localhost:3000/api/v1/..." \
  -H "Authorization: Bearer <TOKEN>" \
  -H "Content-Type: application/json" | jq
```

## Responsible for Implementation Files

### Requirements (read-only inputs)
- `doc/requirements/...`

### Routes
- `config/routes.rb`

### Controllers
- `app/controllers/...`

### Policies
- `app/policies/...`

### Models
- `app/models/...`

### Services / Queries
- `app/services/...`
- `app/queries/...`

### Blueprints
- `app/blueprints/...`

### Specs
- `spec/requests/...`
- `spec/integration/...`
- `spec/blueprints/...`
- `spec/policies/...`
- `spec/models/...`

### Seeds / Config
- `config/...`
- `db/seeds/...`
- `.env.example`

## Drift Delta

Added:
- New contract/doc coverage added in this update.

Changed:
- Updated prior behavior summary or examples.

Removed:
- None.
````
