# Context Engineering & Memory: Cortex PM Chief-of-Staff Agent

> Module 4 · Context Engineering & Memory
>
> ✅ **What this validates:** the agent reasons on the right, safe inputs, by the end you'll have proven a context budget, per-source retrieve-vs-long-context decisions, and a memory map with risk mitigations.
>
> 🗂️ **How the lab maps to this file:** In **Part A** (before the lecture) you don't edit this file, you rough-draft on scratch, focused on the per-source calls in **section 2** plus a quick remember/forget + "how it rots" sketch. In **Part B** (after the lecture) you complete **all five sections**; the Lab Guide's guided builder writes this file for you to copy in and commit.

## 1. Context budget

One loop iteration cannot take every source in full. Process the most volatile items first, followed by aggregated items like the roadmap. Formatting can come last since it's just setup.

1. **Task** (`get_task`, long-context). One bounded doc. Cortex needs to understand the task described in the document.
2. **Norms** (`get_norms`, retrieve). The playbook changes. The contents need to be up to date.
3. **Activity** (`get_activity`, retrieve). Activities change often and we need to be able to cite accurately with changing activities.
4. **Roadmap** (`get_roadmap`, retrieve). Aggregated item. We have better control of embargoed content by using retrieve.
5. **Past updates** (`search_past_updates`, retrieve). Formatting comes last. We may accumulate a large number of status updates and decisions.

## 2. Retrieve vs. long-context: per source

For each data source, decide: **retrieve** (narrow a large/changing corpus to the relevant slice) or **long-context** (just include a bounded set you can reason over).

| Source | Size / volatility | Decision | Deciding factor | Why |
|---|---|---|---|---|
| `get_task` (this week's task brief) | One bounded, static doc | Long-context | Size | It is one bounded doc. Cortex needs to understand the task described in the document. |
| `get_norms` (team playbook) | Medium; must stay current | Retrieve | Volatility | The playbook changes. The contents need to be up to date. |
| `get_activity` (engineering activity) | Large and grows; changes often | Retrieve | Volatility and citation/audit | The activities change often and we need to be able to cite accurately with changing activities. |
| `get_roadmap` | Medium; confidential flags | Retrieve | Citation/audit | We have better control of embargoed content by using retrieve. |
| `search_past_updates` | Unbounded | Retrieve | Size | We may accumulate a large number of status updates and decisions. |

## 3. Retrieval quality plan

`get_task` is long-context, so it is not in this grid. Every retrieve source has at least one agentic move.

| Source | Routing | Document grading | Reranking | Self-verification | Caching | Why this move |
|---|---|---|---|---|---|---|
| `get_norms` |  | Yes |  |  |  | Check that the contents are up to date, relevant, and including the embargo items. |
| `get_activity` |  |  |  | Yes |  | Make sure all the PRs are actually in the payloads. |
| `get_roadmap` |  | Yes, before content shipment |  | Yes, after the draft is completed |  | Make sure embargoed content is excluded. |
| `search_past_updates` |  | Yes | Yes |  |  | Document grading rejects a hit when embargoed content is included. Reranking matches the latest format. |

## 4. Memory map (your PM brain)

Forget almost everything when the run ends. The final copy of the stories and the updates is what gets kept, and only after a human accepts it. Read scope stays the Module 1 line: Cortex may read the tools and draft, and may not post. These TTLs are the bounds to enforce later.

| Memory type | What Cortex stores | Scope / TTL |
|---|---|---|
| **Working** (in-loop) | This run's task, the retrieved slices, and the draft in progress | Forgotten when the run ends |
| **Episodic** (past runs) | Past updates, plus the final copy of the stories and the updates after a human accepts them | History stays in the store and is searched, not loaded whole. A held draft is not written in until the reviewer accepts it |
| **Semantic** (durable facts/prefs) | Not the norms or the roadmap | Those are volatile. They are not kept past this run. Each call pulls a fresh payload |
| **Shared** (across agents) | Only the human-approved stories and updates | Shared with the reviewer. The unapproved draft is expired. Orbit and Pulsar are not in the kept copy |

## 5. Memory risks & mitigations

| Risk | Where it bites Cortex | Mitigation |
|---|---|---|
| Drift | A later draft repeats 41% after activity has moved to 43% | Do not store metrics in memory. Pull a fresh payload when you run `get_activity`. |
| Poisoning | A jailbreak or a leaked name gets saved as next week's format | A human accepts the copy before it is kept. It has to be up to standard: no embargoed content, and correct PR numbers. |
| Staleness | An old playbook or roadmap misses Pulsar or still says 41% | Do not keep norms or the roadmap past this run. The contents we pull are volatile, so pull a fresh payload each call. |
| Confidential / retention | Orbit or Pulsar remain in a saved draft | Only save the human-approved contents. Do not keep a draft that may contain Orbit or Pulsar. |
