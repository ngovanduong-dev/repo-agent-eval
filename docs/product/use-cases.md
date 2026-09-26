# MVP use cases

These are future product requirements, not available commands. Registration initially means a reviewable definition; it does not require an HTTP endpoint or database. Terms are defined in the [glossary](../glossary.md).

| ID | Actor and action | Preconditions and observable outcome | Failure or negative path |
| --- | --- | --- | --- |
| UC-01 | Author registers a benchmark | A named purpose and inclusion criteria produce an identifiable benchmark definition | Ambiguous or duplicate identity must be reported rather than silently replaced |
| UC-02 | Author registers a task version | A pinned repository, statement, checks, resource policy and provenance produce a reviewable version | Missing revision, rights/provenance information, or required metadata blocks admission |
| UC-03 | Reviewer validates a task | Baseline reproduces the intended failure, regression checks pass, protected assets remain hidden, and reset works | Broken setup, flaky baseline, or leaked solution marks the version invalid with evidence; it is not an agent failure |
| UC-04 | Evaluator runs one configuration against one task | A valid task and explicit tool/resource budget produce one identifiable attempt | Preparation error, agent crash, timeout and cancellation remain distinguishable; no silent retry |
| UC-05 | Evaluator captures repository state | The attempt records the starting snapshot and final patch, including additions/deletions | Missing or unsafe artifact paths prevent a claim of complete evidence |
| UC-06 | Evaluator executes deterministic graders | Protected checks run on the candidate with the prescribed environment and produce structured outcomes | Missing reports, tampered checks and grader crashes cannot be treated as passes |
| UC-07 | Operator retains run metadata and artifacts | Inputs, versions/digests, timestamps, patch, bounded logs and result evidence remain associated with the run | Storage failure or truncation is explicit; cleanup must not silently erase required evidence |
| UC-08 | Consumer reads a machine-readable result | A result identifies the attempt, individual metrics/statuses, evidence references and limitations | Unsupported or missing measurements are explicit, not zero-filled or omitted to imply success |
| UC-09 | Evaluator compares repeated attempts | Comparable task/configuration/environment inputs and declared budgets expose outcome variation | Invalid/excluded attempts and unequal budgets are disclosed; infrastructure retries are not new model attempts |
| UC-10 | Evaluator reruns a task reproducibly | Pinned inputs and a clean workspace permit replay; deterministic fixtures yield equivalent grading outcomes | Unavailable dependencies, environment drift, or changed inputs are reported; hosted model output equality is not promised |

## Illustrative journey

An author proposes a bug-fix task whose target check fails on the baseline while existing behavior checks pass. A reviewer confirms that the statement supports the expected behavior and that hidden checks are not accessible to the agent. An evaluator selects a configuration and budget, then runs the validated version.

If the patch fixes the target but breaks a regression check, the report preserves both outcomes. If setup fails before the agent starts, the report records an execution problem rather than a wrong answer. A later attempt starts from the same clean snapshot; it does not inherit the previous patch.

This is a specification example, not a task fixture or a recorded experiment. Contract fields and state-transition rules require architecture design before domain-model implementation.
