# Success criteria and measurement plan

All targets below are prospective. Current evidence consists only of project framing; no task count, solve rate, security guarantee, or calibration result has been achieved.

## Delivery gates

| Gate | Evidence required | Review responsibility |
| --- | --- | --- |
| Product and evaluation foundations | Acceptance criteria in [scope](scope.md) satisfied; assumptions and non-goals inspectable | Maintainer |
| End-to-end evaluation pipeline | One fixture end to end, invalid manifest rejected before execution, deterministic timeout handling, patch captured, structural test results, serializable report, success/failure tests, equivalent deterministic outputs on replay | Maintainer and technical reviewer |
| Minimum credible benchmark | 20–30 internally authored tasks; at least two categories and two adapters; reproducible Docker execution; hidden/regression tests; structured artifacts; multiple attempts per task; failure taxonomy; confidence intervals; platform CI regression gate; limitations | Task reviewer and evaluator |
| Expanded benchmark milestone | 80–150 reviewed tasks; Python and TypeScript repositories; 4–6 categories; at least three configurations; one human-calibrated qualitative dimension; cost/latency/stability analysis; public benchmark card and experiment report | Technical reviewers and evaluator |

These reproduce roadmap milestones, not deadlines. A small task set supports conclusions about that task set; it does not establish general software-engineering competence.

## Planned scorecard

| Dimension | Evidence or proposed measurement | Interpretation guardrail |
| --- | --- | --- |
| Task success | Required behavioral checks satisfied on the candidate | State the check set and task version; passing tests is bounded evidence |
| Regression safety | Previously passing required checks remain passing | Keep separate from target behavior; skipped/missing checks are not passes |
| Security | Findings from defined static and execution checks | Absence of findings does not prove security |
| Quality | Patch scope, lint/type checks; later a reviewed rubric | Patch size alone is not maintainability; qualitative judges need calibration |
| Efficiency | Tool calls, tokens where available, and sandbox resource use | Missing telemetry is unknown, not zero; budgets must be comparable |
| Stability | Per-task distribution of outcomes across declared attempts | Report attempt counts and reuse of tasks; repeats are not independent new tasks |
| Cost | Recorded charges or explicitly labeled estimates with pricing provenance | Proposed cost per success includes unsuccessful attempts' costs; no successes means undefined |
| Latency | End-to-end elapsed time with preparation, execution and grading separated | Include failures/timeouts in distributions; distinguish execution time from later queue delays |

Proposed solve-rate reporting will include the solved count, denominator, invalid/excluded counts and their reasons. Agent failure and timeout must not disappear from the denominator merely to improve results. The exact invalid-run semantics require architecture design; aggregation semantics must be validated during experimentation and statistical analysis.

## Platform evidence quality

Require complete input and evidence references for every run reported as valid. Report missing artifacts, redactions, truncated logs, environment drift, and task-validation failures explicitly. Test deterministic replay with controlled local fixtures separately from stochastic agent repeatability.

Before comparisons, declare attempts, budgets, inclusion/exclusion rules, and uncertainty methods. The statistical method must account for repeated attempts within tasks. No confidence intervals or significance claims are available now, and there is no weighted overall score.
