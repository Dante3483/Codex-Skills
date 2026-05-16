---
name: unity-ui-builder-helper
description: Help create and refine clean Unity UXML and USS for UI Toolkit and UI Builder. Use when the user wants help planning UI structure, choosing naming, creating base files, or implementing and revising Unity UI.
---

# Unity UI Builder Helper

## RULES

### Workflow

RULE: Follow `GO TO STEP N` exactly.
RULE: STOP immediately when a STEP says `STOP`.
RULE: Work step by step.
RULE: Do not start implementation before the layout preview is approved.
RULE: Do not create files before naming is approved.
RULE: Store reusable state in explicit variables.
RULE: If the user comments on a preview or implementation result, treat that as a revision to the source goal.

### Scope

RULE: Focus only on `UXML`, `USS`, `UI Toolkit`, and `UI Builder`.
RULE: Prioritize practical, maintainable Unity UI structure over abstract UI theory.
RULE: Treat hierarchy, naming, layout, and readability as the main concerns.

### Project Style First

RULE: Inspect the existing project style before editing `UXML` or `USS`.
RULE: Match the project current naming, hierarchy, and selector style unless there is a strong reason not to.
RULE: Prefer adapting to the existing project style over introducing a new style system.
RULE: If the user or project already demonstrates a simpler working pattern, prefer that pattern over a more theoretical structure.
RULE: Match the project's existing Unity editor window visual style closely instead of inventing a new visual language.

### Hierarchy

RULE: Prefer a flat and readable `UXML` hierarchy.
RULE: Add a container only when it has a clear job.
RULE: Keep a container only if it groups multiple elements, owns layout, owns a state switch, or is a meaningful logic anchor.
RULE: A single `Label` should not have its own wrapper unless that wrapper has a real role.
RULE: If removing a container does not change layout, readability, or behavior, consider it unnecessary.
RULE: Prefer current clarity and usefulness over future flexibility.

Example:
- Good: `stores-panel -> stores-title + stores-shell`
- Bad: `stores-panel -> title-wrapper -> stores-title`

### Layout Responsibility

RULE: Let parents own layout whenever possible.
RULE: Do not create extra wrappers when the parent can already handle row, column, spacing, or alignment cleanly.
RULE: Before adding a child-specific wrapper or class, check whether the parent can already solve the problem.
RULE: Check structure before styling.

### Naming

RULE: Name elements by role in the interface.
RULE: Prefer names like `global-search-row`, `entries-filter-row`, `details-panel`, `stores-empty-label`, `categories-shell`.
RULE: Avoid vague names like `wrapper`, `holder`, `container`, or `field-label` when a more specific name is possible.
RULE: Keep semantic names even when styles are shared.
RULE: Do not rename semantically different elements into one generic class name only to merge styles.

Example:
- Good: `global-search-label`, `entries-filter-label`
- Bad: renaming both to `field-label` only to reduce duplication

### USS

RULE: Order selectors in `USS` in the same order that related elements appear in `UXML`.
RULE: Keep `USS` readable side-by-side with the hierarchy.
RULE: Remove style rules that do not create a real visual or layout effect.
RULE: Do not keep extra properties as safety measures unless they solve a proven problem.
RULE: Merge duplicate styles only when elements are visually and semantically similar enough.
RULE: Prefer readable local selectors over over-abstracted style systems.
RULE: Do not solve hierarchy problems with cosmetic `USS` patches first.

Example:
- Good: `.global-search-label, .entries-filter-label { ... }`
- Bad: creating generic naming only to reduce duplication

### Empty States and Controls

RULE: Use meaningful empty states for lists and panels that can be empty.
RULE: Empty state text should be short, direct, and contextual.

Example:
- `No Stores`
- `No Categories`
- `Select an entry`

RULE: Prefer native Unity controls when they fit the use case.
RULE: Use `ToolbarSearchField` for editor-like search or filter UI when appropriate.

### Editing Discipline

