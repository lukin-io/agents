# Skill: Flow and PRD Update

Use this skill to update derived Flow, PRD, and task changelog artifacts after quality gates pass.

This skill is an execution aid. `AGENTS.md`, `AGENTS_CONTRACT.md`, and the target requirements remain authoritative.

## Purpose

Translate verified implementation evidence into feature-owned integration and product documentation without creating a second source of truth.

This skill owns post-verification derived documentation only.

## Activation

Activate this skill when:

- `skills/quality_gate_review.md` produced `QUALITY GATE DECISION: PASS`
- the implementation changes contract, auth, response shape, validation, integration behavior, product scope, or rollout evidence that belongs in derived docs
- required Flow/PRD/changelog paths are known or can be resolved from the requirement path

Do not activate this skill:

- before quality gates pass
- for implementation or verification work
- when no derived-document change is required

Initial invocation record:

```text
SKILL INVOCATION
- Skill: skills/flow_prd_update.md
- Phase: Post-verification documentation
- Trigger:
- Inputs resolved: YES/NO
- Required output: DOC UPDATE HANDOFF
- Status: ACTIVATED/BLOCKED/NOT_REQUIRED
- Notes:
```

## Required Inputs

- task id or stable task label
- target requirement path
- implemented requirement version or dated unversioned entry
- `QUALITY GATE DECISION: PASS` evidence
- current Contract Traceability Matrix
- changed endpoint/contract list
- changed implementation and spec files
- canonical Flow and PRD paths
- related changelog fragment path

## Normative References

Load and follow:

- `AGENTS.md` authority and Skill Invocation Contract
- `AGENTS_CONTRACT.md` sections:
  - Authority and Precedence
  - Version Authority
  - Edit Scope
  - Documentation Contract
  - Flow/PRD required structures
  - merge/collapse rules
  - Drift Delta
  - Changelog
  - Final Output Contract
- target `doc/requirements/**` sources and version headings
- `doc/templates/FLOW_TEMPLATE.md`
- `doc/templates/PRD_TEMPLATE.md`

The toolkit source files for those consumer templates are `templates/FLOW_TEMPLATE.md` and `templates/PRD_TEMPLATE.md`.

Use existing Flow/PRD docs as derived history and implementation references, not behavior authority.

## Procedure

### Step 1 — Validate Documentation Gate

Confirm:

- quality gate decision is `PASS`
- evidence matches the current implementation diff
- target requirement and version authority are known
- no unresolved `[IMPL]` discrepancy remains
- intended doc paths mirror the requirement path

If any condition fails, block documentation updates.

### Step 2 — Resolve Ownership and Paths

Determine:

- canonical feature-owned Flow path under `doc/flow/**`
- canonical feature-owned PRD path under `doc/prd/**`
- task-owned changelog fragment under `changelogs/unreleased/**`
- primary owner vs secondary dependency when overlapping docs exist
- merge, rename, delete, or temporary superseded-stub action when duplicate primary owners exist

### Step 3 — Update Flow Documentation

Flow docs are integration-facing technical references.

Update as required:

- requirement source and canonical path
- latest implemented requirement version
- last updated task
- Recent Implemented History
- Task Traceability
- endpoint contract matrix
- request examples
- JSON/Blueprint mapping
- success/error examples
- error taxonomy and client action
- migration impact
- responsible implementation files
- Drift Delta

Keep examples consistent with verified implementation.

### Step 4 — Update PRD Documentation

PRDs are product, scope, outcome, and acceptance references.

Update as required:

- requirement source, canonical path, and related Flow path
- latest implemented requirement version
- last updated task
- problem, goals/non-goals, scope, and user stories
- flows/UX notes
- technical, security, and compliance considerations
- acceptance criteria and metrics
- rollout and dependencies
- requirement-aligned Version History
- Drift Delta

Do not duplicate large technical interface blocks owned by requirements.

### Step 5 — Update Task Changelog

Create or update:

```text
changelogs/unreleased/{TASK_ID}.md
```

Keep the changelog task-owned even when Flow/PRD docs are feature-owned.

### Step 6 — Documentation Consistency Review

Confirm:

- paths mirror requirement ownership
- numbered versions come only from requirements
- unversioned requirements use dated non-numeric history
- endpoint/examples match verified evidence
- migration impact is explicit
- removed behavior is documented only when requirement-authorized
- implementation/spec references are current
- Drift Delta contains `Added`, `Changed`, and `Removed`

## Required Output

```text
SKILL INVOCATION
- Skill: skills/flow_prd_update.md
- Phase: Post-verification documentation
- Trigger:
- Inputs resolved: YES/NO
- Required output: DOC UPDATE HANDOFF
- Status: COMPLETED/BLOCKED/NOT_REQUIRED
- Notes:

DOC UPDATE HANDOFF
- Quality gate evidence:
- Requirement source/version:
- Flow path/action:
- PRD path/action:
- Changelog path/action:
- Endpoints/contracts documented:
- Migration impact documented:
- Drift Delta status:
- Traceability references updated:
- Documentation discrepancies:
- Ready for final report: YES/NO
```

## Completion Check

The skill is `COMPLETED` only when:

- quality gate decision is `PASS`
- all required derived documents use canonical ownership and structure
- versions are requirement-owned
- examples and references match verified implementation
- required task changelog exists
- Drift Delta is complete
- no documentation discrepancy materially misrepresents behavior
- `Ready for final report: YES`

Use `NOT_REQUIRED` when quality gates pass but the task does not require derived-document changes.

## Failure Modes

Set the invocation to `BLOCKED` and stop this procedure when:

- quality gate evidence is missing, stale, or blocked
- canonical requirement/Flow/PRD ownership is ambiguous
- duplicate primary-owner docs cannot be safely resolved
- a requested version does not exist in requirements
- examples cannot be reconciled with verified implementation
- documentation would conceal a contract or migration impact

## Handoff

On `COMPLETED` or valid `NOT_REQUIRED`:

- next phase: final reporting under `AGENTS_CONTRACT.md`
- artifact carried forward: `DOC UPDATE HANDOFF`, updated docs, and changelog paths

On `BLOCKED`:

- report the gate, ownership, version, or evidence conflict
- do not produce a final completion claim until resolved
