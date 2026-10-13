# Proposed roadmap

This is a sequence for proving the idea, not a release schedule or a claim that work has already begun.

## 0. Define the baseline

Use a simple task board, a limit on simultaneous tasks, and fixed human check-ins as the comparison. Record which tasks begin, block, finish, get verified, and require correction. If Relevo only batches notifications, the extra complexity is not justified.

## 1. Build the smallest local core

- Versioned task and event schemas with example fixtures.
- A local task ledger and human decision inbox.
- Deterministic admission recommendations with reasons: start, prepare, or defer.
- A CLI for proposing a task, setting a check-in window, viewing status, and explicitly resolving a decision.
- Recovery behavior for duplicate events and unknown host status.

No dashboard, predictive model, cloud service, or cross-device sync is required for this stage.

## 2. Prove one real host boundary

Build one adapter against a documented host surface. Demonstrate an actual pre-start admission decision, not just a notification after a task has already begun. If the host cannot gate safely, ship it honestly as observe-and-advise and look for another host for the gating experiment.

## 3. Prove portability with a different host

Implement a second adapter using a different extension style. Run the same task scenarios through both. The core contract should need little or no host-specific change; any capability gap should appear as an explicit degraded mode.

## 4. Evaluate usefulness

Compare with the baseline on completed and verified work, human interruptions, blocked time, missed deadlines, and correction rate. Include deferred tasks in the denominator. A faster-looking dashboard with no meaningful improvement is a reason to simplify or stop.

## Open technical questions

- Can a host reliably expose a task *before* launch, or only report work already running?
- What minimal metadata predicts a human decision without inspecting private prompts or code?
- Can a session be continued safely after a long wait, or must the person restart in the host UI?
- How should conflicting deadlines and availability windows be surfaced without silently lowering permissions?
- What should the core do when two adapters report stale or contradictory state?

The answers should come from adapter prototypes and user testing, not from assuming all agent harnesses have the same lifecycle.
