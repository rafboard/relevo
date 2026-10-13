# Draft contract v0.1

**Status:** design proposal. This is not an implemented or stable API. The first working adapters should be used to revise it before calling it v1.

The contract describes task intents, adapter capabilities, events, and human decision requests. It is deliberately smaller than a transcript format. Hosts may use any internal representation as long as their adapter maps observable facts faithfully.

## Task intent

A task may exist before a host session exists.

```json
{
  "schema_version": "0.1",
  "task_id": "task-checkout-tests",
  "summary": "Run the existing checkout test suite",
  "priority": "high",
  "deadline": null,
  "depends_on": [],
  "host": { "adapter": "example-host", "session_ref": null },
  "requires_human_decision": false
}
```

`summary` is a short human-readable label, **not** a raw prompt. `session_ref` is opaque and local to the adapter. Priority is relative to the person's other tasks; it is not an instruction to bypass host permissions. Deadlines and check-in windows, when supplied, use timestamps with explicit time zones. A task may declare a likely decision need, but observed events are authoritative if reality differs.

## Adapter capability declaration

An adapter advertises operations, not a generic "supported" flag:

```json
{
  "schema_version": "0.1",
  "adapter": "example-host",
  "capabilities": {
    "observe": true,
    "gate_start": false,
    "start": true,
    "read_status": true,
    "continue_session": false
  }
}
```

The declaration describes a tested integration mode, not every possible feature of the host. `gate_start: false` means Relevo may recommend deferral but cannot enforce it against a task started directly in that host. `continue_session: false` means a person may need to return to the host UI. Capabilities can change by version, configuration, or session type.

## Events

Events carry `schema_version`, `event_id`, `task_id`, `occurred_at`, `type`, and a small type-specific payload. Proposed event types:

- `task.proposed`
- `admission.recommended` (start, prepare, or defer; with reason)
- `task.started`, `task.blocked`, `task.finished`
- `decision.requested`, `decision.resolved`
- `task.verified`, `task.accepted`
- `adapter.degraded`

`task.finished` is a host-reported terminal observation, not proof of correct output. `task.verified` requires a recorded verification result. `task.accepted` represents the person's acceptance and must not be inferred from either of the preceding events.

For example:

```json
{
  "schema_version": "0.1",
  "event_id": "event-17",
  "task_id": "task-checkout-tests",
  "occurred_at": "2026-10-13T09:00:00+02:00",
  "type": "admission.recommended",
  "payload": { "action": "start", "reason": "Independent work within the active-task limit" }
}
```

The sender must make retries distinguishable from new events, and consumers must tolerate duplicate delivery. Event ordering across different hosts is not assumed. A state projection must account for missing, delayed, or contradictory observations and expose uncertainty rather than inventing a clean history.

## Decision requests

A decision request contains a question, optional alternatives, a task link, and the host-native place where the person can inspect the underlying work. A resolution records the explicit human response and its provenance. It must not contain a model's guess that the person would approve. Expiry means "needs attention" or "cancelled by a stated policy," never "approved."

## Transport

The contract is JSON-compatible so adapters can be written in different languages. Local IPC, a CLI, or an MCP-facing wrapper are possible transports; none is mandatory in v0.1. JSON Schema and conformance fixtures should be added with the first implementation rather than treating examples here as a complete specification.
