# iOS Testing Strategy Agent Skill

<p align="center">
<img width="320" src="https://github.com/user-attachments/assets/a71688d2-54a3-4ea5-8362-e4659b3d46c6"/>
</p>

A reusable AI-agent skill for designing, reviewing, and improving testing strategies in iOS applications.

The skill helps agents choose practical test boundaries, decide when to use real dependencies or test doubles, prioritize coverage by risk, and keep test suites maintainable.

## What this is

This repository contains a focused testing-strategy skill for AI coding agents working with iOS and Swift codebases.

It is designed to help an agent reason about questions such as:

- what behavior should be tested first;
- whether a test should be narrow, feature-level, integration-level, or system-wide;
- when real implementations are preferable to mocks;
- where fakes, stubs, spies, or protocols are justified;
- how much testing effort volatile code deserves;
- when TDD is useful;
- how to reduce brittle assertions, boilerplate, flakiness, and slow CI feedback;
- how to balance automated testing, coverage metrics, and manual verification.

The skill does not prescribe one universal testing style. It uses risk, behavior stability, feedback speed, determinism, and maintenance cost to guide decisions.

## Included skill

### `ios-testing-strategy`

For designing and reviewing automated testing strategies in iOS applications.

Covers:

- narrow unit tests, feature-level tests, integration tests, and system-wide tests;
- real dependencies, in-memory implementations, fakes, stubs, spies, and mocks;
- protocol and test-seam decisions;
- risk-based test prioritization;
- prototypes, volatile features, architectural spikes, and TDD;
- observable assertions and resilience to refactoring;
- fixtures, setup, boilerplate, execution time, flakiness, and CI feedback;
- coverage targets and manual testing trade-offs.

## Installation

Install the skill with:

```bash
npx skills add Livsy90/iOS-Testing-Strategy-Agent-Skill --all
```

The CLI will ask which agents to register the skill with, such as Claude Code, Codex, Cursor, or Gemini, and whether to install it per-project or globally.

If `npx` is missing, install Node:

```bash
brew install node
```

If Homebrew is also missing, install it from [brew.sh](https://brew.sh).

### Claude Code

Install the plugin directly:

```bash
/plugin install Livsy90/iOS-Testing-Strategy-Agent-Skill
```

### Manual installation

Clone the repository and copy the `ios-testing-strategy` directory into the skills location used by your agent.

For repository-scoped Codex skills, place it under:

```text
.agents/skills/ios-testing-strategy/
```

## Repository structure

```text
ios-testing-strategy/
  SKILL.md
  agents/
    openai.yaml
  references/
    assertions-and-refactoring-resilience.md
    coverage-and-manual-testing.md
    risk-based-test-prioritization.md
    test-boundaries.md
    test-doubles-and-real-dependencies.md
    test-suite-maintainability.md
    volatile-code-and-tdd.md

.claude-plugin/
  plugin.json

gemini-extension.json
```

## How the skill works

`SKILL.md` contains the activation rules, workflow, decision rules, gotchas, and expected output.

Detailed guidance is split into focused reference files and loaded only when relevant. This keeps the main skill compact while preserving enough depth for architecture reviews and test-strategy decisions.

## Typical use cases

Use the skill when asking an agent to:

- review an existing test suite;
- decide which tests a new feature needs;
- reduce excessive mocks or protocol-first abstractions;
- define feature-level or system-wide tests;
- prioritize testing across several domains;
- review brittle tests that break during refactoring;
- decide whether a prototype needs automation;
- reduce flaky or slow CI tests;
- assess whether coverage gaps are meaningful.

## License

MIT
