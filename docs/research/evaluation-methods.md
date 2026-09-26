# Evaluation methods and limits

This document frames measurement requirements. It does not freeze a scoring schema or implement a grader; those require later design and tests.

## Deterministic evidence first

| Method | Question answered | Main limitation |
| --- | --- | --- |
| FAIL_TO_PASS checks | Does the candidate satisfy behavior that fails on the pinned baseline? | A weak check can accept an incorrect solution |
| PASS_TO_PASS checks | Does known working behavior survive the change? | Untested behavior can still regress |
| Lint/type checks | Does the patch satisfy specified mechanical constraints? | Tool configuration and versions affect results; this is not proof of correctness |
| Static security checks | Does the candidate trigger defined suspicious patterns? | False positives and missed vulnerabilities require explicit handling |
| Patch analysis | What files and lines changed, and were protected assets altered? | Small patches are not necessarily good patches |
| Performance checks | Does behavior meet a defined resource or timing constraint? | Hardware, load, warmup and noise can invalidate comparisons |

The first grader focuses on target and regression tests. Other deterministic methods are introduced only with a reviewed task need and suitable fixtures. As an external reference, [SWE-bench](https://www.swebench.com/SWE-bench/) evaluates repository patches with a reproducible harness; this project's detailed scoring semantics remain to be designed.

## Validity before scoring

A task must establish its baseline behavior before candidate evaluation. Baseline setup failure, unexplained flakiness, contradictory requirements, or exposed hidden assets undermines task validity. Agent timeout, crash, wrong behavior, platform preparation failure, and grader failure need distinct evidence and eventual statuses.

For example, fixing a failing target check while breaking an existing check yields different correctness and regression outcomes. A missing test report is an evaluation error, not a score of zero or a pass. An unmeasured dimension must remain explicitly unmeasured.

Candidate changes must not be able to rewrite the authoritative checks or forge trusted results. Protecting hidden assets while executing candidate code is an unresolved boundary design problem, not something the existence of a test suite solves.

## Qualitative judgments

Maintainability and unnecessary complexity may need human judgment beyond mechanical checks. Any later LLM grader requires a versioned rubric, blinded independent human labels, agreement/disagreement analysis, and a held-out calibration assessment. No qualitative grader is currently validated.

## Comparisons

Compare configurations with the same task versions and declared budgets, retain raw results, and publish exclusion reasons. Distinguish replay of deterministic fixtures from repeatability of stochastic model behavior. Changing a prompt, tool policy, scorer, or environment changes the experimental conditions.

Statistical aggregation is introduced after repeated-run semantics are established. Select the unit of analysis and uncertainty method before claiming a meaningful difference; repeated attempts on one task are not equivalent to sampling more tasks. Keep task success, regression safety, security, quality, efficiency, stability, cost and latency separate as described in [success metrics](../product/success-metrics.md).
