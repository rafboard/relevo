# Proposed roadmap

This is a sequence for proving the idea, not a release schedule or a claim that work has already begun.

## 0. Define the baseline

Use a simple task board, a limit on simultaneous tasks, and fixed human check-ins as the comparison. Record which tasks begin, block, finish, get verified, and require correction. If Relevo only batches notifications, the extra complexity is not justified.

## 1. Test decision handoff before building a planner

- Map the actual decision-request and answer-delivery surfaces of two hosts, including observer-only paths.
- Specify a minimal decision record: origin, question, alternatives, evidence references, expected check-in, human response, and host acknowledgment.
- Build a local inbox and CLI that link to native tasks and permission UIs instead of replacing them.
- Demonstrate a real human-attributed response returning to a supported host path. If a host does not permit this, link back to its own UI.
- Handle duplicate events, disconnected hosts, and unknown delivery state without inventing a successful handoff.

No task-board migration, dashboard, predictive model, cloud service, or cross-device sync is required for this stage.

## 2. Test decision-aware admission on one real host boundary

Add deterministic start/prepare/defer recommendations with reasons. Demonstrate an actual pre-start gate on one documented launch path, not just a notification after a task has begun. If the host cannot gate safely, ship it honestly as observe-and-advise and look for another host for the gating experiment. Make bypass paths visible.

## 3. Prove portability with a different host

Complete the second adapter using a different extension style. Run the same decision scenarios through both. The core contract should need little or no host-specific change; any capability gap should appear as an explicit degraded mode. Do not infer that admission support follows from decision-handoff support.

## 4. Evaluate usefulness

Compare with the baseline on completed and verified work, human interruptions, blocked time, missed deadlines, and correction rate. Include deferred tasks in the denominator. A faster-looking dashboard with no meaningful improvement is a reason to simplify or stop.

## Open technical questions

- Can a host reliably expose a task *before* launch, or only report work already running?
- What minimal metadata predicts a human decision without inspecting private prompts or code?
- Can a session be continued safely after a long wait, or must the person restart in the host UI?
- How should conflicting deadlines and availability windows be surfaced without silently lowering permissions?
- What should the core do when two adapters report stale or contradictory state?

The answers should come from adapter prototypes and user testing, not from assuming all agent harnesses have the same lifecycle.
