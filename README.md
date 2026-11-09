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

## Layered weekly usage chart

The chart below uses one shared scale for cumulative weekly allowance use. The solid blue line is the only measured value: Pro reached 100% on 10 November 2026. The dashed lines show the comparisons we want to measure, but their height is deliberately left unplotted because we do not have daily allowance readings for an Astra-only week, a smart-routing week, or the A$150 and A$300 tiers.

```mermaid
xychart-beta
    title "Weekly allowance use: measured point and comparison layers"
    x-axis [Mon, Tue, Wed, Thu, Fri, Sat, Sun]
    y-axis "Share of that plan's weekly allowance (%)" 0 --> 100
    line [0, 0, 0, 0, 0, 0, 100]
    line [0, 0, 0, 0, 0, 0, 0]
    line [0, 0, 0, 0, 0, 0, 0]
    line [0, 0, 0, 0, 0, 0, 0]
    line [0, 0, 0, 0, 0, 0, 0]
```

Series order: measured Pro endpoint; Astra-only baseline, not measured; smart-routing week, not measured; A$150 plan limit, unavailable; A$300 plan limit, unavailable. Mermaid requires numeric points, so zeroes stand in for unavailable observations. They do not mean zero use or zero allowance. Read only the labelled measured point; the flat placeholders are not data. This keeps all five layers visible in one chart, but a richer chart with true gaps would be easier to read than Mermaid's line format.

The model mix is a separate layer of evidence from the same local session sample, 4–9 November 2026: 4,676 recorded Astra steps (93.5%), 278 Sol steps (5.6%) and 46 Luna steps (0.9%), out of 5,000. These are model steps, not user messages or allowance percentages. The records do not represent every device or account surface.

For a grounded comparison, record the usage dashboard percentage at the same time each day across comparable Astra-only and smart-routing weeks. Capture each plan's own displayed limit as well. Then replace the placeholder series with measured values and plot each plan against its actual 100% threshold. OpenAI says usage varies with model, task size, context, tools and caching. [Pricing and usage guidance](https://learn.chatgpt.com/docs/pricing#what-are-the-usage-limits-for-my-plan) · [Usage dashboard](https://chatgpt.com/codex/settings/usage).

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
