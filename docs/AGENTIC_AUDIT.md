# Agentic Workflow Audit

`agentic_audit` validates the repository-level agent workflow structure after contract, skill, documentation, template, or audit-tool changes.

It supplements `contract_audit`; it does not replace Rails API contract checks.

## Why a Separate Audit

`contract_audit` protects Rails API representation and documentation conventions.

`agentic_audit` protects the agent-ready workflow layer:

- AGENTS contract files
- skill schema and phase ownership
- skill registry synchronization
- README discoverability
- repository-local references
- source/consumer template mapping
- mandatory contract-to-skill links

Keeping these concerns separate avoids risky expansion of the mature API contract auditor.

## Scopes

The script auto-detects one of two layouts.

### Toolkit source scope

```bash
ruby -c agentic_audit
ruby agentic_audit --all --scope toolkit
```

Expected source layout includes root scripts, `docs/**`, `skills/**`, `templates/**`, README, and changelog.

### Consumer repository scope

After installation into a Rails API repository:

```bash
bin/agentic_audit --all --scope consumer
```

Expected layout includes:

- `AGENTS.md`
- `AGENTS_CONTRACT.md`
- `bin/verify`
- `bin/contract_audit`
- `bin/agentic_audit`
- `skills/**`
- `doc/templates/**`

Explicit scope overrides auto-detection:

```bash
ruby agentic_audit --all --scope toolkit
bin/agentic_audit --all --scope consumer
```

## Checks

### `required_files`

Confirms required files for the detected layout exist.

### `skill_schema`

For every skill, checks:

- title starts with `# Skill:`
- AGENTS authority is declared
- `SKILL INVOCATION` exists
- canonical headings exist in order:
  - Purpose
  - Activation
  - Required Inputs
  - Normative References
  - Procedure
  - Required Output
  - Completion Check
  - Failure Modes
  - Handoff

### `unique_skill_titles`

Rejects duplicate skill titles.

### `registry_sync`

Compares actual `skills/*.md` procedures with the ownership table in `skills/README.md`.

### `ownership_map`

Confirms every registry row has primary phase ownership, required output, and handoff; reports duplicate primary ownership labels.

### `readme_index`

Toolkit-only check confirming README indexes required contracts, docs, skills, tools, and templates.

### `local_references`

Checks scope-relevant repository-local references. Wildcards, placeholders, URLs, and example-only references are ignored.

### `template_mapping`

Toolkit-only check confirming:

```text
toolkit source: templates/SKILL_TEMPLATE.md
consumer repo:  doc/templates/SKILL_TEMPLATE.md
```

### `contract_links`

Confirms root `AGENTS.md` links the complete contract and required skills.

## CLI

```bash
ruby agentic_audit --list
ruby agentic_audit --only skill_schema
ruby agentic_audit --all --verbose
```

Consumer equivalents use `bin/agentic_audit`.

## Exit Codes

- `0` — no failed checks
- `1` — failed check, invalid option/check, or execution exception

`SKIP` is not failure. Toolkit-only checks are skipped in consumer scope.

## Quality-Gate Integration

Run after `bin/contract_audit --all` when the diff touches:

- `AGENTS.md` or `AGENTS_CONTRACT.md`
- `skills/**`
- toolkit `docs/**` or `templates/**`
- consumer `doc/templates/**`
- `README.md` or `changelog.md`
- `agentic_audit` / `bin/agentic_audit`
- audit CI configuration

When no trigger surface changed, record `NOT_REQUIRED`.

A failed, stale, unavailable, or uninspected required audit makes `QUALITY GATE DECISION: BLOCKED`.

## GitHub Actions Integration

`.github/workflows/agentic-audit.yml` runs on relevant pull-request and push paths.

It verifies both deployment shapes:

1. **Toolkit source**
   - checks Ruby syntax
   - runs full toolkit audit
2. **Consumer installation simulation**
   - creates a temporary consumer root
   - copies AGENTS files
   - copies source scripts into `bin/**`
   - copies skills into `skills/**`
   - copies templates into `doc/templates/**`
   - runs full consumer-scope audit

Required successful steps:

```text
Check Ruby syntax
Run toolkit audit
Build consumer installation layout
Run consumer-layout audit
```

This catches errors that pass in the source layout but fail after installation.

## Evidence Format

Toolkit source:

```text
CHECKS
- ruby -c agentic_audit: exit 0
- ruby agentic_audit --all --scope toolkit: exit 0
```

Consumer repository:

```text
CHECKS
- bin/agentic_audit --all --scope consumer: exit 0
```

CI evidence should additionally record workflow run ID, commit SHA, conclusion, and successful step names.

Do not report a pass without executed or inspected evidence matching the current diff.

## Scope Boundaries

The audit validates workflow structure and references. It does not prove:

- semantic correctness of every prose statement
- Rails implementation correctness
- API behavior compliance
- test adequacy
- product correctness

Those remain owned by requirements, implementation/spec review, `bin/verify`, `bin/contract_audit`, and human merge review.
