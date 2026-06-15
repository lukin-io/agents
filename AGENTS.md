<!--
Author: Maksim Lukin <max@lukin.io>
Copyright (c) 2026 Maksim Lukin
SPDX-License-Identifier: MIT
-->

# AGENTS.md - Unified Rails API Contract

Contract metadata:
- updated_at: `2026-04-02`
- why_changed: "Flow/PRD docs now use stable template authorities under doc/templates/**, while feature docs stay feature-owned reference implementations and duplicate-doc merge rules are explicit."

Why/Impact:
- Flow/PRD docs are feature-owned derived docs and should be located by requirement path, not by historical task filename.
- Stable template docs under `doc/templates/**` are now the structural authority instead of live feature docs that may later be renamed or merged.
- `doc/flow/PREFERENCES.md` and `doc/prd/PREFERENCES.md` are current reference implementations used to keep templates practical and up to date, but they are not normative authorities.
- Duplicate Flow/PRD coverage now has an explicit primary-owner merge protocol instead of ad hoc consolidation decisions.

This document consolidates the responsibilities of `AGENTS.md`, `PROMPT.md`, and `GUIDE.md` into one structured contract.

All sections are explicitly labeled as either:
- `[NORMATIVE]` = mandatory rules/process
- `[ILLUSTRATIVE]` = examples/patterns to adapt

If any instruction conflicts with this file, stop and ask for clarification.

---

## 0) [NORMATIVE] Purpose and Scope

This document defines:
- execution workflow and stop-gates
- implementation standards
- verification and compliance checks
- documentation update contracts
- response/API representational invariants
- reusable templates for planning and final reporting

It applies to Rails API work in this repository.

---

## 1) [NORMATIVE] Authority and Precedence

### 1.1 Priority order
Document order reference (informational):
1. `AGENTS.md` (single source for process + standards)
2. `doc/requirements/**` (canonical feature API contract; read-only)
3. `doc/flow/**` and `doc/prd/**` (derived docs, updated only after verification)

### 1.2 Precedence by concern
- This section is the authoritative tie-breaker for precedence decisions.
- Process/policy workflow: this document governs.
- API behavior/response contract for each task: `doc/requirements/**` governs.
- Examples and snippets in this document: illustrative unless a section is marked `[NORMATIVE]`.

### 1.3 Conflict protocol
- If behavior in implementation conflicts with `doc/requirements/**`, treat it as discrepancy:
  - `[IMPL]` for code issue to fix
  - `[DOC]` for requirement mismatch against model/DB reality (report only; do not edit requirements docs)

### 1.4 Version authority
- Numbered feature/API versions such as `Version 1.0`, `1.1`, `v2`, or similar are owned exclusively by `doc/requirements/**`.
- `doc/flow/**`, `doc/prd/**`, changelogs, plans, implementation comments, and final reports must not invent their own numbered feature/API versions.
- When discussing behavior deltas, cite the requirement doc's version heading verbatim when one exists.
- If a requirement doc has no explicit version heading, use dates or change summaries only; do not create a new numeric version label.

---

## 2) [NORMATIVE] Repo Context

- Framework: Rails API
- JSON serialization: Blueprinter
- Authorization: Pundit (deny-by-default)
- Pagination: Kaminari
- Search/filtering: Ransack
- Requirements source: `doc/requirements/**` (read-only)

---

## 3) [NORMATIVE] Edit Scope

### 3.1 Allowed by default
- `app/**`
- `config/**`
- `db/**`
- `lib/**`
- `spec/**`
- `.env.example`
- `changelogs/unreleased/**`
- `doc/flow/**` (after verification)
- `doc/prd/**` (after verification)

### 3.2 Forbidden unless explicitly requested
- `doc/requirements/**` (never edit)
- `doc/templates/**` unless the task is explicitly changing repository-wide documentation structure
- secrets/credentials
- CI workflows
- `.bundle/config`

Requirement handoff exception:
- If a new requirement version has already been intentionally added to `doc/requirements/**` and implementation is happening against that updated requirement text, treat the requirement doc as an upstream handoff input, not as an implementation-owned edit surface.
- In that case, do not modify the requirement doc during the implementation task.
- `bin/contract_audit --all` should auto-allow the requirement doc when the diff adds a new `Version ...` heading and does not remove an existing version heading.
- Use an explicit allowlist only for rare non-standard handoff cases where the automatic additive-version detection does not apply.

---

## 4) [NORMATIVE] Invocation Template

Use this entry format:

```text
Task: TASK_ID - FEATURE_LABEL

Refs:
- Requirements: doc/requirements/...md
- Flow: doc/flow/...md
- PRD: doc/prd/...md

Execute per AGENTS.md
```

