# Agent Line Map: Cortex PM Chief-of-Staff Agent

> Module 1 · The Agent Line
>
> ✅ **What this validates:** every risky action has a clear owner, by the end you'll have proven an above/below-the-line map with HITL checkpoints, scored on reversibility, blast radius, and measurability.

## The workflow, decision by decision

List every discrete decision or action in your agent's workflow, then score each one and place it **above** the line (a human owns it) or **below** (the agent owns it). Borderline calls get an HITL checkpoint.

| Decision / action | Reversibility (H/M/L) | Blast radius (H/M/L) | Measurability (H/M/L) | Above / Below | HITL? |
|---|---|---|---|---|---|
| Pull project state and activity | H | L | H | Below | no |
| Decide relevant context | M | L | H | Below | yes |
| Draft the update | H | L | H | Below | no |
| Decide tone and commitment level | L | H | M | Above | no |
| Flag at-risk or escalation | H | L | H | Below | no |
| Choose what to escalate | M | H | M | Above | no |
| Propose a story batch (capped) | H | M | M | Below | yes |
| Post an update, or approve a company-wide one | L | H | L | Above | no |

## Agent anatomy (sketch)

- **Model:** gpt-4o-mini for every run. No frontier model, because no action requires one. Above-the-line calls stay with the human.
- **Tools:** project state, activity, past updates, roadmap, team norms, and a capped story proposal. No post, create, or merge. Nothing critical is auto-published to a wide audience.
- **Memory:** roadmap, decision log, and team norms persist. Drafts and flags do not, because they have no use in a later run.
- **Loop:** _placeholder, defined in M2 loop-spec.md_
- **Bounds:** _placeholder, defined in M5 bounds-and-evals.md_
- **Evals:** _placeholder, defined in M5 bounds-and-evals.md_

## The golden rule, applied

- Pull project state and activity sits below the line because the three scores contribute a clean signal for agent automation, deciding factor: all three.
- Decide relevant context sits below the line with HITL because it is not exposed to a wide audience and is used by a small group, deciding factor: blast radius.
- Draft the update sits below the line because it is easy to replace, the blast is low, and the claims are checkable, deciding factor: all three.
- Decide tone and commitment level sits above the line because this result cannot be reversed, deciding factor: reversibility.
- Flag at-risk or escalation sits below the line because the flag can be ignored, it is not exposed to a wide audience, and you can tell how effective it is, deciding factor: all three.
- Choose what to escalate sits above the line because the escalation is blasted to a wide audience, deciding factor: blast radius.
- Propose a story batch (capped) sits below the line with HITL because you want to review it before the story is committed as a ticket, deciding factor: blast radius.
- Post an update, or approve a company-wide one, sits above the line because it is exposed to a company-wide audience, deciding factor: blast radius.

## Hardest call

Choose what to escalate. I went back and forth on above versus below because the action was vague. Blast radius settled it above the line, because it is hard to identify the size of the audience from the sentence.
