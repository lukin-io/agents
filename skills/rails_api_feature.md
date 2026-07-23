# Skill: Rails API Feature Implementation

Use this skill for contract extraction, repository scan, planning, confirmation, and implementation of a Rails API feature.

This skill is an execution aid. `AGENTS.md`, `AGENTS_CONTRACT.md`, and the target requirements remain authoritative.

## Purpose

Turn a requirement contract into a minimal Rails API implementation with matching tests and a complete handoff to verification.

This skill owns Phase 0 through implementation. It does not own quality-gate execution or derived documentation updates.

## Activation

Activate this skill when:

- Phase -1 completed with `Ready for Phase 0: YES`
- the task adds or changes Rails API behavior
- the target requirement source and applicable versions are known

Typical triggers:

- new endpoint
- endpoint behavior or response-shape change
- validation or authorization change
- model, blueprint, route, or policy change
- Rails API bug fix or contract-preserving refactor

Do not activate this skill for documentation-only work or verification-only review.

Initial invocation record:

```text
SKILL INVOCATION
- Skill: skills/rails_api_feature.md
- Phase: Phase 0 through implementation
- Trigger:
- Inputs resolved: YES/NO
- Required output: plan gate + implemented change + FEATURE IMPLEMENTATION HANDOFF
- Status: ACTIVATED/BLOCKED
- Notes:
```

## Required Inputs

- task id or stable task label
- feature label
- completed Phase -1 `CONTEXT INVENTORY` and `CONTEXT SUMMARY`
- target requirement path and all applicable versions
- expected Flow and PRD paths
- existing implementation/spec surfaces
- known dependencies and discrepancies

## Normative References

Load and follow:

- `AGENTS.md` Phase -1 and Skill Invocation Contract
- `AGENTS_CONTRACT.md` sections:
  - Authority and Precedence
  - Edit Scope
  - Invocation Template
  - Workflow Engine
  - Non-Negotiable Engineering Rules
  - Canonical Envelope and Error Contract
  - Representational Invariants
  - Safe Defaults
  - Contract Alignment Check
  - Tests and Required Coverage
  - Contract Traceability Matrix
- target `doc/requirements/**` sources and all applicable versions

Use Flow/PRD docs only as derived context.

## Procedure

### Step 1 — Phase 0 Contract Extraction

No code changes.

Extract:

- endpoints, methods, paths, auth, and success statuses
- path/query/body params with types, defaults, enums, and requiredness
- response fields, nesting, arrays, optionals, and timestamps
- validations and error statuses
- cumulative requirement-version deltas
- compatibility and removal rules

Create or update the Contract Traceability Matrix:

```text
| Requirement clause | Endpoint/Field | Implementation file | Spec file | Status |
| --- | --- | --- | --- | --- |
```

### Step 2 — Phase 1 Repository Scan

No code changes.

Inspect relevant:

- routes
- controllers
- models
- blueprints
- policies
- services, queries, and jobs
- request, model, blueprint, policy, and rswag specs
- migrations, schema, seeds, and configuration
- existing changelog, Flow, and PRD references

Summarize current behavior, reusable surfaces, and detected discrepancies.

### Step 3 — Phase 2 Plan and Stop Gate

No code changes.

Produce:

- required components
- file-by-file actions: `NEW`, `MODIFY`, `DELETE`
- responsibilities and short `OLD -> NEW` previews
- test mapping and authorization matrix
- edge/null/boundary coverage
- risks and `[IMPL]`/`[DOC]` discrepancies

End planning-first mode with:

```text
CONFIRM_TO_IMPLEMENT? (yes/no)
```

Do not implement before explicit confirmation when the normative stop gate applies.

### Step 4 — Implementation

After confirmation:

- implement minimal Rails-way/KISS diffs
- preserve requirement-owned request/response contracts
- keep Blueprinter responsible for `data`
- keep controllers responsible for envelopes
- use Pundit, Ransack, and Kaminari where applicable
- use database constraints and DB-agnostic queries
- avoid N+1 behavior on rendered associations
- add or update required tests and factories
- do not update Flow/PRD/changelog artifacts prematurely

### Step 5 — Implementation Handoff Preparation

Before handoff:

- update traceability rows with actual implementation and spec paths
- identify changed schema/seed/process-tooling surfaces
- list unresolved `[IMPL]` and `[DOC]` discrepancies
- confirm no intentional contract change exists without requirement authority
- identify the verification profile likely required without claiming it has run

## Required Output

```text
SKILL INVOCATION
- Skill: skills/rails_api_feature.md
- Phase: Phase 0 through implementation
- Trigger:
- Inputs resolved: YES/NO
- Required output: plan gate + implemented change + FEATURE IMPLEMENTATION HANDOFF
- Status: COMPLETED/BLOCKED
- Notes:

FEATURE IMPLEMENTATION HANDOFF
- Task / feature:
- Requirement source and versions:
- Confirmation received:
- Files changed:
- Specs changed:
- Schema/seed/process-tooling impact:
- Contract Traceability Matrix status:
- [IMPL] discrepancies:
- [DOC] discrepancies:
- Expected verification profile:
- Ready for quality gate review: YES/NO
```

## Completion Check

The skill is `COMPLETED` only when:

- Phase -1 passed
- contract extraction and repository scan are complete
- the plan and required stop gate were respected
- implementation and applicable tests are complete
- traceability points to actual files
- no known `[IMPL]` gap makes verification premature
- no derived documentation was updated before quality gates
- `Ready for quality gate review: YES`

Completion does not mean verification passed.

## Failure Modes

Set the invocation to `BLOCKED` and stop this procedure when:

- Phase -1 is not ready
- requirement source or version authority is ambiguous
- required confirmation is missing
- auth or response behavior cannot be resolved
- implementation requires an unauthorized requirement change
- a material dependency is missing
- tests or traceability cannot be completed sufficiently for verification

Use `[IMPL]` for implementation gaps and `[DOC]` for requirement/model-reality conflicts.

## Handoff

On `COMPLETED`:

- next skill: `skills/quality_gate_review.md`
- artifact carried forward: `FEATURE IMPLEMENTATION HANDOFF` and Contract Traceability Matrix

On `BLOCKED`:

- report the blocking authority, dependency, discrepancy, or confirmation gap
- resolve it before verification or documentation work
