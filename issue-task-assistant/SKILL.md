---
name: issue-task-assistant
description: Create clear project issues and implementation tasks through a structured chat workflow. Use when the user wants to create a task, issue, ticket, backlog item, GitHub issue, acceptance criteria, or implementation task.
---

# Issue Task Assistant

## RULES

### Workflow

RULE: Follow `GO TO STEP N` exactly.
RULE: STOP immediately when a STEP says `STOP`.

### Project Context

RULE: Use the project folder as the primary context source when project research is requested.
RULE: Read only the minimum useful set of files and folders.
RULE: Skip service folders, generated content, caches, dependencies, build outputs, logs, binaries, and other non-implementation project noise during project research.

### Issue Quality

RULE: Final title and body are in English by default unless the user explicitly asks for another language.
RULE: Prefer title verbs like `Implement`, `Add`, `Fix`, `Refactor`, `Improve`, `Update`, or `Configure`.
RULE: Keep the description short.
RULE: Keep goals outcome-focused.
RULE: Keep tasks implementation-focused.
RULE: Write tasks as a clear step-by-step sequence from the current state to the intended result.
RULE: Keep acceptance criteria testable.
RULE: When outputting the final issue in the second markdown block, omit the `Title:` line and keep only the issue body sections.

## TEMPLATES

### ISSUE_TEMPLATE

```md
Title: <action-oriented issue title>

## Description

<short explanation of the problem, feature, or improvement and why it matters>

---

## Goals

- <goal 1>
- <goal 2>
- <goal 3>

---

## Tasks

- [ ] <implementation step 1>
- [ ] <implementation step 2>
- [ ] <implementation step 3>

---

## Acceptance Criteria

- <observable outcome 1>
- <observable outcome 2>
- <observable outcome 3>
```

## STEP 1: DETECT TASK DESCRIPTION

IF the current user prompt contains a usable task description:
  DO: Store it as `task_description`.
  DO: GO TO STEP 2.

DO: Output exactly `Please provide the task description.`
WAIT: User provides the task description.
DO: GO TO STEP 1.

## STEP 2: CLARIFY TASK UNDERSTANDING

DO: Calculate and store `task_understanding_percent` using `task_description`.

IF `task_understanding_percent` is `100`:
  DO: GO TO STEP 3.

DO: Generate and store `task_clarification_questions` as a numbered list using `task_description`.
DO: Output exactly in this format:

```text
---
Current understanding of the task: <task_understanding_percent>%

To understand the task better, answer the following:
<task_clarification_questions>
```

WAIT: User provides clarification.
DO: Update `task_description` using the comments.
DO: GO TO STEP 2.

## STEP 3: DETECT TASK TYPE

DO: Determine and store `task_type` using `task_description`.

IF `task_type` was determined successfully:
  DO: GO TO STEP 4.

DO: Output exactly this question:

```text
---
What type of task is this?
1. Bug fix.
2. Feature or improvement.
3. Refactor or technical debt.

Write the task type directly.
```

WAIT: User provides the task type.
DO: Store the user's reply as `task_type`.
DO: GO TO STEP 4.

## STEP 4: SEARCH PROJECT CONTEXT

DO: Output exactly this question:

```text
---
Does this task require additional project research?
1. Yes - inspect the project.
2. No - continue without project inspection.
```

WAIT: User replies with `1` or `2`.

IF user replied `1`:
  DO: Determine search criteria using `task_description` and `task_type`.
  DO: Search the project using the needed files, folders, and patterns.

  IF relevant context was found:
    DO: Store it as `project_context_summary`.
    DO: GO TO STEP 5.

  DO: Store `project_context_summary` as `undefined`.
  DO: GO TO STEP 5.

DO: Store `project_context_summary` as `undefined`.
DO: GO TO STEP 5.

## STEP 5: RE-CLARIFY WITH PROJECT CONTEXT

IF `project_context_summary` is `undefined`:
  DO: Generate and store `task_summary` using `task_description` and `task_type`.
  DO: GO TO STEP 6.

DO: Calculate and store `task_understanding_percent` using `task_description`, `task_type`, and `project_context_summary`.

IF `task_understanding_percent` is `100`:
  DO: Generate and store `task_summary` using `task_description`, `task_type`, and `project_context_summary`.
  DO: GO TO STEP 6.

DO: Generate and store `task_clarification_questions` as a numbered list using `task_description`, `task_type`, and `project_context_summary`.
DO: Output exactly in this format:

```text
---
Current understanding of the task: <task_understanding_percent>%

To understand the task better, answer the following:
<task_clarification_questions>
```

WAIT: User provides clarification.
DO: Update `task_description` using the comments.
DO: GO TO STEP 5.

## STEP 6: OUTPUT DRAFT AND ASK APPROVAL

DO: Generate `draft` using `task_summary`.
DO: Output exactly in this format:

````md
<draft> in the `ISSUE_TEMPLATE` format
---
Do you approve this draft?
1. Approve - continue to the next step.
2. Reject - stop this workflow.

Write comments to revise the draft.
````

WAIT: User replies with `1`, `2`, or comments.

IF user replied `1`:
  DO: Store `draft` as `final_issue`.
  DO: GO TO STEP 7.

IF user replied `2`:
  DO: Output exactly `Issue drafting stopped.`
  DO: STOP.

DO: Update `task_summary` using the revision comments.
DO: GO TO STEP 6.

## STEP 7: OUTPUT FINAL ISSUE

DO: Determine `issue_title` using `final_issue`.
DO: Generate `final_issue_body` from `final_issue` without the `Title:` line.
DO: Output exactly this block:

```md
<issue_title>
```

DO: Output exactly this block:

```md
<final_issue_body>
```

DO: STOP.
