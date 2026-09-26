# Scope and delivery boundaries

The [roadmap](../ROADMAP.md) describes capability milestones in dependency order. Each milestone builds on the contracts, evidence, and validation established by its prerequisites.

## Milestones

| Milestone | Required outcome | Boundary |
| --- | --- | --- |
| Product and Evaluation Foundations | Problem, actors, use cases, terminology, scope, success criteria, initial research risks | Documentation only; no runtime or domain implementation |
| Architecture and Security Design | System context, component boundaries, initial ADRs, threat-model draft | No provider integration |
| Engineering Quality Baseline | Packaging, checks, tests framework, minimal CI and dependency/security configuration | No evaluation engine |
| End-to-End Evaluation Pipeline | Contracts, manifest, workspace, sandbox, mock agent, deterministic grader, then one CLI-to-JSON fixture run | One local agent and task; no database or web UI |
| Persistence and Cross-Model Evaluation | PostgreSQL persistence, first real adapter, repeat experiments/statistics, failure taxonomy, second adapter | Persistence and single-agent evidence precede repeated experiments and cross-model comparison |
| Advanced Platform Capabilities | Calibrated qualitative grading, justified queueing, API/auth, dashboard | Only after the core is credible |

The roadmap's ten MVP use cases are the intended product boundary, delivered progressively. The first end-to-end pipeline proves the execution path; it does not satisfy the later two-adapter, statistical benchmark milestone. Manual comparison of repeated outputs precedes automated experiment aggregation.

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

Python, typed contracts, PostgreSQL, and Docker are roadmap directions. Their design rationale and exact contracts will be established during architecture design before implementation.

## Product and evaluation foundation acceptance criteria

- Both project titles and the current documentation-only status are visible in the README.
- All five actors and ten MVP use cases are described with evidence and failure considerations.
- Product targets are separated from achieved results and from the first runnable milestone.
- Research notes identify tradeoffs, evidence sources, and open questions without claiming experiments were run.
- The roadmap describes capability dependencies, and the documentation is navigable and distinguishes planned capabilities from implemented behavior.

Open architecture decisions include environment support, artifact retention, hidden-test protection, scoring statuses, digest semantics, and how a hosted agent can access model services without granting arbitrary network access to candidate code.
