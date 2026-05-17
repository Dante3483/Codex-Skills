---
name: session-skill-observer
description: Capture reusable rules from user corrections, preferences, constraints, and workflow feedback during a session that may involve any other skill. Use when the user wants an observer running alongside active work so corrections are normalized into a reviewable log file that can later be used to update the relevant skill or agent instructions.
---

# Session Skill Observer

## Overview

Use this skill as a session-wide observer that listens for reusable feedback while other work is happening. Convert the user's corrections into concise candidate rules, store them in a shared log, and avoid interfering with the main task flow.

## Core Model

Treat this skill as a companion workflow, not as a true background daemon. It stays active for the current session and applies a short observer pass whenever the user gives feedback that may generalize into a rule.

Observer scope:
- Cover all active skills in the current session, not only one target skill.
- Support mixed sessions where the user switches between domains or skills.
- Keep a shared observer log unless the user explicitly asks for separate logs.

Default log path:
- Use `skill-observer-log.md` inside this skill's own folder unless the user specifies another path.
- Resolve the default path relative to the installed `SKILL.md` file for this skill.
- Create the file if it does not exist.

Archive layout:
- Treat `skill-observer-log.md` as the active log for new entries.
- Store older snapshots in an `archives/` folder inside this skill's folder.
- Use archive filenames like `skill-observer-log-YYYY-MM-DD.md`.
- Do not append new entries to archived log files.

## Activation

Enable observer mode when the user asks to:
- watch for mistakes during work
- save corrections as reusable rules
- learn from comments or feedback
- collect guidance for later skill updates

When observer mode is enabled:
- Keep observing until the user disables it, the session ends, or the user switches to a clearly unrelated mode.
- Continue observing even when another skill becomes the main worker.

If observer mode was not requested:
- Do not create or update the log file.
- Do not create ad hoc log files in the active workspace.

## What To Capture

Capture feedback that is reusable beyond the current single step. Good candidates include:
- explicit corrections about how a skill should behave
- workflow preferences such as preferred order of operations
- style or naming conventions that should repeat later
- domain constraints such as "do not use this Unity workflow"
- quality checks the user expects before proposing changes
- repeated friction points that reveal a weakness in a skill

Strong capture signals:
- "in future"
- "always"
- "never"
- "don't do it this way"
- "prefer X over Y"
- "this should be checked first"
- "for this skill, use ..."

Do not capture:
- one-off facts that matter only to the current task
- temporary instructions with no reuse value
- emotional reactions without an actionable rule
- duplicate entries unless the new message raises confidence or clarifies scope
- ordinary implementation or design revisions that do not indicate incorrect or problematic skill behavior

## Observer Pass

Run this pass after any user message that looks like a correction, preference, or process feedback.

1. Identify whether the user feedback is generalizable.
2. Infer the affected skill or domain from the surrounding context.
3. Normalize the feedback into one short rule sentence in imperative form.
4. Classify the rule.
5. Append a structured entry to the shared observer log.
6. Continue the main task without turning the observer into the center of the conversation.

If the feedback is ambiguous:
- Prefer logging a low-confidence candidate rather than inventing a strong rule.
- Mark the uncertainty explicitly in the log entry.

## Rule Classification

Use one of these categories:
- `behavior-rule`: how the skill should act
- `workflow-rule`: preferred order or process
- `style-rule`: naming, structure, formatting, or presentation preference
- `constraint`: forbidden or required limitations
- `quality-check`: validation the skill should perform before responding
- `bug-pattern`: recurring failure or mistake to avoid

Use a confidence label:
- `high`: the user stated the rule directly or repeated it
- `medium`: the rule is strongly implied by a correction
- `low`: the correction may generalize, but scope is still uncertain

## Log Format

Append entries to the default log file in this skill's own folder using this format:

```md
## <YYYY-MM-DD HH:MM>

- Source skill: <active skill or "general session">
- Target skill: <skill that should eventually learn this rule, if known>
- Category: <behavior-rule | workflow-rule | style-rule | constraint | quality-check | bug-pattern>
- Confidence: <high | medium | low>
- Trigger: <short paraphrase of the user's correction>
- Rule: <normalized reusable rule>
- Context: <short note about where the issue appeared>
- Status: candidate
```

Logging rules:
- Append, do not rewrite older entries unless the user asks for cleanup.
- Keep `Trigger`, `Rule`, and `Context` concise.
- Prefer paraphrase over long quotation.
- Avoid more than one rule per entry unless the user clearly stated a compact grouped rule.
- Default to the skill-folder log even when other skills are active in the session.
- Write new entries only to the active `skill-observer-log.md` file unless the user explicitly requests a different destination.
- Keep archived logs unchanged after they are archived.
- Do not promote, rewrite, or relocate archived entries unless the user explicitly asks for archival maintenance or historical review.

## Non-Interference Rules

Do not let the observer take over the main task.

Rules:
- Do not stop the main workflow to discuss every captured rule.
- Do not argue with the user's correction while logging it.
- Do not modify the target skill automatically unless the user explicitly asks.
- Do not log speculative criticism that did not come from the user or from a clear repeated failure.
- Do not create excessive chatter about the observer unless the user asks to review the log.

If the user asks to review the collected rules:
- Summarize the highest-value entries first.
- Group by target skill when possible.
- Point out duplicates or conflicts before proposing a skill update.
- Read the active log by default.
- Read archived logs only when the user explicitly asks for historical review or when the active log does not contain the needed context.
- Do not read files in `archives/` unless the user explicitly asks for archived or historical entries.

## Promotion Workflow

When the user later wants to update a skill from the log:

1. Read `skill-observer-log.md`.
2. Filter for entries targeting the relevant skill or domain.
3. Merge duplicates and resolve conflicts.
4. Convert stable candidates into concise skill instructions.
5. Keep uncertain items out of the final skill until the user approves them.

Promotion boundary:
- Do not update any target skill from observer entries unless the user explicitly asks to apply or promote rules.
- When asked to update skills from the log, use only the active log unless the user explicitly includes archived logs in scope.

When the user later wants to archive the current log:

1. Create `archives/` inside this skill folder if it does not exist.
2. Move the current `skill-observer-log.md` to `archives/skill-observer-log-YYYY-MM-DD.md`.
3. Create a fresh `skill-observer-log.md` with the standard header.
4. Continue writing new entries only to the fresh active log.

## Example Triggers

Examples of feedback that should usually be logged:
- "Don't suggest manual UXML edits before considering UI Builder."
- "Check whether this naming matches the existing convention first."
- "For issue generation, keep acceptance criteria concrete and testable."
- "If I correct the workflow order, remember that as a process rule."

Examples that should usually not be logged:
- "Rename this one button to PlayButton."
- "Use 12 px here for this specific panel."
- "No, I meant the other file."
