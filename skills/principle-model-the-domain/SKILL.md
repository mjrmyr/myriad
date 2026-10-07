---
name: principle-model-the-domain
description: Apply when writing stateful logic, branching on the same concept in several places, or repeating a shape assumption across files.
disable-model-invocation: true
---

# Model the domain

Represent the real concept directly. Use a clear type, table, state machine, map, or boundary instead of scattered conditionals.

Use this principle when:

- Logic branches on the same concept in several places.
- State has lifecycle rules.
- Data shape matters.
- Several files must agree on the same hidden rule.
- A second boolean must stay in sync with the first.

Reach for structures like:

- A state machine for lifecycle rules.
- A typed object for a real domain concept.
- A map, registry, lookup table, or discriminated union for repeated branching.
- A reducer or command model for state transitions.
- A module boundary that owns one set of invariants.

Do not force an abstraction. Boring local code is better when the current shape is clear and unlikely to grow.

The test:

The code should make invalid states harder to represent, not just move the conditionals somewhere else.
