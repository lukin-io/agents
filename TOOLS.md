# TOOLS.md

## 🎯 Purpose
This document defines strict rules for how the agent must use CLI tools in this repository.

The agent MUST:
- Prefer CLI tools over writing code or calling APIs
- Use `acli` for ALL Jira-related operations
- Follow defined command patterns exactly

---

## 🧰 Core Tool: Atlassian CLI (`acli`)

### 🔒 Global Rules
- ALWAYS use `acli`
- NEVER use:
  - curl
  - REST API calls
  - direct HTTP requests
  - other Jira clients
- Assume `acli` is installed and authenticated
- Prefer CLI execution over generating code

---

# 📌 Jira Workflows

---

## 1. Get Jira Ticket Description

### 🎯 Goal
Retrieve the description of a Jira work item.

### ⚡ Triggering Rule
If input contains:
- `jira task_id: <ISSUE_KEY>`
- or mentions a Jira task/issue/ticket WITHOUT description

👉 THEN this MUST be the FIRST step

---

### ✅ Mandatory Command
```bash
acli jira workitem view <ISSUE_KEY> --fields description --json
```

---

### 🧠 Behavior
- Treat:
  - task
  - issue
  - ticket
  as **work item**
- Extract ONLY description when requested
- If using structured output:
```bash
acli jira workitem view <ISSUE_KEY> --fields description --json
```

---

### 📦 Output Handling
- If JSON:
  - extract `.fields.description`
- If ADF format:
  - convert to readable text if possible
- Use description as:
  - source of truth
  - basis for requirement mapping
  - feature understanding

---

### ❗ Error Handling
- If command fails → report error clearly
- If description missing → report explicitly (DO NOT guess)

---

### 🚫 Forbidden
- curl / REST
- deprecated commands (`jira issue get`)

---

## 2. Get Jira Comments (Investigation / Fixing)

### 🎯 Goal
Retrieve and analyze comments of a Jira task.

---

### ⚡ Triggering Rules

#### Case A — Investigation / Fix
If input contains:
```
fix comments jira task_id: <ISSUE_KEY>
investigate comments <ISSUE_KEY>
```

👉 THEN workflow MUST be:

1. Fetch description (MANDATORY FIRST)
2. Fetch comments

---

#### Case B — Just Get Comments
If input, looks like:
```
get comments jira task id: <ISSUE_KEY>
```

👉 THEN directly fetch comments

---

### ✅ Mandatory Command

```bash
acli jira workitem comment list --key <ISSUE_KEY> --json | ruby -r json -e '
puts JSON.parse(STDIN.read)["comments"].map { |i|
  { comment_id: i["id"], body: i["body"] }
}'
```

---

### 📦 Output Format

Return:
```json
{ comment_id: 12345, body: "text" }
```

---

### 🔗 Comment Linking (IMPORTANT)

When:
- fixing bugs
- analyzing issues

👉 Include links:

```
https://wigiwork.atlassian.net/browse/<ISSUE_KEY>?focusedCommentId=<COMMENT_ID>
```

---

### ❗ Rules for Links
- ✅ Use links when:
  - debugging
  - investigating issues
- ❌ DO NOT use links when:
  - modifying documentation

---

### 🧠 Behavior
- Use comments as:
  - issue anchors
  - source of truth for bugs
  - validation of what is done / broken

---

## 🧠 Decision Rules (CRITICAL)

For ANY task:

1. If Jira-related → use `acli`
2. ALWAYS:
   - fetch description FIRST (unless explicitly skipped)
3. THEN:
   - fetch comments if needed
4. NEVER:
   - skip CLI in favor of code
   - guess data without CLI

---

## 🚫 Global Forbidden Actions

The agent MUST NOT:
- Call Jira APIs directly
- Use curl
- Reimplement `acli` logic
- Skip required command sequence
- Guess missing data

---

## ✅ Expected Workflow Summary

### Jira Task Processing
```text
1. Detect ISSUE_KEY
2. Fetch description
3. Analyze description
4. (Optional) Fetch comments
5. Analyze comments
6. Produce result
```

---

## 🧪 Examples

### Example 1
Input:
```
jira task_id: WEB-551
```

Execution:
```bash
acli jira workitem view WEB-551 --fields description,summary
```

---

### Example 2
Input:
```
fix comments jira task_id: WEB-551
```

Execution:
```bash
acli jira workitem view WEB-551 --fields description,summary

acli jira workitem comment list --key WEB-551 --json | ruby -r json -e '
puts JSON.parse(STDIN.read)["comments"].map { |i|
  { comment_id: i["id"], body: i["body"] }
}'
```

---

## ⚠️ Priority

This file has HIGH priority.

If conflicts occur:
- Follow `TOOLS.md` for tool usage
- Follow `AGENTS.md` for general behavior
