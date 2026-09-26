# Product vision

## Problem

A coding agent can pass a target test while breaking existing behavior, modifying unrelated files, or consuming an impractical budget. A headline solve rate does not explain these outcomes or establish whether a model upgrade is safe for an engineering workflow.

RepoAgentEval should help an evaluator decide whether an agent configuration is suitable for a defined set of repository tasks. Every conclusion should be traceable to the task version, starting repository, execution policy, patch, grader version, and recorded outcome.

## Intended value

- Benchmark authors can describe expected behavior and independently validate tasks before admitting them to a benchmark.
- Evaluators can repeat controlled attempts and distinguish agent failures from invalid tasks and platform failures.
- Reviewers can inspect the evidence behind a comparison, including exclusions and uncertainty.
- Consumers can understand where an agent works, where it fails, and what the experiment cannot establish.

The unit of evaluation is an agent configuration operating on a pinned repository task. The measured system includes its prompts, tools, budgets, and environment; a result must not be attributed to the underlying model alone.

## Product hypothesis and feasibility

The hypothesis is that a small, reviewed, original task set with complete evidence is more useful for local upgrade decisions than an unexplained aggregate score. This is a product hypothesis, not a demonstrated improvement over existing evaluation systems.

The first feasibility gate is one repeatable fixture run with structured success and failure evidence. Later gates establish task validity, adapter comparability, and the usefulness of repeated attempts. If those fail, investigate task design or measurement before expanding infrastructure.

No stakeholder interviews, usability study, or benchmark experiment have been completed yet. The personas are working assumptions to validate with authors, evaluators, and reviewers.

## Constraints

Measurement must preserve evidence integrity and respect repository licenses and data restrictions. Public artifacts must omit credentials and protected evaluation material. Resource limits and isolation must be explicit; running unknown code on a developer host is outside the intended evaluation workflow.

See [scope](scope.md) for delivery boundaries and [success metrics](success-metrics.md) for evidence required at each milestone.
