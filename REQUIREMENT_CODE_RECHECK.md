# Requirement-to-Code Recheck Guard

**Effective:** 2026-09-21  
**Owner intent:** Every implementation must be rechecked against Jin's actual request using the real code, not only plans, checkpoints, comments, tests, or assistant memory.

## Mandatory rule
Before saying a task/version is DONE, PASS, READY, MERGEABLE, or asking Jin to review the result, re-open the implemented code and produce a **Requirement -> Code Recheck**.

Do not assume a requested behavior was implemented because it appears in a design/checkpoint, a related test passes, telemetry/UI labels exist, a helper was added, or only part of the flow changed. Trace the actual runtime/UI/data path to the behavior Jin asked for.

## Required recheck matrix
For every user requirement, list one row:

| Req | Jin asked for | Priority / impact | Exact code evidence | Runtime / UI path | Verification evidence | Status | Gap / retained legacy |
| --- | --- | --- | --- | --- | --- | --- | --- |
| REQ-01 | ... | P0/P1/... | file + class/function/route/component | entrypoint -> decision -> effect | test/log/visual/manual | IMPLEMENTED / PARTIAL / NOT IMPLEMENTED | ... |

Code evidence must include exact file path, function/class/route/component/command, important call path/state transition, and where the requested side effect actually happens.

For behavior work, trace from the real entrypoint to the visible/runtime effect:
- backend: route -> service -> persistence/action;
- frontend: route/page -> state -> component -> visible behavior;
- trading: OnTick/signal -> state/decision -> order/hold/exit;
- automation/data: command -> parser/logic -> output/action.

## Mandatory NOT CHANGED section
Explicitly list important inherited/legacy behavior that remains and could override, bypass, or limit the new implementation.

A partial implementation must never be described as complete because the new code exists somewhere in the repository.

## Status discipline
- **IMPLEMENTED** = actual code path supports the full requested behavior.
- **PARTIAL** = some code exists, but one or more requested paths/states/cases are not wired through.
- **NOT IMPLEMENTED** = request is design-only, shadow-only, telemetry-only, TODO, or absent.

If even one major requested item is PARTIAL / NOT IMPLEMENTED, say so before Jin runs or reviews it.

## Major-impact check
Before spending time on small downstream fixes, recheck whether a larger unresolved requirement can invalidate them. When a major/root behavior is still missing, do not let easy isolated subtasks silently become the roadmap priority.

## Visual/manual expectation
If Jin asked for a user-visible or runtime behavior change, state before review:
1. what should visibly change;
2. where to look;
3. what unchanged behavior means FAIL.

If the result is visually/runtime indistinguishable where a major change was expected, re-open the code path before adding another small filter/fix.

## Handoff format
Every implementation handoff should contain, compactly:
1. **REQUEST**
2. **CODE MATCH**
3. **NOT CHANGED**
4. **VERIFY**
5. **GAPS**
6. **VERDICT**

## Anti-drift rule
Re-run this code-match check after implementation, after any runtime/visual FAIL, before merge/release/handoff, and before starting the next phase if the current request may still be incomplete.

Jin should not need to reverse-engineer the repository to find out whether the requested work was really implemented.
