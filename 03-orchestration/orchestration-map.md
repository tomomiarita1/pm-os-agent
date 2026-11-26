# Orchestration Map: Cortex PM Chief-of-Staff Agent

> Module 3 · Orchestration & Subagents, ★ Deliverable 3
>
> ✅ **What this validates:** nothing advances unchecked, by the end you'll have proven a justified topology, a roster, and a validator with a defined fail action.
>
> Builds on your M2 Loop Spec. Only split one agent into a team when there's a real reason, coordination has a cost.

## 1. Why split? (or why not)

Cortex stays the drafter and gains one critic. We split only because the draft needs an independent validator.

- **Separation of concerns: no.** The contexts of the source of the draft and the activities would not conflict within the agent.
- **Parallelism: no.** We don't expect a time lag on the agent, so we don't need parallelism.
- **Independent validator: yes.** The draft should be reviewed by a separate agent without the bias of the draft created by the main agent. The main agent has its bias because it has all the source data and the LLM. The subagent will not share these and will simply judge the draft updates.
- **Context-window pressure: no.** We expect a Cortex run can hold all the activities.

No research agent and no parallel readers. Cortex still pulls state and activity and drafts. A separate critic grades the draft before it reaches the PM.

The critic does see the pulled evidence. What it does not see is the logic Cortex used to build the draft.

## 2. Topology

**Pattern:** single + subagents

```
[Inbound PM task]
  → [Cortex: pulls project state and activity, drafts update + stories]
  → [Critic]
       traceable-figures fail → back to Cortex (max 2) → still failing → escalate
       queue cap / forbidden commitment / embargo leak → escalate on first failure
       pass → [PM review checkpoint] → queued
```

Cortex does not post. Escalate means the PM sees the draft and the reasons.

## 3. Roster

| Agent | Responsibility | Runs which Loop Spec |
|---|---|---|
| Cortex | Pulls project state and activity. Drafts the update and any story batch. Never posts. | M2 Loop Spec |
| Critic | Judges the draft against the four checks. Sends traceable-figures failures back, up to 2. Escalates queue cap, forbidden commitments, and embargo leaks on the first failure. | Field 5 validator rules. It does not run the drafting loop. |

## 4. Communication & hand-offs

In-process. No MCP or A2A.

| From | To | What passes |
|---|---|---|
| PM task | Cortex | The inbound task, including its message ID so the same task does not run twice. |
| Cortex | Critic | The drafted update and story batch, labelled for review. On a redraft, the previous failure note comes with it. |
| Critic | Cortex | Only on a traceable-figures failure, and only while the redraft count is under 2: the failed check and the reason. |
| Critic | PM | On pass: the draft, queued for the review checkpoint. On escalate: the draft plus the verdict and reasons. Nothing is posted. |

## 5. The validator

**What the critic checks**

- **Traceable figures.** Every number, progress claim, and red/yellow/green call matches pulled project state or activity. No invented or duplicated content.
- **Queue cap.** The draft has to be within the cap.
- **No commitments Cortex may not make.** No ship date, public date, tone, or launch gate. Those commitments are a human's.
- **No embargo leak.** Nothing flagged confidential or embargoed appears in an external or company-wide update.

**Fail-action**

- **Revise** only for traceable figures. Send the draft back to Cortex with the failure noted.
- **Escalate** on the first failure for queue cap, a commitment Cortex may not make, or an embargo leak. The PM sees the draft and the reasons. Cortex does not split a batch to get under the cap, and does not commit a date or tone.

**Revision cap**

Max 2 redrafts on the traceable-figures path, then escalate. A first redraft often removes an invented figure. A third pass usually repeats the same mistake, and the PM should see it instead.

**Pass-action**

A draft that passes all four checks advances to the PM review checkpoint. It is queued for approval. Cortex does not post it, send it, or commit a date or tone.

## 6. State: shared vs isolated

Share the evidence, isolate the reasoning. Both see the pulled project state, activity, roadmap flags, norms, and the draft. Cortex does not see the critic's reasoning, only the failed check and the reason on a revise. The critic does not see how Cortex talked itself into the draft. Knowing that logic may drive the critic to do something similar or the opposite. The critic judges based on its own logic.

## 7. Cost & latency budget

_Coordination has a price. Rough token/latency cost of the fleet vs a single agent. (Forward-link to M5 bounds.)_
