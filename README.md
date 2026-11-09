<div align="center">

```text
┌──────────────────────────────────────────────────────────────────────┐
│  MODEL ROUTER                                          CODEX / SKILL │
└──────────────────────────────────────────────────────────────────────┘

       request ──► classify ──► route ──► execute ──► verify
                       │           │
                       └── escalate on evidence ────┘
```

# Model Router

**A practical Codex skill for choosing the lightest model and reasoning level that can reliably finish the job.**

[Install](#install) · [Routing](#routing) · [How switching works](#how-switching-works)

</div>

Model choice is only one part of efficient work. Context transfer, tool calls, retries and verification all count. Model Router keeps those costs in view and escalates when the evidence says it should.

## What it does

- Routes bounded lookups and extraction to a fast, economical model.
- Uses a balanced workhorse for everyday coding and analysis.
- Saves frontier reasoning for difficult diagnosis and consequential tradeoffs.
- Keeps delegation focused, limits retry loops and verifies the result.
- Gives a concise explanation when a supported model switch is made or requested.

## Default and category routes

GPT-6.1 Sol at low reasoning is the default for mixed or uncategorised work. The router changes the route when a task has a clear specialist shape, and can use different routes for separate stages of one job.

| Category | Default route | Escalate when |
|---|---|---|
| Instagram Reels and short-form video | GPT-6.1 Sol · low for concept and script; media tools for production | Claims need careful checking or edit review finds issues |
| Software development | GPT-6.1 Sol · low for bounded fixes; medium for multi-file work | A focused fix fails or design tradeoffs are substantial |
| Research and current information | GPT-6.1 Luna · low for lookup; GPT-6.1 Sol · medium for synthesis | Sources conflict or conclusions carry meaningful consequences |
| Writing and communications | GPT-6.1 Luna · low for short drafts; GPT-6.1 Sol · low for longer work | The subject is sensitive or argument is complex |
| Images and design | GPT-6.1 Terra · low for visual review; image tools for creation and editing | Visual requirements are ambiguous or review finds mismatches |
| Audio and voice | GPT-6.1 Luna · low for transcription; audio tools for production | Attribution, timing or meaning is unclear |
| Spreadsheets and data | GPT-6.1 Luna · low for extraction; GPT-6.1 Sol · medium for reconciliation | Inputs conflict or calculations affect consequential decisions |
| Documents and presentations | GPT-6.1 Sol · low for routine drafting; format-specific tools | Substantive review is needed or layout checks fail |
| Browser and app operations | GPT-6.1 Terra · low for visual workflows; GPT-6.1 Luna · low for simple status checks | UI state is ambiguous; stop for access blockers |
| Monitoring and automation | GPT-6.1 Luna · low for bounded checks; scripts for repetition | An exception needs diagnosis or an action has significant impact |

These are starting defaults to evaluate, not measured savings guarantees or fixed account entitlements. Use the models available in the runtime, and keep tool choice, source quality and verification appropriate to the task.

## Model-weighted allowance scenario

This chart models how quickly the same stream of work could consume a normalized allowance under different model mixes. It is a scenario, not a measurement of your subscription. The model weights use the published standard credit rate ratios as a proxy: for uncached input and output, Sol is about 20% of Astra's rate and Luna about 1%. Actual subscription allowance accounting is not published as a direct conversion from these rates.

```mermaid
xychart-beta
    title "Illustrative cumulative usage against plan thresholds"
    x-axis "Same work completed (normalized units)" [0, 1, 2, 3, 4, 5, 6, 7]
    y-axis "Share of A$300 reference allowance (%)" 0 --> 100
    line [0, 14, 28, 42, 56, 70, 84, 98]
    line [0, 3, 6, 9, 12, 15, 18, 21]
    line [50, 50, 50, 50, 50, 50, 50, 50]
    line [100, 100, 100, 100, 100, 100, 100, 100]
```

The first curve represents an Astra-only baseline. The second represents a hypothetical routed mix of 10% Astra, 70% Sol and 20% Luna, weighted using those approximate rate ratios. The horizontal guides show an assumed A$150 threshold at half the A$300 allowance and an A$300 threshold at 100%. This proportional price-to-allowance assumption is unverified; the account endpoint confirms only that the current Pro weekly allowance reached 100% on 10 November 2026. It does not expose the Australian tiers' limits or daily usage history.

The point of the model weighting is to show direction: on the same token-sized work, routing more tasks to lower-rate models could reduce credit-equivalent consumption. It does not predict when your real plan will run out, because tasks differ in context and output, and subscription allowance use is not simply token rates multiplied by volume. OpenAI lists approximate local message ranges for Plus, but says those vary by task and that Pro currently has no five-hour limit. [Pricing and usage guidance](https://learn.chatgpt.com/docs/pricing#what-are-the-usage-limits-for-my-plan) · [Standard model credit rates](https://learn.chatgpt.com/docs/pricing#token-rates) · [Usage dashboard](https://chatgpt.com/codex/settings/usage).

Your local session records from 4–9 November show 4,676 Astra steps (93.5%), 278 Sol steps (5.6%) and 46 Luna steps (0.9%), out of 5,000 recorded steps. These are not user messages or allowance units, and do not cover every device or account surface. Replace the scenario curves and plan guides with measured dashboard readings from comparable weeks before using them to decide on a downgrade.

## How switching works

A skill runs after a turn has started, so it cannot change the model that is already answering. When Codex exposes a supported model override for a later turn, the router can request a continuation in the same chat. It gives one short notice, for example:

> We changed the model to GPT-6 Luna (low reasoning). Why: this task only needs a straightforward lookup.

A queued request is not proof that the new model actually ran. The skill distinguishes a confirmed switch from a request that was merely accepted. If no supported control is available, it recommends the model picker and carries on with the work.

## Install

Clone this repository, then copy the skill files into your Codex skills directory:

```sh
git clone https://github.com/lockej2005/model-router-skill.git
mkdir -p ~/.codex/skills/model-router
cp model-router-skill/SKILL.md ~/.codex/skills/model-router/
cp -R model-router-skill/agents ~/.codex/skills/model-router/
```

Invoke it explicitly with `$model-router`. To make it a default, add a short instruction to your global `~/.codex/AGENTS.md` telling Codex to apply the skill to each task. Automatic skill discovery and global instructions guide behavior; neither is a pre-request interceptor.

## Design constraints

The router does not promise a fixed savings percentage. It avoids routing overhead on tiny tasks, uses one worker for a coherent delegated job, and escalates after a meaningful failed check. Login problems and missing permissions are treated as access blockers, not reasons to spend tokens on a stronger model.

It does not change subscriptions, purchase credits, consume resets or silently rewrite global model settings.

## Repository layout

```text
.
├── SKILL.md
├── README.md
└── agents/
    └── openai.yaml
```

## License

No license has been added yet. Until a license is chosen, reuse and redistribution remain subject to the repository owner's rights.
