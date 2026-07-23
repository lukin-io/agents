# Skill: Quality Gate Review

Use this skill after implementation and contract alignment, before updating Flow, PRD, or changelog artifacts.

This skill implements the verification procedure. `AGENTS.md`, `AGENTS_CONTRACT.md`, and the target requirements remain authoritative.

## Purpose

Produce reviewable evidence that implementation, tests, generated API documentation, security checks when required, and contract-drift checks have passed in the required order.

The skill ends with a binary readiness decision:

```text
QUALITY GATE DECISION: PASS/BLOCKED
```

`PASS` allows the post-verification documentation phase. `BLOCKED` forbids it.

## Activation

Activate this skill when:

- implementation changes are complete
- the pre-flight contract alignment check is ready
- verification must be run or reviewed
- the task is approaching Flow/PRD/changelog updates or final reporting

Do not activate this skill as a substitute for implementation tests or contract extraction.

Initial invocation record:

```text
SKILL INVOCATION
- Skill: skills/quality_gate_review.md
- Phase: Verification/review
- Trigger: implementation is ready for required quality gates
- Inputs resolved: YES/NO
- Required output: CHECKS + RULE COMPLIANCE AUDIT + Discrepancies Report + QUALITY GATE DECISION
- Status: ACTIVATED/BLOCKED
- Notes:
```

## Required Inputs

- task id or stable task label
- target requirement path
- requirement versions implemented
- changed implementation files
- changed spec files
- changed schema/seed/process-tooling files, if any
- current Contract Traceability Matrix
- known `[IMPL]` and `[DOC]` discrepancies
- repository access sufficient to run or inspect required commands

## Normative References

Load and follow:

- `AGENTS.md` skill invocation and authority rules
- `AGENTS_CONTRACT.md` sections:
  - Contract Alignment Check
  - Verification Checklist
  - Contract Compliance Audit
  - Tests and Required Coverage
  - Documentation Contract
  - Final Output Contract
  - Rule Compliance Audit Format
  - Contract Traceability Matrix
- `docs/QUALITY_GATES.md` as explanatory guidance only

## Step 1 — Contract Alignment Pre-Flight

Re-read the target requirement and all applicable versions.

Confirm:

- endpoint method/path exists
- params match names, types, defaults, enums, and requiredness
- auth and policy behavior match
- every documented response field is owned by the expected blueprint
- no undocumented response fields were introduced
- required fields cannot serialize as `null`
- optional-field behavior matches requirements
- validations and statuses match
- canonical success/error envelopes are used
- no TODO, placeholder, or unimplemented requirement gap remains
- current traceability rows point to implementation and spec evidence

All `[IMPL]` discrepancies must be fixed before command execution.

A `[DOC]` discrepancy may remain only when clearly reported and it does not make the implementation contract ambiguous or unsafe to verify.

## Step 2 — Select Verification Profile

Default command:

```bash
bin/verify
```

Use:

```bash
bin/verify --full
```

only when required by `AGENTS_CONTRACT.md`, including when:

- the user explicitly requests full verification
- schema, migration, structure, or seed surfaces changed
- `bin/verify`, `bin/contract_audit`, `AGENTS.md`, or `AGENTS_CONTRACT.md` changed

Record the exact selected command and why.

Do not silently replace the required profile with individual checks unless the wrapper is unavailable. If fallback commands are used, record every command and exit code.

## Step 3 — Run Verification

Run the selected verification command.

Record:

- exact command
- exit code
- duration when available
- checks executed
- checks automatically skipped and the reason
- failures and the first actionable cause

Do not claim a command passed if it was not executed or its result was not available.

If verification fails:

- set invocation status to `BLOCKED`
- set `QUALITY GATE DECISION: BLOCKED`
- do not run documentation-update procedures
- report the failure evidence

## Step 4 — Run Contract Audit

Only after verification passes, run:

```bash
bin/contract_audit --all
```

When an explicit non-standard requirement handoff allowlist is required, record the exact command including every allowed path.

Record:

- exact command
- exit code
- each audit check result
- failures and affected paths
- any requirement-handoff exception used

If contract audit fails, set the decision to `BLOCKED`.

## Step 5 — Compliance Review

After commands pass, confirm:

- contract-first behavior remains aligned
- Blueprinter owns `data` payloads
- canonical envelopes/errors are preserved
- representational invariants are preserved
- required fields use safe non-null behavior
- authorization coverage exists
- relevant success, failure, edge, and boundary specs exist
- generated Swagger is driven by specs
- requirement docs were not improperly edited
- derived docs have not been updated before the gates
- Contract Traceability Matrix has no unresolved implementation gaps

## Required Output

```text
SKILL INVOCATION
- Skill: skills/quality_gate_review.md
- Phase: Verification/review
- Trigger:
- Inputs resolved: YES/NO
- Required output: CHECKS + RULE COMPLIANCE AUDIT + Discrepancies Report + QUALITY GATE DECISION
- Status: COMPLETED/BLOCKED
- Notes:

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
- Gap rows:

QUALITY GATE DECISION: PASS/BLOCKED
- Reason:
- Next allowed phase:
```

## Completion Check

The skill is `COMPLETED` with `QUALITY GATE DECISION: PASS` only when:

- contract alignment pre-flight passes
- all `[IMPL]` discrepancies are resolved
- required verification command exits `0`
- required contract-audit command exits `0`
- compliance review has no violated mandatory rule
- traceability has no implementation gap
- evidence is recorded without fabrication

On `PASS`, the next allowed procedure is `skills/flow_prd_update.md` when documentation changes are required.

## Failure Modes

Set invocation status and decision to `BLOCKED` when:

- required inputs are missing
- requirement behavior is ambiguous
- an `[IMPL]` discrepancy remains
- verification cannot be run or inspected
- a required command fails
- audit uses an unjustified requirement allowlist
- mandatory tests or traceability evidence are missing
- a compliance rule is violated
- results are stale relative to the current diff

A blocked quality gate cannot be bypassed by editing derived documentation or weakening the skill. Resolve the underlying implementation, contract, environment, or tooling issue first.
