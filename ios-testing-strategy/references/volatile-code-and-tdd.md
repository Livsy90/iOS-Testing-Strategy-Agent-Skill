# Risk-Based Test Prioritization

Use this reference when deciding what to test first, how deeply to test it, or how to distribute effort across domains.

## Contents

- [Core model](#core-model)
- [Prioritize confidence, not coverage](#prioritize-confidence-not-coverage)
- [Assess risk](#assess-risk)
- [Assess stability](#assess-stability)
- [Assess verification cost](#assess-verification-cost)
- [Choose test depth](#choose-test-depth)
- [Distribute effort across domains](#distribute-effort-across-domains)
- [High-priority targets](#high-priority-targets)
- [Lower-priority targets](#lower-priority-targets)
- [Manual versus automated verification](#manual-versus-automated-verification)
- [Team and delivery context](#team-and-delivery-context)
- [Decision workflow](#decision-workflow)
- [Common mistakes](#common-mistakes)
- [Review checklist](#review-checklist)

## Core model

Testing effort is limited. Spend it where it produces the most confidence for the least ongoing cost.

Do not distribute tests uniformly across files, types, or modules. Different domains carry different levels of risk, volatility, and verification cost.

Prioritize stable, high-impact behavior before low-value implementation details.

## Prioritize confidence, not coverage

The objective is not to maximize:

- test count;
- line or branch coverage;
- the number of mocked collaborators;
- the number of types with dedicated test files.

The objective is to reduce meaningful product and engineering risk.

Treat coverage as a diagnostic signal. It can expose untested areas, but it cannot prove that important behavior is protected.

A small set of well-chosen tests may provide more confidence than broad but shallow coverage.

## Assess risk

Evaluate behavior across four dimensions.

### Impact

Ask what happens if it fails.

High-impact failures include:

- data loss or corruption;
- incorrect financial results;
- broken authentication or authorization;
- privacy or security violations;
- failed migrations;
- blocked critical flows.

### Probability

Regression probability increases with:

- complex logic;
- frequent modification;
- many interacting components;
- unclear ownership;
- repeated past defects;
- fragile external contracts.

### Detectability

Risk is higher when failures:

- occur only under rare conditions;
- depend on specific state or timing;
- are difficult to reproduce manually;
- produce plausible but incorrect output;
- appear only after release.

### Recovery cost

Risk is higher when recovery requires:

- data repair;
- a new release;
- migration rollback;
- user support;
- coordination across teams or services.

Prioritize behavior with high impact, high probability, low detectability, or high recovery cost.

## Assess stability

Separate behavior stability from implementation stability.

A feature may have stable business rules but unstable UI composition, orchestration, or module boundaries.

Protect stable outcomes and invariants even while surrounding code changes.

Good early targets:

- calculations;
- validation rules;
- state transitions;
- permissions;
- data transformations;
- persistence semantics;
- security-sensitive decisions.

Avoid detailed tests that freeze temporary structure.

## Assess verification cost

Compare automation cost with manual verification cost.

Automation has high value when manual checking is:

- repetitive;
- slow;
- error-prone;
- difficult to configure;
- required for every change or release;
- dependent on rare conditions.

Manual verification may be sufficient when a scenario is simple, infrequent, cheap to repeat, easy to observe, and low risk.

Do not automate merely because automation is possible. Do not rely on manual testing when repetition or complexity makes it unreliable.

## Choose test depth

Match depth to risk.

### Light coverage

Use for low-risk behavior:

- one representative example;
- coverage through a broader feature test;
- manual verification;
- no dedicated test when another test already protects it.

### Moderate coverage

Use for meaningful but recoverable behavior:

- happy path;
- primary error path;
- important boundary values;
- feature-level collaboration test.

### Deep coverage

Use for high-risk behavior:

- edge cases;
- invalid inputs;
- state transitions;
- error and recovery paths;
- persistence or integration behavior;
- concurrency or timing cases where relevant;
- regression tests for known failures.

Depth should come from risk, not code size.

## Distribute effort across domains

Allocate effort unevenly.

A domain deserves more attention when it:

- contains critical business rules;
- handles money, identity, access, security, or data integrity;
- has complex state or branching;
- changes frequently;
- has a history of regressions;
- is difficult to verify manually;
- integrates with unstable external systems.

A domain may receive less direct coverage when it:

- mostly forwards data;
- is simple and deterministic;
- is already exercised through broader scenarios;
- is temporary or expected to be removed;
- has no stable behavior yet.

Do not assign every module the same target percentage.

## High-priority targets

Prioritize:

- financial calculations;
- authentication and authorization;
- migrations;
- persistence and synchronization rules;
- destructive actions;
- security-sensitive logic;
- complex state transitions;
- external data parsing and mapping;
- cross-domain workflows;
- known regression areas;
- scenarios with expensive manual setup.

For volatile features, test stable invariants and externally meaningful outcomes first.

## Lower-priority targets

Deprioritize detailed tests for:

- private helpers already exercised through public behavior;
- trivial forwarding code;
- generated code;
- disposable prototypes;
- temporary adapters;
- rapidly changing UI structure;
- code likely to be replaced before stabilization;
- low-impact behavior that is easy to verify manually.

Deprioritization does not mean zero validation. Make the accepted risk explicit.

## Manual versus automated verification

Prefer automation when:

- the scenario runs often;
- setup is complex;
- results are easy to assert;
- failures are costly;
- humans may miss subtle regressions.

Prefer manual verification when:

- behavior is primarily visual or exploratory;
- requirements are still changing;
- the scenario is rare and cheap to check;
- automation would be brittle and expensive.

Use both when automated tests protect behavior and manual testing evaluates usability or visual quality.

## Team and delivery context

Increase automation when:

- many developers modify the same areas;
- ownership is distributed;
- releases are frequent;
- onboarding is active;
- QA capacity is limited;
- production incidents are expensive.

A solo prototype may accept more manual verification than a shared production codebase.

Adapt recommendations to team size, release cadence, ownership, and failure cost.

## Decision workflow

1. List the behaviors or domains under consideration.
2. Estimate impact, probability, detectability, and recovery cost.
3. Identify stable rules and volatile implementation details.
4. Estimate manual verification cost.
5. Estimate automation setup and maintenance cost.
6. Rank behaviors by expected risk reduction.
7. Choose test depth for each behavior.
8. Prefer broader coverage when it protects several components efficiently.
9. Avoid duplicate tests that add little new confidence.
10. Revisit priorities as the product and architecture evolve.

## Common mistakes

- Setting one coverage target for every module.
- Testing by file size or architectural prominence.
- Treating high coverage as proof of quality.
- Spending heavily on volatile implementation details.
- Leaving stable high-risk rules untested because the feature is evolving.
- Automating cheap one-off checks with brittle infrastructure.
- Relying on manual testing for repetitive or hard-to-reproduce scenarios.
- Ignoring recovery cost.
- Keeping obsolete tests after the protected behavior disappears.
- Prioritizing easy tests over valuable tests.

## Review checklist

Before recommending priorities, check:

- What failure is being prevented?
- What is the impact if it occurs?
- How likely is it to regress?
- How easily would the team detect it?
- How expensive is recovery?
- Is the behavior stable enough to protect?
- Is manual verification reliable and affordable?
- What depth of coverage is justified?
- Is it already protected by a broader test?
- Is maintenance cost proportionate to confidence gained?
