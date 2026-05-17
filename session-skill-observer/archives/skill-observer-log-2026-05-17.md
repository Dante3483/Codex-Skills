# Skill Observer Log

Use this file to collect reusable rules captured by `session-skill-observer`.

## 2026-05-16 15:38

- Source skill: issue-task-assistant
- Target skill: issue-task-assistant
- Category: style-rule
- Confidence: high
- Trigger: User said all issues should be written in English while direct communication can stay in Russian.
- Rule: Write issue titles and bodies in English by default, even when the working conversation with the user is in Russian.
- Context: Issue drafting workflow for a new ManagedObjectField refactor task.
- Status: candidate

## 2026-05-16 15:41

- Source skill: issue-task-assistant
- Target skill: issue-task-assistant
- Category: workflow-rule
- Confidence: high
- Trigger: User asked for tasks to read more simply and behave like ordered steps from the starting point to the final result.
- Rule: Write implementation tasks as a clear step-by-step sequence that guides the work from the current state to the finished result.
- Context: Revision of a refactor issue for the ManagedObjectField editor workflow.
- Status: candidate

## 2026-05-16 15:47

- Source skill: realtime-task-collaborator
- Target skill: realtime-task-collaborator
- Category: workflow-rule
- Confidence: high
- Trigger: User asked for algorithm-like task breakdowns with moderate detail and optional numbered substeps such as 1.1 and 1.2.
- Rule: When breaking down work, present it as a flexible step-by-step algorithm with practical substeps, enough detail to execute manually without overcommitting to exact implementation details.
- Context: Collaborative refinement of the ManagedObjectField refactor task format.
- Status: candidate

## 2026-05-16 15:50

- Source skill: general session
- Target skill: general session
- Category: style-rule
- Confidence: high
- Trigger: User asked to continue communication in Russian when the conversation is started in Russian.
- Rule: If the user is communicating in Russian, continue the collaboration dialogue in Russian unless they ask to switch languages.
- Context: Ongoing collaborative task breakdown for the ManagedObjectField refactor.
- Status: candidate

## 2026-05-16 16:14

- Source skill: realtime-task-collaborator
- Target skill: realtime-task-collaborator
- Category: workflow-rule
- Confidence: high
- Trigger: User clarified that this behavior belongs to the specific agent, not to the general session.
- Rule: After the discussion phase, transition into design and coding work by showing the proposed code in chat first, and apply file changes only after the user explicitly approves.
- Context: Refinement of how realtime-task-collaborator should behave after collaborative planning.
- Status: candidate

## 2026-05-16 16:17

- Source skill: realtime-task-collaborator
- Target skill: realtime-task-collaborator
- Category: workflow-rule
- Confidence: high
- Trigger: User said the agent should not dump the whole implementation at once and should stay focused on the current planned step only.
- Rule: In step-by-step implementation mode, discuss and propose code only for the current task step, without jumping ahead to later parts of the workflow.
- Context: ManagedObjectField implementation discussion after planning the PropertyDrawer-based refactor.
- Status: candidate

## 2026-05-16 16:18

- Source skill: realtime-task-collaborator
- Target skill: realtime-task-collaborator
- Category: style-rule
- Confidence: high
- Trigger: User asked that suggestions like this should be presented as a numbered list and end with a direct recommendation.
- Rule: When presenting comparable options in collaborative implementation mode, use a numbered list and finish with a concise recommendation in the form "I would choose ...".
- Context: Naming discussion for the managed reference attribute.
- Status: candidate

## 2026-05-16 16:32

- Source skill: realtime-task-collaborator
- Target skill: realtime-task-collaborator
- Category: style-rule
- Confidence: high
- Trigger: User asked not to rewrite the whole code during discussion and to show only the context that actually changes.
- Rule: During implementation discussion, show only the relevant changed fragment instead of rewriting the whole file unless broader context is necessary.
- Context: Ongoing step-by-step implementation of SerializeManagedField drawer behavior.
- Status: candidate

## 2026-05-16 19:57

- Source skill: realtime-task-collaborator
- Target skill: realtime-task-collaborator
- Category: style-rule
- Confidence: high
- Trigger: User said it is better to invert conditionals where possible.
- Rule: Prefer inverted conditionals with early returns when they improve readability and keep the main path flatter.
- Context: Step-by-step implementation of the SerializableManagedReference drawer.
- Status: candidate

## 2026-05-16 21:06

- Source skill: unity-ui-builder-helper
- Target skill: unity-ui-builder-helper
- Category: naming-rule
- Confidence: high
- Trigger: User said not to repeat the full component name in UXML and USS selectors and to use clean role-based names like root and header.
- Rule: For local UXML and USS structure, prefer clean role-based names such as root, header, body, title, and select-button instead of repeating the full component prefix on every element.
- Context: Layout planning for a managed reference drawer UI in Unity UI Toolkit.
- Status: candidate

## 2026-05-16 21:16

- Source skill: session-skill-observer
- Target skill: session-skill-observer
- Category: workflow-rule
- Confidence: high
- Trigger: User clarified that normal design revisions should not be logged as observer issues and only incorrect skill behavior should be captured that way.
- Rule: Log feedback as observer-rule corrections only when it points to incorrect or problematic skill behavior, not when the user is simply revising the implementation or design.
- Context: UI layout revision during the ManagedReferenceField2 workflow.
- Status: candidate
