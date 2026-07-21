# Test Doubles and Real Dependencies

Use this reference when choosing real implementations, in-memory components, fakes, stubs, spies, mocks, or protocols.

## Contents

- [Core model](#core-model)
- [Start with real collaboration](#start-with-real-collaboration)
- [Real implementations](#real-implementations)
- [In-memory implementations](#in-memory-implementations)
- [Fakes, stubs, spies, and mocks](#fakes-stubs-spies-and-mocks)
- [Protocols and test seams](#protocols-and-test-seams)
- [Interaction assertions](#interaction-assertions)
- [Boundary placement](#boundary-placement)
- [Decision workflow](#decision-workflow)
- [Common mistakes](#common-mistakes)
- [Review checklist](#review-checklist)

## Core model

A test double replaces a dependency so a test can control inputs, observe effects, or avoid slow and unstable behavior.

Do not replace dependencies by default.

First ask whether the real implementation is:

- deterministic;
- lightweight;
- safe;
- easy to configure;
- part of the behavior being tested.

If so, keeping it real usually produces a simpler and more valuable test.

Use a double only when it solves a specific problem.

## Start with real collaboration

Prefer real collaboration between deterministic components when several types jointly implement one behavior.

This is especially useful when:

- replacing collaborators would reproduce production orchestration in test code;
- the main risk lies in how components work together;
- the real implementations are cheap and reliable.

Excessive doubles can make a test verify assumptions about collaboration rather than the collaboration itself.

Treat extensive mocking as a signal to reconsider the test boundary.

## Real implementations

Prefer real implementations for:

- pure domain logic;
- validators and mappers;
- lightweight use cases;
- deterministic coordinators;
- state reducers;
- simple repositories backed by in-memory state;
- components whose collaboration is part of the behavior under test.

Benefits:

- less setup;
- fewer test-only abstractions;
- stronger regression protection;
- better resilience to refactoring.

Do not keep a real implementation only for realism when it makes the test slow, destructive, flaky, or difficult to configure.

## In-memory implementations

An in-memory implementation preserves useful behavior without using the real external system.

Good candidates:

- repositories;
- caches;
- queues;
- key-value stores;
- lightweight persistence abstractions;
- event collectors.

Prefer an in-memory implementation over a mock when:

- stored state affects later behavior;
- multiple operations must interact;
- reads and writes are part of the scenario;
- realistic collaboration matters more than verifying calls.

Do not use it when the real integration is itself the risk, such as migrations, serialization, or database constraints.

## Fakes, stubs, spies, and mocks

### Fakes

A fake is a working but simplified implementation.

Use one when tests need reusable, stateful behavior, such as a fake repository, controllable clock, or scheduler.

Keep it focused. Do not recreate the full production system.

### Stubs

A stub returns configured values or errors.

Use one when the test only needs to control dependency output, such as a fixed response, date, feature flag, or failure.

If the stub grows stateful or heavily conditional, prefer a fake.

### Spies

A spy records interactions for later inspection.

Use one when the interaction itself is an observable effect, such as:

- an analytics event;
- a navigation request;
- a persistence command;
- a generated external payload.

Assert meaningful payloads rather than incidental call counts.

### Mocks

A mock encodes expected interactions and fails when expectations are not met.

Use one sparingly when:

- a call must occur or must not occur;
- ordering is contractually significant;
- the behavior cannot be observed through a more stable outcome.

Avoid mocks that duplicate implementation details, exact helper calls, or temporary orchestration.

Prefer state or outcome verification when possible.

## Protocols and test seams

Do not introduce a protocol solely because a concrete type appears in a test.

Before adding one, consider:

- using the real implementation;
- injecting configuration or data;
- using an in-memory implementation;
- widening the test boundary;
- extracting a genuine external boundary.

A protocol is justified when it represents:

- an external system;
- multiple real implementations;
- a replaceable infrastructure boundary;
- a concurrency or isolation boundary;
- behavior that must vary independently.

Avoid protocol-first design around every service, use case, or repository.

A test seam should reflect an architectural boundary, not create one artificially.

## Interaction assertions

Use interaction assertions only when the interaction is part of the behavior being protected.

Good targets:

- emitted events;
- generated requests;
- commands sent to external systems;
- absence of a prohibited side effect;
- contractually required ordering.

Avoid assertions about:

- private calls;
- incidental helper calls;
- exact internal call counts;
- temporary orchestration details;
- interactions already proven by final observable state.

Prefer one meaningful assertion over a transcript of internal execution.

## Boundary placement

Place doubles at unstable edges rather than throughout the internal graph.

Common replacement boundaries:

- networking;
- persistent storage;
- clocks and calendars;
- randomness;
- push notifications;
- analytics SDKs;
- system services;
- third-party SDKs.

Keep internal deterministic components connected whenever practical.

This preserves realism while keeping tests fast and controlled.

## Decision workflow

1. Define the behavior the test protects.
2. List the dependencies involved.
3. Keep deterministic, lightweight dependencies real.
4. Replace only slow, destructive, nondeterministic, or externally controlled boundaries.
5. Prefer an in-memory implementation when stateful collaboration matters.
6. Choose the least powerful double that solves the problem.
7. Prefer observable outcomes over interaction assertions.
8. Reconsider the boundary if mocks reproduce production collaboration.
9. Verify that the double does not hide the real risk.
10. Remove test-only abstractions that no longer provide value.

## Common mistakes

- Mocking every dependency by default.
- Creating one protocol per concrete type.
- Treating mocks as proof of good architecture.
- Reproducing production orchestration in test setup.
- Verifying incidental call order.
- Using a stub when stateful behavior is required.
- Using a fake when the real integration is the risk.
- Keeping real dependencies that make tests flaky or destructive.
- Building a fake that becomes a second production system.
- Adding test seams with no architectural meaning.

## Review checklist

Before recommending a double, check:

- What problem does replacement solve?
- Can the real implementation remain?
- Would an in-memory implementation preserve useful behavior?
- Is a simple stub enough?
- Is interaction observation actually required?
- Is a mock encoding behavior or implementation detail?
- Does the double hide the real risk?
- Does the protocol represent a real boundary?
- Will the test survive safe refactoring?
- Is the setup simpler than the collaboration it replaces?
