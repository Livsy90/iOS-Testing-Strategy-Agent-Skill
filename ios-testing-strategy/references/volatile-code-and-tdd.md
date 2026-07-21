# Volatile Code and TDD

Use this reference when testing prototypes, unstable features, architectural spikes, or deciding whether TDD is appropriate.

## Contents

- [Core model](#core-model)
- [Separate behavior stability from implementation stability](#separate-behavior-stability-from-implementation-stability)
- [Testing prototypes](#testing-prototypes)
- [Testing unstable features](#testing-unstable-features)
- [Testing architectural spikes](#testing-architectural-spikes)
- [When TDD is a good fit](#when-tdd-is-a-good-fit)
- [When TDD may create churn](#when-tdd-may-create-churn)
- [What to test first](#what-to-test-first)
- [What to defer](#what-to-defer)
- [Alternative validation during exploration](#alternative-validation-during-exploration)
- [Decision workflow](#decision-workflow)
- [Common mistakes](#common-mistakes)
- [Review checklist](#review-checklist)

## Core model

Testing effort should match the maturity of the design.

Early in development, code structure may change quickly while some behavior is already stable and valuable. Protect the stable behavior without freezing temporary implementation details.

Do not treat volatility as a reason to skip all tests. Treat it as a reason to choose boundaries and depth more carefully.

## Separate behavior stability from implementation stability

A feature can have stable rules and unstable structure at the same time.

For example:

- business rules may be clear;
- UI composition may still change;
- orchestration may be temporary;
- module boundaries may still be moving;
- adapters may be disposable.

Test the stable rule through an observable outcome. Avoid asserting the exact internal decomposition used today.

Ask two separate questions:

1. Is the behavior likely to remain?
2. Is the implementation likely to remain?

A stable behavior with unstable implementation still deserves protection, but usually through a broader or more outcome-oriented test.

## Testing prototypes

A prototype exists to learn, not to preserve structure.

Use tests selectively for:

- risky algorithms;
- critical calculations;
- security-sensitive decisions;
- data transformations;
- state transitions;
- assumptions the prototype is meant to validate.

Avoid heavy infrastructure for:

- disposable UI;
- temporary adapters;
- experimental orchestration;
- code expected to be rewritten soon.

When no automated test is justified, make the alternative validation explicit, such as a manual scenario, a focused spike, or instrumentation.

## Testing unstable features

For an unstable feature, prioritize:

- stable invariants;
- externally meaningful outcomes;
- known regression risks;
- integration points unlikely to change;
- behavior that is expensive to verify manually.

Avoid detailed tests for:

- temporary view hierarchy;
- private helper structure;
- exact collaborator call order;
- provisional module boundaries;
- short-lived abstractions.

As the feature stabilizes, add broader regression coverage around the final behavior.

Do not wait for complete architectural stability if the feature already contains high-risk rules.

## Testing architectural spikes

An architectural spike explores feasibility, trade-offs, or API shape.

Tests are useful when they validate the question the spike is trying to answer.

Examples:

- whether a concurrency design preserves ordering;
- whether persistence behavior is correct;
- whether an adapter can handle a real payload;
- whether a feature boundary supports the intended scenario.

Do not build a production-grade test suite around a spike unless the code is likely to survive.

Before promoting spike code into production:

- remove disposable scaffolding;
- identify stable contracts;
- add tests around retained behavior;
- replace exploratory assertions with durable regression tests.

## When TDD is a good fit

Use TDD when:

- expected behavior is clear;
- inputs and outputs are well defined;
- the problem is algorithmic or rule-driven;
- edge cases matter;
- the API boundary is stable enough to shape;
- a regression is being fixed;
- tests can guide design without excessive rewrite.

Good candidates include:

- parsers;
- validators;
- calculations;
- state machines;
- permissions;
- domain policies;
- transformations;
- bug fixes with reproducible failure cases.

TDD is most useful when the test expresses a durable contract rather than a temporary structure.

## When TDD may create churn

TDD may be a poor fit when:

- requirements are still being discovered;
- the team is exploring multiple UI flows;
- architecture is intentionally provisional;
- interfaces are changing rapidly;
- the code may be discarded;
- the main question is feasibility rather than correctness.

In these cases, writing tests first can lock the team into assumptions before the problem is understood.

This does not make TDD wrong. It means the cost of maintaining early tests may exceed their design value.

## What to test first

In volatile code, test the parts least likely to change and most costly to get wrong.

Prioritize:

- business invariants;
- financial rules;
- authentication and authorization;
- persistence semantics;
- migrations;
- destructive operations;
- error recovery;
- state transitions;
- data conversion;
- known regressions.

Prefer a stable entry point and observable result.

Use a narrow test for isolated algorithms. Use a feature-level or system-wide test when the stable behavior spans several components.

## What to defer

Consider deferring detailed automation for:

- exploratory UI;
- temporary navigation;
- provisional coordinators;
- short-lived adapters;
- code generated for a spike;
- low-risk behavior that is cheap to verify manually;
- code likely to be removed before release.

Deferral is acceptable only when:

- the risk is understood;
- the code is genuinely volatile;
- another validation method exists;
- the decision is revisited when the feature stabilizes.

Do not defer high-risk stable behavior merely because surrounding code is changing.

## Alternative validation during exploration

When durable automated tests are premature, use lighter feedback methods.

Options include:

- focused manual scenarios;
- assertions in spike code;
- logging and instrumentation;
- sample fixtures;
- playground or command-line experiments;
- temporary comparison scripts;
- proof-of-concept integration checks.

These methods support learning but do not replace regression tests once the behavior becomes part of the product.

Remove temporary validation code that should not ship.

## Decision workflow

1. Define what the team is trying to learn or protect.
2. Separate stable behavior from unstable implementation.
3. Estimate the impact of failure.
4. Decide whether the code is likely to survive.
5. Choose the smallest durable boundary.
6. Use TDD when the contract is clear enough to guide design.
7. Use lighter validation when the work is exploratory.
8. Avoid freezing temporary structure.
9. Add broader regression coverage as behavior stabilizes.
10. Revisit deferred tests before release or handoff.

## Common mistakes

- Skipping all tests because the feature is new.
- Writing detailed tests for code expected to be discarded.
- Treating TDD as mandatory for every task.
- Treating exploratory validation as permanent regression coverage.
- Testing provisional structure instead of stable behavior.
- Leaving risky algorithms untested inside a prototype.
- Keeping obsolete tests after the design changes.
- Deferring tests without recording the accepted risk.
- Promoting spike code without adding durable tests.
- Waiting for perfect stability before testing critical behavior.

## Review checklist

Before recommending tests for volatile code, check:

- What behavior is already stable?
- What implementation details are likely to change?
- What is the impact if the behavior is wrong?
- Is the code expected to survive?
- Would TDD clarify or prematurely constrain the design?
- Can a narrower invariant be tested now?
- Is a broader outcome more stable than internal structure?
- What validation will be used if automation is deferred?
- When will deferred coverage be reconsidered?
- Are temporary tests or scaffolding being mistaken for production coverage?
