---
name: model-router
description: Choose economical models and reasoning levels for Codex tasks, delegate bounded work when it saves usage, and escalate based on failed checks. Use when the user requests efficient routing, lower usage, or invokes model-router for a task. Supports authorised model changes for subsequent turns when the runtime exposes a switching control; it is not a billing monitor.
---

# Efficient model routing

Complete the user's task with the least total usage that reliably achieves the required quality. Count context transfer, supervision, retries and verification as part of the work. Preserve explicitly requested models, tools and task scope.

## Choose a route

Inspect the models and reasoning levels actually exposed by the current runtime. The names below are starting preferences, not a guarantee of account entitlement. If a preferred model is unavailable, use an available suitable model and disclose a material substitution. Do not query account limits or fetch model documentation for every ordinary task. Verify current pricing and availability when making plan or savings recommendations.

Use GPT-6 Luna at low reasoning for bounded searches, extraction, classification, short summaries, simple file operations and service-status checks with clear completion criteria. A search still needs adequate coverage and evidence; cheaper routing does not justify guessing.

Use GPT-6.1 Terra at low reasoning as a trial route for routine browser operation, visual inspection and writing. Escalate when visual ambiguity, complex state or repeated action failures makes this route unreliable. Treat its suitability as something to validate through results.

Use GPT-6.1 Sol at low reasoning for ordinary implementation and at medium reasoning for multi-file changes, evidence synthesis and debugging that requires comparing plausible causes. Separate mechanical extraction from consequential tax, financial or legal interpretation; retain appropriate source checks and substantive review for the latter.

Use GPT-6 Astra at medium reasoning for difficult diagnosis, architecture with substantial tradeoffs, or unresolved reasoning failures on a cheaper route. Increase to high reasoning only when the problem warrants it. Do not default to maximum effort. GPT-6 Sol is a fallback when the preferred workhorse is unavailable or observed results justify it.

## Execute without excessive supervision

A skill cannot change the model already executing the current turn. The user has authorised automatic model selection for their own task, with a short explanation. Prefer a supported thread-model setting control if one is exposed. Otherwise, the Codex send_message_to_thread tool accepts model and thinking overrides for an authorised follow-up in the same chat. This is a continuation mechanism, not a pre-request interceptor. Verify its current schema before using it. Never bypass access checks or modify application databases to switch models.

Only send a routing continuation when there is substantial unfinished work and the benefit outweighs another turn. Use the verified current chat ID and preserve the original user request, progress, constraints and remaining work in a concise, user-readable prompt. Include a statement that this is a routing continuation, the chosen model and effort, and an instruction not to route the same handoff again. Send at most one routing continuation per substantive user request. Do not send a continuation merely to answer an acknowledgement, announce a switch or manufacture a test unless the user requested a test. Do not route to another user-owned chat without specific authorisation.

After the tool accepts a continuation, stop duplicate task execution and end the current turn with a brief status. Treat a delivered continuation as the same task. If it appears during the existing turn, do not infer that the model changed mid-turn and do not recursively send it again. The next model should complete the remaining work. A tool acknowledgement proves acceptance, not execution on the requested model. If the control rejects self-targeting or cannot establish a useful continuation, continue the task and recommend the model picker instead of retrying equivalent calls.

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