Invocation notes:
- `Flow` must point to the feature-owned Flow doc whose relative path mirrors the requirement doc under `doc/flow/**`.
- Example: `doc/requirements/PREFERENCES.md` -> `doc/flow/PREFERENCES.md`
- Example: `doc/requirements/ai/RECRUITER_CHAT_SERVICE.md` -> `doc/flow/ai/RECRUITER_CHAT_SERVICE.md`
- `PRD` must point to the feature-owned PRD doc whose relative path mirrors the requirement doc under `doc/prd/**`.
- Example: `doc/requirements/PREFERENCES.md` -> `doc/prd/PREFERENCES.md`
- Example: `doc/requirements/ai/RECRUITER_CHAT_SERVICE.md` -> `doc/prd/ai/RECRUITER_CHAT_SERVICE.md`

---

## 5) [NORMATIVE] Workflow Engine

### 5.1 Hard planning gate
When task says planning-first or "Execute per AGENTS.md":
- Phase 0-2: **NO CODE CHANGES**
- Output only:
  - file paths
  - excerpts <=20 lines
  - class/method signatures (no bodies)
  - short `OLD:` -> `NEW:` diff previews
- Stop after Phase 2 and wait for explicit confirmation.

### 5.2 Phase 0 - Contract extraction (NO CODE)
Do:
1. Read this document (relevant normative sections).
2. Read `doc/requirements/**` for target feature (all versions).
3. Extract exact backend contract (snake_case keys).

Output:
- Endpoints: method + path + auth + success status
- Params: path/query/body (required/optional, types, defaults, enums)
- Response shape: data-only fields, required/optional, nesting, arrays, min/max, timestamps
- Validations and error codes (400/401/403/404/422)
- Version deltas (cumulative behavior across requirement doc versions only)
- Contract Traceability Matrix (required): requirement clause -> endpoint/field -> implementation file -> spec file -> status

### 5.3 Phase 1 - Repo scan (NO CODE)
Search for existing surfaces:
- endpoint path fragments
- controller/model/blueprint/policy names
- specs + rswag
- migrations/seeds/changelog
- `TASK_ID` mentions

Output:
- existing implementation surface (file list)
- current behavior summary
- targeted excerpts <=20 lines where needed

### 5.4 Phase 2 - Plan (NO CODE; STOP AFTER THIS)
Step 1: Required components
- models/associations/validations/enums
- DB changes (constraints/indexes/FKs)
- routes/controllers
- blueprints
- policies
- service/query objects only if justified
- specs: model, blueprint, request, policy, rswag, factories
- seeds (if schema/reference data changes)
- docs and changelog updates (after verification only)

Step 2: File-by-file plan
- path
- action: NEW / MODIFY / DELETE
- responsibility
- `OLD:` -> `NEW:` preview <=20 lines
- notes: auth, preload/N+1, safe defaults, requirement mapping

Step 3: Test plan
- requirement clause -> spec mapping
- endpoint status coverage
- edge/null/boundary cases
- authorization matrix

Step 4: Risks/discrepancies (max 5)
- `[IMPL]` expected vs actual implementation issue
- `[DOC]` requirement text vs model/DB reality mismatch

Stop gate output:
```text
CONFIRM_TO_IMPLEMENT? (yes/no)
```

### 5.5 After confirmation - implementation allowed
Implement with minimal diffs and Rails-way/KISS.

Mandatory sequence:
1. implement
2. contract alignment check
3. verification checklist
4. contract compliance audit
5. docs/changelog update

---

## 6) [NORMATIVE] Non-Negotiable Engineering Rules

### 6.1 Contract-first
- Read requirement doc versions before coding.
- API shape must match requirement docs.
- Upgrade existing implementation to latest non-breaking cumulative requirements.

### 6.2 Rails-way + KISS
- Prefer Rails primitives (AR, controllers, concerns).
- Add service/query objects only for multi-step orchestration or external side effects.

### 6.2.1 Service/query object documentation
- When creating or materially modifying files under `app/services/**` (or equivalent query/orchestration objects), add Ruby doc comments for the class/module and every public entry point.
- Prefer concise Ruby/RDoc-style comments with:
  - a lead sentence describing purpose
  - `Usage:` and/or `@example` only when it clarifies invocation or return shape
  - `@param` / `@return` when inputs, outputs, side effects, or fallback behavior are not obvious
- Document private methods when they encode non-obvious business rules, fallback resolution, normalization, caching, or external IO.
- Comments must explain intent, side effects, and representative returned data without restating trivial Ruby syntax.

### 6.3 JSON rendering contract
- Blueprinter is the only source of `data` payload.
- Controllers handle envelope keys only.
- Do not hand-craft data JSON in controllers.

### 6.4 Status codes
- Index: `200 :ok`
- Create: `201 :created`
- Update: `200 :ok`
- Destroy: `200 :ok`
- Validation: `422 :unprocessable_content`
- Bad request: `400 :bad_request`
- Unauthorized: `401 :unauthorized`
- Forbidden: `403 :forbidden`
- Not found: `404 :not_found`

### 6.5 Search/pagination/authz
- Search/filtering via Ransack
- Pagination via Kaminari
- Authorization via Pundit (deny-by-default)

### 6.6 Performance
- No known N+1 on rendered associations (`includes`/`preload`).

### 6.7 Time and formats
- UTC internally
- ISO8601 timestamps in JSON

### 6.8 Database rules
- Prefer DB constraints (NOT NULL, FK, unique index)
- Database-agnostic queries only (ActiveRecord/Arel, no DB-specific SQL)

