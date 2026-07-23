# Quality Gates

This document explains the verification and audit gates used by the toolkit.

`AGENTS.md` and `AGENTS_CONTRACT.md` remain authoritative. `skills/quality_gate_review.md` executes and reports the gate sequence.

## Gate Model

A task is ready for derived documentation only after:

```text
contract alignment
  -> bin/verify
  -> bin/contract_audit --all
  -> agentic_audit when agentic surfaces changed
  -> compliance and traceability review
  -> QUALITY GATE DECISION: PASS
  -> derived docs / changelog
  -> final evidence report
```

Any failed, unavailable, stale, or uninspected required command produces:

```text
QUALITY GATE DECISION: BLOCKED
```

## Quality Gate Skill

Activate:

```text
skills/quality_gate_review.md
```

after implementation and contract alignment, before Flow/PRD/changelog updates.

It produces:

- `SKILL INVOCATION`
- exact commands and exit codes
- profile-selection rationale
- checks and auto-skips
- API contract-audit evidence
- conditional agentic-audit evidence
- Rule Compliance Audit
- Discrepancies Report
- traceability status
- `PASS/BLOCKED` decision

The skill cannot override command order, waive failures, or invent evidence.

## Gate 1 — Contract Alignment

Re-read the requirement contract and applicable versions.

Confirm:

- endpoint method/path exists
- auth and policy match
- params match names, types, requiredness, defaults, and enums
- Blueprinter owns documented response fields
- no undocumented fields were introduced
- required fields cannot serialize as `null`
- optional behavior matches
- validations/statuses match
- canonical envelopes are used
- no TODO or requirement gap remains
- traceability points to code and specs

Fix all `[IMPL]` discrepancies before command execution.

## Gate 2 — Verification

Default:

```bash
bin/verify
```

Use:

```bash
bin/verify --full
```

only when required by `AGENTS_CONTRACT.md`, including when:

- explicitly requested
- schema/migration/structure/seed surfaces changed
- verification/process tooling or AGENTS contract files changed

Record the exact profile and reason. Do not report success for an unexecuted or uninspected command.

## Gate 3 — API Contract Audit

After verification passes:

```bash
bin/contract_audit --all
```

It protects:

- requirement read-only policy
- spec-driven Swagger
- DB-agnostic query rules
- route representation
- controller serialization ownership
- Flow/PRD structure

Record each check and any justified requirement-handoff exception.

## Gate 4 — Agentic Workflow Audit When Triggered

Run when the diff touches:

- `AGENTS.md` or `AGENTS_CONTRACT.md`
- `skills/**`
- toolkit `docs/**`
- toolkit `templates/**`
- consumer `doc/templates/**`
- `README.md` or `changelog.md`
- `agentic_audit` / `bin/agentic_audit`
- the agentic-audit CI workflow

Toolkit source commands:

```bash
ruby -c agentic_audit
ruby agentic_audit --all --scope toolkit
```

Consumer command:

```bash
bin/agentic_audit --all --scope consumer
```

The audit checks:

- required files
- skill schema and authority statements
- unique skill titles
- registry/file synchronization
- phase ownership map
- README index
- local references
- skill-template mapping
- AGENTS links to required procedures

When no trigger surface changed, record:

```text
agentic audit: NOT_REQUIRED
```

A triggered audit that fails, is stale, unavailable, or uninspected blocks the quality decision.

## Gate 5 — Compliance and Traceability Review

After all triggered commands pass, confirm:

- contract-first behavior remains aligned
- Blueprinter owns `data`
- canonical envelopes and representational invariants remain intact
- safe defaults and authorization coverage exist
- required test categories exist
- generated docs are spec-driven
- requirement edit policy is respected
- agentic workflow structure is valid when changed
- derived docs were not updated prematurely
- Contract Traceability Matrix has no implementation gap

## Handoff Exception

A requirement doc may appear in the diff as upstream handoff input when:

- a new requirement version heading was added before implementation
- no existing version heading was removed
- implementation follows that version

Do not use allowlists to hide requirement edits made during implementation.

## Required Output

```text
CHECKS
- <exact verify command>: exit <code>
- <exact contract audit command>: exit <code>
- <exact agentic audit command or NOT_REQUIRED>: exit <code or n/a>

RULE COMPLIANCE AUDIT
- R1 Contract-first: COMPLIANT/VIOLATED
- R2 Blueprinter-only data payload: COMPLIANT/VIOLATED
- R3 Canonical envelopes/errors: COMPLIANT/VIOLATED
- R4 Representational invariants: COMPLIANT/VIOLATED
- R5 Safe defaults: COMPLIANT/VIOLATED
- R6 Verification checklist: COMPLIANT/VIOLATED
- R7 Agentic workflow integrity: COMPLIANT/VIOLATED/NOT_REQUIRED

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

- alignment passes
- all `[IMPL]` discrepancies are resolved
- verification exits `0`
- contract audit exits `0`
- triggered agentic audit exits `0`, or is correctly `NOT_REQUIRED`
- no mandatory compliance violation exists
- traceability has no gap
- evidence matches the current diff

Anything else is `BLOCKED`.

A blocked gate cannot be bypassed by editing derived docs or weakening procedures.

## Why These Gates Matter

They reduce failures such as:

- coding before requirements/context are loaded
- undocumented contract changes
- generated docs drifting from specs
- controller JSON bypassing Blueprinter
- requirements rewritten to match code
- skills drifting from their schema or registry
- broken internal references
- stale/fabricated command evidence
- docs updated before gates pass

The goal is mergeable-by-default work: small diffs, explicit mappings, green gates, and reviewable proof.
