# Requirement-to-Code Recheck Guard

**Effective:** 2026-09-21  
**Scope:** All POS-managed repositories and all implementation work  
**Owner intent:** The owner should be able to trust that what was requested is what the code actually does, without reverse-engineering the repository.

## Non-negotiable rule

Before saying a task/version is **DONE, PASS, READY, MERGEABLE, COMPLETE**, or asking the owner to spend time compiling/running/reviewing it:

1. Re-open the implemented code.
2. Re-read the owner's current request and acceptance criteria.
3. Trace the real runtime/UI/data path from entrypoint to visible effect.
4. Produce a **Requirement -> Code Recheck**.
5. Disclose every major requested item that is still partial, shadow-only, telemetry-only, design-only, bypassed by legacy behavior, or not wired end-to-end.

A design document, checkpoint, test, helper function, telemetry label, UI label, or partial branch does **not** prove the requested behavior is implemented.

## Priority rule — major impact first

When a large unresolved requirement can invalidate downstream work, it is the active blocker.

Priority order:

**root-cause impact > user-visible/runtime behavior > causal isolation/ease of testing > local metric improvement**

Do not spend a chain of tasks tuning small filters, thresholds, management details, cosmetics, or downstream branches while a major upstream behavior is still missing.

Before every new implementation task ask:

1. What is the largest unresolved behavior that can invalidate later work?
2. Does this task directly reduce that blocker?
3. If not, why is it safe to work downstream?
4. What should visibly/runtime-wise improve if this task succeeds?

If the answer to #4 is effectively "nothing the owner can notice yet" while a major blocker remains open, the task is not the current priority.

## Required Requirement -> Code Recheck matrix

For every user requirement, list one row:

| Req | Owner asked for | Priority / impact | Exact code evidence | Runtime / UI / data path | Verification evidence | Status | Gap / retained legacy |
| --- | --- | --- | --- | --- | --- | --- | --- |
| REQ-01 | ... | P0/P1/... | file + function/class/route/component | entrypoint -> decision -> effect | test/log/visual/manual | IMPLEMENTED / PARTIAL / NOT IMPLEMENTED | ... |

### Exact code evidence means

Include the concrete:
- file path;
- function / class / route / component / command;
- call path or state transition;
- point where the requested side effect actually occurs.

Examples:
- backend: route -> service -> persistence/action;
- frontend: route/page -> state -> component -> visible behavior;
- trading: OnTick/signal -> state/decision -> order/hold/exit;
- automation/data: command -> parser/logic -> output/action.

## Mandatory pre-run implementation card

Before asking the owner to compile, run, visually inspect, or manually test a new implementation, publish:

### BEFORE
What the current executable behavior actually does.

### CHANGE
What exact executable behavior changed in this version, including the functions/routes/components whose authority changed.

### NOT CHANGED
Important inherited or legacy behavior that still remains and could override, bypass, or limit the new implementation.

### RUNTIME AUTHORITY MAP
For each major lifecycle state relevant to the request, show who owns the decision.

Example:

| Lifecycle / state | Current authority | New logic active? | Legacy bypass? |
| --- | --- | --- | --- |
| Initial / flat | ... | YES/NO | ... |
| Pending / loading / candidate | ... | YES/NO | ... |
| Active / in-position / editing | ... | YES/NO | ... |
| Success / destination / submit | ... | YES/NO | ... |
| Degrade / error / contraction | ... | YES/NO | ... |
| Retry / continuation | ... | YES/NO | ... |
| Handoff / redirect / opposite action | ... | YES/NO | ... |

Adapt the states to the project, but do not omit a path-critical state.

### VISIBLE EXPECTATION
What the owner should be able to see or verify, and where.

### FAIL IF UNCHANGED
What unchanged behavior means the implementation did not reach the real execution path.

### EVIDENCE OUTPUT
Tests, logs, traces, screenshots, CSVs, DB rows, API responses, or other evidence that proves the path.

## Status discipline

- **IMPLEMENTED** — the actual end-to-end code path supports the full requested behavior.
- **PARTIAL** — some code exists, but one or more requested paths/states/cases are not wired through.
- **NOT IMPLEMENTED** — design-only, shadow-only, telemetry-only, TODO, absent, or bypassed.

If any **major** requested item is PARTIAL or NOT IMPLEMENTED, say so before runtime/manual review.

Never describe a partial path as "full", "integrated", "all", "complete", or equivalent.

## Mandatory NOT CHANGED audit

Every implementation handoff must explicitly list important behavior retained from the previous version.

This is especially important when legacy behavior can dominate the result.

Examples:
- old management still owns exits;
- old redirect still runs after refresh;
- old persistence layer still overwrites state;
- old provider/transport is still mocked;
- old signal path still creates the action despite a new observer.

## Visual / manual behavior gate

If the owner asked for a visible or runtime behavior change:

1. Name the exact place/time/screen/state to inspect.
2. State what should be different before review.
3. If the result looks materially unchanged where a major change was expected, stop.
4. Re-open the code path and recheck authority/bypass before adding another filter or small fix.

A metric PASS with a user-visible/runtime regression is not a valid PASS when visible behavior is part of the requirement.

## Anti-drift review cadence

Re-run the code-match check:

- before every new executable/behavior-changing task;
- immediately after implementation;
- after every runtime/visual/manual verdict;
- before opening a new phase/version;
- before merge/release/handoff;
- immediately when the owner says the result feels unchanged, disconnected, or below target.

Do not chain many small subtasks without returning to the active major blocker and acceptance criteria.

## Handoff format

Every implementation handoff should contain, compactly:

1. **REQUEST** — what the owner asked for.
2. **CODE MATCH** — exact files/functions/routes/components implementing each request.
3. **NOT CHANGED** — inherited behavior still active.
4. **AUTHORITY MAP** — who owns each relevant runtime/UI/data state.
5. **VERIFY** — tests/logs/visual/manual evidence.
6. **GAPS** — partial or not implemented items.
7. **VERDICT** — whether the request is actually satisfied.
8. **NEXT** — only after rechecking the major blocker.

## User review burden

The owner should normally review:
- the intended behavior;
- the visible/manual result;
- concise evidence.

The owner should **not** need to read source code line-by-line to discover whether the assistant actually implemented the request.

If the owner must reverse-engineer the repository to understand what changed, the implementation handoff is incomplete.

## Anti-claim rule

Do not claim a behavior is implemented based only on:
- comments;
- docs;
- tests that do not execute the real path;
- telemetry labels;
- shadow states;
- mock/stub paths;
- helper functions not called by the actual entrypoint.

Trace the real path first.
