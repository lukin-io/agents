# agents

Agent-ready, contract-first workflow toolkit for Rails API teams.

This repository is not an application or a prompt pack. It is a repository-level execution, context, and verification system so human engineers and coding agents work against the same requirements, contract, procedures, and quality gates.

## System at a Glance

```text
Phase -1 context loading
  -> contract extraction
  -> repository scan
  -> implementation plan
  -> confirmation gate
  -> implementation
  -> quality gate review
  -> QUALITY GATE DECISION
  -> derived docs when PASS
  -> final evidence report
```

The architecture follows **fat skills, thin harness**:

- `AGENTS.md` — small mandatory entrypoint for loading, authority, context, and skill invocation.
- `AGENTS_CONTRACT.md` — preserved complete normative Rails API contract.
- `skills/**` — reusable phase-specific procedures with stable artifacts and handoffs.
- `verify` and `contract_audit` — deterministic quality gates.
- `docs/**` — explanatory and migration documentation, never a second authority.
- `templates/**` — canonical authoring schemas.

## Authority Model

1. `AGENTS.md` owns repository entry, Phase -1, skill invocation, authority boundaries, and conflicts.
2. `AGENTS_CONTRACT.md` owns Phase 0 onward, engineering rules, verification order, documentation timing, and final evidence.
3. `doc/requirements/**` owns feature/API behavior and numbered requirement versions.
4. `doc/flow/**` and `doc/prd/**` are derived context updated only after green gates.
5. `skills/**` applies the contract; skills cannot override contract layers or requirements.
6. `docs/**` explains the system but does not govern it.

The two AGENTS files form one normative contract system. The original full contract was preserved rather than rewritten.

## Problems It Solves

- **Prompt drift:** repository contracts replace one-off instructions.
- **Context pollution:** Phase -1 classifies and progressively loads context.
- **Premature implementation:** planning and confirmation gates happen first.
- **Contract drift:** implementation is checked against requirement-owned behavior.
- **Representational drift:** stable response, route, serialization, and docs rules.
- **Verification gaps:** exact command evidence and `PASS/BLOCKED` readiness.
- **Workflow duplication:** recurring procedures become phase-owned skills.
- **Unreviewable output:** traceability, discrepancies, checks, and handoffs are explicit.

## Core Files

### Normative contract

- `AGENTS.md` — entrypoint, Phase -1, skill invocation, authority, and boundaries.
- `AGENTS_CONTRACT.md` — complete Rails API workflow and engineering contract.

### Verification tooling

- `verify` — source installed as `bin/verify` in a consumer repository.
- `contract_audit` — source installed as `bin/contract_audit`.

### Skills

- `skills/README.md` — registry, ownership, activation, statuses, and authoring rules.
- `skills/context_loading.md` — Phase -1 context inventory and readiness.
- `skills/rails_api_feature.md` — Phase 0 through implementation and implementation handoff.
- `skills/quality_gate_review.md` — verification evidence and `QUALITY GATE DECISION`.
- `skills/flow_prd_update.md` — post-`PASS` derived-document update and handoff.

### Explanatory docs

- `docs/CONCEPTS.md` — terminology and system model.
- `docs/WORKFLOW.md` — compact execution and responsibility map.
- `docs/QUALITY_GATES.md` — quality-gate explanation.
- `docs/WORKFLOW_MIGRATION.md` — staged migration plan.
- `changelog.md` — research, decisions, stage status, and behavior impact.

### Templates

- `templates/SKILL_TEMPLATE.md` — canonical skill structure; install as `doc/templates/SKILL_TEMPLATE.md`.
- `templates/FLOW_TEMPLATE.md` — canonical Flow structure.
- `templates/PRD_TEMPLATE.md` — canonical PRD structure.

## Canonical Skill Schema

Every skill follows this order:

1. title and authority statement
2. `Purpose`
3. `Activation`
4. `Required Inputs`
5. `Normative References`
6. `Procedure`
7. `Required Output`
8. `Completion Check`
9. `Failure Modes`
10. `Handoff`

Each skill has one primary phase responsibility and one inspectable output/handoff.

