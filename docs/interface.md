# Interface proposals

**Status:** product design, not an implemented interface. Relevo should fit the tools people already use. The three options below can share one local core and the same task/event contract; they differ in how a person sees and controls it.

## 1. Host-native commands and cards

Relevo appears inside each agent harness through a small plugin, command, or extension. A person might type `/relevo plan`, `/relevo status`, or `/relevo inbox`. A pending decision appears next to the relevant host session, with a link to the underlying work.

**Example:** In an existing agent conversation, the person opens `/relevo inbox` and sees: "Checkout fix: choose the minimal patch or the broader refactor." The answer is captured by a host-native, human-attributed action and returned to Relevo.

**Strength:** Almost no context switch. **Cost:** Every host needs a separate UI implementation; some hosts may not offer a reliable way to distinguish a person's explicit action from model-generated text. In that case, the host view must be read-only and direct the person to another trusted decision surface.

## 2. One local CLI, with an optional terminal UI

A `relevo` command is the consistent interface across hosts. It can set up adapters, show capabilities, schedule check-ins, inspect admission reasons, and resolve decision requests. A terminal UI could be added without changing the underlying commands.

Illustrative commands, **not implemented**:

```text
relevo setup
relevo connect <host>
relevo plan
relevo status
relevo inbox
relevo decide <request-id> --choice <option-id>
```

`relevo connect` should report what the adapter has actually tested: observe, gate new task starts, start work, read status, and continue a session. `relevo plan` should show *why* a task is proposed for start, preparation, or deferral. `relevo decide` is a direct human action; an agent-facing MCP tool must not be allowed to impersonate it.

**Strength:** One portable reference interface and a good foundation for documentation, scripting, and conformance tests. **Cost:** Terminal-first interaction is less convenient on a phone or for people who prefer visual setup.

## 3. Optional local web control panel

A browser interface on localhost could show a small cross-host board: proposed tasks, active tasks, deferred tasks, pending decisions, and the next check-in. A setup wizard could help connect adapters and edit preferences. It would use the same local core; it should not require a hosted account or become a second agent chat application.

**Strength:** Easier to understand scheduling and dependencies across several tools. **Cost:** More implementation and maintenance, another window to check, and a larger security boundary. It risks becoming the dashboard Relevo was meant to avoid if it merely lists sessions instead of making admission and decision timing useful.

## Recommended starting point

Build **option 2 as the canonical control surface**, plus the smallest useful slice of **option 1** in the first adapter: status, pending decisions, and a link back to the host session. Add a native decision action only where human attribution is reliable. Defer option 3 until user testing shows the CLI and host surface cannot adequately explain or manage cross-host work.

The daily flow should be short:

1. Connect a host once and see its real capabilities.
2. Set a default active-task limit and the next expected check-in.
3. Review the admission plan: start, prepare, or defer, each with a reason.
4. At check-in, answer a compact decision inbox. Unanswered requests stay pending.
5. See separate states for agent completion, verification, and human acceptance.

## Configuration boundaries

The CLI should manage a **private per-user configuration** for time zone, check-in preferences, active-task limit, connected adapters, and notification preferences. An optional project file may hold non-sensitive task defaults and shared conventions, but must not contain personal availability, host credentials, or authorization tokens. The exact file paths and format should be selected with an implementation prototype rather than treated as a stable API now.

Relevo's preferences are scheduling policy, **not** a replacement for the host's sandbox or permissions. A Relevo decision can select a task direction; it cannot silently approve a deployment, a privileged tool call, or another action requiring host-native authorization. A missed check-in never becomes consent.