---

## 7) [NORMATIVE] Canonical Envelope and Error Contract

### 7.1 Success envelope
Always:
```json
{ "success": true, "message": "string", "data": "...", "meta": "optional" }
```

Use envelope helpers:
- `render_collection_envelope(collection:, blueprint:, meta: ...)`
- `render_success_envelope(message:, resource: nil, blueprint: nil, status: :ok, meta: nil)`

Collections must include pagination `meta` keys:
- `page`
- `per_page`
- `total_pages`
- `total_count`

### 7.2 Error envelope
All errors include:
```json
{ "success": false, "message": "string" }
```

Additional canonical fields by case:
- Validation 422 via `render_validation_errors(record)`:
  - `error`
  - `errors`
- Bad request 400:
  - `error`
  - optional `details`
- Unauthorized 401:
  - `error`
- Forbidden 403:
  - `error`
- Not found 404:
  - `error`

Do not invent custom per-controller error shapes.

---

## 8) [NORMATIVE] Representational Invariants (No Drift)

- Routes/URLs: snake_case only; no kebab-case; no trailing slash unless requirement doc explicitly requires.
- JSON/params: snake_case only.
- Envelope top-level keys: exactly `success`, `message`, `data`, `meta`.
- Errors: canonical shape only.
- Pagination meta keys: exactly `page`, `per_page`, `total_pages`, `total_count`.
- Timestamps: ISO8601 strings only.
- Enums: strings only.
- Optionals: omit when empty unless requirement doc requires presence.
- Collections: always arrays (`[]`), never `null`.

---

## 9) [NORMATIVE] Safe Defaults (Required Fields Never Null)

If a requirement marks a field as required, do not return `nil`/`null`.

Defaults:
- number: `0` (or minimum valid value)
- string: `""`
- boolean: `false`
- array: `[]` (respect min length via normalization)
- object: minimal valid object with required nested fields
- timestamp: `Time.current.iso8601`

Optional fields:
- omit when empty unless requirement doc says otherwise

---

## 10) [NORMATIVE] Contract Alignment Check (Pre-Flight)

Before verification commands:
1. Re-read requirement docs (target version + prior versions).
2. Confirm:
   - endpoint exists (method/path)
   - required fields exist in blueprint
   - auth rules match
   - no TODO/placeholder gaps
3. Report discrepancies as `[IMPL]` / `[DOC]`.
4. Fix all `[IMPL]` issues before moving forward.

Proceed to verification only after alignment is confirmed.

---

## 11) [NORMATIVE] Verification Checklist (Must Be Green)

Required command order (default local and CI runs):
1. `bin/verify`
2. `bin/contract_audit --all`

Before push, run the same order locally:
1. `bin/verify`
2. `bin/contract_audit --all`

Requirement handoff exception:
- If the task is implementing against an already-edited requirement doc that contains the new version being implemented, `bin/contract_audit --all` should pass automatically when that requirement diff adds a new `Version ...` heading and does not remove an existing version heading.
- If the requirement handoff is non-standard and cannot be expressed as an additive new-version diff, use:
  - `bin/contract_audit --all --allow-requirement doc/requirements/...`
- This exception is only for the explicit requirement source paths being used as upstream handoff inputs.
- Do not use the allowlist to hide unrelated requirement edits or requirement changes made during the implementation task itself.

[DO NOT RUN THIS] Use `bin/verify --full` only when:
- the user explicitly requests full verification
- changes touch schema/seed surfaces: `db/schema.rb`, `db/structure.sql`, `db/migrate/**`, `db/seeds.rb`, `db/seeds/**`
- changes touch verification/process tooling: `bin/verify`, `bin/contract_audit`, `AGENTS.md`

`bin/verify` defaults to the fast profile and must include these underlying checks:
1. `bundle exec rubocop`
2. `bundle exec rspec` (excluding specs tagged `:full_only`)
3. `bundle exec rails rswag:specs:swaggerize` (only when API/spec/doc surfaces changed)

`bin/verify --full` is the optional extended profile and must include these underlying checks:
1. `bundle exec rubocop`
2. `bundle exec rspec`
3. `bundle exec brakeman -q -w2`
4. `bundle exec bundle audit check --update`
5. `bundle exec rails rswag:specs:swaggerize`
6. `bundle exec rails db:seed:replant`

Fallback (if wrappers are unavailable):
1. if running the fast profile, run the three `bin/verify` underlying commands in order
2. if running the full profile, run the six `bin/verify --full` underlying commands in order
3. run `bin/contract_audit --all`

Output requirement:
- include a `CHECKS` section with each command and final exit code.
- `CHECKS` must include the exact verify command used (`bin/verify` or `bin/verify --full`) and the exact contract audit command used, including any explicit `--allow-requirement` paths when the non-standard requirement handoff exception applies.

---

## 12) [NORMATIVE] Contract Compliance Audit (No Gaps)

After verification passes, compare implementation vs requirement docs.

### 12.1 Endpoints
- path/method exists
- params match names/types/requiredness
- auth constraints match

