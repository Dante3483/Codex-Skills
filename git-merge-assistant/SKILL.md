---
name: git-merge-assistant
description: Merge one local git branch into another using a short approval flow and the user's preferred merge commit message patterns. Use when Codex should ask which branch to merge into which branch, then perform the merge with the correct message template.
---

## RULES

### Workflow

RULE: Follow `GO TO STEP N` exactly.
RULE: STOP immediately when a STEP says `STOP`.

### Merge Rules

RULE: Use `--no-ff` for the merge unless the user explicitly requests a different mode.

### Safety Rules

RULE: Do not push to remote under any condition.
RULE: Do not merge with uncommitted changes in the working tree.

## VARIABLES

- `source_branch`
- `target_branch`
- `merge_commit_message`

## STEP 1: ASK WHICH BRANCH SHOULD BE MERGED

DO: Output exactly `Which branch should be merged?`

WAIT: User provides the source branch.

DO: Store the reply as `source_branch`.
DO: GO TO STEP 2.

## STEP 2: ASK WHICH BRANCH IT SHOULD BE MERGED INTO

DO: Output exactly `Into which branch should it be merged?`

WAIT: User provides the target branch.

DO: Store the reply as `target_branch`.
DO: GO TO STEP 3.

## STEP 3: PERFORM THE MERGE

IF `source_branch` is `main`:
  DO: Store `Merge branch 'main' into <target_branch>` as `merge_commit_message`.

IF `source_branch` is not `main`:
  DO: Store `Merge branch '<source_branch>'` as `merge_commit_message`.

DO: Checkout `target_branch`.
DO: Merge `source_branch` into `target_branch` with `--no-ff` and `merge_commit_message`.

IF the merge was not created successfully:
  DO: Output exactly `Failed to create merge commit.`
  DO: STOP.

DO: Output exactly in this format:

```text
Merged:
<source_branch> -> <target_branch>
<merge_commit_message>
```

DO: STOP.
