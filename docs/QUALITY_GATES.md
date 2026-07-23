# Quality Gates

This document explains the verification and audit gates used by the toolkit.

`AGENTS.md` and `AGENTS_CONTRACT.md` remain authoritative. `skills/quality_gate_review.md` is the reusable procedure that executes and reports these gates.

## Gate Model

Implementation is not complete when code is written.

A task is ready for derived documentation only after:

```text
contract alignment check
  -> bin/verify
  -> bin/contract_audit --all
  -> compliance/traceability review
  -> QUALITY GATE DECISION: PASS
  -> derived docs / changelog
  -> final report with evidence
```

Any failed, unavailable, stale, or uninspected required command results in:

```text
QUALITY GATE DECISION: BLOCKED
```

## Quality Gate Skill

Activate:

```text
skills/quality_gate_review.md
```

after implementation and contract alignment, before Flow/PRD/changelog updates.

The skill produces:

- a `SKILL INVOCATION` record
- exact verification commands and exit codes
- profile-selection rationale
- checks executed and automatic skips
- contract-audit evidence
- Rule Compliance Audit
- Discrepancies Report
- traceability status
- final `PASS/BLOCKED` decision

The skill cannot override command order, waive failures, or invent evidence.

## Gate 1: Contract Alignment Check

Before running commands, re-check implementation against the requirement contract and all applicable versions.

Confirm:

- every required endpoint exists
- HTTP method and path match
- auth and policy requirements match
- params match names, types, requiredness, defaults, and enums
- response fields are owned by Blueprinter
- no undocumented response fields were introduced
- required fields cannot serialize as `null`
- optional-field behavior matches requirements
- documented validations and statuses are enforced
- canonical success/error envelopes are used
- no TODO or placeholder gap remains
- traceability rows point to implementation and spec evidence

Discrepancies use:

```text
[IMPL] expected per docs vs actual code
[DOC] docs conflict with model/DB reality
```

Fix every `[IMPL]` discrepancy before command execution.

## Gate 2: `bin/verify`

Default command:

```bash
bin/verify
```

The default fast profile is intended for normal pre-merge use and includes:

- RuboCop
- RSpec excluding `:full_only`
- RSwag Swagger generation when relevant surfaces changed

Use:

```bash
bin/verify --full
```

only when required by `AGENTS_CONTRACT.md`, including when:

- the user explicitly requests full verification
- schema, migration, structure, or seed surfaces changed
- `bin/verify`, `bin/contract_audit`, `AGENTS.md`, or `AGENTS_CONTRACT.md` changed

The full profile includes:

- RuboCop
- RSpec
- Brakeman
- Bundle Audit
- RSwag Swagger generation
- seed replant

Record the selected profile and reason. Do not claim a command passed if it was not executed or inspected.

## Gate 3: `bin/contract_audit --all`

Run only after verification passes:

```bash
bin/contract_audit --all
```

It checks:

- requirement docs remain read-only during implementation
- generated Swagger YAML is driven by rswag specs
- database-specific SQL tokens are not introduced
- routes avoid kebab-case and trailing slashes
- controllers do not introduce ad-hoc JSON hashes
- Flow/PRD docs keep required structures

Record each audit result and any requirement-handoff exception used.

## Handoff Exception

A requirement doc may appear in the diff as upstream handoff input when:

- a new requirement version heading was added before implementation
- no existing requirement version heading was removed
- implementation follows that new requirement version

Do not use requirement allowlists to hide edits made during implementation.

## Gate 4: Compliance and Traceability Review

After commands pass, confirm:

- contract-first behavior remains aligned
- Blueprinter owns `data` payloads
- canonical envelopes/errors remain intact
- representational invariants remain intact
- required fields use safe non-null behavior
- authorization and required test categories are covered
- generated Swagger is spec-driven
- requirement edit policy is respected
- derived docs were not updated prematurely
- Contract Traceability Matrix has no implementation gap

## Required Output

```text
SKILL INVOCATION
- Skill: skills/quality_gate_review.md
- Phase: Verification/review
- Status: COMPLETED/BLOCKED

CHECKS
- <exact verify command>: exit <code>
  - profile/reason:
  - checks executed:
  - auto-skips:
- <exact contract audit command>: exit <code>
  - audit results:

RULE COMPLIANCE AUDIT
- R1 Contract-first: COMPLIANT/VIOLATED (evidence)
- R2 Blueprinter-only data payload: COMPLIANT/VIOLATED (evidence)
- R3 Canonical envelopes/errors: COMPLIANT/VIOLATED (evidence)
- R4 Representational invariants: COMPLIANT/VIOLATED (evidence)
- R5 Safe defaults for required fields: COMPLIANT/VIOLATED (evidence)
- R6 Verification checklist completed: COMPLIANT/VIOLATED (evidence)

Discrepancies Report
- [IMPL] ...
- [DOC] ...
- None

TRACEABILITY STATUS
- PASS/GAP

QUALITY GATE DECISION: PASS/BLOCKED
- Reason:
- Next allowed phase:
```

## Decision Rules

`PASS` requires:

- alignment pre-flight passes
- all `[IMPL]` discrepancies are resolved
- selected verification command exits `0`
- contract audit exits `0`
- no mandatory compliance rule is violated
- traceability has no implementation gap
- results match the current diff
- evidence is recorded without fabrication

Anything else is `BLOCKED`.

A blocked gate cannot be bypassed by editing derived docs or weakening the procedure.

## Why These Gates Matter

They reduce common AI-assisted implementation failures:

- code generated before the contract was fully read
- undocumented response-shape changes
- generated docs drifting from specs
- controller JSON bypassing Blueprinter
- requirement docs rewritten to match implementation
- docs updated before verification passes
- stale or fabricated command evidence
- final reports lacking traceability

The goal is mergeable-by-default work: small diffs, explicit mapping, green checks, and reviewable proof.
