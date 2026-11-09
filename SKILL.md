---
name: model-router
description: Choose economical models and reasoning levels for Codex tasks, delegate bounded work when it saves usage, and escalate based on failed checks. Use when the user requests efficient routing, lower usage, or invokes model-router for a task. Supports authorised model changes for subsequent turns when the runtime exposes a switching control; it is not a billing monitor.
---

# Efficient model routing

Complete the user's task with the least total usage that reliably achieves the required quality. Count context transfer, supervision, retries and verification as part of the work. Preserve explicitly requested models, tools and task scope.

## Choose a route

Inspect the models and reasoning levels actually exposed by the current runtime. The names below are starting preferences, not a guarantee of account entitlement. If a preferred model is unavailable, use an available suitable model and disclose a material substitution. Do not query account limits or fetch model documentation for every ordinary task. Verify current pricing and availability when making plan or savings recommendations.

Use GPT-6.1 Sol at low reasoning as the default route when a task spans multiple categories or does not clearly fit a specialist route. It is a practical balance for mixed work. Lower the route for bounded subtasks and raise it for consequential or difficult parts; do not force one model across every stage of a workflow.

When the runtime exposes a reliable remaining-allowance percentage, treat the final 10% as a hard economy mode: route all model work to GPT-6 Luna at low reasoning, including delegated work, until the allowance resets. Before any model call, check whether the threshold is active if a supported usage signal is already available; do not make a costly or speculative extra query just to check. Give a brief notice when economy mode starts. Preserve quality by narrowing scope, using deterministic tools, and checking important outputs. If Luna cannot safely or adequately handle a consequential task, explain the limitation and ask whether the user wants to override this rule before using a stronger model. An explicit user request for a named stronger model overrides economy mode for that request. Do not claim this policy can intercept a turn before it starts unless the runtime actually supports that control.

Use GPT-6 Luna at low reasoning for bounded searches, extraction, classification, short summaries, simple file operations and service-status checks with clear completion criteria. A search still needs adequate coverage and evidence; cheaper routing does not justify guessing.

Use GPT-6.1 Terra at low reasoning as a trial route for routine browser operation, visual inspection and writing. Escalate when visual ambiguity, complex state or repeated action failures makes this route unreliable. Treat its suitability as something to validate through results.

Use GPT-6.1 Sol at low reasoning for ordinary implementation and at medium reasoning for multi-file changes, evidence synthesis and debugging that requires comparing plausible causes. Separate mechanical extraction from consequential tax, financial or legal interpretation; retain appropriate source checks and substantive review for the latter.

Use GPT-6 Astra at medium reasoning for difficult diagnosis, architecture with substantial tradeoffs, or unresolved reasoning failures on a cheaper route. Increase to high reasoning only when the problem warrants it. Do not default to maximum effort. GPT-6 Sol is a fallback when the preferred workhorse is unavailable or observed results justify it.

## Execute without excessive supervision

A skill cannot change the model already executing the current turn. At the start of each substantive request, choose the target model and reasoning effort before doing the work. Reading this skill, announcing a route, or selecting a preferred model in your reasoning does not itself switch the model.

The user has authorised automatic model selection for their own task, with a short explanation. Prefer a supported thread-model setting control if one is exposed. Otherwise, use the Codex `send_message_to_thread` tool with explicit `model` and `thinking` overrides to continue the same chat on the selected route. This is a continuation mechanism, not a pre-request interceptor. Verify its current schema before using it. Never bypass access checks or modify application databases to switch models.

When the selected route differs from the exposed active model, request the same-thread continuation before producing the requested deliverable. If the active model is unknown and a supported continuation control is available, request the selected route once before producing the deliverable. This applies to short drafts, summaries, extraction and focused questions as well as longer tasks. A 100-word draft should request GPT-6 Luna at low reasoning before writing the draft. Only acknowledgements and messages that require no substantive deliverable can skip the handoff for overhead reasons. A confirmed matching model and effort can continue directly. Do not use “applying the router” as a substitute for requesting a switch. Preserve an explicit user model choice and the single-handoff guard.

Use the verified current chat ID and preserve the original user request, progress, constraints and remaining work in a concise, user-readable prompt. State that this is a routing continuation, name the chosen model and effort, and instruct the next turn not to route the same handoff again. Send at most one routing continuation per substantive user request. After acceptance, give the routing notice in commentary. Inspect how the continuation is delivered. If it arrives during the current turn as a same-chat message, complete the original deliverable in that turn without routing again. If the host explicitly queues a separate turn, yield to that continuation. The final response must contain the requested deliverable once execution resumes. Do not send a continuation merely to answer an acknowledgement or manufacture a test unless the user requested it. Do not route to another user-owned chat without specific authorisation.

A successful tool response proves the request was accepted. A same-turn delivery does not prove that the executing model changed. Use runtime metadata to report the actual model when available. Do not end with only a routing notice when the original deliverable remains unfinished and the continuation has already been delivered in the current turn. If the control rejects self-targeting, complete the task and recommend the model picker without retrying equivalent calls.

Show one short notice for each actual change: “We changed the model to [model] ([effort] reasoning). Why: [one short task-specific reason].” Only use this wording when the effective model is confirmed by runtime or execution metadata. If only the request is accepted, say “Requested [model] ([effort] reasoning) for the continuation. Why: [reason].” If a later check contradicts the request, explain the actual state. Use ordinary text unless the host exposes a supported notice component; do not invent a clickable information icon or claim the app UI was modified.

