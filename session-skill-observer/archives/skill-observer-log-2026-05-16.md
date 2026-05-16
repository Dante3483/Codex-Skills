# Skill Observer Log

Use this file to collect reusable rules captured by `session-skill-observer`.

## 2026-05-14 17:37

- Source skill: general session
- Target skill: general session
- Category: style-rule
- Confidence: high
- Trigger: User asked for very short, to-the-point replies and objected to drifting out of context.
- Rule: Keep responses minimal and stay tightly within the exact current subtask.
- Context: Ongoing Unity editor and UXML styling discussion.
- Status: candidate

## 2026-05-14 17:37

- Source skill: general session
- Target skill: general session
- Category: workflow-rule
- Confidence: high
- Trigger: User asked to move slowly, step by step, and only do what was explicitly requested.
- Rule: Follow the user's requested sequence strictly and do not jump ahead with extra work.
- Context: Reset of the SerializableDictionary styling workflow.
- Status: candidate

## 2026-05-14 17:37

- Source skill: unity-ui-builder-helper
- Target skill: unity-ui-builder-helper
- Category: style-rule
- Confidence: high
- Trigger: User said project UI should match their existing editor-window style exactly rather than introducing a new visual language.
- Rule: Match the project's existing Unity editor UI style closely instead of inventing a more decorative variant.
- Context: SerializableDictionary preview styling.
- Status: candidate

## 2026-05-14 17:37

- Source skill: unity-ui-builder-helper
- Target skill: unity-ui-builder-helper
- Category: workflow-rule
- Confidence: high
- Trigger: User said they would create the UXML while the assistant should only create the USS.
- Rule: When the user explicitly splits responsibilities, respect that ownership split and edit only the assigned asset type.
- Context: Preview-only UXML and USS iteration.
- Status: candidate

## 2026-05-15 19:50

- Source skill: git-commit-assistant
- Target skill: git-commit-assistant
- Category: formatting-rule
- Confidence: high
- Trigger: User said commit bodies may contain more than 4 bullets as long as they are not oversaturated, non-duplicative, and each bullet covers one distinct theme.
- Rule: Allow commit bodies to use more than 4 bullets when useful, but avoid saturation and duplication and keep each bullet focused on one distinct theme.
- Context: Commit proposal revision for SerializableDictionaryDrawer changes.
- Status: candidate

## 2026-05-15 19:55

- Source skill: issue-task-assistant
- Target skill: issue-task-assistant
- Category: formatting-rule
- Confidence: high
- Trigger: User said the second output block should not include a `Title:` line.
- Rule: When outputting the final issue in the second markdown block, omit the `Title:` line and keep only the issue body sections.
- Context: Tuple PropertyDrawer issue drafting.
- Status: candidate

## 2026-05-15 19:55

- Source skill: realtime-task-collaborator
- Target skill: general session
- Category: workflow-rule
- Confidence: high
- Trigger: User asked not to write all code immediately and to discuss before applying changes.
- Rule: Discuss the solution first, then apply code changes incrementally in small patches instead of one large implementation.
- Context: Tuple drawer task workflow preference for implementation pacing.
- Status: candidate

## 2026-05-15 20:11

- Source skill: general session
- Target skill: general session
- Category: style-rule
- Confidence: high
- Trigger: User asked for shorter, more concise, only essential answers and confirmed it as a new rule.
- Rule: Respond briefly, concisely, and only with the essential points unless the user asks for more depth.
- Context: Realtime tuple drawer discussion response style.
- Status: candidate
