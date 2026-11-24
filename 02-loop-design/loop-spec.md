# Loop Spec: Cortex PM Chief-of-Staff Agent

> Module 2 · Loop Engineering, ★ Deliverable 2
>
> ✅ **What this validates:** the agent knows when to run and when to stop, by the end you'll have proven a one-page Loop Spec with a trigger, a definition of "done," and explicit stop conditions.
>
> Your one-page blueprint for how the work you handed to the agent (M1) actually *runs*.
> An agent is just a prompt that fires itself, this spec says when it fires, what "done" means, and what it needs to do the job. Living document; refine as the course progresses.

## 1. Trigger & loop type

**Chosen type:** hook

When new tasks come in, that starts the sequence. We don't expect the tasks to come frequently, but a cron job may not be soon enough, so using the new task as it comes in is the better trigger.

**Ruled out:**
- **Heartbeat:** we don't expect new tasks frequently, so a heartbeat every several minutes would create wasted runs.
- **Cron:** a cron job may not be soon enough when a new task arrives.
- **Goal:** the action can end in one shot at the time of the new task.

**Idempotency:** use the message ID on the task description so the same task does not trigger the sequence twice.

## 2. Goal / definition of done

The update is written and queued for approval, and any story batch is inside the cap and queued too. Cortex never posts.

We want to avoid any update draft, story, or post being done without human review, because they may not be easily reversible.

## 3. Stop conditions

| Condition | What it looks like | What happens |
|---|---|---|
| **Success** | The update is queued for approval, and any story batch is inside the cap and queued too. Nothing is posted. | Stop. Leave the queue for a human. |
| **Stuck / give up** | Project state or activity is still missing after 3 failed pulls. | Stop and log. Do not invent numbers. The pull limit avoids an infinite loop. |
| **Escalate to human** | The draft would commit a public date or tone, touch an embargoed project, or the story batch is over the cap. | Hand off at the human checkpoint. Do not post, do not commit the date or tone, and do not split the batch to get under the cap. |

## 4. State

Persist the final results: the roadmap, the decision log, team norms, and the message IDs already seen. Scope is per project, so one project's confidential notes do not leak into another.

Do not persist drafts or flags. They are just a step toward the actual goal.

## 5. The five things a loop can lean on

| Component | For Cortex |
|---|---|
| **Work tree** (isolated workspace per run, a git worktree) | Not needed yet, because we have not decided whether to do a parallel run. |
| **Skills** (reusable capabilities) | Not needed yet, because the tool is not complex enough. |
| **Plugins / connectors** (tools & access, optional if you don't have one yet) | A plan, not wired. We are creating the tool on fixtures. Once it is solid, we hook it to the actual data source. |
| **Subagents** (independent check when the loop can't grade itself) | Not needed yet, because we are not asking the tool to do an independent check. |
| **State tracking** | Roadmap, decision log, team norms, and seen message IDs persist per project. Drafts and flags do not. |

> Context plan (M4) and the hand-off to bounds & evals (M5) come in later modules, you'll add them to their own deliverables then, not here.

## Link to live loop

_[path to your agent in `00-build/`]_
