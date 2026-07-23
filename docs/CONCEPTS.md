# Agentic Workflow Concepts

This document defines the vocabulary used by the toolkit. It maps agent/context terminology to concrete repository authorities, procedures, artifacts, and gates.

It is explanatory. `AGENTS.md`, `AGENTS_CONTRACT.md`, and target requirements remain authoritative.

## System Model

```text
Phase -1 context loading
  -> requirement contract extraction
  -> repository scan and plan
  -> confirmation gate
  -> implementation
  -> quality gate review
  -> contract audit
  -> agentic audit when triggered
  -> QUALITY GATE DECISION
  -> derived docs when PASS
  -> final evidence report
```

Cross-phase procedures:

```text
context_loading
  -> rails_api_feature
  -> quality_gate_review
  -> flow_prd_update when required
```

Cross-phase artifacts:

```text
CONTEXT INVENTORY + CONTEXT SUMMARY
  -> FEATURE IMPLEMENTATION HANDOFF
  -> QUALITY GATE DECISION
  -> DOC UPDATE HANDOFF
  -> final evidence
```

## Core Concepts

### Agent Operating Contract

The Agent Operating Contract has two normative layers:

- `AGENTS.md` — small mandatory entrypoint for load order, Phase -1, skill invocation, authority, conflicts, and conditional agentic audit.
- `AGENTS_CONTRACT.md` — preserved complete Rails API contract for Phase 0 onward, engineering invariants, verification, documentation, and final reporting.

The split reduces default context pressure without deleting or weakening the original contract.

### Thin Harness

The thin harness is the small root `AGENTS.md`.

It should own cross-phase control concerns:

- what must be loaded
- which authority wins
- when a phase can start
- how skills are activated
- which additional gate is triggered
- when execution must stop

It should not duplicate the complete Rails engineering contract.

### Source-of-Truth Context

`doc/requirements/**` owns feature/API behavior and numbered requirement versions.

Requirements own:

- endpoint behavior
- request/response contracts
- validation and authorization expectations
- compatibility/removal rules
- version labels

Implementation evidence describes current behavior; it does not redefine the requirement.

### Derived Context

`doc/flow/**`, `doc/prd/**`, and task changelogs are derived records.

They:

- are updated only after required quality gates pass
- mirror requirement ownership where required
- reference requirement-owned versions
- describe verified implementation and migration impact
- cannot override requirements

### Context Engineering

Context engineering is the deliberate selection, classification, expansion, and release of context needed for correct execution.

Normative Phase -1 classifies sources as:

- `ENTRYPOINT`
- `NORMATIVE`
- `SOURCE`
- `PROCEDURE`
- `DERIVED`
- `EVIDENCE`
- `EXPLANATORY`

It produces:

```text
CONTEXT INVENTORY
CONTEXT SUMMARY
Ready for Phase 0: YES/NO
```

Material gaps block planning.

### Progressive Disclosure

Progressive disclosure means loading the minimum complete context for the current phase.

It does not mean omitting correctness-relevant authority or dependencies.

Rules:

- load current-phase procedures, not all skills
- expand context when references/dependencies/conflicts require it
- carry artifacts and decisions between phases
- release unrelated procedure text when possible
- re-evaluate conclusions when context expands

### Skill

A skill is a reusable phase-specific procedure, not an independent authority.

Every skill defines:

- Purpose
- Activation
- Required Inputs
- Normative References
- Procedure
- Required Output
- Completion Check
- Failure Modes
- Handoff

Canonical source schema:

- `templates/SKILL_TEMPLATE.md`

Consumer installation path:

- `doc/templates/SKILL_TEMPLATE.md`

### Skill Invocation

Activated skills are classified as `PROCEDURE` and require:

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

Missing skills never waive mandatory behavior. The normative contract is the fallback.

### Phase Ownership

Each current skill has one primary responsibility:

- `context_loading.md` — Phase -1 context readiness
- `rails_api_feature.md` — Phase 0 through implementation
- `quality_gate_review.md` — verification/review and readiness decision
- `flow_prd_update.md` — post-verification derived docs

Handoffs prevent one skill from silently owning the whole lifecycle.

### Contract-First Execution

Requirements are extracted before implementation planning.

Required mapping:

```text
requirement clause -> endpoint/field -> implementation file -> spec file -> status
```

This becomes the Contract Traceability Matrix.

### Planning-First Gate

Phase 0–2 planning remains no-code when the normative planning-first gate applies.

Implementation starts only after:

```text
CONFIRM_TO_IMPLEMENT? (yes/no)
```

receives explicit confirmation.

### Verification-Driven Development

Implementation completion does not equal task completion.

A task proceeds through:

1. contract alignment
2. `bin/verify` or required full profile
3. `bin/contract_audit --all`
4. `agentic_audit` when triggered
5. compliance/traceability review
6. `QUALITY GATE DECISION: PASS/BLOCKED`

Only `PASS` permits derived docs.

### Quality Gate Decision

The quality decision is binary:

- `PASS` — all required current-diff evidence is green; documentation phase is allowed.
- `BLOCKED` — a required input, command, audit, compliance rule, or traceability row is missing/failing/stale.

It is documentation readiness, not merge approval.

### Contract Drift Audit

`bin/contract_audit` protects Rails API and documentation invariants, including requirement edits, generated Swagger, route style, DB-specific SQL, controller JSON ownership, and Flow/PRD structure.

### Agentic Workflow Audit

`agentic_audit` protects the workflow layer:

- required files
- skill schema/order
- authority statements
- unique skill titles
- registry synchronization
- ownership declarations
- README index
- local references
- template mapping
- AGENTS-to-skill links

It supports toolkit and consumer scopes and runs conditionally for agentic surfaces.

### Structured Context Templates

The toolkit provides:

- `templates/SKILL_TEMPLATE.md`
- `templates/FLOW_TEMPLATE.md`
- `templates/PRD_TEMPLATE.md`

Templates make procedure and derived-doc structure predictable and auditable.

### Discrepancy Taxonomy

- `[IMPL]` — implementation does not match requirements and must be fixed before quality-gate `PASS`.
- `[DOC]` — requirement text conflicts with model/DB reality; report without silently rewriting requirements.

## What This Toolkit Is Not

- a chatbot prompt pack
- a general-purpose autonomous-agent runtime
- a multi-agent orchestration platform
- a no-code automation system
- a replacement for requirements or Rails conventions
- a system that infers command success without evidence

It is a repository-level execution, context, skill, and verification system for reliable Rails API delivery.

## Preferred Terms

- Agent Operating Contract
- Thin Harness
- Fat Skills
- Context Engineering
- Progressive Disclosure
- Source-of-Truth Context
- Derived Context
- Contract-First Execution
- Planning-First Gate
- Skill Invocation
- Phase Ownership
- Verification-Driven Development
- Quality Gate Decision
- Contract Drift Audit
- Agentic Workflow Audit
- Requirement-to-Code Traceability
- Structured Context Templates
- Agent-Ready Engineering Workflow
- AI-Assisted SDLC

Avoid vague descriptions such as:

- prompt pack
- AI wrapper
- ChatGPT scripts
- coding shortcuts