### 12.2 Response shape
- every documented field exists in blueprint
- no extra undocumented fields
- required fields never null
- optional handling matches docs
- nested objects/arrays match documented shapes

### 12.3 Validation and errors
- documented validations enforced
- statuses match docs
- error payload shape canonical

### 12.4 Version compliance
- cumulative support across requirement doc versions unless explicitly removed/breaking in requirement docs
- never invent an implementation-only version number to describe compliance state

### 12.5 Discrepancy format (required if any)
```text
[IMPL] [FIELD/ENDPOINT] - expected per doc vs actual in code
[DOC]  [FIELD/ENDPOINT] - doc says X vs DB/model reality Y
```

### 12.6 Contract stability guard (frontend safety)
- If `doc/requirements/**` for the task (including any documented TypeScript interface contracts) does not introduce a contract change, request/response/envelope shapes must remain unchanged.
- Any intentional contract change must be explicitly traceable to a requirement clause and reflected in specs and docs.

---

## 13) [NORMATIVE] Migrations and Seeds

Default migration policy (project not released):
- edit existing migrations when practical
- create new migration only when editing old one is unsafe/confusing/dependent
- one migration file per structural responsibility

Seeds:
- if schema introduces tables/reference data, update `db/seeds.rb` with representative records (3-5)

---

## 14) [NORMATIVE] Tests and Required Coverage

Every feature/bugfix/refactor requires tests.

Must include where applicable:
- model spec
- blueprint spec
- request spec
- policy spec
- rswag spec
- factories with edge traits

Coverage categories:
- success
- failure
- edge/null/boundary
- authorization

---

## 15) [NORMATIVE] Documentation Contract (Flow/PRD + Changelog)

Docs are updated only after verification + compliance audit pass.

Section 15 has two documentation subsections:
- `Contract requirements` = mandatory rules used for compliance checks.
- `Authoring aid` = copy/paste scaffolds for faster writing; not a second source of truth.

### 15.0 Documentation exemplars (mandatory)
- Flow docs must mirror `doc/templates/FLOW_TEMPLATE.md` layout and section ordering.
- PRD docs must mirror `doc/templates/PRD_TEMPLATE.md` section model.
- `doc/templates/FLOW_TEMPLATE.md` is the structural authority for Flow docs.
- `doc/templates/PRD_TEMPLATE.md` is the structural authority for PRD docs.
- `doc/flow/PREFERENCES.md` and `doc/prd/PREFERENCES.md` are the current reference implementations used to keep templates practical and up to date, but they are not normative authorities.
- Other live feature docs and legacy example docs may be cited as examples, but they must not be treated as structural references.
- Flow details should follow the template integration style: endpoint contract matrix, JSON/blueprint mapping, error taxonomy, migration impact.
- Do not invent alternate Flow/PRD structures for new tasks.
- Flow/PRD docs are feature-owned derived docs; changelogs remain task-owned artifacts.
- Do not edit `doc/templates/**` as part of a normal feature task unless the task is explicitly changing repository-wide documentation structure.

### 15.0.1 Versioning source of truth (mandatory)
- Numbered versions in Flow/PRD docs may only reference version headings that already exist in `doc/requirements/**`.
- Do not add independent numbered labels such as `v2`, `Implementation stage: 1.1`, or similar to derived docs.
- Flow docs must use a `Recent Implemented History` section instead of a single-line implementation-stage banner when they need to summarize version coverage.
- `Recent Implemented History` rules:
  - include the 3-5 most recent implemented requirement versions for that feature (or fewer if fewer than 3 exist)
  - newest-first ordering
  - one short sentence per version describing the implemented delta and implementing task ID(s)
  - use requirement version labels verbatim, for example `Version 1.5`
  - when a legacy implementation task ID is unknown, state `legacy task ID not recorded`
- Flow docs must include a `Task Traceability` section.
- `Task Traceability` rules:
  - newest-first ordering
  - required columns: `Task ID`, `Requirement version`, `Date`, `Summary`, `Endpoints`
  - add rows only for contract, auth, response-shape, validation, or other integrator-relevant changes
  - requirement versions may repeat across multiple task rows when one requirement version is implemented across more than one task
- The required `Version History` section in PRDs must be requirement-aligned:
  - if the requirement doc is versioned, cite those requirement versions
  - if the requirement doc is unversioned, use dated change history entries with no numeric version labels
- PRD `Version History` entries must include:
  - requirement version or dated non-numeric entry when the requirement doc is unversioned
  - implementing task ID
  - short PRD-style summary written in product/scope language
- PRDs may include a short `Recent Implemented History` near the top, but it is optional and `Version History` remains the canonical history section

### 15.1 Flow doc required structure
`doc/flow/**` must follow `doc/templates/FLOW_TEMPLATE.md` and this sequence.

Naming and identity rules:
- each Flow doc must be feature-owned and mirror its source requirement doc path under `doc/flow/**`
- example: `doc/requirements/PREFERENCES.md` -> `doc/flow/PREFERENCES.md`
- example: `doc/requirements/ai/RECRUITER_CHAT_SERVICE.md` -> `doc/flow/ai/RECRUITER_CHAT_SERVICE.md`
- legacy task-owned Flow filenames should be renamed to the mirrored feature-owned path when that feature is next touched

