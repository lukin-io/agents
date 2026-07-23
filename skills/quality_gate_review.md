# Skill: Quality Gate Review

Use this skill after implementation and contract alignment, before updating Flow, PRD, or changelog artifacts.

This skill is an execution aid. `AGENTS.md`, `AGENTS_CONTRACT.md`, and the target requirements remain authoritative.

## Purpose

Produce reviewable evidence that implementation, tests, generated API documentation, required security checks, API contract audit, and agentic workflow audit when triggered passed in the required order.

The skill owns verification evidence and the binary documentation-readiness decision:

```text
QUALITY GATE DECISION: PASS/BLOCKED
```

## Activation

Activate this skill when:

- implementation changes are complete
- the contract-alignment pre-flight is ready
- verification must be run or reviewed
- the task is approaching derived documentation or final reporting

Do not activate this skill as a substitute for implementation tests, contract extraction, or implementation work.

Initial invocation record:

```text
SKILL INVOCATION
- Skill: skills/quality_gate_review.md
- Phase: Verification/review
- Trigger:
- Inputs resolved: YES/NO
- Required output: CHECKS + RULE COMPLIANCE AUDIT + Discrepancies Report + TRACEABILITY STATUS + QUALITY GATE DECISION
- Status: ACTIVATED/BLOCKED
- Notes:
```

## Required Inputs

- task id or stable task label
- target requirement path and applicable versions
- `FEATURE IMPLEMENTATION HANDOFF` or equivalent implementation evidence
- changed implementation files
- changed spec files
- changed schema/seed/process-tooling files, if any
- changed agentic workflow surfaces, if any
- current Contract Traceability Matrix
- known `[IMPL]` and `[DOC]` discrepancies
- repository access sufficient to run or inspect required commands

## Normative References

Load and follow:

- `AGENTS.md`:
  - Skill Invocation Contract
  - Agentic Workflow Audit Gate
  - authority rules
- `AGENTS_CONTRACT.md` sections:
  - Contract Alignment Check
  - Verification Checklist
  - Contract Compliance Audit
  - Tests and Required Coverage
  - Documentation Contract
  - Final Output Contract
  - Rule Compliance Audit Format
  - Contract Traceability Matrix
- target `doc/requirements/**` sources and all applicable versions

Use `docs/QUALITY_GATES.md` and `docs/AGENTIC_AUDIT.md` as explanatory guidance only.

## Procedure

### Step 1 — Contract Alignment Pre-Flight

Re-read the target requirement and applicable versions.

Confirm:

- endpoint method/path exists
- params match names, types, defaults, enums, and requiredness
- auth and policy behavior match
- every documented response field is owned by the expected blueprint
- no undocumented response field was introduced
- required fields cannot serialize as `null`
- optional behavior matches requirements
- validations and statuses match
- canonical success/error envelopes are used
- no TODO, placeholder, or unimplemented requirement gap remains
- traceability rows point to implementation and spec evidence

All `[IMPL]` discrepancies must be fixed before command execution.

A `[DOC]` discrepancy may remain only when clearly reported and it does not make verification ambiguous or unsafe.

### Step 2 — Select Verification Profile

Default:

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

Record the selected command and reason.

Do not replace a required profile with individual checks unless the wrapper is unavailable. Record every fallback command and exit code.

### Step 3 — Run or Inspect Verification

Run the selected command, or inspect authoritative execution evidence when command execution is delegated.

Record:

- exact command
- exit code
- duration when available
- checks executed
- automatic skips and reasons
- failures and first actionable cause
- evidence timestamp or commit/diff scope when available

Do not claim a command passed when it was not run or its result was not inspected.

A failed, unavailable, uninspectable, or stale required result blocks the skill.

### Step 4 — Run or Inspect Contract Audit

Only after verification passes, run or inspect:

```bash
bin/contract_audit --all
```

When a non-standard requirement handoff allowlist is required, record the exact command and paths.

Record:

- exact command
- exit code
- audit check results
- failures and affected paths
- requirement-handoff exception used, if any

An audit failure or unjustified allowlist blocks the skill.

### Step 5 — Run or Inspect Agentic Workflow Audit When Triggered

Determine whether the diff touches the trigger surfaces listed in `AGENTS.md`.

