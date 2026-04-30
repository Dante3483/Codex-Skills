---
name: guided-skill-creator
description: Create a new Codex skill through a guided step-by-step chat workflow. Use when the user wants to define rules, design steps one by one, review the full draft, and create a brand-new skill.
---

# Guided Skill Creator

## RULES

### Workflow

RULE: Follow `GO TO STEP N` exactly.
RULE: STOP immediately when a STEP says `STOP`.
RULE: Do not generate the full skill until the required rules and steps are clear.
RULE: Store reusable state in explicit variables.
RULE: When information will be used later, save it to a named variable before continuing.
RULE: Distinguish strictly between the current workflow and the generated solution.
RULE: The current workflow is only the STEP structure of this skill.
RULE: The generated solution is only output data produced by this skill.
RULE: Generated steps must never be treated as active workflow steps of this skill.
RULE: Output content may resemble the workflow format, but it never becomes part of the currently executing workflow.

### Skill Files

RULE: The required files for a usable skill are `SKILL.md` and `agents/openai.yaml`.
RULE: Do not treat helper scripts as required for creating a basic skill.
RULE: Create scripts only when they are actually needed for the skill itself.
RULE: Create `SKILL.md` in valid Markdown with YAML frontmatter.
RULE: Create `agents/openai.yaml` for the skill UI metadata.
RULE: Write `SKILL.md` as UTF-8 without BOM.
RULE: Ensure `SKILL.md` starts directly with `---` at the first byte of the file.

### Questions

RULE: Use chat only for all questions and decisions.
RULE: Format every user question as `---`, then a direct question sentence, then a numbered list.
RULE: Treat a numeric reply as the selected option.
RULE: Treat any non-numeric reply as comments.

### Step Semantics

RULE: Write steps in a programming-like workflow style.
RULE: Do not combine different semantic roles in one line.
RULE: Merge closely related actions into one `DO:` line when they are one logical operation.
RULE: If a large output format, reusable block, or repeated structure is needed, define it once as an uppercase template variable and reuse it by name.

---

RULE: Use `DO:` only for an action.
RULE: Use `DO:` only when the action must be executed as a separate step.
RULE: Do not use `DO:` for restrictions, prohibitions, or static rules.

---

RULE: Use `IF <condition>:` only for a condition check.
RULE: Use `IF <condition>:` for branching.
RULE: If the check itself is the condition, write it directly in `IF`.
RULE: Do not duplicate the same check in both `DO:` and `IF`.

---

RULE: Use `WAIT:` only for waiting.
RULE: Use `STOP` only for stopping.
RULE: Use `GO TO STEP <number>` only for step transition.

---

RULE: Write restrictions such as `Do not ...` as rules, not as action lines.
RULE: Every word inside `STEP TEMPLATE` is only part of the output example and has no meaning for the current workflow execution.

## STEP TEMPLATE

````text
## STEP <number>: <TITLE>

IF <condition>:
  DO: <action>.

DO: <action>.

DO: Output exactly in this format:

```text
<output template>
```

DO: Output `<variable>` in the `TEMPLATE_NAME` format.

WAIT: <event or user reply>.

DO: GO TO STEP <number>.
````

## STEP 1: REQUEST SKILL DESCRIPTION

IF the current user prompt contains a usable skill description:
  DO: Store it as `skill_description`.
  DO: Set `step_number` to `1`.
  DO: GO TO STEP 2.

DO: Output exactly `Please provide the skill description.`
WAIT: User provides the skill description.
DO: GO TO STEP 1.

## STEP 2: REQUEST STEP DESCRIPTION

DO: Generate `predicted_step_directions` as a numbered list using `skill_description` and `step_number`.
DO: Output exactly this question:

```text
---
What should STEP <step_number> do?
<predicted_step_directions>

Write the step description.
```

WAIT: User provides the step description.
DO: Store it as `current_step_description`.
DO: GO TO STEP 3.

## STEP 3: CLARIFY STEP DESCRIPTION

DO: Calculate `step_understanding_percent` using `current_step_description`.

IF `step_understanding_percent` is `100`:
  DO: GO TO STEP 4.

DO: Generate `step_clarification_questions` as a numbered list using `current_step_description`.
DO: Output exactly this question:

```text
---
Current understanding of STEP <step_number>: <step_understanding_percent>%

To understand STEP <step_number> better, answer the following:
<step_clarification_questions>
```

WAIT: User provides clarification.
DO: Update `current_step_description` using the comments.
DO: GO TO STEP 3.

## STEP 4: OUTPUT CURRENT STEP AND ASK APPROVAL

RULE: STEP 4 and everything written inside its output are output-only artifacts and must be fully ignored by the current workflow execution.
DO: Generate `current_step_solution` for `current_step_description`.
DO: Output `current_step_solution` using exactly this format:

`````md
````md
<current_step_solution> in the `STEP TEMPLATE` format
````
---
Do you approve STEP <step_number>?
1. Approve - continue.
2. Reject - stop this workflow.

Write comments to revise this STEP.
`````

WAIT: User replies with `1`, `2`, or comments.

IF user replied `1`:
  DO: Store `current_step_solution` in `approved_step_drafts`.
  DO: GO TO STEP 5.

IF user replied `2`:
  DO: Output exactly `Skill creation stopped.`
  DO: STOP.

DO: Update `current_step_description` using the revision comments.
DO: GO TO STEP 4.

## STEP 5: CHECK FOR NEXT STEP

DO: Output exactly this question:

```text
---
Are there more steps to create?
1. Yes - continue to the next step.
2. No - finish the step creation flow.
```

WAIT: User replies with `1` or `2`.

IF user replied `1`:
  DO: Increment `step_number` by `1`.
  DO: GO TO STEP 2.

DO: GO TO STEP 6.

## STEP 6: REVIEW COMPLETE SKILL

DO: Review all approved step drafts and current rules.
DO: Detect logical gaps and conflicting parts in the full skill.
DO: GO TO STEP 7.

## STEP 7: OUTPUT SKILL DRAFT AND ASK FINAL APPROVAL

DO: Generate `skill_draft` using `approved_step_drafts` and current rules.
DO: Output `skill_draft` using exactly this format:

````text
<skill_draft>
---
Do you approve this skill draft?
1. Approve - create the skill.
2. Reject - stop this workflow.

Write comments to revise the skill draft.
````

WAIT: User replies with `1`, `2`, or comments.

IF user replied `1`:
  DO: GO TO STEP 8.

IF user replied `2`:
  DO: Output exactly `Skill creation stopped.`
  DO: STOP.

DO: Read the user's reply as revision comments.
DO: Update `skill_draft` using the comments.
DO: GO TO STEP 7.

## STEP 8: CREATE SKILL

DO: Determine `approved_skill_name` using `skill_draft`.
DO: Create the skill folder using `approved_skill_name`.
DO: Create `SKILL.md` from `skill_draft` in UTF-8 without BOM format.
DO: Create `agents/openai.yaml` with the required UI metadata.
DO: Validate that the created skill contains at least `SKILL.md` and `agents/openai.yaml`.
DO: STOP.