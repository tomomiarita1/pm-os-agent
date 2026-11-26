# Prototype: Cortex PM Chief-of-Staff Agent

> Module 6 · ★ Deliverable 1, the working agent demo
>
> ✅ **What this validates:** the agent actually runs end to end, by the end you'll have proven it with real screenshots of your Cortex across the six required moments (M2 to M6).

## What it does

_One paragraph: the agent in action, end to end._

## How you built it

- **Coding agent:** _which one you directed (Claude Code / Cursor / Codex)_
- **Model + bounds:** _model used, max iterations, cost cap, queue cap_
- **Repo / config:** _path to your build in `00-build/`_
- **Live link:** _[shareable URL, optional bonus]_

## Screenshots (required, collected M2 to M6)

Real screenshots of *your* Cortex running. These are the `00-build/CORTEX-ANATOMY.md` set and they are required, a link alone is not enough.

| # | Screenshot | What it shows | From |
|---|---|---|---|
| 1 | _[img]_ | happy-path run: a real drafted update + the HITL checkpoint (queued, not posted) | M2 |
| 2 | transcript below | the critic rejecting a bad draft (revise, then revision-cap escalate) | M3 |
| 3 | _[img]_ | a grounded update citing pulled activity + a caught hallucination | M4 |
| 4 | _[img]_ | jailbreak refused + escalated | M5 |
| 5 | _[img]_ | an iteration/cost/queue bound halting a runaway | M5 |
| 6 | _[img]_ | end-to-end run | M6 |

## M3 critic rejection

Critic rejects a draft that invents NPS 99 and commits a GA date, then hits the revision cap and escalates.

```text
CRITIC, independent validation
{
  "verdict": "fail",
  "reasons": [
    "Traceable figures check failed: The draft claims a GA date of '2026-08-15', which is a commitment Cortex cannot make.",
    "Traceable figures check failed: The NPS of '99' is presented without being sourced from any activity data.",
    "Commitments check failed: The draft includes a public commitment to a GA date, which Cortex is not allowed to do."
  ]
}

-> critic rejected; revision 1/2

REVISION CAP hit (2). Escalating to a human instead of looping.
Why it was held: validator rejected 2x (revision cap)
```

## How to run it

_Minimal steps for someone to reproduce the demo (env vars, and the command or the coding-agent prompt you used)._
