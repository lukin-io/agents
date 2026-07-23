# Agentic Workflow Audit

`agentic_audit` validates the repository-level agent workflow structure after documentation, skill, template, or contract changes.

It supplements `contract_audit`; it does not replace API contract checks.

## Why a Separate Audit

`contract_audit` protects Rails API representation and documentation conventions.

`agentic_audit` protects the agent-ready workflow layer:

- AGENTS contract files
- skill schema and ownership
- skill registry synchronization
- README discoverability
- local references
- source/consumer template mapping
- mandatory contract-to-skill links

Keeping this logic focused avoids risky expansion of the mature API contract auditor.

## Scopes

The audit auto-detects one of two layouts.

### Toolkit source scope

Run from this repository:

```bash
ruby agentic_audit --all
```

Source layout includes:

- root source scripts
- `docs/**`
- `skills/**`
- `templates/**`
- README and changelog

### Consumer repository scope

After installation into a Rails API repository:

```bash
bin/agentic_audit --all
```

Consumer layout expects:

- `AGENTS.md`
- `AGENTS_CONTRACT.md`
- `bin/verify`
- `bin/contract_audit`
- `bin/agentic_audit`
- `skills/**`
- `doc/templates/**`

Override auto-detection when needed:

```bash
ruby agentic_audit --all --scope toolkit
bin/agentic_audit --all --scope consumer
```

## Checks

### `required_files`

Confirms required files for the detected scope exist.

### `skill_schema`

For every skill, checks:

- title starts with `# Skill:`
- AGENTS authority is declared
- `SKILL INVOCATION` artifact exists
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

Compares actual `skills/*.md` files with the ownership table in `skills/README.md`.

### `ownership_map`

Confirms each registry row has:

- one primary phase ownership
- one required output
- one handoff

Also reports duplicate primary ownership labels.

### `readme_index`

Toolkit-only check confirming README indexes required contracts, docs, skills, tools, and templates.

### `local_references`

Checks repository-local Markdown/code references that belong to the detected scope.

Wildcard, placeholder, URL, and example-only references are ignored.

### `template_mapping`

Toolkit-only check confirming both mappings are documented:

```text
toolkit source: templates/SKILL_TEMPLATE.md
consumer repo:  doc/templates/SKILL_TEMPLATE.md
```

### `contract_links`

Confirms root `AGENTS.md` links the complete contract and all required skills.

## CLI

```bash
ruby agentic_audit --list
ruby agentic_audit --only skill_schema
ruby agentic_audit --all --verbose
```

Consumer equivalents use `bin/agentic_audit`.

## Exit Codes

- `0` — no failed checks
- `1` — one or more checks failed, an invalid check was requested, or execution raised an exception

`SKIP` is not failure. Toolkit-only checks are skipped in consumer scope.

## Quality-Gate Integration

Run the agentic audit after `bin/contract_audit --all` when the diff touches agentic workflow surfaces:

- `AGENTS.md`
- `AGENTS_CONTRACT.md`
- `skills/**`
- `docs/**`
- `templates/**` or consumer `doc/templates/**`
- `README.md`
- `changelog.md`
- `agentic_audit` / `bin/agentic_audit`
- agentic audit CI configuration

Commands:

```bash
# Toolkit source repository
ruby agentic_audit --all --scope toolkit

# Consumer Rails repository
bin/agentic_audit --all --scope consumer
```

A failed or uninspected required agentic audit makes the quality-gate decision `BLOCKED`.

## Evidence

Report:

```text
CHECKS
- ruby -c agentic_audit: exit 0
- ruby agentic_audit --all --scope toolkit: exit 0
```

or in a consumer repo:

```text
CHECKS
- bin/agentic_audit --all --scope consumer: exit 0
```

Do not report a pass without executed or inspected evidence matching the current diff.
