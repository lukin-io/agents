# Skill Registry

This directory contains reusable execution procedures for the Agent Operating Contract.

`AGENTS.md`, `AGENTS_CONTRACT.md`, and target requirements remain authoritative. Skills apply those authorities during specific phases; they do not own policy or feature behavior.

## Canonical Skill Schema

The toolkit source template is:

- `templates/SKILL_TEMPLATE.md`

In a consumer repository, install it as:

- `doc/templates/SKILL_TEMPLATE.md`

All skills follow its required structure.

Required section order:

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

A skill must have one primary phase responsibility and one inspectable output/handoff.

## Skill Model

A skill defines:

- activation and exclusions
- required inputs
- normative references
- phase-specific procedure
- stable output artifact or decision
- objective completion conditions
- blocking conditions
- next allowed phase/skill

The architecture is **fat skills, thin harness**:

- `AGENTS.md` provides loading, authority, and invocation rules.
- `AGENTS_CONTRACT.md` provides the complete Rails API contract.
- `skills/**` provides reusable phase procedures.
- `verify` and `contract_audit` provide deterministic gates.

## Ownership Map

| Skill | Primary phase ownership | Required output | Handoff |
| --- | --- | --- | --- |
| `context_loading.md` | Phase -1 context loading | `CONTEXT INVENTORY` + `CONTEXT SUMMARY` | Rails feature skill or applicable Phase 0 procedure |
| `rails_api_feature.md` | Phase 0 through implementation | plan gate + `FEATURE IMPLEMENTATION HANDOFF` | quality gate review |
| `quality_gate_review.md` | Verification/review | evidence + `QUALITY GATE DECISION` | Flow/PRD update or final reporting |
| `flow_prd_update.md` | Post-verification derived docs | `DOC UPDATE HANDOFF` | final reporting |

Responsibilities must not overlap silently:

- feature implementation does not claim verification success
- quality review does not implement features or edit derived docs
- derived-doc update does not run before quality-gate `PASS`
- context loading does not perform contract extraction or code changes

## Authority Relationship

Precedence:

1. higher-level safety rules and explicit user constraints
2. `AGENTS.md`
3. `AGENTS_CONTRACT.md`
4. `doc/requirements/**` for feature/API behavior
5. activated skill procedures
6. derived and explanatory docs

When a skill conflicts with higher authority:

- mark invocation `BLOCKED`
- report the conflict
- stop the procedure
- do not silently choose the easier rule
- do not edit the skill inside a feature task merely to conceal the conflict

## Activation Rules

Activate a skill only when:

- phase/task matches its purpose
- inputs are available or safely resolvable
- its output is required or materially improves repeatability, traceability, or verification
- no higher authority forbids use

Do not activate a skill merely because it exists. Do not bulk-load all skills.

## Invocation Lifecycle

For each activated skill:

1. resolve required inputs
2. load it as `PROCEDURE` context
3. confirm authority alignment
4. execute only its owned procedure
5. produce its required artifact
6. evaluate completion/failure conditions
7. hand off the artifact and decisions rather than unnecessary skill-body context

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

## Status Meanings

- `ACTIVATED` — selected and executing.
- `COMPLETED` — output exists and completion checks pass.
- `BLOCKED` — conflict or missing material input prevents safe execution.
- `NOT_REQUIRED` — task/phase does not require the skill.
- `UNAVAILABLE` — relevant skill cannot be loaded; execute normative behavior directly.

Missing skills never waive contract requirements.

## Loading Boundaries

- load only current-phase skills
- multiple active skills require distinct ownership and composable outputs
- prefer one owner skill over overlapping procedures
- reference normative sections instead of copying large policy blocks
- load every referenced normative dependency
- retain outputs/decisions between phases; release unrelated skill-body context when possible

## Authoring Rules

Skills may:

- make execution repeatable
- reduce context pollution
- define stable artifacts
- isolate phase responsibilities
- improve traceability and reviewer confidence

Skills may not:

- change API contracts
- rewrite requirements to match implementation
- bypass context, planning, confirmation, verification, or documentation gates
- override serialization, envelope, authorization, or version rules
- fabricate command/tool evidence
- weaken final evidence requirements

## Standard Feature Sequence

1. `context_loading.md`
2. `rails_api_feature.md`
3. `quality_gate_review.md`
4. `flow_prd_update.md` when derived docs are required
5. final reporting under `AGENTS_CONTRACT.md`

This sequence is phase ownership, not a requirement to bulk-load all four skills at once.
