---
name: ios-testing-strategy
description: Use this skill when designing, reviewing, or improving automated tests for an iOS feature or codebase, including test boundaries, system-wide tests, test doubles, coverage priorities, volatile code, TDD, and test maintenance. Do not use it for general XCTest or Swift Testing syntax unless the task also involves testing strategy or architecture.
---

# iOS Testing Strategy

## Purpose

Use this skill to choose high-value test boundaries, protect meaningful behavior, and avoid test suites that are slow, fragile, or coupled to implementation details.

## When to use this skill

Use this skill when the task involves:

- deciding what to test and at which level;
- unit, integration, system-wide, or cross-domain tests;
- excessive mocks, stubs, spies, protocols, or test-only abstractions;
- brittle tests that break during refactoring;
- coverage priorities or uneven test coverage;
- prototypes, volatile features, or rapidly changing architecture;
- choosing whether to use TDD;
- reducing setup, fixtures, boilerplate, flakiness, or execution time;
- choosing between automated and manual verification.

## When not to use this skill

Do not use this skill for:

- testing framework syntax with no strategy question;
- UI automation mechanics with no architectural concern;
- performance profiling;
- debugging a defect unless the task also asks how to prevent regression.

## Core workflow

1. Define the behavior, invariant, or failure risk that needs protection.
2. Identify a stable entry point through which the behavior can be exercised.
3. Choose the broadest boundary that remains fast, deterministic, and diagnosable.
4. Keep real deterministic domain and application components connected.
5. Replace only slow, destructive, nondeterministic, or externally controlled boundaries.
6. Assert observable outputs, state, persisted data, emitted effects, or user-visible outcomes.
7. Evaluate execution time, diagnostic value, flakiness, setup cost, and expected maintenance.
8. Recommend the smallest suite that provides sufficient confidence.
9. State whether the test belongs in local feedback, pull-request validation, or a slower CI suite.

## Decision rules

### Test boundaries

- Test meaningful behavior rather than every type in isolation.
- Prefer a broader test when several components jointly implement one behavior or mocks would reproduce most of their collaboration.
- Prefer a narrower test for algorithms, combinatorial cases, or logic that needs many fast examples.
- Do not equate XCTest or Swift Testing with testing one class; a unit-test target can exercise a complete feature slice.
- Reject a broad test when it becomes slow, flaky, or too difficult to diagnose.

### Real dependencies and test doubles

- Prefer real implementations for deterministic, lightweight components whose collaboration is part of the behavior.
- Prefer in-memory stores or repositories when they preserve useful behavior without external cost.
- Use a fake, stub, spy, or mock for networking, storage, clocks, randomness, system APIs, external SDKs, or other unstable boundaries.
- Use interaction assertions only when the interaction itself is part of the contract.
- Do not introduce a protocol solely because a type appears in a test.

### Prioritization

Prioritize:

- business-critical, security-sensitive, financial, authentication, authorization, migration, and data-loss risks;
- complex calculations and state transitions;
- behavior with a history of regressions;
- scenarios that are slow, repetitive, or unreliable to verify manually;
- stable invariants inside otherwise volatile features.

Deprioritize:

- disposable prototypes and temporary adapters;
- rapidly changing UI composition;
- implementation details likely to be rewritten;
- trivial forwarding code already covered by a broader scenario.

Treat coverage as a diagnostic signal, not the objective. Allocate testing effort by risk, volatility, and maintenance cost rather than uniformly across modules.

### Volatile code and TDD

- During exploration, protect critical invariants and risky algorithms without freezing temporary structure.
- Add broader regression tests as behavior and boundaries stabilize.
- Use TDD when requirements and outcomes are clear enough for tests to guide design.
- Do not prescribe TDD for exploratory UI work, architectural spikes, or rapidly changing product flows.
- Defer a test only when the accepted risk and alternative validation are explicit.

### Assertions and maintainability

- Assert returned values, observable state, persisted data, emitted effects, meaningful errors, and recovery behavior.
- Avoid assertions about private methods, incidental call order, exact object graphs, or temporary decomposition.
- Keep setup close to the behavior under test.
- Extract helpers only when they reduce repetition without hiding intent.
- Keep fixtures minimal and scenario-specific.
- Split slow suites from fast feedback tests rather than weakening all tests.

## Gotchas

- Extensive mocking may indicate that the test boundary is too narrow.
- A broad test is not automatically better if failures are difficult to localize.
- Real databases, clocks, networks, and system services can introduce flakiness.
- Tests coupled to internal calls may block safe refactoring without protecting behavior.
- High coverage can coexist with missing critical scenarios.
- Rapid delivery is not itself a reason to skip tests; make the accepted risk explicit.
- A test suite developers rarely run provides limited feedback value.

## Output expectations

When reviewing a testing strategy, respond with:

1. The behavior or risk that should be protected.
2. The recommended test boundary and why.
3. Which dependencies should remain real and which should be replaced.
4. The key scenarios and observable assertions.
5. Trade-offs, maintenance risks, and exceptions.
6. How and where the tests should run.

When reviewing existing tests, also identify implementation coupling, unnecessary doubles, missing high-risk scenarios, duplicated setup, slow or flaky boundaries, and tests that should be removed, widened, or narrowed.

## References

Read these only when relevant:

- `references/test-boundaries.md` — read when choosing between narrow unit tests, feature-level tests, integration tests, and system-wide tests.
- `references/test-doubles-and-real-dependencies.md` — read when choosing real implementations, in-memory components, fakes, stubs, spies, mocks, or protocols.
- `references/risk-based-test-prioritization.md` — read when deciding what to test first, how deeply to test it, or how to distribute effort across domains.
- `references/volatile-code-and-tdd.md` — read when testing prototypes, unstable features, architectural spikes, or deciding whether TDD is appropriate.
- `references/assertions-and-refactoring-resilience.md` — read when tests depend on private methods, call order, interaction counts, or other implementation details.
- `references/test-suite-maintainability.md` — read when reducing fixtures, setup, boilerplate, execution time, flakiness, or CI feedback time.
- `references/coverage-and-manual-testing.md` — read when discussing coverage targets, automation gaps, or the trade-off between automated and manual verification.
