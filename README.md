# agents

Reusable Rails API contract and enforcement tooling for teams that want a strict implementation workflow, consistent API docs, and repeatable verification before merge.

This repository is not an application. It is a small toolkit you copy into a Rails API codebase so engineers or coding agents can work against one contract:

- `AGENTS.md` defines the implementation workflow, response contract, verification order, and documentation rules.
- `verify` is the source script for `bin/verify`, which runs the required verification profile.
- `contract_audit` is the source script for `bin/contract_audit`, which checks for contract drift and documentation/template violations.
- `templates/FLOW_TEMPLATE.md` and `templates/PRD_TEMPLATE.md` provide the canonical structure for derived Flow and PRD docs.

## What This Tool Is For

Use this toolkit in a Rails API repository when you want to enforce:

- contract-first implementation against `doc/requirements/**`
- canonical response envelopes and error shapes
- Blueprinter-owned `data` payloads
- Pundit/Ransack/Kaminari conventions
- post-implementation verification via `bin/verify`
- contract drift checks via `bin/contract_audit --all`
- stable Flow/PRD document structure under `doc/templates/**`

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
3. Implement the feature while following `AGENTS.md`.
4. Run `bin/verify`.
5. Run `bin/contract_audit --all`.
6. Update derived docs and changelog only after verification passes.

## Repository Contents

```text
AGENTS.md
verify
contract_audit
templates/
  FLOW_TEMPLATE.md
  PRD_TEMPLATE.md
```

## License

MIT
