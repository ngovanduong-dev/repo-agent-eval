# Benchmark and framework landscape

Initial desk research, consulted 2026-09-26. This is a scoped comparison of task units and evaluation approaches, not a leaderboard survey or an integration commitment.

| Reference | What the primary source describes | Implication for this project |
| --- | --- | --- |
| [SWE-bench](https://www.swebench.com/SWE-bench/) | Repository issues from GitHub, candidate patches, and a Docker-based evaluation harness | Study pinned issue-to-patch evaluation and test evidence; public examples alone are insufficient for the intended original task set |
| [BigCodeBench](https://github.com/bigcode-project/bigcodebench) | Function-level generation with complex instructions and diverse function calls | Useful contrast for task granularity; function completion does not directly exercise repository navigation and multi-file changes |
| [Inspect](https://inspect.aisi.org.uk/) | Evaluation tasks combine datasets, solvers and scorers; agents can use tools and sandboxes | Study separation of generation and scoring; possible compatibility is deferred until the project contracts are established |

The implications above are project judgments. No claim is made that these systems lack evidence handling or that RepoAgentEval outperforms them. No packages, datasets, or execution harnesses were installed or run for this review.

## Positioning

The proposed contribution is a small, inspectable engineering platform with original repository tasks, explicit task validity, controlled repeated attempts, and evidence behind separate outcome dimensions. Reusing external ideas does not imply copying task content or adopting a framework dependency.

## Questions before adopting external material

- Does the task require repository reasoning or only isolated code completion?
- Are the repository revision, environment, grader and task provenance reproducible?
- Can hidden checks remain separate from agent-visible assets?
- Do licenses permit the intended use and redistribution of each artifact?
- Has public exposure compromised the intended held-out comparison?

Dataset import, framework compatibility, and agent integrations remain future work. See [contamination notes](data-contamination-notes.md).
