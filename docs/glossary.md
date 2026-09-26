# Glossary

These definitions establish product vocabulary. They do not define implementation fields or database tables.

| Term | Meaning in this project |
| --- | --- |
| Coding agent | A system that uses a model, prompts and tools to inspect and change a repository |
| Model | The underlying inference service or weights; only one part of an agent |
| Agent configuration | The selected adapter, model identity, settings, prompts, tool policy and budget inputs for an evaluation |
| Adapter | A boundary that translates a particular agent or provider into the platform's future common contract |
| Benchmark | A named collection of tasks with an evaluation purpose and inclusion policy |
| Benchmark version | An immutable selection of task versions and associated benchmark definition |
| Task | A stable repository-level engineering problem, such as fixing a bug or adding bounded behavior |
| Task version | An immutable task definition tied to a repository snapshot, setup, checks and resource policy |
| Repository snapshot | The exact starting source state, identified by revision and content provenance |
| Manifest | A reviewable machine-readable specification; its schema is introduced during task-specification implementation |
| Digest | A content-derived identifier used to detect changes; algorithm and canonicalization require design |
| Attempt | One budgeted opportunity for an agent configuration to solve a task |
| Run | The recorded lifecycle and evidence for executing an attempt, including preparation and grading |
| Infrastructure retry | Recovery from a platform execution problem; must remain distinguishable from a new model attempt |
| Experiment | A declared selection of tasks, configurations, budgets, attempt counts and comparison rules |
| Artifact | Stored evidence such as a patch, log, test report, trace or resource record |
| Grader/scorer | A versioned procedure that derives an outcome or metric from evidence |
| FAIL_TO_PASS | Required checks that fail on the baseline and should pass after the candidate change |
| PASS_TO_PASS | Required regression checks that pass on the baseline and should continue to pass |
| Hidden evaluation asset | A check or reference withheld from the agent during execution; it still needs protected grading access |
| Gold/reference patch | An authoring or validation aid; not an answer available to the evaluated agent or required by the runner |
| Invalid task | A task whose specification, baseline or evaluation setup cannot support the intended measurement |
| Exclusion | A disclosed decision to omit a case from a stated analysis, with a recorded reason |
| Reproducibility | Reconstructing experimental inputs and procedure; deterministic fixture outcomes should agree |
| Repeatability/stability | Observed consistency across comparable attempts; stochastic model outputs may differ |
| Calibration | Comparing grader judgments with independent human technical labels and analyzing disagreements |
| Contamination | Prior exposure to evaluation content or solutions that compromises the intended inference about generalization |
| Sandbox | A constrained execution environment with explicit resource, filesystem and network policies and residual risks |
| Scorecard | Separate dimensions and their evidence/statuses, rather than an unexplained aggregate score |
| ADR | Architecture decision record documenting a significant choice, alternatives and consequences |

See [evaluation methods](research/evaluation-methods.md) for measurement limits and [scope](product/scope.md) for milestone boundaries.
