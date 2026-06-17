# Skill Registry

This directory contains reusable execution skills for the Agent Operating Contract.

`AGENTS.md` remains the normative authority. Skills are smaller operational procedures that help people and coding tools apply the contract consistently.

## Skill Model

A skill is not just a prompt.

A skill defines:

- when to use it
- required inputs
- execution steps
- expected outputs
- verification gates
- failure modes
- handoff notes

The goal is a **fat skills, thin harness** workflow:

- `AGENTS.md` provides the operating harness and authority model.
- `skills/**` provides reusable procedures for common work.
- `bin/verify` and `bin/contract_audit` provide verification gates.

## Current Skills

| Skill | Purpose |
| --- | --- |
| `context_loading.md` | Load the right context before planning or implementation. |
| `rails_api_feature.md` | Implement a Rails API feature against requirement contracts. |
| `quality_gate_review.md` | Run and report verification and contract audit evidence. |
| `flow_prd_update.md` | Update derived Flow/PRD docs after verification. |

## Relationship to AGENTS.md

Skills must not weaken `AGENTS.md`.

If a skill conflicts with `AGENTS.md`, stop and follow `AGENTS.md`.

Skills may:

- make execution easier to repeat
- reduce context pollution
- clarify expected outputs
- isolate common procedures
- improve reviewer confidence

Skills may not:

- change API contracts
- skip planning gates
- skip verification
- rewrite requirement docs during implementation
- invent independent Flow/PRD versions

## Recommended Use

For a typical feature task:

1. Use `context_loading.md`.
2. Use `rails_api_feature.md` through the planning and implementation phases.
3. Use `quality_gate_review.md` before docs are updated.
4. Use `flow_prd_update.md` after gates pass.

This keeps the workflow modular without turning the system into a heavy agent framework.