If not triggered, record:

```text
- agentic audit: NOT_REQUIRED
```

If triggered, run after contract audit:

```bash
# Toolkit source repository
ruby -c agentic_audit
ruby agentic_audit --all --scope toolkit

# Consumer repository
bin/agentic_audit --all --scope consumer
```

Record:

- trigger surfaces
- exact command(s)
- scope
- exit codes
- check summary
- evidence commit/diff scope

A failed, unavailable, uninspectable, or stale required agentic audit blocks the skill.

The agentic audit supplements but does not replace `contract_audit`.

### Step 6 — Compliance and Traceability Review

After all required commands pass, confirm:

- contract-first behavior remains aligned
- Blueprinter owns `data` payloads
- canonical envelopes/errors are preserved
- representational invariants are preserved
- required fields use safe non-null behavior
- authorization coverage exists
- relevant success, failure, edge, and boundary specs exist
- generated Swagger is driven by specs
- requirement docs were not improperly edited
- agentic workflow structure passes when changed
- derived docs were not updated before all gates
- Contract Traceability Matrix has no unresolved implementation gap

### Step 7 — Decide Documentation Readiness

Set:

```text
QUALITY GATE DECISION: PASS
```

only when every triggered gate and completion condition passes.

Otherwise set:

```text
QUALITY GATE DECISION: BLOCKED
```

Do not produce an intermediate or assumed-success state.

## Required Output

```text
SKILL INVOCATION
- Skill: skills/quality_gate_review.md
- Phase: Verification/review
- Trigger:
- Inputs resolved: YES/NO
- Required output: CHECKS + RULE COMPLIANCE AUDIT + Discrepancies Report + TRACEABILITY STATUS + QUALITY GATE DECISION
- Status: COMPLETED/BLOCKED
- Notes:

CHECKS
- <exact verify command>: exit <code>
  - profile/reason:
  - checks executed:
  - auto-skips:
  - evidence scope:
- <exact contract audit command>: exit <code>
  - audit results:
  - evidence scope:
- <exact agentic audit command or NOT_REQUIRED>: exit <code or n/a>
  - trigger surfaces:
  - scope:
  - audit results:
  - evidence scope:

RULE COMPLIANCE AUDIT
- R1 Contract-first: COMPLIANT/VIOLATED (evidence)
- R2 Blueprinter-only data payload: COMPLIANT/VIOLATED (evidence)
- R3 Canonical envelopes/errors: COMPLIANT/VIOLATED (evidence)
- R4 Representational invariants: COMPLIANT/VIOLATED (evidence)
- R5 Safe defaults for required fields: COMPLIANT/VIOLATED (evidence)
- R6 Verification checklist completed: COMPLIANT/VIOLATED (evidence)
- R7 Agentic workflow integrity when triggered: COMPLIANT/VIOLATED/NOT_REQUIRED (evidence)

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

- contract-alignment pre-flight passes
- all `[IMPL]` discrepancies are resolved
- required verification exits `0`
- required contract audit exits `0`
- triggered agentic audit exits `0`, or is correctly `NOT_REQUIRED`
- no mandatory compliance rule is violated
- traceability has no implementation gap
- evidence matches the current diff and is recorded without fabrication

Completion means documentation readiness, not merge approval.

## Failure Modes

Set the invocation and decision to `BLOCKED` when:

- required inputs are missing
- requirement behavior is ambiguous
- an `[IMPL]` discrepancy remains
- verification cannot be run or inspected
- a required command fails
- audit uses an unjustified allowlist
- triggered agentic audit cannot be run or inspected
- mandatory tests or traceability evidence are missing
- a compliance rule is violated
- results are stale relative to the current diff

A blocked gate cannot be bypassed by editing derived documentation or weakening the skill.

## Handoff

On `COMPLETED` with `PASS`:

- next skill: `skills/flow_prd_update.md` when derived documentation is required
- next phase otherwise: final reporting under `AGENTS_CONTRACT.md`
- artifact carried forward: complete quality-gate output and current traceability matrix

On `BLOCKED`:

- report the failed command, missing evidence, discrepancy, or compliance violation
- resolve the underlying issue before documentation or final completion claims