Required section sequence:
1. metadata preamble
2. base URL + auth/json headers block
3. short auth/behavior note block when useful for integrators
4. table of contents (linked sections)
5. Recent Implemented History
6. Task Traceability
7. general description
8. validation use cases
9. Pundit policy
10. `Primary Phone/Primary Email` bridge heading
11. Primary Phone (must include Endpoint Contract Matrix + Request Contracts)
12. Primary Email (must include JSON/Blueprint Contract Map + canonical envelopes + response examples)
13. Additional Location Verification (must include Error Taxonomy & Client Actions + Migration Impact)
14. implementation notes
15. common headers
16. status codes
17. flow (minimum)
18. responsible for implementation files (must include Requirements read-only inputs + implementation surfaces)
19. Drift Delta

If new implementation file types are introduced (service/job/blueprint), update "responsible for implementation files".

Flow docs must include integration-ready artifacts:
- top metadata lines for:
  - `Requirement source`
  - `Canonical flow doc`
  - `Latest implemented requirement version`
  - `Last updated by task`
  - optional `Related changelog fragments`
- `Recent Implemented History` with the 3 most recent implemented requirement versions (or fewer when the requirement doc has fewer implemented versions)
- `Task Traceability` table with the required columns from section `15.0.1`
- endpoint contract matrix with stable endpoint IDs
- at least one `curl` example per endpoint
- request contract notes (auth optional/required, params)
- JSON/blueprint contract mapping for `data` payload fields
- concrete success response samples per endpoint
- at least one canonical error response sample for each endpoint family
- error taxonomy with client retry/action guidance
- migration impact table (`before` vs `after`, integrator action, breaking yes/no)
- request header expectations (`Authorization`, `Content-Type`) and auth optionality/requirements
- no TypeScript blocks by default; keep TypeScript interfaces in `doc/requirements/**` unless explicitly requested

### 15.1.1 Flow merge/collapse rules
When more than one Flow doc touches the same requirement area, classify each doc before deciding to merge.

Classification:
- `Primary owner` = the Flow doc whose title, endpoint set, and implementation surfaces are centered on that requirement area.
- `Secondary dependency` = a Flow doc that references the same requirement area only because it depends on the same payload, permission, or supporting service.

Merge is required when:
- more than one Flow doc acts as a `Primary owner` for the same requirement area
- contract/auth/response-shape/validation history for one requirement area would otherwise be split across multiple Flow docs

Do not merge when:
- the overlap is only `Primary owner` + `Secondary dependency`
- a specialized doc serves a clearly distinct audience or integration surface and does not duplicate ownership of the same requirement area

Canonical target selection:
1. prefer the existing feature-owned mirrored requirement path under `doc/flow/**`
2. if no canonical feature-owned doc exists yet, create it from the requirement path and merge into it
3. if multiple requirement docs legitimately feed one integrator surface, choose the primary requirement owner and record the dependency in the merged doc

Handling old files after merge:
- preferred: delete the old file after references are updated and history is migrated
- allowed temporarily: leave a short stub/redirect doc that states `Superseded by: doc/flow/...`
- a stub doc must not retain an independent contract narrative
- if a stub is kept, the canonical Flow doc should include optional transition metadata such as `Previous flow filenames:` or `Supersedes:`

### 15.2 PRD doc required structure
`doc/prd/**` must follow `doc/templates/PRD_TEMPLATE.md`.

Naming and identity rules:
- each PRD doc must be feature-owned and mirror its source requirement doc path under `doc/prd/**`
- example: `doc/requirements/PREFERENCES.md` -> `doc/prd/PREFERENCES.md`
- example: `doc/requirements/ai/RECRUITER_CHAT_SERVICE.md` -> `doc/prd/ai/RECRUITER_CHAT_SERVICE.md`
- legacy task-owned PRD filenames should be renamed to the mirrored feature-owned path when that feature is next touched

Required section sequence:
1. metadata preamble / frontmatter
2. top metadata lines
3. Recent Implemented History
4. Problem Statement
5. Goals & Non-Goals
6. Scope
7. User Stories
8. Flows & UX Notes
9. Technical Considerations
10. Security & Compliance
11. Acceptance Criteria
12. Metrics & KPIs
13. Rollout / Launch Plan
14. Open Questions
15. Dependencies / Prerequisites
16. Version History
17. Drift Delta

PRD docs must include top metadata lines for:
- `Requirement source`
- `Canonical PRD doc`
- `Related Flow doc`
- `Latest implemented requirement version`
- `Last updated by task`
- optional `Related changelog fragments`

PRD docs are outcome docs. Do not duplicate TypeScript interface blocks from requirement docs unless explicitly requested.
PRD `Version History` entries must not introduce independent numeric versions; they must either reference requirement versions verbatim or use dated non-numeric history entries.
PRD history should be written in product/scope/outcome language rather than endpoint/contract language.
PRD docs must include a `Drift Delta` block.

