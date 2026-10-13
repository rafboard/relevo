# Contributing

Relevo is currently a design proposal. Issues that sharpen the user problem, expose an integration constraint, or challenge an assumption are especially useful. Please keep project documentation and issue/PR descriptions in English.

## Good first contributions

- Describe a real workflow in which two or more agent tasks compete for a person's attention.
- Identify a documented, testable pre-start boundary in an existing agent harness.
- Point out an unsafe assumption in the draft task/event contract.
- Propose a smaller baseline that would solve the same problem.

For host-specific claims, link to official documentation and name the host version and execution path. Distinguish tested behavior from an inferred possibility. Do not post tokens, private transcripts, project code, or personal calendars in issues or examples; use synthetic data.

Before adding implementation code, discuss how it fits the [architecture](docs/architecture.md) and [roadmap](docs/roadmap.md). A new adapter should document its observed events, supported commands, permission boundary, and degraded behavior.