| Skill | Primary ownership | Output | Handoff |
| --- | --- | --- | --- |
| `context_loading.md` | Phase -1 | `CONTEXT INVENTORY` + `CONTEXT SUMMARY` | feature skill or applicable Phase 0 procedure |
| `rails_api_feature.md` | Phase 0 through implementation | plan gate + `FEATURE IMPLEMENTATION HANDOFF` | quality gate review |
| `quality_gate_review.md` | verification/review | evidence + `QUALITY GATE DECISION` | docs update or final report |
| `flow_prd_update.md` | post-verification docs | `DOC UPDATE HANDOFF` | final report |

Ownership boundaries:

- context loading does not extract the final contract or edit code
- feature implementation does not claim verification success
- quality review does not implement features or edit derived docs
- docs update cannot start before `QUALITY GATE DECISION: PASS`

## Progressive Skill Loading

For a normal feature task:

1. Read `AGENTS.md`.
2. Load required `AGENTS_CONTRACT.md` sections and dependencies.
3. Activate `skills/context_loading.md`.
4. Load target requirements and evidence.
5. Activate `skills/rails_api_feature.md` when implementation is required.
6. Activate `skills/quality_gate_review.md` after implementation and alignment.
7. Activate `skills/flow_prd_update.md` only after quality-gate `PASS`.

Do not bulk-load all skills or explanatory docs. Carry forward artifacts and decisions, not unnecessary procedure text.

## Phase -1 Context Artifact

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

Context classifications:

- `ENTRYPOINT`
- `NORMATIVE`
- `SOURCE`
- `PROCEDURE`
- `DERIVED`
- `EVIDENCE`
- `EXPLANATORY`

Material gaps return `Ready for Phase 0: NO` and block planning/code changes.

## Skill Invocation Artifact

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

Missing skills never waive contract requirements. Execute the normative workflow directly and record `UNAVAILABLE`.

## Quality Gate Decision

The quality review records exact commands, exit codes, checks, skips, audit results, compliance evidence, discrepancies, and traceability.

```text
QUALITY GATE DECISION: PASS/BLOCKED
```

Only `PASS` permits derived-document updates. Unexecuted, uninspected, failed, or stale command evidence produces `BLOCKED`.

## Expected Consumer Layout

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
        ├── SKILL_TEMPLATE.md
        ├── FLOW_TEMPLATE.md
        └── PRD_TEMPLATE.md
```

The scripts compute `APP_ROOT` from `bin/..` and are intended to be copied into `bin/`.

## Consumer Assumptions

- Rails API
- Bundler and Git
- RuboCop and RSpec
- RSwag
- Blueprinter
- Pundit
- Kaminari
- Ransack
- `doc/requirements/**`, `doc/flow/**`, and `doc/prd/**`

## Typical Execution

1. Read `AGENTS.md`.
2. Complete Phase -1.
3. Load required contract sections and requirement versions.
4. Extract the contract and traceability matrix.
5. Scan repository evidence.
6. Produce the file/test plan and stop for confirmation when required.
7. Implement and create `FEATURE IMPLEMENTATION HANDOFF`.
8. Run quality-gate review.
9. Continue only on `QUALITY GATE DECISION: PASS`.
10. Update derived docs when required and create `DOC UPDATE HANDOFF`.
11. Produce the final evidence report.

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
  SKILL_TEMPLATE.md
  FLOW_TEMPLATE.md
  PRD_TEMPLATE.md
```

## Preferred Terminology

- Agent Operating Contract
- Context Engineering
- Context Inventory
- Contract-First Execution
- Verification-Driven Development
- Quality Gate Decision
- Contract Drift Audit
- Requirement-to-Code Traceability
- Skill Registry
- Skill Invocation
- Phase Ownership
- Progressive Disclosure
- Fat Skills, Thin Harness

## What This Toolkit Is Not

- a general-purpose autonomous-agent runtime
- a multi-agent platform
- a replacement for explicit requirements or Rails conventions
- a no-code automation system
- a model-specific prompt collection

## License

MIT
