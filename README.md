# agents

Agent-ready, contract-first workflow toolkit for Rails API teams.

This repository is not an application or a prompt pack. It is a toolkit copied into a Rails API codebase so human engineers and coding agents work against the same requirement authority, execution contract, reusable procedures, and verification gates.

## System at a Glance

```text
requirements
  -> context loading
  -> contract extraction
  -> repository scan
  -> implementation plan
  -> confirmation gate
  -> implementation
  -> contract alignment
  -> verification
  -> contract audit
  -> derived docs
  -> final evidence report
```

The architecture follows a **fat skills, thin harness** model:

- `AGENTS.md` is the small repository entrypoint and authority/loading/invocation harness.
- `AGENTS_CONTRACT.md` preserves the complete normative Rails API contract.
- `skills/**` contains reusable phase-specific procedures.
- `verify` and `contract_audit` provide deterministic quality gates.
- `docs/**` explains concepts, adoption, workflow, and migration decisions.
- `templates/**` provides stable schemas for derived Flow and PRD context.

## Authority Model

The repository intentionally has one normative contract system with two layers:

1. `AGENTS.md` governs repository entry, mandatory load order, Phase -1 context loading, skill invocation, authority boundaries, and conflict handling.
2. `AGENTS_CONTRACT.md` governs the complete process from Phase 0 onward, edit scope, implementation rules, verification order, documentation timing, and final reporting.
3. `doc/requirements/**` in a consumer repository governs feature/API behavior and owns numbered requirement versions.
4. `doc/flow/**` and `doc/prd/**` are derived context updated only after verification.
5. `skills/**` are execution procedures. They cannot override either AGENTS contract layer or requirement contracts.
6. `docs/**` are explanatory and adoption-oriented. They are not a second policy source.

The split does not weaken or replace the original contract: `AGENTS_CONTRACT.md` is the unchanged former full `AGENTS.md`, while the root file provides a small, predictable entrypoint.

## What Problems It Solves

- **Prompt drift:** execution is governed by repository contracts, not one-off prompts.
- **Context pollution:** context is loaded deliberately and classified by authority.
- **Premature implementation:** no-code planning and confirmation gates happen first.
- **Contract drift:** request/response behavior is checked against requirements.
- **Representational drift:** envelopes, routes, serialization, and docs follow stable rules.
- **Verification gaps:** completion requires both verification and contract audit evidence.
- **Workflow duplication:** recurring procedures are extracted into reusable skills.
- **Unreviewable AI output:** final reports include commands, exit codes, discrepancies, and traceability.

## Core Files

### Normative contract system

- `AGENTS.md` — small Agent Operating Contract entrypoint: load order, Phase -1, skill invocation, authority, boundaries, and system map.
- `AGENTS_CONTRACT.md` — complete normative Rails API workflow and engineering contract.

### Verification tooling

- `verify` — source script installed as `bin/verify` in a consumer repository.
- `contract_audit` — source script installed as `bin/contract_audit`.

### Explanatory documentation

- `docs/CONCEPTS.md` — terminology and system model.
- `docs/WORKFLOW.md` — compact execution and responsibility map.
- `docs/QUALITY_GATES.md` — verification and contract-audit model.
- `docs/WORKFLOW_MIGRATION.md` — staged migration from the original unified contract to the agent-ready architecture.
- `changelog.md` — research path, decisions, implemented stages, and remaining work.

### Reusable skills

- `skills/README.md` — skill registry, activation, status, and authority rules.
- `skills/context_loading.md` — reusable procedure implementing Phase -1.
- `skills/rails_api_feature.md` — feature implementation procedure.
- `skills/quality_gate_review.md` — verification evidence and documentation-readiness decision procedure.
- `skills/flow_prd_update.md` — derived Flow/PRD update procedure.

### Structured context templates

- `templates/FLOW_TEMPLATE.md` — canonical Flow-doc structure.
- `templates/PRD_TEMPLATE.md` — canonical PRD structure.

## Recommended Skill Loading Order

For a normal feature task, progressively load only what is needed:

1. `AGENTS.md`.
2. `AGENTS_CONTRACT.md` relevant normative sections plus every referenced dependency.
3. Activate `skills/context_loading.md` for Phase -1.
4. Load target `doc/requirements/**` documents and relevant implementation surfaces.
5. Activate `skills/rails_api_feature.md` for planning and implementation when the task matches.
6. Activate `skills/quality_gate_review.md` after implementation and contract alignment.
7. Activate `skills/flow_prd_update.md` only after `QUALITY GATE DECISION: PASS`.

Do not load every explanatory document and skill by default. Progressive disclosure keeps the working context focused, but it must never omit a normative rule or requirement dependency that can affect correctness.

## Normative Phase -1: Context Loading

Before Phase 0 planning output, the agent or engineer must create a context inventory and classify loaded sources:

- `ENTRYPOINT` — root loading and authority instructions.
- `NORMATIVE` — mandatory workflow and engineering rules.
- `SOURCE` — canonical requirement contracts.
- `PROCEDURE` — activated skill files.
- `DERIVED` — Flow/PRD/changelog records.
- `EVIDENCE` — code, routes, schema, specs, generated docs, migrations, seeds, and config.
- `EXPLANATORY` — guides, concepts, examples, and migration notes.

Required pre-planning artifact:

