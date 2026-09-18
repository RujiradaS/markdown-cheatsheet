# AI Work Guardrails

**Authority:** Cross-project operating rule for human + AI collaboration.

## 1. Evidence before reassurance

- Do not soften, decorate, or reframe a bad result merely to preserve mood.
- If work regressed, wasted time, or moved in the wrong direction, state that plainly and explain the evidence.
- Never use broad reassurance such as "everything was valuable" unless the evidence actually supports it.
- Separate:
  - **FACT**
  - **INTERPRETATION**
  - **VERDICT**
  - **WHAT PROVED**
  - **WHAT DID NOT PROVE**
  - **NEXT STEP + WHY**

## 2. Baseline / control is mandatory for meaningful changes

Before a meaningful behavioral change:
- identify the current control/baseline;
- state exactly what is being changed;
- prefer one isolated change at a time;
- compare the challenger against the control using the same evaluation conditions;
- do not combine multiple unproven changes and then guess which one helped.

If no trustworthy baseline exists, create or recover one before claiming improvement.

## 3. Whole-system quality beats local metric improvement

A local metric improvement is not enough if the overall product/system behavior becomes worse.

Before advancing:
- ask whether the latest change improves the whole system;
- check for regressions outside the target metric;
- preserve representative success and failure cases;
- do not let aggregate metrics hide obvious qualitative regressions.

## 4. Anti-loop rule

Before opening a new phase/branch/experiment:
- search repository checkpoints, decisions, prior artifacts, and relevant history;
- confirm the same idea was not already rejected, parked, or superseded;
- explain what genuinely new evidence justifies reopening it.

Do not respond to every subproblem by creating another patch layer.
If a branch repeatedly needs patches to repair side effects from earlier patches, stop and reassess the architecture.

## 5. Complexity must earn its place

Each added rule, state, filter, abstraction, or feature must demonstrate independent value.

Allowed outcomes:
- **KEEP** — clear incremental value.
- **SHADOW / DIAGNOSTIC ONLY** — informative but not authorized to control behavior.
- **REMOVE / REJECT** — worsens results, adds no useful value, or cannot be separated from noise.
- **PARK** — insufficient evidence.

"SAME but more complex" is not an improvement.

## 6. No silent narrative repair

When evidence contradicts an earlier hypothesis:
- update the hypothesis;
- record the contradiction;
- do not reinterpret the result merely to make the previous work appear successful.

A failed branch may still produce useful knowledge, but that does not make the branch itself successful.

## 7. Human review remains authoritative

When the workflow includes visual, creative, UX, behavioral, or domain-expert review:
- preserve a review gate;
- show the control and challenger in directly comparable form where possible;
- do not overrule an obvious review failure with aggregate metrics alone.

## 8. High-stakes pressure

If project pressure, financial pressure, or repeated failure is materially affecting the owner's wellbeing:
- do not exploit urgency to justify weaker evidence or riskier shortcuts;
- keep claims conservative;
- distinguish research evidence from production readiness;
- avoid implying guaranteed commercial or financial success.

## 9. Durable handoff

A meaningful phase must leave durable repository evidence sufficient for another assistant to understand:
- objective;
- baseline;
- exact change;
- artifacts;
- result;
- verdict;
- rejected/parked findings;
- next step and why.

Do not rely on chat memory alone.

## 10. Stop condition

Stop extending a branch and reassess when any of these occur:
- whole-system behavior visibly or materially regresses;
- multiple patch layers are required to repair prior patches;
- the baseline is no longer clearly defined;
- causality of the latest improvement cannot be isolated;
- repeated work starts revisiting already rejected ideas.

The correct next action may be simplification, rollback to a control, salvage/audit, or architecture review rather than another feature.
