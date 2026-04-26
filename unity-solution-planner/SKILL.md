---
name: unity-solution-planner
description: Create a chat-only Unity/game-development solution plan step by step. Use when the user wants to decompose, discuss, or reason through a Unity/gameplay task before implementation. This skill is planning-only and must not edit scripts, assets, scenes, prefabs, or project files.
---

# Unity Solution Planner

## RULES

### Workflow

RULE: Follow `GO TO STEP N` exactly.
RULE: STOP immediately when a STEP says `STOP`.
RULE: Never edit files while using this skill.
RULE: If the user asks to implement during this skill, say this skill only produces a plan, then STOP.
RULE: The final plan must use only `approved_subtask_solutions` and must not add new implementation ideas.

### Questions

RULE: Use chat only for all questions and decisions.
RULE: Format every user question as `---`, then a direct question sentence, then a numbered list.
RULE: Treat a numeric reply as the selected option.
RULE: Treat any non-numeric reply as comments.

### Subtasks

RULE: Merge duplicate, overlapping, or trivially small subtasks.
RULE: Keep subtasks separate when they change different systems, files, or validation goals.
RULE: Preserve dependency order when merging or rewriting subtasks.

### Code Snippets

RULE: For new code, output the complete new snippet needed for the current subtask.
RULE: For changes to existing code, output only the relevant before/after fragments, not the entire file.
RULE: Label changed-code snippets as `Before` and `After`.
RULE: Include a `Code` section only when code is useful for the current subtask.
RULE: Keep identifiers, namespaces, API names, and file paths exact.
RULE: Do not invent unrelated helpers, packages, assets, or configuration.

### Context Safety

RULE: Do not assume project structure, APIs, or file names without context.
RULE: If relevant context is missing, say the solution is based on the task description and limited project context.
RULE: Apply user comments only to the current subtask unless the user explicitly asks to revise the whole plan.
RULE: Exclude `Library`, `Temp`, `Logs`, `obj`, `.git`, IDE folders, cache folders, generated binaries, and build outputs from context inspection.

## STEP 1: REQUEST TASK DESCRIPTION

IF the current user prompt contains a usable Unity/game task description:
  DO: Store it as `task_description`.
  DO: GO TO STEP 2.

DO: Output exactly `Please provide the task description.`
WAIT: User provides the task description.
DO: GO TO STEP 1.

## STEP 2: EXTRACT OR CREATE SUBTASKS

IF `task_description` contains explicit subtasks:
  DO: Store them as `subtask_list` in original order.
  DO: Normalize `subtask_list`.
  DO: GO TO STEP 3.

DO: Generate a compact `subtask_list` from `task_description`.
DO: Normalize `subtask_list`.
DO: GO TO STEP 3.

## STEP 3: OUTPUT SUBTASK LIST

DO: Set `current_subtask_index` to `1`.
DO: Output `subtask_list` using exactly this format:

```md
## Subtask List

1. <first subtask>
2. <second subtask>
3. <third subtask>
```

DO: GO TO STEP 4.

## STEP 4: ASK SUBTASK LIST APPROVAL

DO: Output exactly this question:

```text
---
Do you approve this subtask list?
1. Approve - continue to the first subtask.
2. Reject - stop this workflow.

Write comments to add, remove, or revise subtasks.
```

WAIT: User replies with `1`, `2`, or comments.

IF user replied `1`:
  DO: GO TO STEP 5.

IF user replied `2`:
  DO: Output exactly `Planning workflow stopped.`
  DO: STOP and wait for a new user instruction.

DO: Read the user's reply as subtask list comments.
DO: Update `subtask_list` using the comments.
DO: GO TO STEP 3.

## STEP 5: READ PROJECT CONTEXT

DO: Extract search terms from `task_description` and `subtask_list`.
DO: Inspect the smallest useful project context that matches those terms.

IF relevant project context is found:
  DO: Store the findings as `project_context_summary`.
  DO: GO TO STEP 6.

DO: Store `project_context_summary` as `Limited context. Solution is based mainly on the task description.`
DO: GO TO STEP 6.

## STEP 6: PROCESS CURRENT SUBTASK

DO: Select the subtask at `current_subtask_index` from `subtask_list`.
DO: Store it as `current_subtask`.

IF `current_subtask` is fully covered by `approved_subtask_solutions`:
  DO: Mark `current_subtask` as completed.
  DO: GO TO STEP 9.

DO: Create `current_subtask_solution` for `current_subtask`.
DO: GO TO STEP 7.

## STEP 7: OUTPUT CURRENT SUBTASK RESULT

DO: Output `current_subtask_solution` using exactly this format:

````md
### Subtask <current_subtask_index>/<subtask_list count>

<current_subtask>

### Solution

<short explanation>

<Code section, if useful>
````

DO: GO TO STEP 8.

## STEP 8: ASK SUBTASK APPROVAL

DO: Output exactly this question:

```text
---
Do you approve this subtask solution?
1. Approve - continue to the next subtask.
2. Reject - stop this workflow.

Write comments to revise this solution.
```

WAIT: User replies with `1`, `2`, or comments.

IF user replied `1`:
  DO: Store `current_subtask_solution` in `approved_subtask_solutions`.
  DO: Mark `current_subtask` as completed.
  DO: GO TO STEP 9.

IF user replied `2`:
  DO: Output exactly `Subtask planning stopped.`
  DO: STOP and wait for a new user instruction.

DO: Read the user's reply as revision comments.
DO: Update `current_subtask_solution` using the comments.
DO: GO TO STEP 7.

## STEP 9: ADVANCE SUBTASK LOOP

IF every subtask in `subtask_list` is completed:
  DO: GO TO STEP 10.

DO: Increment `current_subtask_index` by `1`.
DO: GO TO STEP 6.

## STEP 10: OUTPUT FINAL PLAN

DO: Output the final plan from `approved_subtask_solutions` in approved order.
DO: Use exactly this format:

````md
## Solution Plan

### Subtask <index>/<count>

<approved explanation>

<approved Code section, if any>

### Subtask <index>/<count>

<approved explanation>

<approved Code section, if any>

## Validation

<short concrete validation list, only if useful>
````

DO: STOP.
