<!--
Author: Maksim Lukin <max@lukin.io>
Copyright (c) 2026 Maksim Lukin
SPDX-License-Identifier: MIT
-->

# AGENTS.md — Agent Operating Contract

This file is the repository entrypoint and authority harness for human engineers and coding agents.

The complete normative Rails API contract is preserved in:

- `AGENTS_CONTRACT.md`

## Mandatory Load Order

Before planning, editing code, running verification, or updating derived documentation:

1. Read this file.
2. Read `AGENTS_CONTRACT.md` in full, or all sections relevant to the task plus every section it references.
3. Read the target `doc/requirements/**` documents and all requirement-owned versions.
4. Load additional skills and explanatory docs only when relevant to the current phase.

Do not make code changes before the normative contract and target requirements have been loaded.

## Authority and Precedence

1. Explicit user instructions govern the requested outcome unless they conflict with a higher-level safety rule.
2. This file defines the repository-level operating and loading contract.
3. `AGENTS_CONTRACT.md` defines the complete normative process, engineering rules, verification gates, documentation rules, and final reporting contract.
4. `doc/requirements/**` owns feature/API behavior and numbered requirement versions.
5. `doc/flow/**` and `doc/prd/**` are derived context updated only after verification and compliance gates pass.
6. `skills/**` contains reusable procedures for applying the normative contract; skills cannot override it.
7. `docs/**` is explanatory and adoption-oriented; it is not a second policy authority.

If this file and `AGENTS_CONTRACT.md` appear to conflict, stop and report the conflict. Do not silently choose the less strict rule.

## Architecture Map

- `AGENTS_CONTRACT.md` — complete normative Rails API contract.
- `skills/README.md` — skill registry and skill authority rules.
- `skills/context_loading.md` — deliberate context-loading procedure.
- `skills/rails_api_feature.md` — contract-first feature implementation procedure.
- `skills/flow_prd_update.md` — post-verification derived-doc procedure.
- `docs/CONCEPTS.md` — system terminology and definitions.
- `docs/WORKFLOW.md` — compact execution map.
- `docs/QUALITY_GATES.md` — verification and contract-audit explanation.
- `docs/WORKFLOW_MIGRATION.md` — staged migration plan.
- `changelog.md` — research path, decisions, stage status, and behavior impact.

## Operating Principle

The repository follows a **fat skills, thin harness** model:

- this file stays small and establishes loading, authority, and boundaries
- `AGENTS_CONTRACT.md` preserves the complete normative contract
- skills provide reusable phase-specific procedures
- verification scripts provide deterministic completion gates

Use progressive disclosure: load the minimum complete context needed for the current phase, while never omitting a normative rule or requirement dependency that can affect correctness.

## Non-Negotiable Entry Rules

- Requirements are source-of-truth inputs and are not rewritten to match implementation.
- Planning-first tasks remain no-code until their confirmation gate is cleared.
- Skills are optional execution aids, not independent authorities.
- `bin/verify` and `bin/contract_audit --all` remain mandatory in the order defined by `AGENTS_CONTRACT.md`.
- Flow/PRD/changelog updates happen only at the stage allowed by the normative contract.
- Final output must include the evidence required by `AGENTS_CONTRACT.md`.

Continue with `AGENTS_CONTRACT.md` before performing repository work.
