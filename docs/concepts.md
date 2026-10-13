# Concepts and workflow

Relevo coordinates agent work around *human decision windows*: periods when the person expects to review choices, correct direction, or grant a host-native permission. A window is a planning signal, **not** consent or a promise that the person will answer.

## Terms

- **Task intent:** Work proposed but not necessarily started. It carries a human-readable goal, priority, optional deadline, dependencies, and an opaque reference to its host.
- **Admission:** A decision to start, prepare, or defer a task *before* starting it. This is distinct from pausing a running agent.
- **Decision request:** A concrete question, with options and consequences where possible, that only the person can resolve.
- **Check-in:** A time when pending decision requests may be presented together. An unanswered request remains pending.
- **Outcome:** What happened to a task. Agent completion, local verification, and human acceptance are separate observations.
- **Adapter:** A host-specific bridge. It reports its capabilities and maps host events into Relevo's common vocabulary.

## A worked example

At 09:00, the person says: "I can review choices at 10:00 for ten minutes. Fix checkout first. Run no more than two agents at once. You may investigate and test, but do not deploy."

Three task intents are proposed:

1. Run the existing checkout tests. This can proceed without a new human decision.
2. Investigate two possible fixes. This can proceed, but the final design choice belongs to the person.
3. Refactor an unrelated module. It is low priority and may soon ask for guidance.

Relevo proposes admitting the first two tasks and deferring the third. The second agent returns two alternatives. At 10:00, the person sees one short decision request with the alternatives, evidence, and a link back to the host session. They choose one. The host's own controls—not Relevo's assumptions—determine whether and how that session continues. If the person does not answer, the choice stays pending and no deployment is authorized.

The deferred third task is still recorded. That matters: a list of running sessions alone cannot explain work that was deliberately never started.

## Admission outcomes

- **Start:** The task appears useful and fits the current limits. Starting requires a host capability or an explicit action by the person.
- **Prepare:** Gather evidence or alternatives that are already allowed, then stop at a human decision boundary.
- **Defer:** Do not start yet; record the reason and a reconsideration trigger. A deadline conflict should be surfaced, not silently hidden to preserve the schedule.

Admission is a recommendation until a host adapter demonstrates it can reliably gate task starts. Existing sessions that Relevo did not create may be observable but not controllable.

## What success would mean

Fewer tasks stuck waiting for avoidable human input **and** more work finished and verified, compared with a simple task board, a fixed work-in-progress limit, and scheduled check-ins. Merely batching notifications is not enough.
