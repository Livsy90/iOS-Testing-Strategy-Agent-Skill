# Assertions and Refactoring Resilience

Use this reference when tests depend on private methods, call order, interaction counts, or other implementation details.

## Contents

- [Core model](#core-model)
- [Assert outcomes, not structure](#assert-outcomes-not-structure)
- [Observable behavior](#observable-behavior)
- [Private methods](#private-methods)
- [Call order](#call-order)
- [Interaction counts](#interaction-counts)
- [When interaction assertions are valid](#when-interaction-assertions-are-valid)
- [State versus interaction verification](#state-versus-interaction-verification)
- [Refactoring-resistant assertions](#refactoring-resistant-assertions)
- [Failure diagnosis](#failure-diagnosis)
- [Decision workflow](#decision-workflow)
- [Common mistakes](#common-mistakes)
- [Review checklist](#review-checklist)

## Core model

A useful test should fail when behavior changes in a harmful way and remain stable when internal implementation changes safely.

Prefer assertions about observable outcomes over assertions about how the outcome was produced.

Implementation-coupled tests often create maintenance work without adding equivalent regression protection.

## Assert outcomes, not structure

Prefer assertions about:

- returned values;
- observable state;
- persisted data;
- emitted domain events;
- generated requests at an external boundary;
- meaningful errors;
- recovery behavior;
- user-visible outcomes.

Avoid asserting:

- private helper calls;
- temporary object graphs;
- incidental ordering;
- exact internal method counts;
- current decomposition into types or functions.

A test should describe the contract that matters, not replay the implementation.

## Observable behavior

Choose assertions that a real caller or external collaborator could observe.

Good examples:

- a command produces the expected state transition;
- invalid input returns the expected error;
- a repository contains the expected data;
- an external request contains the correct payload;
- a prohibited side effect does not occur;
- a feature emits the expected domain event.

If the final outcome already proves the behavior, avoid also asserting every internal interaction.

## Private methods

Do not expose or directly test private methods solely to increase coverage.

Private helpers are usually implementation details and should be exercised through public or feature-level behavior.

Direct testing may be justified only when the helper represents an independently valuable concept, such as:

- a complex algorithm;
- a reusable parser;
- a deterministic rule;
- a transformation with many edge cases.

In that case, consider extracting it into a dedicated type with a meaningful API rather than weakening access control for tests.

## Call order

Do not assert call order unless order is part of the contract.

Valid examples:

- a transaction must begin before writes and commit afterward;
- authorization must occur before a protected operation;
- a cancellation must happen before replacing active work;
- an external protocol requires ordered commands.

Invalid examples:

- one private helper currently runs before another;
- logging occurs before mapping;
- collaborators happen to be called in the present implementation order.

If reordering calls does not change observable behavior, the test should not fail.

## Interaction counts

Use exact counts only when quantity is behaviorally significant.

Valid examples:

- a notification must be scheduled once;
- a destructive command must not be retried;
- duplicate analytics events must be prevented;
- a network request must be deduplicated;
- a side effect is contractually limited.

Avoid exact counts when:

- repeated calls are harmless;
- batching or caching may change the number;
- the count reflects temporary orchestration;
- final state already proves correctness.

Prefer assertions such as “contains the expected effect” or “does not contain duplicates” over rigid call counts when possible.

## When interaction assertions are valid

Interaction assertions are appropriate when the interaction itself is an externally meaningful effect.

Good targets:

- analytics events;
- navigation commands;
- persistence commands;
- network requests;
- push notification scheduling;
- calls to destructive or irreversible systems;
- required protocol sequencing.

Assert the meaningful payload, destination, or rule.

Do not turn a spy or mock into a transcript of every internal step.

## State versus interaction verification

Prefer state verification when the effect produces an observable state.

Examples:

- verify stored data instead of asserting `save()` was called;
- verify rendered state instead of asserting mapper invocation;
- verify emitted output instead of checking internal helper calls.

Prefer interaction verification when:

- no stable resulting state is available;
- the interaction is the contract;
- the external effect must be prevented or limited;
- observing the real side effect would be unsafe.

Use both only when each assertion protects a distinct risk.

## Refactoring-resistant assertions

A resilient test:

- enters through a stable boundary;
- asserts a stable outcome;
- uses minimal scenario-specific setup;
- ignores incidental collaborator behavior;
- avoids overspecifying order or count;
- survives renaming, extraction, inlining, or redistribution of internal work.

A brittle test often breaks after:

- splitting one type into several;
- merging helpers;
- introducing caching;
- changing scheduling;
- moving logic between layers;
- replacing one internal algorithm with another equivalent one.

Before keeping a failing test after refactoring, ask whether behavior changed or only structure changed.

## Failure diagnosis

Broad behavioral assertions should still provide actionable failures.

Improve diagnosis by:

- naming tests after behavior and condition;
- asserting focused outcomes;
- comparing meaningful values;
- keeping fixtures small;
- separating unrelated scenarios;
- adding context to custom assertion messages.

Do not compensate for poor diagnosis by asserting every internal call.

If a broad test is difficult to debug, add focused lower-level tests for risky logic rather than coupling the broad test to implementation.

## Decision workflow

1. Define the behavior or contract being protected.
2. Identify the most stable observable result.
3. Assert that result through a stable entry point.
4. Remove assertions already implied by the outcome.
5. Add interaction assertions only for externally meaningful effects.
6. Use order or exact counts only when contractually significant.
7. Avoid exposing private APIs for tests.
8. Extract complex private logic only when it has independent value.
9. Check whether the test survives safe structural refactoring.
10. Improve failure messages without overspecifying execution.

## Common mistakes

- Exposing private methods only for test access.
- Verifying every collaborator call.
- Treating exact call order as correctness.
- Using interaction counts to mirror implementation.
- Asserting both internal calls and the outcome they already prove.
- Breaking tests during safe refactoring with no behavioral change.
- Using mocks to define the implementation path.
- Keeping obsolete assertions after responsibilities move.
- Writing broad tests with vague failure messages.
- Replacing focused behavioral tests with snapshot-like internal transcripts.

## Review checklist

Before approving assertions, check:

- What observable behavior does each assertion protect?
- Could the test pass while the real behavior is wrong?
- Could it fail after a safe refactor?
- Is a private method being tested directly?
- Is call order contractually significant?
- Is an exact interaction count required?
- Would state verification be more stable?
- Is the asserted interaction an external effect?
- Are any assertions redundant?
- Will a failure provide actionable information?
