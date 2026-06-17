# Skill: Rails API Feature Implementation

Use this skill to implement a Rails API feature from a requirement contract.

## Purpose

Turn a requirement document into a small, verifiable Rails API implementation with matching specs, docs, and final evidence.

## When To Use

Use this skill for:

- new API endpoints
- endpoint behavior changes
- response-shape changes
- validation changes
- auth/policy changes
- blueprint updates
- feature-owned Flow/PRD updates after verification

## Required Inputs

- task id
- feature label
- requirement doc path
- expected Flow doc path
- expected PRD doc path

## Phase 0: Contract Extraction

No code changes.

Extract:

- method and path
- auth requirements
- params
- success status
- response shape
- required and optional fields
- validation rules
- error statuses
- version deltas
- compatibility expectations

Required output:

```text
CONTRACT TRACEABILITY MATRIX
| Requirement clause | Endpoint/Field | Implementation file | Spec file | Status |
| --- | --- | --- | --- | --- |
```

## Phase 1: Repo Scan

No code changes.

Find existing:

- routes
- controllers
- models
- blueprints
- policies
- services/queries
- specs
- migrations
- seeds
- changelog fragments
- Flow/PRD docs

Summarize current behavior and reuse surfaces when possible.

## Phase 2: Plan

No code changes.

Plan:

- models/associations/validations/enums
- database changes
- routes/controllers
- blueprints
- policies
- services/queries only when justified
- specs
- seeds if needed
- docs/changelog updates after verification

End with:

```text
CONFIRM_TO_IMPLEMENT? (yes/no)
```

## Phase 3: Implementation

After confirmation:

- implement minimal diffs
- keep Rails-way/KISS
- use Blueprinter for `data` payloads
- keep controllers responsible for envelopes
- use Pundit for authorization
- use Ransack for search/filtering when applicable
- use Kaminari for pagination when applicable
- avoid ad-hoc response shapes
- avoid undocumented API contract changes

## Phase 4: Verification

Run:

```bash
bin/verify
bin/contract_audit --all
```

Use `bin/verify --full` only when required by `AGENTS.md` or explicitly requested.

## Phase 5: Derived Docs

Only after verification passes:

- update `doc/flow/**`
- update `doc/prd/**`
- add `changelogs/unreleased/{TASK_ID}.md`

Flow/PRD docs must mirror the requirement path and must not invent independent API versions.

## Expected Final Output

Final response must include:

- WHAT & HOW
- RATIONALE
- CHECKS
- RULE COMPLIANCE AUDIT
- Discrepancies Report
- Contract Traceability Matrix

## Failure Modes

Stop and report when:

- requirement contract is missing or ambiguous
- auth behavior is unclear
- response shape conflicts with existing blueprint behavior
- implementation requires a requirement change
- verification fails
- contract audit fails

Use `[IMPL]` for implementation issues and `[DOC]` for requirement/model reality mismatches.