```text
CONTEXT INVENTORY
| Path/Source | Classification | Why loaded | Status |
| --- | --- | --- | --- |

CONTEXT SUMMARY
- Task / feature:
- Requirement source:
- Requirement versions considered:
- Normative sections loaded:
- Procedures activated:
- Derived docs loaded:
- Implementation surfaces scanned:
- Related specs found:
- Dependencies discovered:
- Context gaps:
- Ready for Phase 0: YES/NO
```

If a material gap can change the plan, Phase -1 returns `Ready for Phase 0: NO` and stops before planning or code changes.

## Normative Skill Invocation

A skill is activated only when the current phase/task matches its purpose, required inputs are available, its output is useful or required, and no higher authority forbids it.

Invocation lifecycle:

1. Resolve required inputs.
2. Load the skill as `PROCEDURE` context.
3. Confirm it aligns with both AGENTS contract layers and requirements.
4. Execute only the phase-relevant procedure.
5. Produce its required artifact.
6. Evaluate completion checks and failure modes.
7. Carry forward the artifact and decisions, not unnecessary copies of the skill body.

Required record:

```text
SKILL INVOCATION
- Skill:
- Phase:
- Trigger:
- Inputs resolved:
- Required output:
- Status: ACTIVATED/COMPLETED/BLOCKED/NOT_REQUIRED/UNAVAILABLE
- Notes:
```

A missing skill never waives a normative requirement. When a skill is unavailable, execute the contract directly and record `UNAVAILABLE`.

Do not bulk-load all skills. Multiple skills may be active only when their responsibilities are distinct and their outputs compose without authority conflicts.

## Quality Gate Decision

`skills/quality_gate_review.md` runs after implementation and contract alignment.

It records:

- selected `bin/verify` profile and reason
- exact commands and exit codes
- checks executed and automatic skips
- contract-audit results
- rule-compliance evidence
- discrepancies
- traceability status

It ends with:

```text
QUALITY GATE DECISION: PASS/BLOCKED
```

Only `PASS` permits Flow/PRD/changelog updates. A command that was not executed or inspected cannot be reported as passed.

## Expected Installation Layout

The scripts and contract files in this repository are source artifacts. In a consumer Rails API repository, install them like this:

```text
<rails_app>/
├── AGENTS.md
├── AGENTS_CONTRACT.md
├── bin/
│   ├── verify
│   └── contract_audit
├── skills/
│   ├── README.md
│   ├── context_loading.md
│   ├── rails_api_feature.md
│   ├── quality_gate_review.md
│   └── flow_prd_update.md
└── doc/
    └── templates/
        ├── FLOW_TEMPLATE.md
        └── PRD_TEMPLATE.md
```

The scripts compute `APP_ROOT` from `bin/..`, so `verify` and `contract_audit` are intended to be copied into `bin/`.

## Consumer Repository Assumptions

The toolkit assumes a Rails API repository that uses or follows:

- Bundler and Git
- RuboCop and RSpec
- RSwag for Swagger generation
- Blueprinter for serialization
- Pundit for authorization
- Kaminari for pagination
- Ransack for filtering/search
- `doc/requirements/**`, `doc/flow/**`, and `doc/prd/**` as the documentation layout

## Typical Execution

1. Read `AGENTS.md`.
2. Complete Phase -1 and produce the context inventory/summary.
3. Load the normative contract required for the task.
4. Activate the phase-relevant skills and create invocation records.
5. Receive or identify the requirement under `doc/requirements/**`.
6. Extract the API contract and produce the traceability matrix.
7. Scan existing repository surfaces.
8. Produce a file-by-file implementation and test plan.
9. Stop at the confirmation gate when planning-first mode is active.
10. Implement minimal Rails-way changes.
11. Perform the contract alignment check.
12. Activate `skills/quality_gate_review.md`.
13. Run `bin/verify` and `bin/contract_audit --all` through the quality-gate procedure.
14. Continue only when `QUALITY GATE DECISION: PASS`.
15. Update Flow, PRD, and changelog artifacts when required.
16. Produce a final report with checks, discrepancies, and traceability.

## Repository Contents

```text
AGENTS.md
AGENTS_CONTRACT.md
README.md
changelog.md
verify
contract_audit
docs/
  CONCEPTS.md
  QUALITY_GATES.md
  WORKFLOW.md
  WORKFLOW_MIGRATION.md
skills/
  README.md
  context_loading.md
  rails_api_feature.md
  quality_gate_review.md
  flow_prd_update.md
templates/
  FLOW_TEMPLATE.md
  PRD_TEMPLATE.md
```

## Preferred Terminology

- Agent Operating Contract
- Agent-Ready Engineering Workflow
- AI-Assisted SDLC
- Context Engineering
- Context Inventory
- Source-of-Truth Context
- Derived Context Docs
- Contract-First Execution
- Planning-First Gate
- Verification-Driven Development
- Quality Gate Decision
- Contract Drift Audit
- Requirement-to-Code Traceability
- Structured Context Schemas
- Skill Registry
- Skill Invocation
- Progressive Disclosure
- Fat Skills, Thin Harness

## What This Toolkit Is Not

- a general-purpose autonomous-agent runtime
- a multi-agent orchestration platform
- a replacement for explicit requirements
- a replacement for Rails conventions
- a no-code automation system
- a collection of model-specific prompts

It is a repository-level execution, context, and verification system for reliable Rails API delivery.

## License

MIT
