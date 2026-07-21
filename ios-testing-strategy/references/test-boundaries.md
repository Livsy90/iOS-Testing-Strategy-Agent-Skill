# Test Boundaries

Use this reference when choosing between narrow unit tests, feature-level tests, integration tests, and system-wide tests.

## Contents

- [Core model](#core-model)
- [Start from behavior](#start-from-behavior)
- [Choose the highest practical boundary](#choose-the-highest-practical-boundary)
- [Narrow unit tests](#narrow-unit-tests)
- [Feature-level tests](#feature-level-tests)
- [Integration tests](#integration-tests)
- [System-wide tests](#system-wide-tests)
- [Boundary warning signs](#boundary-warning-signs)
- [Real components and replaced boundaries](#real-components-and-replaced-boundaries)
- [Volatility and design maturity](#volatility-and-design-maturity)
- [Decision workflow](#decision-workflow)
- [Common mistakes](#common-mistakes)
- [Review checklist](#review-checklist)

## Core model

A test boundary is the portion of the system exercised as one test.

Choose it from the behavior that must be protected, not from a type name or framework label. XCTest or Swift Testing can exercise one function, one domain object, a feature slice, or several connected domains without launching the UI.

The framework does not determine the boundary.

## Start from behavior

First define:

- the user-visible outcome, business rule, or invariant;
- the failure the test should detect;
- the stable entry point through which the behavior can be exercised;
- the observable result that proves it worked.

Prefer assertions about returned values, observable state, persisted data, emitted effects, meaningful errors, or user-visible outcomes.

Avoid starting with “this class needs tests.” A class may be only one implementation detail inside a larger behavior.

## Choose the highest practical boundary

Prefer the broadest boundary that remains:

- fast enough for its intended feedback loop;
- deterministic;
- safe and easy to configure;
- understandable when it fails.

A broader boundary verifies real collaboration and may remove the need to reproduce that collaboration with mocks.

Narrow or split the test when a broader boundary causes slow execution, flakiness, difficult diagnosis, or heavy setup.

## Narrow unit tests

Use a narrow test when the behavior is naturally isolated.

Good candidates:

- calculations and algorithms;
- parsers and validators;
- combinatorial edge cases;
- deterministic state transitions;
- small rules with many input variations.

Benefits:

- fast feedback;
- precise failures;
- simple setup;
- efficient edge-case coverage.

Risks:

- coupling tests to implementation details;
- testing every type in isolation;
- unnecessary mocks;
- missing errors in real collaboration.

Use a narrow boundary when it improves speed or diagnosis without removing the behavior that matters.

## Feature-level tests

A feature-level test exercises a meaningful slice of application behavior through a stable feature entry point.

It may include a use case, domain logic, mappers, coordinators, state containers, effect handling, and in-memory repositories.

Prefer this boundary when several components jointly implement one outcome and their collaboration is part of the risk.

Feature-level tests often provide a useful balance between realism, speed, refactoring resilience, and diagnostic value.

Do not split a feature into isolated tests merely because each collaborator is a separate type.

## Integration tests

Use an integration test when the main risk lies between real subsystems or technologies.

Typical cases:

- persistence mapping and migrations;
- request and response encoding;
- database, file storage, or keychain behavior;
- SDK adapters;
- module contracts;
- application code connected to a controlled local service.

Use the real boundary when an in-memory replacement would hide the behavior being validated.

Keep integration tests focused, and separate slower suites from fast local feedback when necessary.

## System-wide tests

A system-wide test exercises a large connected part of the application through a unit-test runner rather than full UI automation.

It may connect multiple real domains and application services while replacing only boundaries that make the test slow, unsafe, or nondeterministic.

Use this boundary when:

- behavior spans several domains;
- the main risk is orchestration or collaboration;
- narrow tests would require extensive mocks;
- the scenario can remain fast and deterministic;
- the observable outcome is more stable than the internal structure.

Benefits:

- verifies real collaboration;
- survives more internal refactoring;
- requires fewer interaction assertions;
- protects complete business or user scenarios.

Limitations:

- failures may be harder to localize;
- setup and runtime may grow.

System-wide does not mean “test everything.” Keep each scenario focused on one meaningful behavior.

## Boundary warning signs

A boundary may be too narrow when:

- mocks reproduce most production collaboration;
- assertions focus on call order or method counts;
- safe refactoring breaks many tests without changing behavior;
- isolated tests pass while real collaboration remains unverified;
- the tested type has little meaning outside the feature flow.

A boundary may be too broad when:

- failures do not identify the likely cause;
- setup hides the behavior under test;
- the suite is too slow for its intended workflow;
- external services make it flaky;
- one scenario asserts many unrelated behaviors;
- small edge cases require running the whole system.

Broaden the test when isolation creates duplication. Narrow it when speed, diagnosis, or focused edge-case coverage requires it.

## Real components and replaced boundaries

Keep real implementations when they are deterministic, lightweight, safe, and part of the behavior under test.

Common candidates:

- pure domain logic;
- use cases;
- mappers and validators;
- state reducers;
- lightweight coordinators;
- in-memory stores.

Replace dependencies that are externally controlled, slow, destructive, nondeterministic, or difficult to configure safely.

Common replacement boundaries:

- network services;
- persistent storage;
- clocks and calendars;
- randomness;
- system APIs;
- external SDKs.

Do not introduce a protocol solely because a type appears in a test. First consider the real implementation, configuration injection, an in-memory implementation, or a broader test boundary.

## Volatility and design maturity

Match the boundary to the maturity of the code.

For volatile or exploratory code:

- protect stable invariants;
- test risky algorithms;
- avoid freezing temporary UI composition or orchestration;
- prefer outcomes likely to survive redesign.

As behavior and boundaries stabilize:

- add broader feature regression tests;
- verify collaboration between domains;
- reduce reliance on manual verification.

Do not leave high-risk stable rules untested because the surrounding feature is still changing.

## Decision workflow

1. Define the behavior or risk.
2. Identify its stable entry point.
3. Start with the broadest boundary that could exercise it.
4. Check speed, determinism, setup cost, and diagnostic value.
5. Keep real deterministic components connected.
6. Replace only unstable external boundaries.
7. Narrow the test for edge cases, speed, or diagnosis.
8. Broaden it when mocks duplicate collaboration or implementation coupling dominates.
9. Assert outcomes rather than internal structure.
10. Place the test in the appropriate local or CI feedback loop.

## Common mistakes

- Treating every class as an independent test unit.
- Assuming a unit-test target must test one type.
- Mocking all dependencies by default.
- Exposing private APIs only to test them.
- Using call order as a substitute for behavioral assertions.
- Building system-wide tests around many unrelated scenarios.
- Using real external services in fast deterministic suites.
- Choosing boundaries from coverage targets rather than risk.
- Keeping a narrow test after its setup becomes more complex than the real collaboration.
- Keeping a broad test after it becomes slow or impossible to diagnose.

## Review checklist

Before recommending a boundary, check:

- What behavior or risk does the test protect?
- Is the entry point stable and meaningful?
- Would a broader boundary verify useful collaboration?
- Would a narrower boundary improve speed or diagnosis?
- Which components can remain real?
- Which boundaries must be replaced?
- Are assertions observable and refactoring-resistant?
- Is the test fast enough for where it will run?
- Will a failure provide actionable information?
- Is the maintenance cost proportionate to the protected risk?
