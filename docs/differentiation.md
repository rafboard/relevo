# What might Relevo add?

**Status:** research hypothesis, not an implemented feature or a verified market gap. Reviewed 2026-10-14. See the [detailed gap analysis](gap-analysis.md) for sources, integration limits, and tests.

## The uncomfortable baseline

Existing tools already cover much of Relevo's first sketch. [OpenClaw Workboard](https://docs.openclaw.ai/plugins/workboard) and [Hermes Kanban](https://hermes-agent.nousresearch.com/docs/user-guide/features/kanban) have durable tasks, dependencies, dispatch, and human review. [Claude Code agent teams](https://code.claude.com/docs/en/agent-teams) coordinate tasks and agents; [routines](https://code.claude.com/docs/en/routines) schedule work. [Codex App Server](https://learn.chatgpt.com/docs/app-server) exposes sessions and turn control. Cross-client dashboards and dispatchers also exist, such as [Agent Deck](https://agent-deck.devopstoolkit.ai/). Even [ACP](https://github.com/agentclientprotocol/agent-client-protocol/blob/main/docs/protocol/v2/schema.mdx) specifies structured human input. A new board, scheduler, or generic “ask the human” protocol would largely duplicate existing work.

## A narrower candidate

Relevo might be useful as an **optional human-decision handoff layer** across independent harnesses. It would collect *non-permission* decisions that genuinely block progress; keep the originating session, question, alternatives, evidence references, expected human check-in, and explicit response together; then return that response through a host-supported path. Host-native permissions remain authoritative. A question observed through a hook is not automatically answerable through that hook.

The second, harder candidate is **decision-aware admission**: before starting another task, advise whether it can finish useful authorized work before the next human check-in, should prepare evidence for a choice, or should wait. A host-specific start gate can enforce this only on paths it actually controls. Otherwise Relevo is advisory. No universal, lossless pause/resume is assumed.

The product claim is the *combination* of durable handoff and attention-aware admission across existing tools, not a new primitive unavailable anywhere else. Its incremental value over a native board plus a work-in-progress limit is unproven.

## What would change our minds

First demonstrate a reliable handoff from two independently running hosts without stealing permission authority. Then demonstrate a real pre-start gate for at least one launch path, with bypasses visible. Finally compare the resulting workflow with a native board, a small work-in-progress limit, and fixed human check-ins on real tasks. Measure verified completions, interruptions, missed deadlines, time waiting for a human decision, and setup effort. If the simpler baseline is as good, stop building a separate scheduler and contribute the useful adapter or UX improvement upstream.
