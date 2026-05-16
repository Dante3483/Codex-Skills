---
name: git-commit-assistant
description: Create local git commits from staged changes using a proposal-and-approval flow with the user's preferred commit format. Use when Codex should review staged changes, suggest a commit message, revise it with feedback, and create a local commit without pushing.
---

## RULES

### Workflow

RULE: Follow `GO TO STEP N` exactly.
RULE: STOP immediately when a STEP says `STOP`.

### Commit Rules

RULE: Base the commit message on the logical purpose of the changes, not on arbitrary guesses.
RULE: Write the final commit title and body in English unless the user explicitly requests another language.
RULE: Use a short, action-oriented title without a period.
RULE: Prefer the purpose of the change over a file list.
RULE: Keep the commit body concise and include only important points.
RULE: Group related changes logically instead of describing every edited file or detail.
RULE: Keep each commit body bullet to no more than 2 sentences.
RULE: Do not mention Unity service files such as `.meta` files unless they are the actual subject of the change.
RULE: Do not use Conventional Commit prefixes like `feat:`, `fix:`, `chore:`, or `refactor:` unless requested.
RULE: For non-trivial commits, use 2-5 meaningful bullet points.
RULE: Allow more than 4 commit body bullets when useful, but avoid saturation and duplication and keep each bullet focused on one distinct theme.
RULE: Omit the body for tiny obvious commits.
RULE: Omit non-essential information and mention config, package, generated, asset, or project setting changes only when they materially affect the change.
RULE: Inspect the full git status before building a commit proposal.
RULE: Treat added, modified, deleted, and renamed files as equally important sources for the commit proposal.
RULE: Do not build the proposal around added files only.
RULE: If a file was deleted or renamed as part of the change, represent that explicitly in the proposal.
RULE: Include all approved file operations in the staged set before creating the commit.
RULE: Verify that the staged set matches the approved proposal before creating the commit.

### Safety Rules

RULE: Do not push to remote under any condition.
RULE: Do not silently omit modified, deleted, or renamed files that belong to the approved change.
RULE: Do not include unrelated files in the commit just to make the staging step easier.

## COMMIT PROPOSAL

````md
Current branch: <branch-name>

Commit message:
<Short English title>

- <Meaningful change>.
- <Meaningful change>.

Files to add:
- path/to/file

Files to modify:
- path/to/file

Files to delete:
- path/to/file

Files to rename:
- old/path -> new/path

Excluded files:
- path/to/other-file
````

## STEP 1: CHECK FOR UNCOMMITTED CHANGES

IF there are uncommitted changes:
  DO: Store the uncommitted changes summary as `uncommitted_changes`.
  DO: GO TO STEP 2.

DO: Output exactly `No uncommitted changes found. Nothing to commit.`
DO: STOP.

## STEP 2: PREPARE COMMIT PROPOSAL DATA

DO: Store the current branch name as `branch_name`.
DO: Generate the commit proposal using `uncommitted_changes` and store it as `commit_proposal`.
DO: GO TO STEP 3.

## STEP 3: REVIEW COMMIT PROPOSAL

DO: Output exactly in this format:

````md
<commit_proposal> in the `COMMIT PROPOSAL` format
---
Do you approve this commit proposal?
1. Approve - continue.
2. Reject - stop this workflow.

Write comments to revise the commit proposal.
````

WAIT: User replies with `1`, `2`, or comments.

IF user replied `1`:
  DO: GO TO STEP 4.

IF user replied `2`:
  DO: Output exactly `Skill creation stopped.`
  DO: STOP.

DO: Update `commit_proposal` using the revision comments.
DO: GO TO STEP 3.

## STEP 4: CREATE LOCAL COMMIT

DO: Generate `final_commit_message` from `commit_proposal`.
DO: Ensure the staged files match the approved proposal, including approved additions, modifications, deletions, and renames.
DO: Use `final_commit_message` to create the local commit.

IF the local commit was not created successfully:
  DO: Output exactly `Failed to create local commit.`
  DO: STOP.

DO: Output exactly in this format:

````md
Committed:
<hash> <subject>
````

DO: STOP.
