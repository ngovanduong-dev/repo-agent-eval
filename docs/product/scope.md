# Scope and delivery boundaries

The [roadmap](../ROADMAP.md), especially Part VI, governs implementation order. A phase heading is not permission to implement all of that phase in one PR.

## Milestones

| Milestone | Required outcome | Boundary |
| --- | --- | --- |
| Framing: PR 1 | Problem, actors, use cases, terminology, scope, success criteria, initial research risks | Documentation only; no runtime or domain implementation |
| Design: PR 2 | System context, component boundaries, initial ADRs, threat-model draft | No provider integration |
| Quality foundation: PR 3 | Packaging, checks, tests framework, minimal CI and dependency/security configuration | No evaluation engine |
| First vertical slice: PRs 4–10 | Contracts, manifest, workspace, sandbox, mock agent, deterministic grader, then one CLI-to-JSON fixture run | One local agent and task; no database or web UI |
| Evidence and comparison: PRs 11–15 | PostgreSQL persistence, first real adapter, repeat experiments/statistics, failure taxonomy, second adapter | Each remains a separate PR with its own prerequisites |
| Later capabilities: PR 16+ | Calibrated qualitative grading, justified queueing, API/auth, dashboard | Only after the core is credible |

The roadmap's ten MVP use cases are the intended product boundary, delivered progressively. The first vertical slice proves the execution path; it does not satisfy the later two-adapter, statistical benchmark milestone. Manual comparison of repeated outputs precedes automated experiment aggregation in PR 13.

## MVP product requirements

Support benchmark and task registration, validation, a single controlled attempt, before/after repository evidence, deterministic grading, metadata/artifact retention, machine-readable results, repeated-attempt comparison, and reproducible reruns. See [use cases](use-cases.md) for observable outcomes.

Begin with authored fixture repositories and bounded tasks. Bug fixes and small behavioral changes are proposed initial categories, subject to task review. The first fixture need not represent the final task distribution.

## Explicit non-goals

- IDE features, autonomous code-generation SaaS, prompt marketplace, or billing.
- Kubernetes, microservices, multi-region deployment, or speculative worker queues.
- Enterprise SSO, multi-user permissions, HTTP APIs, or a React dashboard in the CLI milestone.
- Unrestricted sandbox internet access or exposure of host secrets.
- Hundreds of tasks before authoring and validation quality is established.
- An arbitrary weighted overall score or uncalibrated LLM judgment presented as reliable measurement.

Python, typed contracts, PostgreSQL, and Docker are roadmap directions. Their design rationale and exact contracts belong to PR 2; adding packages, migrations, or execution code now would bypass review.

## PR 1 acceptance criteria

- Both project titles and the current documentation-only status are visible in the README.
- All five actors and ten MVP use cases are described with evidence and failure considerations.
- Product targets are separated from achieved results and from the first runnable milestone.
- Research notes identify tradeoffs, evidence sources, and open questions without claiming experiments were run.
- The supplied roadmap is preserved, navigation works, and no production implementation or dependency is introduced.

Open decisions for PR 2 include environment support, artifact retention, hidden-test protection, scoring statuses, digest semantics, and how a hosted agent can access model services without granting arbitrary network access to candidate code.
