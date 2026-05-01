---
name: realtime-task-collaborator
description: Collaboratively process a task in real time by clarifying the request, building an approved task list, and then working through tasks one by one with the user.
---

# Realtime Task Collaborator

## RULES

RULE: Follow `GO TO STEP N` exactly.
RULE: STOP immediately when a STEP says `STOP`.
RULE: Treat the workflow of this skill as separate from any generated task list, implementation result, or discussion output.
RULE: Work on only one `current_task` at a time.
RULE: Reuse and update stored variables explicitly.
RULE: Treat a numeric reply as the selected option when a step asks a numbered question.
RULE: Treat any non-numeric reply as clarification or revision comments for the current step.

## IMPLEMENTATION RULES

### Code Organization

RULE: Split large files by real responsibility, not by vague or purely decorative categories.
RULE: Keep fields near the part of the code where they have their primary meaning and usage.
RULE: Keep orchestration separate from low-level implementation.
RULE: Do not keep unrelated responsibilities in the same method when they can be separated without making the code harder to follow.
RULE: Do not create files, sections, or abstractions with vague roles such as `Other`, `Misc`, or similar catch-all names.
RULE: Do not split code into many tiny files unless the split clearly improves navigation and responsibility boundaries.
RULE: Prefer semantic grouping of fields over purely cosmetic ordering.
RULE: Use type-based ordering only inside an already meaningful group.

Examples:
- Good: split a large class into `Core`, `Data`, `Interaction`, and `Presentation` when each part has a stable role.
- Bad: split a class into `Other`, `Helpers`, and `Stuff` because the original file became long.
- Good: keep data collections with data-loading logic and keep UI references with UI setup logic.
- Good: group fields by responsibility first, and only then order similar fields consistently inside that group.

### Methods and Abstractions

RULE: Keep action handlers thin.
RULE: Separate methods by role such as loading data, computing state, refreshing presentation, and handling user actions.
RULE: Use computed properties for simple boolean checks with no side effects.
RULE: Use methods for transformations, derived values, or logic that is more than a trivial check.
RULE: Do not create micro-abstractions unless they improve readability, centralize a rule, or reduce maintenance risk.
RULE: Prefer helpers that operate on a meaningful block of logic or UI rather than helpers that only hide one line of code.
RULE: Inline tiny orchestration methods when they only wrap one or two obvious calls and do not improve clarity.
RULE: Extract repeated logic only when the extracted form is clearer than the repetition.
RULE: Separate infrastructure helpers from domain or presentation helpers.
RULE: Choose one abstraction level for a local code area and keep it consistent.

Examples:
- Good: a handler updates state and triggers the next meaningful step, but does not contain the entire downstream implementation.
- Good: use a computed property like `HasSelection` for a trivial boolean check.
- Bad: create four tiny setter methods when one block-level helper would express the same intent more clearly.
- Good: extract repeated block updates into one helper when the helper name explains the rule.
- Good: keep low-level helper methods separate from higher-level block refresh methods.

### Naming

RULE: Choose names based on role and intent, not implementation details.
RULE: Prefer short, direct, unambiguous names.
RULE: Do not add extra naming words unless they improve understanding.
RULE: Avoid vague names such as `Other`, `Misc`, `Helper`, or `Stuff` unless the role is still explicit from context.
RULE: Keep naming consistent across files, methods, helpers, and fields.
RULE: Respect established local naming conventions when they are already consistent.

Examples:
- Good: `RefreshHeader`, `LoadItems`, `FindMatches`, `HasSelection`.
- Bad: `DoWork`, `HandleOtherThing`, `MiscLogic`, `UpdateStateDataInfo`.
- Good: use the same naming pattern across related methods instead of mixing unrelated verbs.
- Good: keep a project-specific convention if it is already coherent and intentional.

### Flow and State

RULE: Identify the source of truth before changing code flow.
RULE: Understand whether updates happen directly, indirectly, or through an existing cascade before adding new calls.
RULE: Do not duplicate update chains that already exist as part of the current architecture.
RULE: Treat indirect or cascaded updates as valid design choices until proven otherwise.
RULE: When refactoring flow, preserve the existing behavioral model unless the task explicitly requires changing it.
RULE: Reconstruct the current architecture before proposing a different one.
RULE: Do not infer a bug from a missing direct call until all indirect triggers have been checked.
RULE: Respect cascade-based orchestration if it is consistent and intentional.

Examples:
- Good: verify whether a reset already triggers the next update before adding another explicit refresh.
- Bad: add a second refresh path because the first one was indirect and looked suspicious.
- Good: keep the current update model if it is intentional and consistent, even if it is not the model you would design from scratch.
- Good: trace the full update chain before deciding that a downstream step is missing.

### Data and Iteration Style

RULE: Prefer declarative pipelines when they clearly improve readability.
RULE: Prefer imperative flow when the code expresses a step-by-step process.
RULE: Do not use a declarative style only to make code shorter if it becomes harder to read.
RULE: Return lazy sequences by default unless materialization is required by usage or contract.
RULE: Materialize collections only when indexing, repeated enumeration, counting, or snapshot semantics are required.
RULE: Keep sequence semantics aligned with actual usage, not with habit.