RULE: Make small, reversible changes.
RULE: Fix structural causes before adding cosmetic style patches.
RULE: If the user manually removes something and nothing breaks, treat that as strong evidence that it was unnecessary.
RULE: Prefer proven simplification over theoretical justification.
RULE: Do not keep containers, classes, or styles only for possible future use.
RULE: When the user explicitly splits responsibilities, respect that ownership split and edit only the assigned asset type.

### Final Cleanup

RULE: Before finalizing `UXML` or `USS`, run this cleanup check:
- remove wrappers that only hold one text element without a real role
- remove containers that do not change layout or behavior
- remove style properties that do not create a visible or layout effect
- check whether the parent can own the layout instead of a child wrapper
- check whether any class name is more abstract than the role it represents
- check whether any selector was merged at the cost of semantic clarity
- check whether the final hierarchy became flatter and easier to read than before

## STEP 1: REQUEST GOAL

IF the current user prompt contains a usable goal for the `UXML` or `USS` being created:
  DO: Store it as `goal`.
  DO: GO TO STEP 2.

DO: Output exactly `Please describe the goal for the UXML or USS.`
WAIT: User provides the goal.
DO: GO TO STEP 1.

## STEP 2: CLARIFY GOAL

DO: Calculate `goal_understanding_percent` using `goal`.

IF `goal_understanding_percent` is `100`:
  DO: GO TO STEP 3.

DO: Generate `clarifying_questions` as a numbered list using `goal`.
DO: Output exactly this format:

```text
---
Current understanding of the goal: <goal_understanding_percent>%

To understand the goal better, answer the following:
<clarifying_questions>
```

WAIT: User provides clarification.
DO: Update `goal` using the clarification.
DO: GO TO STEP 2.

## STEP 3: CREATE UI LAYOUT PREVIEW

DO: Generate `layout_preview` for `goal` as a short explanation and a simple text UI preview.

DO: Output exactly in this format:

```text
<layout_preview>
---
Do you approve this layout preview?
1. Approve - continue.
2. Reject - stop this workflow.

Write comments to revise the layout preview.
```

WAIT: User replies with `1`, `2`, or comments.

IF user replied `1`:
  DO: Store `layout_preview` in `approved_layout_preview`.
  DO: GO TO STEP 4.

IF user replied `2`:
  DO: Output exactly `Skill creation stopped.`
  DO: STOP.

DO: Update `goal` using the revision comments.
DO: GO TO STEP 3.

## STEP 4: ASK TO START IMPLEMENTATION

DO: Output exactly in this format:

```text
---
Do you approve starting the UXML and USS implementation?
1. Approve - continue.
2. Reject - stop this workflow.
```

WAIT: User replies with `1` or `2`.

IF user replied `1`:
  DO: GO TO STEP 5.

DO: Output exactly `Skill creation stopped.`
DO: STOP.

## STEP 5: GENERATE AND APPLY NAMING

DO: Generate `naming_options` from `goal` and `approved_layout_preview`.

DO: Output exactly in this format:

```text
Recommended naming options:
1. <best name>
2. <alternative name>
3. <alternative name>
```

WAIT: User selects one option.

DO: Store the selected option as `selected_name`.
DO: Create the required empty files and related names using `selected_name`.
DO: GO TO STEP 6.

## STEP 6: IMPLEMENT UXML AND USS

DO: Generate `revision_tag` for the current implementation state.
DO: Apply the required `UXML` and `USS` changes to the created files using `goal`, `approved_layout_preview`, and `selected_name`.

DO: Output exactly in this format:

```text
The UI files were updated.
Revision tag: <revision_tag>

Do you approve the result?
1. Approve - finish.
2. Reject - revise.

Write comments to revise the result.
```

WAIT: User replies with `1`, `2`, or comments.

IF user replied `1`:
  DO: STOP.

IF user replied `2`:
  DO: Output exactly `Skill creation stopped.`
  DO: STOP.

DO: Update `goal` using the revision comments.
DO: GO TO STEP 6.
