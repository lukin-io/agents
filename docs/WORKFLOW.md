# Agentic Rails API Workflow

This document is the compact adoption map for the workflow governed by `AGENTS.md` and `AGENTS_CONTRACT.md`.

It is explanatory, not an independent policy source.

## Workflow Summary

```text
1. Phase -1 context loading
2. Requirement contract extraction
3. Repository scan
4. Implementation plan
5. Confirmation gate
6. Implementation
7. Implementation handoff
8. Contract alignment
9. Verification
10. API contract audit
11. Agentic workflow audit when triggered
12. Quality gate decision
13. Derived docs when PASS
14. Final evidence report
```

Primary skill sequence:

```text
context_loading
  -> rails_api_feature
  -> quality_gate_review
  -> flow_prd_update when required
  -> final reporting
```

## 1. Phase -1 Context Loading

Activate `skills/context_loading.md`.

Load and classify:

- root entrypoint
- normative contract sections and dependencies
- requirement sources and versions
- activated procedures
- derived docs
- implementation/spec evidence
- explanatory aids when needed

Produce:

```text
CONTEXT INVENTORY
CONTEXT SUMMARY
Ready for Phase 0: YES/NO
```

Material gaps return `NO` and stop before planning or code changes.

## 2. Requirement Contract Extraction

Requirements under `doc/requirements/**` own feature/API behavior and numbered versions.

Extract:

- endpoints, methods, paths, auth, and statuses
- params, types, defaults, enums, and requiredness
- response fields and nesting
- validations and errors
- cumulative version deltas
- compatibility/removal rules

Build the Contract Traceability Matrix.

## 3. Repository Scan

Inspect relevant:

- routes
- controllers
- models
- blueprints
- policies
- services, queries, and jobs
- request/model/blueprint/policy/rswag specs
- migrations, schema, seeds, generated docs, and config
- existing Flow/PRD/changelog references

Evidence describes current behavior but does not redefine requirements.

## 4. Implementation Plan

Produce:

- required components
- file-by-file `NEW`, `MODIFY`, `DELETE` actions
- short old/new previews
- test and authorization mapping
- edge/null/boundary coverage
- risks and `[IMPL]`/`[DOC]` discrepancies

## 5. Confirmation Gate

When planning-first mode applies, stop with:

```text
CONFIRM_TO_IMPLEMENT? (yes/no)
```

No implementation starts before explicit confirmation.

## 6. Implementation

Activate `skills/rails_api_feature.md` for matching Rails API work.

Implement minimal Rails-way/KISS changes while preserving:

- requirement-owned contracts
- Blueprinter-owned `data`
- controller envelope ownership
- Pundit/Ransack/Kaminari conventions
- database constraints and DB-agnostic queries
- required test coverage
- documentation timing rules

## 7. Implementation Handoff

Produce:

```text
FEATURE IMPLEMENTATION HANDOFF
```

It carries:

- files/specs changed
- requirement versions
- traceability state
- schema/seed/process impact
- known discrepancies
- expected verification profile
- readiness for quality review

Implementation completion does not imply verification success.

## 8. Contract Alignment

Before commands, re-check requirements against implementation and specs.

Fix every `[IMPL]` discrepancy. Report `[DOC]` discrepancies without rewriting requirements.

## 9. Verification

Activate `skills/quality_gate_review.md`.

Run the required profile:

```bash
bin/verify
```

Use `bin/verify --full` only when contract conditions require it.

Record exact command, exit code, checks, skips, and evidence scope.

## 10. API Contract Audit

After verification passes:

```bash
bin/contract_audit --all
```

This protects API/representation/documentation invariants.

## 11. Agentic Workflow Audit

When agentic surfaces changed, run after contract audit:

```bash
# Toolkit source
ruby -c agentic_audit
ruby agentic_audit --all --scope toolkit

# Consumer repository
bin/agentic_audit --all --scope consumer
```

It validates skill schema, registry, ownership, references, README index, template mapping, and AGENTS links.

For ordinary feature changes with no trigger surface, record `NOT_REQUIRED`.

## 12. Quality Gate Decision

The quality skill produces:

```text
CHECKS
RULE COMPLIANCE AUDIT
Discrepancies Report
TRACEABILITY STATUS
QUALITY GATE DECISION: PASS/BLOCKED
```

Only `PASS` permits derived-document work.

## 13. Derived Docs

When required, activate `skills/flow_prd_update.md` only after `PASS`.

Update:

- feature-owned `doc/flow/**`
- feature-owned `doc/prd/**`
- task-owned `changelogs/unreleased/{TASK_ID}.md`

Use requirement-owned versions, verified examples, migration impact, traceability, and Drift Delta.

Produce:

```text
DOC UPDATE HANDOFF
```

## 14. Final Evidence Report

Final output follows `AGENTS_CONTRACT.md` and includes:

- `WHAT & HOW`
- `RATIONALE`
- `CHECKS`
- `RULE COMPLIANCE AUDIT`
- `Discrepancies Report`
- Contract Traceability Matrix
- skill handoff/evidence relevant to the task

## Human / Agent Responsibility Split

Humans own:

- product intent
- requirement approval
- architectural tradeoffs
- confirmation gates
- merge decisions

Agents may execute:

- context loading and classification
- contract extraction and scanning
- planning and implementation
- test/verification commands
- contract and agentic audits
- evidence reporting
- post-`PASS` derived-doc updates

Agents cannot fabricate tool evidence or override authority.

## Why This Workflow Exists

It reduces:

- coding before context/requirements are understood
- hidden auth/response-shape gaps
- context noise and duplicated procedures
- contract and generated-doc drift
- skill/registry/link drift
- premature documentation updates
- stale or fabricated verification claims
- lost requirement-to-code traceability

The result should be mergeable-by-default work: focused context, explicit handoffs, green gates, and reviewable evidence.
