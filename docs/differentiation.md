# What would Relevo add?

**Status:** hypothesis, not an implemented feature or a verified market gap. Last reviewed: 2026-10-14. Host capabilities change quickly, and a sufficiently custom script can reproduce many of the behaviors below inside one environment.

## What existing tools already do

Relevo should not claim that scheduling, agent teams, task boards, dependencies, human input, or notifications are new. For example:

- [OpenClaw Automations](https://docs.openclaw.ai/automation) run scheduled work, while its [Workboard](https://docs.openclaw.ai/plugins/workboard) manages cards, dependencies, worker dispatch, and review state.
- [Hermes Agent Kanban](https://hermes-agent.nousresearch.com/docs/user-guide/features/kanban) is a durable multi-agent board that can handle dependencies and work needing human input.
- [Claude Code agent teams](https://code.claude.com/docs/en/agent-teams) have shared tasks, dependencies, messaging, and a lead; [routines](https://code.claude.com/docs/en/routines) schedule work.
- [Codex App Server](https://learn.chatgpt.com/docs/app-server) exposes thread and turn operations to a controlling client. Codex-based workflows can be orchestrated in an application.

These tools are strong substitutes when all work stays inside one harness. A second task board or a prettier notification inbox is not enough reason to build Relevo.

## The proposed difference

Relevo's distinct job would be **cross-harness, pre-start admission based on future human decision availability**. It would ask whether a proposed task should begin now, prepare only decision-ready evidence, or wait because it is likely to block on the person at the wrong time. This is a policy over *tasks before and across launches*, not merely a status view of running sessions.

The combined behavior has three parts:

1. **One attention budget across hosts.** A Codex task, a Claude Code task, and an OpenClaw task compete for the same person's next ten-minute review window. Each host may manage its own queue well; Relevo would make a shared admission recommendation before starting more work. Enforcement depends on the adapter's actual gate capability.
2. **Decision-aware preparation.** A task likely to require a design choice can be admitted only for permitted investigation and comparison, so the next check-in contains a concrete question and evidence. Relevo must not invent a safe mid-turn pause or authorize an action by timeout.
3. **Visible counterfactuals and outcomes.** Record tasks that were deferred *before* a host session existed, why they were deferred, whether the expected human block occurred, and whether admitted work was verified. This lets a person inspect and correct the admission policy instead of judging it from notifications alone.

Consider three proposed tasks at 09:00: one independent test run in Codex, one design exploration in Claude Code, and one low-priority refactor in OpenClaw. The person can review decisions at 10:00 for ten minutes and wants no more than two active agents. Relevo would propose starting tests, preparing two design alternatives, and deferring the refactor with a recorded reason. At 10:00 it would present the design choice. A host that cannot gate starts would receive advice, not a false claim that the refactor was blocked.

## What is not yet proven

- Existing hooks and APIs may not expose a reliable pre-start boundary for every host or execution path.
- Predicting when a task will need human input may require more context than a privacy-minimal adapter should collect.
- A shared policy may do no better than a task board, a work-in-progress limit, and fixed check-ins.
- Cross-harness coordination might matter to too few users to justify its installation and maintenance cost.

The test is not whether Relevo can show another board. It is whether it changes admissions **and** improves completed, verified work without increasing missed deadlines or unwanted interruptions. If it cannot beat the simple baseline, the honest outcome is to stop or contribute smaller improvements to existing harnesses.
