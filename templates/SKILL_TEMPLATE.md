# Skill Template

Use this template for reusable procedures under `skills/**`.

A skill applies the Agent Operating Contract during a specific phase. It is not an independent authority and must not duplicate large normative policy sections.

````markdown
# Skill: <Skill Name>

<One sentence describing the phase-specific procedure.>

This skill is an execution aid. `AGENTS.md`, `AGENTS_CONTRACT.md`, and the target requirements remain authoritative.

## Purpose

<Describe the single responsibility and the artifact or decision produced.>

## Activation

Activate this skill when:

- <task/phase trigger>
- <required readiness condition>

Do not activate this skill when:

- <out-of-scope condition>

Initial invocation record:

```text
SKILL INVOCATION
- Skill: skills/<file>.md
- Phase: <phase>
- Trigger:
- Inputs resolved: YES/NO
- Required output:
- Status: ACTIVATED/BLOCKED
- Notes:
```

## Required Inputs

- <input>
- <input>

## Normative References

Load and follow:

- `AGENTS.md` <relevant section>
- `AGENTS_CONTRACT.md` <relevant sections>
- target `doc/requirements/**` sources when feature behavior is involved

Use explanatory docs only as non-authoritative guidance.

## Procedure

### Step 1 — <Name>

<Procedure.>

### Step 2 — <Name>

<Procedure.>

## Required Output

```text
<Stable artifact format>
```

## Completion Check

The skill is `COMPLETED` only when:

- <condition>
- <condition>

## Failure Modes

Set the invocation to `BLOCKED` and stop this procedure when:

- <failure>
- <failure>

## Handoff

On `COMPLETED`:

- next phase/skill: `<path or phase>`
- artifact carried forward: <artifact>

On `BLOCKED`:

- report: <evidence/gap>
- next action: <resolve underlying issue>
````

## Required Section Order

1. Title and authority statement
2. `Purpose`
3. `Activation`
4. `Required Inputs`
5. `Normative References`
6. `Procedure`
7. `Required Output`
8. `Completion Check`
9. `Failure Modes`
10. `Handoff`

## Authoring Rules

- Give each skill one primary phase responsibility.
- Reference normative rules instead of copying long policy blocks.
- Define stable inputs and an inspectable output artifact.
- Include an invocation record with status.
- Define objective completion and blocking conditions.
- Define the next allowed phase or skill.
- Do not claim tool or command success without evidence.
- Do not let a skill override requirements or either AGENTS contract layer.
