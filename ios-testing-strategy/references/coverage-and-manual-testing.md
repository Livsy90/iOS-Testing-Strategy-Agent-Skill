# Coverage and Manual Testing

Use this reference when discussing coverage targets, automation gaps, or the trade-off between automated and manual verification.

## Contents

- [Core model](#core-model)
- [What coverage can tell you](#what-coverage-can-tell-you)
- [What coverage cannot tell you](#what-coverage-cannot-tell-you)
- [Coverage targets](#coverage-targets)
- [Finding automation gaps](#finding-automation-gaps)
- [When automation has high value](#when-automation-has-high-value)
- [When manual verification is reasonable](#when-manual-verification-is-reasonable)
- [Combining automated and manual testing](#combining-automated-and-manual-testing)
- [Volatile code and temporary gaps](#volatile-code-and-temporary-gaps)
- [Risk acceptance](#risk-acceptance)
- [Decision workflow](#decision-workflow)
- [Common mistakes](#common-mistakes)
- [Review checklist](#review-checklist)

## Core model

Coverage is a signal about which code executed during tests. It is not a measure of confidence, correctness, or test quality.

Manual testing is not automatically inferior to automation. It is one validation method with different strengths and costs.

Choose between automation and manual verification by considering:

- failure impact;
- regression probability;
- detectability;
- repetition;
- setup cost;
- behavior stability;
- maintenance cost.

The goal is sufficient confidence, not maximum automation or maximum coverage.

## What coverage can tell you

Coverage can help identify:

- files or branches no test exercises;
- unexpected gaps in a critical path;
- dead or rarely executed code;
- areas where a test suite is narrower than expected;
- changes that removed previously exercised behavior.

Use coverage to ask better questions.

Examples:

- Why is this error branch never exercised?
- Is this migration path intentionally uncovered?
- Is this critical rule protected through another boundary?
- Does this file contain behavior worth testing directly?

Coverage is most useful when interpreted alongside risk and test intent.

## What coverage cannot tell you

Coverage cannot prove that:

- assertions are meaningful;
- edge cases are protected;
- collaboration between components is correct;
- user-visible behavior works;
- failure and recovery paths are valid;
- tests are resilient to refactoring;
- the most important scenarios are present.

A test can execute a line without checking its result.

A suite can report high coverage while missing the behavior most likely to harm users.

Do not treat a coverage percentage as a quality score.

## Coverage targets

Avoid one universal target for every module.

Different domains justify different levels of coverage.

Higher expectations may be appropriate for:

- business-critical rules;
- financial calculations;
- authentication and authorization;
- migrations;
- persistence semantics;
- destructive actions;
- complex state transitions;
- known regression areas.

Lower direct coverage may be acceptable for:

- trivial forwarding code;
- generated code;
- disposable prototypes;
- temporary adapters;
- rapidly changing UI composition;
- code already protected through broader scenarios.

Use targets as review prompts, not as substitutes for judgment.

A target should never encourage low-value tests written only to raise a number.

## Finding automation gaps

An automation gap exists when meaningful behavior depends on validation that is absent, unreliable, or too costly to repeat manually.

Look for gaps in:

- critical happy paths;
- error and recovery behavior;
- data migrations;
- state transitions;
- cross-domain workflows;
- known regression areas;
- scenarios with complex setup;
- rare but high-impact conditions;
- behavior currently checked only from memory.

Do not infer a gap from low coverage alone.

A line may be uncovered because:

- behavior is trivial;
- another test boundary already protects the outcome;
- the code is temporary;
- manual verification is sufficient;
- the branch is unreachable or obsolete.

Identify the missing behavior, not just the uncovered line.

## When automation has high value

Prefer automation when verification is:

- repeated frequently;
- slow to perform manually;
- easy to forget;
- difficult to configure;
- error-prone;
- dependent on rare data or timing;
- required before each release;
- protecting high-impact behavior.

Automation is especially valuable when it provides fast feedback during development or pull-request review.

Good candidates include:

- deterministic business rules;
- regression scenarios;
- parsers and transformations;
- state transitions;
- persistence behavior;
- controlled integration boundaries;
- cross-domain workflows that can remain deterministic.

Automate the stable contract, not temporary structure.

## When manual verification is reasonable

Manual verification may be appropriate when behavior is:

- visual or experiential;
- exploratory;
- still changing rapidly;
- rare and cheap to check;
- difficult to assert meaningfully;
- low risk;
- likely to be removed.

Examples include:

- early UI exploration;
- subjective animation quality;
- visual polish;
- one-off prototype behavior;
- usability and discoverability checks.

Manual verification should still be explicit: record the scenario, expected result, required environment, and when it must be repeated.

Do not rely on vague instructions such as “test it manually.”

## Combining automated and manual testing

Automation and manual testing often protect different risks.

Use automation for:

- deterministic behavior;
- repeated regression checks;
- edge cases;
- state and data correctness;
- fast feedback.

Use manual testing for:

- usability;
- accessibility experience;
- visual correctness;
- interaction feel;
- exploratory discovery;
- device-specific experience.

For one feature, automated tests may prove that state and effects are correct while manual testing confirms that the experience is understandable and polished.

Do not duplicate the same check manually when automation already provides reliable feedback unless the manual pass evaluates a different quality.

## Volatile code and temporary gaps

Rapidly changing code may not justify deep automation immediately.

During exploration:

- protect stable invariants;
- automate risky algorithms;
- validate critical assumptions;
- use focused manual scenarios for temporary structure;
- revisit gaps as behavior stabilizes.

A temporary gap is acceptable only when the behavior is genuinely volatile, the risk is understood, another validation method exists, and ownership and revisit timing are clear.

Do not leave high-risk stable behavior manual merely because the surrounding feature is changing.

## Risk acceptance

Choosing not to automate is a risk decision.

Make explicit what remains manual, why automation is not currently worthwhile, how verification will happen, what failure could escape, and when the decision should be revisited.

Valid reasons may include:

- low impact;
- low regression probability;
- high automation cost;
- short expected lifetime;
- cheap and reliable manual verification.

Invalid reasons include:

- chasing delivery speed without assessing impact;
- assuming QA will catch everything;
- relying on coverage from unrelated tests;
- avoiding difficult but critical scenarios.

## Decision workflow

1. Define the behavior, not the coverage gap.
2. Estimate impact, probability, detectability, and recovery cost.
3. Check whether another test already protects the outcome.
4. Assess behavior stability.
5. Estimate manual verification cost and reliability.
6. Estimate automation setup and maintenance cost.
7. Automate when repeated confidence outweighs maintenance cost.
8. Use explicit manual scenarios when automation is not justified.
9. Treat coverage as supporting evidence, not the decision.
10. Revisit manual gaps as risk, ownership, or stability changes.

## Common mistakes

- Treating a percentage as proof of quality.
- Applying one target to every module.
- Writing low-value tests only to raise coverage.
- Assuming uncovered code always needs a dedicated test.
- Leaving critical behavior manual because automation is difficult.
- Automating volatile details that change every iteration.
- Using vague manual test instructions.
- Duplicating automated checks manually without a distinct purpose.
- Ignoring error and recovery paths.
- Allowing temporary gaps to become permanent without review.
- Confusing executed code with asserted behavior.
- Treating automation as the only valid form of testing.

## Review checklist

Before recommending coverage or manual testing changes, check:

- What behavior is at risk?
- Does existing automation already protect it?
- Is the current coverage gap meaningful?
- Are assertions strong enough to justify the reported coverage?
- What is the impact if the behavior fails?
- How often must it be verified?
- Is manual verification reliable and affordable?
- Is the behavior stable enough to automate?
- Would automation survive safe refactoring?
- Is the accepted manual gap documented?
- When should the decision be revisited?
- Is effort focused on confidence rather than the metric?
