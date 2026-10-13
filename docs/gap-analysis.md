# Gap analysis: where could Relevo help?

**Status:** hypothesis review, not a claim that a competitor lacks a feature. Last reviewed 2026-10-14. Product interfaces change quickly; the findings below reflect the linked documentation, not an exhaustive implementation audit. Relevo is documentation-only today.

## 1. Capabilities we must not rebuild as “the differentiator”

| Capability | Documented examples | Implication |
| --- | --- | --- |
| Durable tasks, dependencies, dispatch, and review | [OpenClaw Workboard](https://docs.openclaw.ai/plugins/workboard); [Hermes Kanban](https://hermes-agent.nousresearch.com/docs/user-guide/features/kanban); [Claude Code agent teams](https://code.claude.com/docs/en/agent-teams) | Import or reference native tasks. Do not require migration to another task board. |
| Scheduled work and notifications | [OpenClaw Automations](https://docs.openclaw.ai/automation); [Claude Code routines](https://code.claude.com/docs/en/routines); [Hermes Kanban](https://hermes-agent.nousresearch.com/docs/user-guide/features/kanban) | A cron wrapper is not enough. |
| Multi-client supervision | [Agent Deck](https://agent-deck.devopstoolkit.ai/) and [AgentDeck](https://github.com/puritysb/AgentDeck) describe multi-client supervision | “One dashboard for multiple agents” is not a distinctive claim. |
| Session control and structured human input | [Codex App Server](https://learn.chatgpt.com/docs/app-server); [ACP schema](https://github.com/agentclientprotocol/agent-client-protocol/blob/main/docs/protocol/v2/schema.mdx) | Do not invent a parallel input protocol where host or ACP semantics are sufficient. |
| Verification and acceptance gates | [Hermes Kanban](https://hermes-agent.nousresearch.com/docs/user-guide/features/kanban); [OpenClaw Workboard](https://docs.openclaw.ai/plugins/workboard) | Relevo may record outcome evidence, but quality gates themselves are not novel. |

## 2. Candidate gaps, ranked

### A. A durable *decision handoff* across independent hosts — promising, unverified

A person using more than one harness may face design or product decisions in several separate sessions. The candidate workflow is to gather those questions for a planned check-in without losing their original context or inventing an answer. One decision record would contain the originating host/session/task, a finite question, alternatives, evidence or artifact references, a human-availability window, a response explicitly attributed to the human, and an acknowledgment by the host. It would distinguish “question prepared,” “presented,” “answered,” and “host accepted answer.”

This is **not** a claim that human input is absent elsewhere. ACP already specifies `session/request_permission` and structured `elicitation/create`. Hosts may already present pending questions. The possible gap is a *durable, cross-host queue with planning and provenance*, not the existence of a prompt. The gap is strongest for non-permission choices; native permission prompts must stay native.

**First slice:** an optional local sidecar that records decision requests from one host, shows a concise check-in list, and links back to the original session. Add response delivery only through a documented host API. A hook that only observes cannot be used to answer on the user's behalf: [Hermes human-input hooks](https://hermes-agent.nousresearch.com/docs/user-guide/features/hooks) are observational and do not expose the answer to the hook. No response path means a link back to the native UI, not fake automation.

**Kill test:** can two independently started hosts emit or expose enough non-sensitive decision context, and can the person's answer return exactly once with correct identity and session attribution? If not, restrict the product to host-local UX or upstream an integration rather than promise a universal inbox.

### B. Admission before launch, informed by future human availability — potentially valuable, technically fragile

The candidate policy considers proposed tasks *before* starting them. Given a work-in-progress limit and a declared check-in (“I can decide at 10:00”), it recommends one of: start autonomous work, prepare evidence for a specific choice, defer until the check-in, or request an immediate decision because a deadline makes deferral unsafe. It records deferred tasks as well as started ones. This is different from merely muting notifications after an agent has become blocked.

The important caveat is that hosts have different control surfaces. [OpenClaw plugin hooks](https://docs.openclaw.ai/plugins/hooks) expose some pre-run gates but not a universal gate for every underlying runner. [Codex App Server](https://learn.chatgpt.com/docs/app-server) has start/steer/interrupt controls; interruption is not a general lossless pause. A native user launch, a different client, or a direct CLI call may bypass Relevo. Therefore the first implementation should *advise* and gate only a specific launch path whose behavior has been demonstrated. Never advertise global enforcement.

**First slice:** take proposed tasks from one existing board or CLI, return `start | prepare | defer` with a short rationale, and control one documented launch path. Do not control mid-turn permissions or claim resumability where the host does not provide it.

**Kill test:** can the gate actually prevent a launch on its declared path while allowing the host's own safety and permission flow to proceed? How often do users start through bypass paths? If the answer is “most of the time,” an admission engine may be the wrong product shape.

### C. The outcome ledger for *not-started* work — useful for validation, not a standalone product

Record proposed, admitted, deferred, and later completed tasks with their stated reasons. Separate technical finish, host-native verification, and human acceptance. This gives a check on whether deferring work reduces avoidable human blocks, rather than just making the dashboard quiet. Native boards already hold much task state and some verification evidence; Relevo should add only the missing decision-policy events and references.

**First slice:** a local event log with minimal metadata, no prompts or repository content by default. Export is unnecessary for the initial community tool.

## 3. Hard integration boundaries

- **Permission is not a product decision.** Relevo may remind the person of a host-native approval, but it cannot grant it, infer it from silence, or move it to a weaker channel.
- **Observe is not control.** Hooks may see events without the authority to hold a run or inject an answer. Adapters must declare separate capabilities for observe, start gate, deliver answer, and session continuation.
- **Cancellation is not suspension.** Avoid universal pause/resume claims. Often the safe strategy is to finish an authorized unit of work, start only a preparation task, or defer a new launch.
- **Prediction is uncertain.** Estimating when a question will arise should start with explicit task metadata and be labeled uncertain; avoid collecting full prompts, code, or histories just to improve a speculative score.
- **Different harnesses already overlap.** A user might need only one host's board, scheduling, and review tools. Relevo should remain optional and provide value without replacing them.

## 4. Evaluation before expansion

1. **Integration feasibility:** map documented capabilities and bypasses for two hosts. Demonstrate a real, attributed decision handoff; label observe-only paths honestly. Verify a single pre-start gate if one exists.
2. **Simple baseline:** on real tasks compare the host's native board, a low work-in-progress limit, and fixed check-ins against the Relevo-assisted workflow. Record setup time and manual corrections as costs.
3. **Outcome measures:** count verified completed work, decision-wait time, interruptions, missed deadlines, and admitted versus deferred tasks. Do not treat agent-reported “done” as independently verified success. Small pilots reveal failure modes, not statistically proven productivity gains.
4. **Stop or narrow:** if the handoff is unreliable, the gate is routinely bypassed, or the simpler workflow performs as well with less friction, contribute a smaller integration or UX fix to an existing tool. Do not build a new general harness.

## Tentative product shape

Start with a **local, optional decision-handoff adapter and CLI** that uses native tasks and native permission UIs. Add admission policy only after a real host-specific gate and an actual user benefit are demonstrated. MCP may expose Relevo functions to an agent, but it does not by itself intercept a host launch or authorize a human response.

This is a direction for experiments, not a conclusion that the market lacks such a feature. The premise is falsifiable: if a native board plus a small work-in-progress limit and planned check-ins solves the problem, Relevo should not exist as a separate coordination layer.
