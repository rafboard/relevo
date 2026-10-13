# Relevo

**A proposed way to coordinate AI agents around the time a person actually has to make decisions.**

Imagine you have two coding agents working while you are away. One can run tests on its own. The other will soon need you to choose between two designs. A third task is not urgent. Starting all three immediately may leave several agents waiting for you. Relevo would start the useful independent work, prepare the design choice for your next check-in, and defer the third task with a visible reason.

Relevo is **not another AI assistant or coding harness**. It is intended to sit alongside tools people already use, such as OpenClaw, Hermes Agent, Codex, and Claude Code. The goal is to coordinate *when work begins and when a human decision is needed*, without taking over the host tool's permissions or pretending that silence means approval.

> **Project status:** design and documentation only. There is no working implementation, installable package, or supported integration yet. The interfaces below are proposals, not promises about any host's current capabilities.

## The idea in one minute

1. You record a few tasks, priorities, deadlines, and the next time you expect to be available.
2. Relevo checks which tasks can make progress without you and which are likely to block on your input.
3. It recommends starting, preparing, or deferring each task. Where a host supports a reliable start gate, an adapter may enforce that choice before a task begins.
4. It gathers genuine questions into a short decision inbox for your next check-in.
5. After you answer, the relevant agent continues **only if its host supports that operation**. No answer is treated as no authorization.

For example: "I have ten minutes at 10:00. Prioritize the checkout fix. No more than two active agents. Tests may run; deployments require my explicit approval." Relevo should not merely mute notifications until 10:00. It should change which work is admitted in the first place.

## What Relevo is—and is not

- A **local-first coordination layer** with a small, versioned task/event contract and thin adapters for existing tools.
- A way to make deferred work and pending human decisions explicit, including tasks that were never started.
- **Not** a replacement for host permissions, a universal pause/resume button, or an autonomous approver.
- **Not** a new dashboard that merely lists running agent sessions. The interesting question is whether admission decisions improve completed, verified work.

## Proposed shape

```text
OpenClaw / Hermes / Codex / Claude Code
                  ↕ host-specific adapters
             Relevo core (local)
     task ledger · admission · decision inbox
                  ↕
           local CLI / optional MCP
```

Adapters declare what they can actually do. An observe-only adapter can recommend; a start-gating adapter can defer new work; an adapter with supported session controls can launch or continue work. Relevo must never claim a capability the host has not provided.

## Read the design

- [Concepts and example workflow](docs/concepts.md)
- [Architecture and trust boundaries](docs/architecture.md)
- [Draft task and event contract](docs/protocol.md)
- [Adapter model and integration limits](docs/adapters.md)
- [Interface proposals and recommended starting point](docs/interface.md)
- [Proposed milestones and validation](docs/roadmap.md)
- [How to contribute](CONTRIBUTING.md)

## Guiding principles

**Human decisions stay human.** **Host permissions stay authoritative.** **Local data stays local by default.** **Unsupported capabilities remain visible.** These are design requirements, not features already delivered.

This repository is an invitation to refine a useful, interoperable tool before building it. See the [roadmap](docs/roadmap.md) for the smallest proposed implementation and the tests that would tell us whether the idea actually helps.

## License

MIT. See [LICENSE](LICENSE).