### 15.2.1 PRD merge/collapse rules
When more than one PRD exists for the same feature area, use the same `Primary owner` vs `Secondary dependency` classification.

Merge is required when:
- product scope, rollout, or acceptance history for one requirement area is split across multiple primary-owner PRDs
- task-owned PRDs and feature-owned PRDs for the same requirement area both exist

Do not merge when:
- one PRD is only documenting a dependent feature and does not own the same product scope
- multiple requirement docs intentionally map to different product areas, even if they share implementation surfaces

Canonical target selection:
1. prefer the existing feature-owned mirrored requirement path under `doc/prd/**`
2. if no canonical PRD exists yet, create it from the requirement path and merge into it
3. keep `Version History` entries from the merged documents inside the canonical PRD using requirement-aligned versions or dated entries only

Handling old files after merge:
- preferred: delete the old file after references are updated and history is migrated
- allowed temporarily: leave a short stub/redirect doc that states `Superseded by: doc/prd/...`
- a stub doc must not retain an independent PRD narrative
- if a stub is kept, the canonical PRD may include optional transition metadata such as `Previous PRD filenames:` or `Supersedes:`

### 15.3 Drift delta block (required on doc update)
Each Flow/PRD update must include:
- `Added:`
- `Changed:`
- `Removed:` (allowed only if requirement docs explicitly mark removal/breaking)

### 15.4 Changelog
- create `changelogs/unreleased/{TASK_ID}.md` on completion
- changelogs remain task-owned even when Flow docs are feature-owned

### 15.5 Authoring aid snippets (non-authoritative convenience)
Flow scaffold (must mirror `doc/templates/FLOW_TEMPLATE.md`):
```text
# frozen_string_literal: true
---
title: Feature Flow
description: ...
date: YYYY-MM-DD
---

**Requirement source:** `doc/requirements/...`
**Canonical flow doc:** `doc/flow/...`
**Latest implemented requirement version:** `Version ...`
**Last updated by task:** `WEB-XXX`
**Related changelog fragments:** `changelogs/unreleased/WEB-XXX.md` (optional)

**Base URL:** `http://localhost:3000/api/v1`
**Auth header:** `Authorization: Bearer <TOKEN>`
**JSON header:** `Content-Type: application/json`

## Recent Implemented History
- `Version 1.5` implemented by `WEB-XXX`: short requirement-aligned summary.
- `Version 1.4` implemented by `WEB-YYY`: short requirement-aligned summary.
- `Version 1.3` implemented by `legacy task ID not recorded`: short requirement-aligned summary.

## Task Traceability
| Task ID | Requirement version | Date | Summary | Endpoints |
| --- | --- | --- | --- | --- |
| `WEB-XXX` | `Version 1.5` | `YYYY-MM-DD` | short summary | `GET /api/v1/...` |

## Primary Phone
### Endpoint Contract Matrix
| Endpoint ID | Method | Path | Auth | Params | Success | Error |
| --- | --- | --- | --- | --- | --- | --- |

### Request Contracts (curl)
curl -X GET "..."

## Primary Email
### JSON/Blueprint Contract Map
| Endpoint ID | data[] keys | Blueprint | Notes |
| --- | --- | --- | --- |

### Canonical Success Envelope
{ "success": true, "message": "string", "data": [] }

### Canonical Error Envelope
{ "success": false, "message": "string", "error": "INTERNAL_ERROR" }

## Additional Location Verification
### Error Taxonomy & Client Actions
| Status | Error | Retry | Client action |
| --- | --- | --- | --- |

### Migration Impact
| Change type | Before | After | Integrator action | Breaking |
| --- | --- | --- | --- | --- |

## Responsible for Implementation Files
### Requirements (read-only inputs)
- doc/requirements/...
```

PRD scaffold (must mirror `doc/templates/PRD_TEMPLATE.md`):
```text
**Requirement source:** `doc/requirements/...`
**Canonical PRD doc:** `doc/prd/...`
**Related Flow doc:** `doc/flow/...`
**Latest implemented requirement version:** `Version ...`
**Last updated by task:** `WEB-XXX`
**Related changelog fragments:** `changelogs/unreleased/WEB-XXX.md` (optional)

