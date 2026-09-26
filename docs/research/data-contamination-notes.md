# Data contamination and task provenance

This is an initial risk analysis, not a contamination audit. No model training data, benchmark corpus, or private task collection has been inspected.

## Threats to interpretation

Public issues, reference patches, tests, or near-duplicate tasks may have appeared in training data or retrieval sources. Familiarity can inflate apparent generalization. Exposure can also occur during repeated evaluation when prompts, logs, or human feedback reveal held-out answers.

Public repository tasks are useful external references: [SWE-bench](https://www.swebench.com/SWE-bench/) explicitly derives issues from GitHub. Exposure risk is a project inference, not evidence that any particular model memorized those issues.

## Proposed governance

- Record author, reviewer, version, creation date, source/provenance, license note, category, difficulty notes and known limitations for each task.
- Author original tasks for the primary dataset. Separate development examples from held-out evaluation tasks and record what was exposed to each agent configuration.
- Keep reference patches and hidden checks outside the agent-visible workspace and published run traces. Inspect archives and repository history as well as the working tree for accidental disclosure.
- Freeze published task versions; corrections create a new version with a reason and an impact note. Preserve which version each historical result used.
- Check for duplicates and close variants before admission, recording the method and its limitations. Renaming variables is not evidence of independence.
- Review rights and redaction before publication. Do not copy external datasets merely because their repositories are public.

## Limits and response

Original authorship and private storage reduce some exposure paths but do not prove uncontaminated training data. A creation date after a claimed model cutoff is insufficient by itself, especially when provider training and retrieval details are unknown.

If leakage or duplication is discovered, preserve the evidence, flag affected versions and reports, and apply a documented exclusion or sensitivity analysis. Do not silently remove unfavorable runs or rewrite old results.

The maintainer and task reviewer must decide dataset release policy, access rules, and what evidence can be public before collecting held-out material. Public example fixtures may establish reproducibility but must not be described as permanently secret evaluation assets.
