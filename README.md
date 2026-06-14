# agents

Agent-ready contract-first workflow toolkit for Rails API teams.

This repository is not an application. It is a small toolkit you copy into a Rails API codebase so human engineers or coding agents can work against one operating contract, one requirement source, and one verification path before merge.

## What This Tool Is

This toolkit provides an **Agent Operating Contract** for reliable Rails API implementation work.

It treats implementation as a repeatable workflow:

```text
requirements -> context extraction -> repo scan -> plan -> implementation -> verification -> contract audit -> derived docs -> final report
```

Core files:

- `AGENTS.md` defines the implementation workflow, response contract, verification order, documentation rules, and final output contract.
- `verify` is the source script for `bin/verify`, which runs the required verification profile.
- `contract_audit` is the source script for `bin/contract_audit`, which checks for contract drift and documentation/template violations.
- `templates/FLOW_TEMPLATE.md` and `templates/PRD_TEMPLATE.md` provide the canonical structure for derived Flow and PRD docs.
- `docs/CONCEPTS.md` explains the agentic workflow vocabulary used by this repository.
- `docs/WORKFLOW.md` gives a compact execution map for teams adopting the toolkit.

## What This Tool Is For

Use this toolkit in a Rails API repository when you want to enforce:

- contract-first implementation against `doc/requirements/**`
- context loading before code changes
- planning-first execution gates
- canonical response envelopes and error shapes
- Blueprinter-owned `data` payloads
- Pundit/Ransack/Kaminari conventions
- post-implementation verification via `bin/verify`
- contract drift checks via `bin/contract_audit --all`
- stable Flow/PRD document structure under `doc/templates/**`
- requirement-to-code traceability in the final report

## Problems It Solves

- **Prompt drift:** work is governed by `AGENTS.md`, not one-off prompts.
- **Context pollution:** agents load requirements, related docs, and implementation surfaces in a defined order.
- **Contract drift:** request/response shape, docs, and implementation are checked before merge.
- **Manual verification gaps:** `bin/verify` and `bin/contract_audit --all` define the required quality gates.
- **Unreviewable AI output:** final reports must include checks, discrepancies, and a traceability matrix.

## Expected Installation Layout

The scripts in this repo are source files. In a consumer Rails repository, place them at the paths expected by the contract:

```text
<rails_app>/
├── AGENTS.md
├── bin/
│   ├── verify
│   └── contract_audit
└── doc/
    └── templates/
        ├── FLOW_TEMPLATE.md
        └── PRD_TEMPLATE.md
```

The `verify` and `contract_audit` scripts compute `APP_ROOT` from `bin/..`, so they are intended to live in `bin/`, not at the repository root.

## Consumer Repo Assumptions

This toolkit assumes the target repository is a Rails API project that uses or follows:

- Bundler and git
- RuboCop and RSpec
- RSwag for Swagger generation
- Blueprinter for serialization
- Pundit for authorization
- Kaminari for pagination
- Ransack for filtering/search
- `doc/requirements/**`, `doc/flow/**`, and `doc/prd/**` as the documentation layout

## Typical Workflow

1. Add the files from this repository into the Rails API repo using the installation layout above.
2. Write or update requirement docs under `doc/requirements/**`.
3. Execute the no-code planning phases from `AGENTS.md`.
4. Implement the feature only after the planning gate is cleared.
5. Run `bin/verify`.
6. Run `bin/contract_audit --all`.
7. Update derived docs and changelog only after verification passes.
8. Produce the final report with checks, discrepancies, and traceability.

## Repository Contents

```text
AGENTS.md
verify
contract_audit
docs/
  CONCEPTS.md
  WORKFLOW.md
templates/
  FLOW_TEMPLATE.md
  PRD_TEMPLATE.md
```

## Preferred Terminology

Use these terms when describing this toolkit:

- Agent Operating Contract
- Contract-First Execution
- Context Engineering
- Verification-Driven Development
- Contract Drift Audit
- Requirement-to-Code Traceability
- Structured Context Docs
- AI-Assisted SDLC
- Agent-Ready Engineering Workflow

## License

MIT