## 1. Problem Statement
## 2. Goals & Non-Goals
## 3. Scope
## 4. User Stories
## 5. Flows & UX Notes
## 6. Technical Considerations
## 7. Security & Compliance
## 8. Acceptance Criteria
## 9. Metrics & KPIs
## 10. Rollout / Launch Plan
## 11. Open Questions
## 12. Dependencies / Prerequisites
## 13. Version History
Use requirement-owned version labels only, or dated non-numeric entries if the requirement doc is unversioned.
- `Version 1.5` implemented by `WEB-XXX`: short PRD-style summary.
- `Version 1.4` implemented by `WEB-YYY`: short PRD-style summary.
```

---

## 16) [NORMATIVE] Final Output Contract

Final response must include:
1. `WHAT & HOW` (1-2 sentences, shareable with engineers)
2. `RATIONALE` (3-5 bullets)
3. `CHECKS` (every command + exit code)
4. `RULE COMPLIANCE AUDIT` (brief evidence)
5. `Discrepancies Report` (`[IMPL]`/`[DOC]` if any)
6. `Contract Traceability Matrix`

`CHECKS` must explicitly include:
- the exact verify command used (`bin/verify` or `bin/verify --full`)
- `bin/contract_audit --all`

---

## 17) [NORMATIVE] Rule Compliance Audit Format

Use this compact form:

```text
RULE COMPLIANCE AUDIT
- R1 Contract-first: COMPLIANT/VIOLATED (evidence)
- R2 Blueprinter-only data payload: COMPLIANT/VIOLATED (evidence)
- R3 Canonical envelopes/errors: COMPLIANT/VIOLATED (evidence)
- R4 Representational invariants: COMPLIANT/VIOLATED (evidence)
- R5 Safe defaults for required fields: COMPLIANT/VIOLATED (evidence)
- R6 Verification checklist completed: COMPLIANT/VIOLATED (evidence)
```

---

## 18) [NORMATIVE] Contract Traceability Matrix (Required Artifact)

Use:

| Requirement clause | Endpoint/Field | Implementation file | Spec file | Status |
|---|---|---|---|---|
| REQ-x.y | `GET /api/v1/...` | `app/controllers/...` | `spec/requests/...` | PASS/GAP |

---

## 19) [ILLUSTRATIVE] Rails Implementation Patterns

### 19.1 Index pattern (preload + pagination)
```ruby
def index
  items = policy_scope(Item).includes(:owner).order(created_at: :desc)
  q = items.ransack(params[:q])
  paged = q.result.page(params[:page]).per(params[:per_page] || 20)

  render_collection_envelope(collection: paged, blueprint: ItemBlueprint)
end
```

### 19.2 Create/Update/Destroy pattern
```ruby
def create
  item = current_user.items.new(item_params)
  return render_validation_errors(item) unless item.save

  render_success_envelope(
    message: "Item created successfully",
    resource: item,
    blueprint: ItemBlueprint,
    status: :created
  )
end

def update
  return render_validation_errors(item) unless item.update(item_params)

  render_success_envelope(
    message: "Item updated successfully",
    resource: item,
    blueprint: ItemBlueprint
  )
end

def destroy
  item.destroy!
  render_success_envelope(message: "Resource deleted successfully")
end
```

### 19.3 Error helper usage
```ruby
def render_error_envelope(message:, error:, status:, details: nil); end

render_error_envelope(
  message: "Invalid operation for user role",
  error: "INVALID_USER_ROLE",
  status: :bad_request,
  details: { field: "role", allowed: ["client"] }
)
```

### 19.4 Strong params with nested attrs
```ruby
def item_params
  params.require(:item).permit(
    :title, :description, :price_cents, :status,
    files_attributes:  [:id, :doc_type, :file, :_destroy],
    images_attributes: [:id, :alt, :file, :position, :_destroy]
  )
end
```

### 19.5 Model query (DB-agnostic)
```ruby
users = User.where(User.arel_table[:name].matches("%john%"))
```

### 19.6 Service object gate
Use a service only when it:
1. orchestrates >1 model, or
2. triggers side effects/IO, or
3. materially improves clarity/testability

Otherwise keep logic in controller/model.

### 19.7 Service object documentation pattern
```ruby
module Users
  # Builds a paginated history collection for the dashboard.
  #
  # Side effects:
  # - Hydrates missing search terms from upstream and caches them locally.
  #
  # Usage:
  #   collection = Users::ExampleQuery.call(user: current_user, page: 1, per_page: 10)
  #   collection.first # => #<struct Users::ExampleQuery::Entry ...>
  class ExampleQuery
    class << self
      # @param user [User]
      # @param page [Integer]
      # @param per_page [Integer]
      # @return [Kaminari::PaginatableArray<Entry>]
      def call(user:, page:, per_page:); end
    end

    private

    # Returns normalized metadata with string keys.
    #
    # @param record [ApplicationRecord]
    # @return [Hash<String, Object>]
    def normalized_metadata(record); end
  end
end
```

---

## 20) [ILLUSTRATIVE] Blueprinter Safety Checklist

Before done:
1. list fields from requirement interface
2. ensure each exists in blueprint (no extras)
3. ensure required/optional behavior matches
4. ensure required fields cannot serialize nil
5. ensure timestamps are ISO8601
6. ensure blueprint does not emit envelope keys

Conditional field example:
```ruby
field :permissions, if: ->(_name, user, _opts) { user.client? } do |user|
  UserPermissionBlueprint.render_as_hash(user.user_permission)
