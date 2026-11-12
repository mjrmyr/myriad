---
name: principle-behavior-first
description: Apply when investigating a mismatch, designing a change, or proposing a new tool. Establish expected behavior independently of the current implementation before choosing a repair.
disable-model-invocation: true
---

# Behavior first

Start with what should happen, not with how the current code could be extended.
The implementation is evidence of what happens today, not authority for what should happen.

- Describe the expected observable behavior independently of a proposed API or implementation. Ground it in the user's goal, existing contracts, and relevant callers. Distinguish established intent from assumptions.
- For a suspected defect, obtain concrete evidence of the mismatch: a reproduction, failing behavioral test, trace, or observed state. For a new capability, identify the missing behavior rather than treating the absence of a particular tool as the problem.
- Trace intended state ownership and lifetime. Which entity should own the identity or invariant? What should survive a retry, restart, replacement worker, or concurrent actor? Compare that with where the implementation actually stores and changes it.
- Compare the smallest repair at that owner with a caller workaround or new tool. Prefer restoring the intended behavior over exposing an implementation accident as a new concept. A workaround can be justified by real constraints, but make its limits clear.
- Where relevant, turn the expected behavior into runnable behavioral or lifecycle tests. Assert the user-visible result and what survives transitions, not merely that a new helper was called.

Use [Fix root causes](../principle-fix-root-causes/SKILL.md) for tracing and repair, and [Prove it works](../principle-prove-it-works/SKILL.md) for verification. This is a reasoning aid, not a required report format or approval step. Scale the investigation to the uncertainty and risk. Skills guide judgment, not enforce correctness.

## Example: publication after worker replacement

A replacement worker cannot continue publishing an existing ticket's pull request.
Before proposing `github_resume`, establish the expected behavior: continuing the same ticket should retain its publication identity even when its worker changes. Publication identity belongs to the ticket; the checkout is worker-local.

Reproduce publication before and after worker replacement, then trace where publication identity is stored and recovered. Compare repairing ticket-owned identity with adding a resume tool around worker-owned state. Do not infer the need for a new public tool from the current storage layout.

A lifecycle test should publish from one worker, replace it, and publish again for the same ticket, asserting that the existing pull request is reused. Also check that a different ticket does not inherit that identity. These outcomes matter more than the name or call sequence of a helper.
