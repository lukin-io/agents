# Skill: Flow and PRD Update

Use this skill after implementation verification is complete.

## Purpose

Update derived Flow and PRD docs so they reflect the implemented requirement contract without becoming a second source of truth.

## Required Inputs

- task id
- requirement doc path
- Flow doc path
- PRD doc path
- implemented requirement version or dated requirement entry
- changed endpoint list
- changed implementation files
- changed spec files

## Flow Doc Rules

Flow docs are integration-facing technical references.

They should include:

- requirement source
- canonical Flow doc path
- latest implemented requirement version
- last updated task
- recent implemented history
- task traceability
- endpoint contract matrix
- request examples
- JSON and Blueprint contract map
- success and error examples
- migration impact
- responsible implementation files
- Drift Delta

Flow docs must mirror the source requirement path under `doc/flow/**`.

## PRD Doc Rules

PRD docs are product, scope, and acceptance references.

They should include:

- requirement source
- canonical PRD doc path
- related Flow doc
- latest implemented requirement version
- last updated task
- problem statement
- goals and non-goals
- scope
- user stories
- flows and UX notes
- technical considerations
- security and compliance
- acceptance criteria
- metrics
- rollout plan
- dependencies
- version history
- Drift Delta

PRD docs must mirror the source requirement path under `doc/prd/**`.

## Version Rules

Requirement docs own numbered versions.

Flow and PRD docs may reference requirement version headings verbatim.

Flow and PRD docs must not invent independent numeric API versions.

If the requirement doc is unversioned, use dated non-numeric history entries.

## Drift Delta

Each doc update should include:

```text
Added:
- ...

Changed:
- ...

Removed:
- None.
```

Use `Removed:` only when the requirement contract explicitly marks removal.

## Expected Output

```text
DOC UPDATES
- Flow:
- PRD:
- Changelog:

TRACEABILITY
- Requirement version:
- Task id:
- Endpoints:
- Implementation files:
- Spec files:
```

## Review Notes

Check that:

- Flow and PRD paths mirror the requirement path
- docs reference requirement-owned versions only
- examples match verified implementation
- migration impact is clear
- Drift Delta is present