Examples:
- Good: use a simple pipeline for filtering and mapping a straightforward sequence.
- Good: use an explicit loop when the code represents a multi-step traversal or algorithm.
- Bad: replace clear nested flow with a dense chain of transformations that is shorter but harder to scan.
- Good: return an iterable sequence when the caller only needs enumeration.

### Diff Discipline

RULE: Make the smallest meaningful change that satisfies the task.
RULE: Do not rewrite neighboring code unless the task requires it or the change directly improves the edited area.
RULE: Apply renames as narrowly as possible.
RULE: Do not add side improvements that were not requested unless they are necessary to keep the code correct.
RULE: Prefer the option that creates the clearest code with the least unnecessary diff noise.
RULE: Keep a patch easy to review by preserving surrounding structure when possible.

Examples:
- Good: rename only the relevant symbol usage instead of reformatting the entire method.
- Bad: combine a local fix with broad cleanup, naming changes, and unrelated style edits in one patch.
- Good: keep the diff focused enough that a reviewer can see the intent immediately.

### Cleanup

RULE: Remove temporary debugging code once it is no longer needed.
RULE: Remove stale imports, dead helpers, and displaced fields after refactoring.
RULE: After moving logic, verify that no outdated wrappers, duplicate flows, or unused abstractions remain behind.
RULE: Treat cleanup as part of the refactor, not as an optional later step.
RULE: After every structural move, run a leftovers check for obsolete code and partially migrated logic.

Examples:
- Good: remove leftover logs, unused imports, and dead wrappers in the same pass as the refactor.
- Bad: move methods to new files but leave behind unused helpers and obsolete state.
- Good: after restructuring, do a final pass for leftovers before considering the work complete.
- Good: verify that moved code did not leave behind stale wrappers, duplicate triggers, or unused fields.

### Project-Local Conventions

RULE: Prefer established project-local conventions over generic style preferences when those conventions are coherent.
RULE: Do not rename, reshape, or relocate code only to satisfy a generic best practice if the local convention is already stable.
RULE: Match the local naming, grouping, and file organization style before proposing a new pattern.
RULE: Introduce a new pattern only if it clearly improves the code and can be applied consistently.

Examples:
- Good: keep a local file-role naming scheme if it is already used consistently across the project.
- Bad: replace a stable local pattern with a textbook one just because it looks more universal.
- Good: adapt improvements to the project’s existing structure instead of forcing an unrelated structure onto it.

## VARIABLES

- `task_description`
- `task_understanding_percent`
- `task_clarification_questions`
- `task_list`
- `current_task`
- `current_task_revision_tag`

## STEP 1: REQUEST TASK DESCRIPTION

IF the current user prompt contains a usable task description:
  DO: Store it as `task_description`.
  DO: GO TO STEP 2.

DO: Output exactly `Please provide the task description.`

WAIT: User provides the task description.

DO: GO TO STEP 1.

## STEP 2: CLARIFY TASK DESCRIPTION

DO: Calculate `task_understanding_percent` using `task_description`.

IF `task_understanding_percent` is `100`:
  DO: GO TO STEP 3.

DO: Generate `task_clarification_questions` as a numbered list using `task_description`.

DO: Output exactly this format:

```text
---
Current understanding of the task: <task_understanding_percent>%

To continue, answer the following:
<task_clarification_questions>
```

WAIT: User provides clarification.

DO: Update `task_description` using the clarification.

DO: GO TO STEP 2.

## STEP 3: BUILD TASK LIST

DO: Generate `task_list` as a numbered list using `task_description`.

DO: Output exactly this format:

```text
Task list:
<task_list>

---
Do you approve this task list?
1. Approve - continue.
2. Reject - stop this workflow.

Write comments to revise the task description.
```

WAIT: User replies with `1`, `2`, or comments.

IF user replied `1`:
  DO: Select the first unfinished task from `task_list` and store it as `current_task`.
  DO: GO TO STEP 4.

IF user replied `2`:
  DO: Output exactly `Skill creation stopped.`
  DO: STOP.

DO: Update `task_description` using the revision comments.

DO: GO TO STEP 3.

## STEP 4: COLLABORATE ON CURRENT TASK

DO: Generate `current_task_revision_tag` using `current_task`.

DO: Perform the needed discussion and working actions for `current_task`, including asking follow-up questions, generating code, updating files, or producing other practical results.

DO: Output exactly in this format:

```text
Current task: <current_task>
Revision tag: <current_task_revision_tag>

Do you approve the current result?
1. Approve - continue to the next task.
2. Reject - stop this workflow.

Write comments to revise the current task result.
```

WAIT: User replies with `1`, `2`, or comments.

IF user replied `1`:
  DO: GO TO STEP 5.

IF user replied `2`:
  DO: Output exactly `Skill creation stopped.`
  DO: STOP.

DO: Update `current_task` using the revision comments.

DO: GO TO STEP 4.

## STEP 5: CHECK FOR REMAINING TASKS

IF there are unfinished tasks remaining in `task_list`:
  DO: Select the next unfinished task from `task_list` and store it as `current_task`.
  DO: GO TO STEP 4.

DO: Output exactly `All tasks are complete.`

DO: STOP.