Do not silently edit global model configuration. Respect an explicit user model choice over automatic routing. When the current model is unknown, do not invent its identity or claim a downgrade; use the task requirements and available controls, and avoid speculative switching.
For a small task, complete it directly when launching and reviewing a worker would likely cost more. For substantial bounded work, this skill explicitly permits using a subagent with an appropriate available model, subject to higher-priority instructions. Use fresh context when selecting an override requires it. Give the worker the objective, essential facts and file paths, constraints, relevant authorization boundaries, and observable completion criteria. Do not copy the full conversation. Ensure the worker has the required tools and access before handing off work.

Prefer one worker per coherent task. Use parallel workers only for independently useful work whose reduced delay justifies duplicated context and supervision. Avoid duplicate implementations, routine model voting, and recursive delegation. Do not use an expensive supervisor to repeat every step of a cheaper worker's work.

Verify outcomes with proportionate evidence such as a focused test, a calculation, a source check or inspection of the final artifact. Review the evidence and material risks rather than asking a stronger model to redo an already verified result. Report the actual route and any escalation briefly when useful; do not narrate routing on every turn.

## Escalate on evidence

Define success before delegating. After a failed check, allow one focused correction if the failure has an identifiable fix. If the same substantive failure persists, escalate with a compact description of the evidence and failed approach. Start with a stronger model immediately when the task clearly warrants it rather than paying for a predictable failed attempt.

Missing login, permissions, inaccessible files, unavailable tools and missing user information are access or information problems. Do not escalate models to solve them. Once a repeated blocker is established, stop equivalent retries, complete independent authorized work, and request the necessary input. Do not abandon difficult but actionable work merely to conserve tokens.

## Reduce workflow overhead

Prefer a connector, API or deterministic script for repetitive work when available and appropriate. Use a model to interpret exceptions rather than continuously polling a stable service. Create recurring monitoring only when requested, and notify only on meaningful changes unless the user asks for periodic reports.

Keep tool responses bounded and retrieve only useful detail. Batch independent reads. Avoid repeated screenshots of unchanged states and repeated broad searches without new evidence. At a meaningful task boundary, prepare a concise handoff if the context has become unwieldy; do not create a new chat without explicit authorization or discard information still needed for correctness. Account for useful caching rather than assuming a fresh context is always cheaper.

## Evaluate savings honestly

Do not promise a percentage saving or recommend a downgrade from task counts alone. Distinguish input, cached input and output tokens, recorded activity and billed usage, and API prices and subscription allowances. Account-wide usage changes may include concurrent chats and other devices.

When the user requests an efficiency trial or assessment, use available usage records to compare a representative period, including success rates, retries, latency, paid credits and peak usage windows. Avoid extra evaluation runs merely to produce metrics. Do not change a subscription, purchase credits, consume a reset or schedule monitoring as part of routing alone.

## Category defaults

Start mixed or uncategorised work with GPT-6.1 Sol at low reasoning. Split a task by stage when its research, creation and review needs differ. Use the least costly exposed route that meets each stage's quality requirements.

| Category | Default route | Escalate when |
|---|---|---|
| Instagram Reels and short-form video | GPT-6.1 Sol · low for concept and script; use the relevant media tools for production | Factual claims need verification, the story is nuanced, or edit review reveals problems |
| Software development | GPT-6.1 Sol · low for bounded fixes; medium for multi-file changes | A failure persists after a focused correction or design tradeoffs are substantial |
| Research and current information | GPT-6.1 Luna · low for focused lookup; GPT-6.1 Sol · medium for synthesis | Sources conflict, evidence is ambiguous, or the conclusion is consequential |
| Writing and communications | GPT-6.1 Luna · low for short drafts; GPT-6.1 Sol · low for longer pieces | The subject is sensitive, complex, or requires careful argument |
| Images and design | GPT-6.1 Terra · low for visual review; use the relevant image tools for generation or editing | Visual requirements are ambiguous or repeated review finds mismatches |
| Audio and voice | GPT-6.1 Luna · low for transcription or brief text work; use the relevant audio tools for production | Speaker attribution, timing or meaning is unclear, or fidelity needs close review |
| Spreadsheets and data | GPT-6.1 Luna · low for extraction and routine calculations; GPT-6.1 Sol · medium for reconciliation | Data is inconsistent, formulas affect decisions, or errors could have material consequences |
| Documents and presentations | GPT-6.1 Sol · low for drafting and routine edits; use format-specific tools | Legal, financial or assessed content needs substantive review, or layout checks fail |
| Browser and app operations | GPT-6.1 Terra · low for routine visual workflows; GPT-6.1 Luna · low for a simple status check | The interface is ambiguous or state changes repeatedly fail; stop for access blockers |
| Monitoring and automation | GPT-6.1 Luna · low for bounded checks; prefer deterministic scripts for repetition | An exception needs diagnosis, or the automation could cause consequential changes |

Treat these model names as preferences. Check runtime availability and preserve source checking, specialist tools and user authorization regardless of route. These are initial defaults to evaluate against task quality and actual usage, not measured guarantees.

