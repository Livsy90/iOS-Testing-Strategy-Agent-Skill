# Test Suite Maintainability

Use this reference when reducing fixtures, setup, boilerplate, execution time, flakiness, or CI feedback time.

## Contents

- [Core model](#core-model)
- [Keep setup proportional](#keep-setup-proportional)
- [Fixtures and builders](#fixtures-and-builders)
- [Shared helpers](#shared-helpers)
- [Test data](#test-data)
- [Execution time](#execution-time)
- [Flakiness](#flakiness)
- [CI feedback loops](#ci-feedback-loops)
- [Removing and reshaping tests](#removing-and-reshaping-tests)
- [Decision workflow](#decision-workflow)
- [Common mistakes](#common-mistakes)
- [Review checklist](#review-checklist)

## Core model

A test suite should provide useful confidence without becoming a second system that is expensive to understand and maintain.

Maintainability depends on:

- clear intent;
- focused setup;
- stable assertions;
- deterministic execution;
- proportionate runtime;
- low duplication;
- useful failure messages.

Do not optimize for the number of tests. Optimize for confidence, feedback speed, and long-term maintenance cost.

## Keep setup proportional

Test setup should be simpler than the production collaboration it exercises.

Prefer:

- constructing only the components needed for the scenario;
- keeping scenario-specific setup close to the test;
- using real lightweight components where practical;
- replacing only unstable external boundaries;
- expressing defaults explicitly but briefly.

Reconsider the boundary when setup:

- reproduces most production orchestration;
- requires many unrelated mocks;
- hides the behavior under test;
- changes after every internal refactor;
- is harder to understand than the production path.

A broader test may remove duplicated mock setup. A narrower test may reduce an oversized system fixture.

## Fixtures and builders

Use fixtures and builders to reduce meaningful repetition, not to hide intent.

A useful builder:

- provides sensible defaults;
- lets the test override only relevant values;
- produces valid data by default;
- has a small, stable API;
- keeps the scenario readable.

A useful fixture:

- represents one clear state;
- contains only data needed by the scenario;
- is easy to inspect;
- avoids unrelated fields and history.

Avoid:

- one universal fixture for every test;
- large object graphs with hidden defaults;
- mutable shared fixtures;
- builders that mirror every production initializer;
- fixture inheritance.

Prefer local construction when abstraction would save only a few obvious lines.

## Shared helpers

Extract a helper when it:

- removes repeated setup across several tests;
- preserves the behavior being described;
- makes failure diagnosis easier;
- exposes only meaningful configuration.

Keep setup inline when it is specific to one scenario.

Avoid helpers that:

- hide assertions;
- perform several unrelated actions;
- encode one test's assumptions as global defaults;
- require readers to jump through multiple files;
- return partially configured objects.

Name helpers by intent, such as `makeAuthenticatedSession()` or `givenStoredAccount()`, rather than generic names such as `setup()`.

Do not build deep test utility hierarchies.

## Test data

Keep test data minimal and intentional.

Use values that make failures easy to understand.

Prefer:

- small collections;
- explicit identifiers;
- stable dates;
- focused payloads;
- one variation per scenario.

Avoid:

- production-sized fixtures in fast tests;
- random data without a recorded seed;
- timestamps based on the current clock;
- unrelated fields copied from real payloads;
- opaque fixture files when inline data would be clearer.

Use larger or realistic datasets only when scale, parsing, migration, or integration behavior is the risk being tested.

## Execution time

Fast feedback encourages developers to run tests frequently.

Keep the default local suite focused on tests that are:

- deterministic;
- isolated from real external services;
- quick to configure;
- valuable during normal development.

Investigate slow tests before adding parallelism or increasing CI resources.

Common causes include:

- repeated application bootstrap;
- unnecessary database creation;
- real network or system services;
- oversized fixtures;
- broad scenarios repeated for small edge cases;
- waiting for time instead of controlling time;
- redundant coverage.

Split a broad scenario from narrow edge-case tests when repeatedly exercising the full system adds little value.

Do not weaken meaningful assertions merely to reduce runtime.

## Flakiness

A flaky test produces inconsistent results without a meaningful behavior change.

Common causes:

- real clocks and timers;
- uncontrolled concurrency;
- shared mutable state;
- test-order dependence;
- persistent data left between runs;
- real networking;
- asynchronous polling with arbitrary delays;
- locale, calendar, or time-zone dependence;
- random values without control.

Prefer:

- injected or controllable clocks;
- awaited completion conditions;
- isolated stores;
- deterministic schedulers;
- explicit locale and calendar;
- fixed seeds;
- cleanup owned by the test.

Do not solve flakiness by adding longer sleeps or automatic retries. Retries may hide a real race or an invalid test boundary.

Quarantine a flaky test only as a temporary measure with clear ownership and follow-up.

## CI feedback loops

Place tests according to the feedback speed they provide.

### Local and pull-request suite

Include tests that are:

- fast;
- deterministic;
- highly relevant to changed code;
- easy to diagnose;
- required for normal confidence.

### Slower CI suite

Move tests here when they require:

- real persistence technologies;
- migrations;
- controlled services;
- large datasets;
- broader integration;
- longer-running system scenarios.

A slower suite should still be deterministic and actionable.

Do not move unstable tests to CI merely to hide them from local development.

Prefer staged feedback:

1. fast checks;
2. feature and integration tests;
3. slower system-wide checks.

Report failures close to the responsible scenario and avoid one opaque job containing unrelated suites.

## Removing and reshaping tests

Tests are maintainable only while they protect relevant behavior.

Remove or rewrite a test when:

- the protected behavior no longer exists;
- another test provides equivalent confidence more clearly;
- it asserts obsolete implementation details;
- maintenance cost exceeds the remaining risk;
- it is permanently disabled or ignored;
- it duplicates a broader scenario without improving diagnosis.

Widen a test when mock setup reproduces real collaboration.

Narrow a test when a broad scenario is slow, flaky, or difficult to diagnose.

Do not preserve tests solely because they increase coverage.

## Decision workflow

1. Identify the behavior and risk each test protects.
2. Remove unrelated setup and data.
3. Keep scenario-specific setup close to the test.
4. Extract only repeated, intention-revealing helpers.
5. Replace unstable external boundaries.
6. Measure runtime before changing suite structure.
7. Fix sources of nondeterminism instead of adding sleeps or retries.
8. Separate fast feedback from slower integration and system checks.
9. Remove duplicate or obsolete tests.
10. Reassess maintenance cost as architecture and product behavior change.

## Common mistakes

- Creating a large shared base test class.
- Hiding scenario intent inside generic setup helpers.
- Reusing mutable fixtures across tests.
- Mocking many lightweight deterministic components.
- Using real time, randomness, or networking in fast suites.
- Adding arbitrary sleeps for asynchronous behavior.
- Retrying flaky tests without finding the cause.
- Running every test in one undifferentiated CI job.
- Keeping obsolete tests for coverage.
- Extracting abstractions before repetition is proven.
- Using production-sized data when scale is not under test.
- Treating slow tests as unavoidable without measuring them.

## Review checklist

Before approving a test suite change, check:

- Is setup proportionate to the behavior?
- Are fixtures minimal and readable?
- Do shared helpers reveal intent?
- Is mutable state isolated?
- Are time, randomness, and concurrency controlled?
- Is the test fast enough for its feedback loop?
- Does a failure identify the likely problem?
- Are sleeps or retries hiding nondeterminism?
- Is this scenario duplicated elsewhere?
- Should the test be widened, narrowed, moved, or removed?
- Is CI staging aligned with test cost and value?
- Is maintenance cost proportionate to the risk protected?