end
```

---

## 21) [ILLUSTRATIVE] Testing Templates

### 21.1 Coverage matrix
| Category | Location | Focus |
|---|---|---|
| Model | `spec/models/` | validations/associations/enums/scopes |
| Blueprint | `spec/blueprints/` | shape/safe defaults/format |
| Request | `spec/requests/` | success + all relevant errors |
| Policy | `spec/policies/` | auth matrix |
| RSwag | `spec/integration/` | OpenAPI response contracts |
| Factories | `spec/factories/` | base + edge traits |

### 21.2 Typical status coverage
- GET: `200` + `401` + `404` (+ `403` when applicable)
- POST: `201` + `400` + `401` + `403` + `404` + `422`
- PATCH: `200` + `400` + `401` + `403` + `404` + `422`
- DELETE: `200` + `401` + `403` + `404`

### 21.3 Model spec template
```ruby
RSpec.describe Item, type: :model do
  describe "associations" do
    it { is_expected.to belong_to(:owner).class_name("User") }
  end

  describe "validations" do
    it { is_expected.to validate_presence_of(:title) }
  end
end
```

### 21.4 Blueprint spec template
```ruby
RSpec.describe ItemBlueprint do
  let(:item) { create(:item) }
  let(:json) { described_class.render_as_hash(item) }

  it "does not include envelope fields" do
    expect(json.keys).not_to include(:success, :message, :data, :meta)
  end
end
```

### 21.5 Request spec template
```ruby
RSpec.describe "Items API", type: :request do
  describe "GET /api/v1/items" do
    it "returns 200 with data + meta" do
      get "/api/v1/items", headers: auth_headers
      expect(response).to have_http_status(:ok)
      expect(json_response).to include("success" => true)
      expect(json_response["data"]).to be_an(Array)
      expect(json_response["meta"]).to include("page", "per_page", "total_pages", "total_count")
    end
  end
end
```

### 21.6 Policy spec template
```ruby
RSpec.describe ItemPolicy, type: :policy do
  subject(:policy) { described_class.new(user, item) }

  let(:item) { create(:item) }

  context "as owner" do
    let(:user) { item.owner }
    it { is_expected.to permit_action(:update) }
  end
end
```

### 21.7 Callback-created records pitfall
```ruby
# BAD
create(:user_permission, user: client_user)

# GOOD
permission = client_user.user_permission
permission.update!(field: true)
```

### 21.8 Common pitfalls (fast review)
- Missing `400`/`403`/`404`/`422` specs (not just happy path)
- Returning `nil` for required fields instead of safe defaults
- Extra Blueprint fields not present in requirement docs
- N+1 from rendering associations without `includes`/`preload`
- Hand-edited `swagger/v1/swagger.yaml` instead of rswag generation
- DB-specific SQL sneaking into scopes/queries

---

## 22) [NORMATIVE] Reusable Output Templates

### 22.1 Phase 0 output template
```text
CONTRACT EXTRACTION
Endpoints:
- METHOD /path (auth) -> status

Params:
- path/query/body: field (type, req/opt, default, enum)

Response (data):
- field_name (req/opt, type, defaults)

Errors:
- 400/401/403/404/422 with shape notes

Version deltas:
- requirement version(s) only, e.g. `PROFILE_SKILLS.md -> Version 1.4`:
- if requirement doc is unversioned, use dated change notes only:

Contract Traceability Matrix:
| Requirement clause | Endpoint/Field | Implementation file | Spec file | Status |
|---|---|---|---|---|
```

### 22.2 Phase 1 output template
```text
REPO SCAN
Files found:
- path

Current behavior:
- bullet

Excerpts (<=20 lines):
path:line
```

### 22.3 Phase 2 output template
```text
PLAN
Step 1 Required components:
- ...

Step 2 File-by-file:
- path | action | responsibility
  OLD:
  NEW:

Step 3 Test plan:
- requirement clause -> specs

Step 4 Risks/discrepancies:
- [IMPL] ...
- [DOC] ...

CONFIRM_TO_IMPLEMENT? (yes/no)
```

### 22.4 Final output template
```text
WHAT & HOW
- ...

RATIONALE
- ...

CHECKS
- command -> exit code

RULE COMPLIANCE AUDIT
- R1 ... (evidence)

Discrepancies Report
- [IMPL] ...
- [DOC] ...

Contract Traceability Matrix
| Requirement clause | Endpoint/Field | Implementation file | Spec file | Status |
|---|---|---|---|---|
```

---

## 23) [NORMATIVE] Never List

- Never edit `doc/requirements/**`.
- Never bypass Blueprinter for the `data` payload.
- Never emit envelope keys from Blueprints.
- Never use camelCase or kebab-case in routes/params/JSON.
- Never hand-edit `swagger/v1/swagger.yaml`.
- Never ship known N+1 in rendered endpoints.
- Never use database-specific SQL.
- Never update Flow/PRD before verification is green.

---

## 24) [NORMATIVE] Contract Maintenance Protocol

When this contract file changes:
1. update contract metadata date/why_changed header
2. do not introduce a numeric versioning scheme outside `doc/requirements/**`
3. add short "Why/Impact" note
4. verify referenced templates/commands still align
5. include change in changelog fragment if tied to task process change

---

## 25) [NORMATIVE] Completion Checklist

Before closing task, confirm:
- planning gates respected
- contract alignment check done
- verification commands green
- compliance audit complete
- discrepancies reported with proper tags
- Flow/PRD updated after verification
- changelog added
- final output includes all required sections
