# Agentic Workflow Concepts

This document defines the vocabulary used by this toolkit. The goal is to make the workflow understandable as an agent-ready engineering system, not just a collection of Rails scripts.

## System Model

This repository provides an **Agent Operating Contract** for Rails API implementation work.

It is designed for human engineers and coding agents working in the same repository, against the same source-of-truth requirement documents, with the same verification gates before merge.

```text
Requirement source
  -> context extraction
  -> repo scan
  -> implementation plan
  -> implementation
  -> verification
  -> contract audit
  -> derived docs
  -> final report
```

## Core Concepts

### Agent Operating Contract

`AGENTS.md` is the operating contract for implementation work.

It defines:

- authority and precedence rules
- allowed and forbidden edit surfaces
- planning phases and stop gates
- implementation standards
- API response invariants
- verification requirements
- documentation update rules
- final reporting format

The contract keeps human and AI-assisted work aligned by reducing ambiguity before implementation begins.

### Source-of-Truth Context

`doc/requirements/**` is the canonical feature contract.

Requirements own:

- endpoint behavior
- request and response shape
- version labels
- validation expectations
- authorization requirements
- compatibility rules

Implementation must adapt to the requirement contract, not the other way around.

### Derived Context Docs

`doc/flow/**` and `doc/prd/**` are derived context documents.

They are updated only after verification and contract audit pass.

- Flow docs are integration-facing technical references.
- PRD docs are product/scope/acceptance references.
- Both mirror their source requirement path.
- Neither may invent independent API versions.

### Context Engineering

Context engineering is the discipline of loading the right information before execution.

In this toolkit, context loading is explicit:

1. read relevant `AGENTS.md` normative sections
2. read the target requirement document and all requirement versions
3. scan existing implementation surfaces
4. map requirement clauses to code and specs
5. produce a plan before editing code

The goal is to avoid context pollution while keeping the implementation traceable.

### Contract-First Execution

Contract-first execution means the requirement document defines the expected behavior before code changes begin.

The agent or engineer must extract:

- endpoints
- params
- response fields
- validations
- error cases
- version deltas
- traceability mapping

Only then may implementation planning proceed.

### Planning-First Gate

The planning-first gate prevents premature code edits.

When a task invokes `Execute per AGENTS.md`, phases 0-2 are no-code phases:

- Phase 0: contract extraction
- Phase 1: repo scan
- Phase 2: implementation plan

Implementation starts only after explicit confirmation.

### Verification-Driven Development

Verification-driven development means implementation is not complete when code is written.

It is complete only after:

1. contract alignment check
2. `bin/verify`
3. `bin/contract_audit --all`
4. docs/changelog update
5. final report with checks and traceability

### Contract Drift Audit

`bin/contract_audit` is the static guardrail against contract drift.

It checks for violations such as:

- edited requirement docs during implementation
- hand-edited generated Swagger YAML
- database-specific SQL tokens
- route style violations
- ad-hoc controller JSON rendering
- Flow/PRD template drift

### Contract Traceability Matrix

The Contract Traceability Matrix maps requirement clauses to implementation evidence.

```text
requirement clause -> endpoint/field -> implementation file -> spec file -> status
```

This gives reviewers a compact way to verify that implementation, tests, and docs remain aligned.

### Discrepancy Taxonomy

Discrepancies use two labels:

- `[IMPL]` means code does not match the requirement and should be fixed.
- `[DOC]` means the requirement text conflicts with model/DB reality and should be reported, not silently edited.

This separates implementation issues from upstream contract issues.

### Structured Context Templates

`templates/FLOW_TEMPLATE.md` and `templates/PRD_TEMPLATE.md` are context schemas.

They keep derived docs predictable, searchable, and integration-ready.

The point is not documentation volume. The point is stable structure for humans and agents.

## What This Toolkit Is Not

This toolkit is not:

- a chatbot prompt pack
- a general-purpose agent framework
- a replacement for Rails conventions
- a no-code automation layer
- a runtime for autonomous production agents

It is a workflow and verification layer for reliable Rails API development with human or AI-assisted execution.

## Preferred Terms

Use these terms when describing this project:

- Agent Operating Contract
- Contract-First Execution
- Context Engineering
- Verification-Driven Development
- Contract Drift Audit
- Requirement-to-Code Traceability
- Structured Context Docs
- AI-Assisted SDLC
- Agent-Ready Engineering Workflow

Avoid weak or vague terms when describing the system:

- prompt pack
- AI wrapper
- ChatGPT scripts
- coding shortcuts
