# Skill Registry

This directory contains reusable execution procedures for the Agent Operating Contract.

`AGENTS.md` and `AGENTS_CONTRACT.md` remain normative. Skills help humans and coding agents apply the contract consistently; they do not own policy or feature behavior.

## Skill Model

A skill is not just a prompt.

A skill defines:

- purpose and activation conditions
- required inputs
- execution steps
- expected outputs/artifacts
- completion checks
- failure modes
- handoff notes
- normative references

The goal is a **fat skills, thin harness** workflow:

- `AGENTS.md` provides loading, authority, and invocation rules.
- `AGENTS_CONTRACT.md` provides the complete normative Rails API contract.
- `skills/**` provides reusable phase-specific procedures.
- `bin/verify` and `bin/contract_audit` provide deterministic completion gates.

## Current Skills

| Skill | Phase | Purpose | Status |
| --- | --- | --- | --- |
| `context_loading.md` | Phase -1 | Load, classify, and validate context before planning. | Available |
| `rails_api_feature.md` | Phase 0 through implementation | Implement a Rails API feature against requirement contracts. | Available |
| `quality_gate_review.md` | Verification/review | Collect and report verification and contract-audit evidence. | Planned until Stage 15 |
| `flow_prd_update.md` | Post-verification docs | Update derived Flow/PRD docs after gates pass. | Available |

## Authority Relationship

Skills must not weaken or reinterpret the contract.

Precedence:

1. higher-level safety rules and explicit user constraints
2. `AGENTS.md`
3. `AGENTS_CONTRACT.md`
4. `doc/requirements/**` for feature/API behavior
5. activated skill procedures
6. derived and explanatory docs

If a skill conflicts with a higher authority:

- mark the invocation `BLOCKED`
- report the conflict
- stop that procedure
- do not silently choose the easier rule
- do not edit the skill inside a feature task merely to remove the conflict

## Activation Rules

Activate a skill only when:

- the current phase and task match its purpose
- its required inputs are available or resolvable
- its output is required or materially improves repeatability, traceability, or verification
- no higher authority forbids its use

Do not activate a skill merely because it exists.

Do not bulk-load all skills into every task or phase.

## Invocation Lifecycle

For each activated skill:

1. Resolve required inputs.
2. Load the skill and classify it as `PROCEDURE` in the context inventory.
3. Confirm authority alignment.
4. Execute only the relevant procedure.
5. Produce the required artifact.
6. Evaluate completion checks and failure modes.
7. Carry forward the artifact and decisions, not unnecessary copies of the full skill body.

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

- `ACTIVATED` — skill selected and inputs resolved; execution is in progress.
- `COMPLETED` — required output exists and completion checks pass.
- `BLOCKED` — conflict or missing material input prevents safe execution.
- `NOT_REQUIRED` — phase/task does not require this skill.
- `UNAVAILABLE` — relevant skill does not exist or cannot be loaded; execute the normative workflow directly.

A missing or unavailable skill never waives a contract requirement.

## Loading Boundaries

- Load only skills relevant to the current phase.
- Multiple active skills must have distinct responsibilities and composable outputs.
- Prefer one applicable owner skill over overlapping procedures.
- Skills should reference normative sections instead of copying large policy blocks.
- Referenced normative sections must be loaded.
- When a phase ends, retain outputs and decisions; release unrelated skill body context when possible.

## What Skills May Do

- make execution repeatable
- reduce context pollution
- define stable inputs and outputs
- isolate phase-specific procedures
- improve traceability and reviewer confidence
- provide explicit completion and failure conditions

## What Skills May Not Do

- change API contracts
- rewrite requirement docs to match implementation
- bypass context, planning, confirmation, verification, or documentation gates
- override response/envelope/authorization rules
- invent independent Flow/PRD versions
- weaken required final evidence

## Recommended Use

For a typical Rails API feature:

1. Activate `context_loading.md` for Phase -1.
2. Activate `rails_api_feature.md` for contract extraction through implementation.
3. Activate `quality_gate_review.md` for verification once available; until then execute normative quality gates directly and record `UNAVAILABLE`.
4. Activate `flow_prd_update.md` only after verification and contract audit pass.

This keeps the workflow modular without turning the repository into a heavy agent framework.
