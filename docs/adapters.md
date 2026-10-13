# Adapter model

The purpose of an adapter is to make one existing agent environment useful to Relevo **without replacing that environment**. Adapters should be small and independently installable. Every capability must be measured against the actual host version and execution path.

## Three integration levels

1. **Observe and advise:** Report task progress and pending questions; the person or host remains responsible for starting work. Useful, but it cannot claim to have deferred a host task.
2. **Gate admission:** Intercept a supported pre-start boundary and start, prepare, or defer new work according to an explicit policy. Directly launched sessions outside that boundary may still bypass Relevo.
3. **Coordinate execution:** In addition to admission, use supported host APIs to start work, read status, and continue a session after a human response. Each operation is advertised separately; this level does not imply safe mid-turn pause.

The core degrades to the strongest level actually available. A plugin installation alone is not proof that a pre-start gate fires for every runner or session type.

## Possible host surfaces to investigate

- **[OpenClaw plugin hooks](https://docs.openclaw.ai/plugins/hooks):** Typed hooks can observe and alter selected run and tool boundaries. The official catalog explicitly warns that not every runtime emits every hook. In particular, a gate on one runner must not be presented as a universal Codex gate.
- **[Hermes Agent plugins and hooks](https://hermes-agent.nousresearch.com/docs/user-guide/features/plugins):** Plugins can register tools, commands, and lifecycle hooks. An adapter must distinguish observer hooks from directive hooks and verify where each fires.
- **[Claude Code hooks](https://code.claude.com/docs/en/hooks):** Hook events offer extension points around parts of the coding workflow. A tool-use hook is not automatically a gate for every user task or a durable session-resume API.
- **[Codex App Server](https://learn.chatgpt.com/docs/app-server):** Thread and turn operations can support a controlling client. A turn interrupt is cancellation, not proof that an in-flight turn can be safely paused and resumed.

These are investigation targets, **not** announced integrations. Each proposed adapter needs an explicit compatibility matrix: host version, runner or UI, events observed, actions tested, failure behavior, and permission boundary.

## MCP and ACP

[MCP](https://modelcontextprotocol.io/specification/2026-07-28/server/index) is a useful optional way to expose Relevo tools and resources to compatible hosts. It is model-facing; it does not by itself grant Relevo control over host admission, scheduling, or approval flow. A host-specific hook or API may still be required.

[Agent Client Protocol (ACP)](https://github.com/agentclientprotocol/agent-client-protocol/blob/main/docs/protocol/v2/schema.mdx) may help a client communicate with compatible agents. Its cancellation method describes stopping ongoing work. Cancellation must not be relabeled as lossless pause/resume.

## Adapter acceptance criteria

Before an adapter claims a capability, an automated or repeatable manual test should show:

- the operation fires on the documented host runner and session type;
- a deferred task really did not start;
- a started or continued task can be linked to the intended host session;
- failures and reconnections do not duplicate starts or human responses;
- host-native approvals still require the host's normal authorization;
- when a capability is missing, users see a truthful degraded mode.
