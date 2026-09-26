# ROADMAP — Repository-Level Evaluation Platform for Coding Agents

## 0. Project identity

**Vietnamese title**\
**Xây dựng nền tảng đánh giá và benchmark tác nhân lập trình sử dụng mô hình ngôn ngữ lớn trên các tác vụ kỹ nghệ phần mềm ở mức kho mã nguồn**

**English title**\
**A Repository-Level Evaluation and Benchmarking Platform for LLM-Based Software Engineering Agents**

**Suggested repository name**\
`repo-agent-eval`

**Working product name**\
`RepoAgentEval`

---

## 1. Problem statement

Coding models are increasingly capable of operating at repository level: reading source trees, locating relevant files, editing code, running tests, using developer tools, and iterating on failures. A pass/fail score from a public benchmark is not enough to answer the questions a technical evaluator or engineering team actually needs:

- Did the agent satisfy the requested behavior?
- Did it introduce regressions?
- Was the patch unnecessarily broad?
- Did it create a security or performance regression?
- Is the result reproducible across repeated attempts?
- Which task categories and failure modes are driving the score?
- What is the cost and latency per successful task?
- Can a model upgrade be accepted without silently reducing quality?
- Are automated graders aligned with human technical judgments?

The project will build a model-agnostic evaluation platform that executes coding agents against versioned repository-level tasks inside isolated environments, collects complete run artifacts, scores results with deterministic and calibrated evaluators, and produces reproducible comparisons and regression reports.

The platform is not intended to be a coding assistant or an IDE plugin. Its product is **measurement**.

---

## 2. Design principles

1. **Eval-driven development**\
   Evaluation definitions, datasets, expected outcomes, and failure taxonomies are first-class artifacts.

2. **Reproducibility before scale**\
   A run must be repeatable locally before distributed execution is introduced.

3. **Deterministic scoring before LLM judging**\
   Hidden tests, static checks, patch analysis, and measurable outcomes should be preferred whenever possible.

4. **Provider/model independence**\
   Core domain logic must not depend on one LLM vendor, one API, or one agent implementation.

5. **Repository isolation by default**\
   Candidate code is untrusted. Execution should occur in ephemeral sandboxes with explicit resource and network policies.

6. **Small, reviewable changes**\
   One issue → one coherent pull request. Avoid feature bundles and repository-wide rewrites.

7. **Document decisions, not just code**\
   Important architectural decisions use ADRs. Requirements, threat assumptions, data contracts, and scoring semantics are versioned.

8. **No benchmark theatre**\
   A single aggregate score must never hide unstable runs, excluded cases, invalid tasks, or scoring uncertainty.

9. **Human calibration matters**\
   Any LLM-based grader must be validated against human labels before its output is treated as a meaningful metric.

10. **UI comes after the evaluation core**\
    The first useful interface is a CLI and machine-readable report, not a dashboard.

---

## 3. Reference engineering practices

The roadmap intentionally follows practices used in mature software and AI-evaluation organizations:

- Small, self-contained code changes and code review discipline.
- Threat modeling and secure-development practices integrated into the SDLC rather than bolted on at the end.
- Eval-driven development, representative datasets, continuous evaluation, and logging.
- Versioned datasets, graders, environments, and model configurations.
- Sandboxed execution for untrusted/model-generated code.
- Structured traces and artifacts for diagnosing agent behavior.
- Least-privilege CI permissions and secret management.

Key external references:

- OpenAI — Evaluation best practices: https://developers.openai.com/api/docs/guides/evaluation-best-practices
- OpenAI — Agent evaluations: https://developers.openai.com/api/docs/guides/agent-evals
- OpenAI — Why SWE-bench Verified no longer measures frontier coding capabilities: https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/
- UK AI Security Institute — Inspect: https://www.aisi.gov.uk/blog/open-sourcing-our-testing-framework-inspect
- NIST SP 800-218 — Secure Software Development Framework: https://csrc.nist.gov/pubs/sp/800/218/final
- Google Engineering Practices — Small CLs: https://google.github.io/eng-practices/review/developer/small-cls.html
- GitHub Actions security guidance: https://docs.github.com/en/actions/how-tos/secure-your-work

These references inform the design; the project should not copy their implementation blindly.

---

# PART I — DISCOVERY AND REQUIREMENTS

## Phase 0 — Discovery, framing, and feasibility

### Goal

Prove that the proposed product is worth building and define what “correct” means before writing application code.

### Deliverables

Create:

```text
README.md
docs/
  product/
    vision.md
    scope.md
    personas.md
    use-cases.md
    success-metrics.md
  research/
    benchmark-landscape.md
    evaluation-methods.md
    sandbox-options.md
    data-contamination-notes.md
  glossary.md
```

### 0.1 Actors and stakeholders

Document at least these roles:

**Benchmark author**
- Creates task specifications.
- Defines repository snapshot and expected behavior.
- Defines visible and hidden evaluation assets.

**Evaluator/researcher**
- Selects model/agent configuration.
- Executes experiments.
- Interprets results and compares runs.

**Reviewer**
- Reviews task validity, scoring semantics, exclusions, and benchmark changes.

**Platform operator**
- Maintains sandbox execution, queues, storage, CI, secrets, and operational controls.

**Read-only consumer**
- Reads benchmark reports and experiment summaries without mutating data.

Actor definitions and trust boundaries precede authentication design.

### 0.2 Core use cases

The MVP must support:

1. Register a benchmark.
2. Register a versioned repository-level task.
3. Validate a task before it can enter the benchmark.
4. Run one agent configuration against one task.
5. Capture repository state before/after execution.
6. Execute deterministic graders.
7. Store run metadata and artifacts.
8. Produce a machine-readable result.
9. Compare repeated attempts.
10. Re-run the same task reproducibly.

Later phases add:
- Batch runs.
- Multiple agent providers.
- statistical comparison,
- dashboards,
- continuous regression evaluation,
- multi-user controls.

### 0.3 Explicit non-goals for MVP

The initial MVP excludes:

- A general-purpose IDE.
- An autonomous code-generation SaaS.
- A marketplace of prompts.
- Kubernetes orchestration.
- Multi-region deployment.
- Enterprise SSO.
- Billing.
- A complex React dashboard.
- Arbitrary internet access from sandboxes.
- Hundreds of benchmark tasks.

### 0.4 Success criteria

A minimum credible milestone:

- 20–30 internally authored repository-level tasks.
- At least two task categories.
- At least two different agent/model adapters.
- Reproducible Docker-based execution.
- Hidden and regression tests.
- Structured run artifacts.
- Multiple attempts per task.
- Failure taxonomy.
- Statistical summary with confidence intervals.
- CI regression gate on the platform itself.
- Documented limitations.

An expanded benchmark milestone:

- 80–150 reviewed tasks.
- Python + TypeScript repositories.
- 4–6 task categories.
- 3+ agent/model configurations.
- Human-calibrated rubric grader for at least one non-deterministic dimension.
- Cost/latency/stability analysis.
- Public benchmark card and experiment report.

---

## Phase 1 — Requirements, architecture, and contracts

### Goal

Define domain semantics and interfaces before implementing business logic.

### Deliverables

```text
docs/
  architecture/
    system-context.md
    container-view.md
    component-boundaries.md
    execution-sequence.md
    data-flow.md
  adr/
    0001-language-and-runtime.md
    0002-storage.md
    0003-sandbox-boundary.md
    0004-agent-adapter-contract.md
    0005-task-versioning.md
    0006-scoring-model.md
  api/
    domain-contracts.md
  security/
    threat-model.md
    trust-boundaries.md
```

### 1.1 Recommended architecture

Start as a **modular monolith** with explicit internal boundaries.

Suggested stack:

- Python 3.13+
- FastAPI only when HTTP API is actually needed
- Pydantic v2 for contracts/configuration
- SQLAlchemy 2 + Alembic
- PostgreSQL
- Typer or Click for CLI
- Docker for sandbox execution
- pytest
- Ruff
- mypy/pyright
- pre-commit
- GitHub Actions

Delay Redis/worker queues until synchronous execution semantics are stable.

### 1.2 Suggested component boundaries

```text
repo_agent_eval/
  domain/
  application/
  adapters/
    agents/
    repositories/
    persistence/
  execution/
    sandbox/
    workspace/
  evaluation/
    deterministic/
    rubric/
    aggregation/
  reporting/
  telemetry/
  cli/
```

Rules:

- `domain` imports no infrastructure package.
- Provider SDKs live only under adapters.
- Sandbox implementation is behind an execution interface.
- Scoring does not directly call persistence.
- Reporting consumes normalized result objects rather than database rows.

### 1.3 Initial domain entities

Define these before creating tables:

**Benchmark**
- id
- slug
- name
- description
- status
- current_version

**BenchmarkVersion**
- benchmark_id
- version
- created_at
- manifest_digest

**Task**
- id
- stable_key
- title
- category

**TaskVersion**
- task_id
- version
- repository_snapshot_id
- problem_statement
- setup_spec
- grader_spec
- resource_policy
- immutable_digest

**RepositorySnapshot**
- id
- source
- commit_sha
- archive_digest
- language_metadata

**AgentConfiguration**
- id
- adapter_type
- model_name
- configuration_digest
- tool_policy
- prompt/template version

**Run**
- id
- benchmark_version
- task_version
- agent_configuration
- seed/attempt index
- state
- timestamps
- environment_digest

**RunArtifact**
- logs
- patch
- stdout/stderr
- test reports
- tool trace
- resource usage

**MetricResult**
- metric key
- raw value
- normalized value
- status
- scorer version
- evidence reference

**FailureLabel**
- category
- severity
- evidence

### 1.4 State machines

Define explicit states.

Task version:

```text
draft → validating → valid
                 ↘ invalid
valid → deprecated
```

Run:

```text
created
→ preparing
→ executing
→ grading
→ completed

Any active state → failed
Any active state → timed_out
Any active state → cancelled
```

Illegal state transitions must be tested.

---

# PART II — SECURITY AND DATA DESIGN

## Phase 2 — Threat model and execution security

### Goal

Treat model-generated code and repository fixtures as untrusted input.

### Required threat scenarios

Document at least:

- malicious repository script,
- fork bomb,
- excessive memory/CPU consumption,
- filesystem escape attempt,
- network exfiltration,
- secret discovery,
- package-install script side effects,
- symlink attacks,
- oversized logs,
- path traversal in artifacts,
- command injection through task metadata,
- poisoned test fixture,
- malicious patch content,
- compromised CI dependency.

### Sandbox baseline

For the MVP:

- disposable container per attempt,
- non-root execution,
- CPU limit,
- memory limit,
- PID limit,
- wall-clock timeout,
- writable workspace limited to the task directory,
- read-only infrastructure/config mounts,
- no host Docker socket,
- no privileged mode,
- network disabled by default,
- explicit allowlist if a task genuinely needs network,
- no production secrets mounted into the sandbox,
- artifact size limits,
- explicit cleanup.

The Docker boundary is not equivalent to a hardened VM; its residual risks require documentation and validation.

### CI security baseline

- Explicit minimal `permissions` in GitHub Actions.
- Avoid long-lived cloud credentials.
- Pin important third-party actions to immutable versions/SHAs where practical.
- Enable dependency scanning.
- Keep secrets out of test fixtures and logs.
- Add secret scanning.
- Separate trusted CI from evaluation of untrusted repository contents.

---

## Phase 3 — Database and persistence design

### Goal

Store experimental evidence without losing reproducibility.

### PostgreSQL rationale

Use relational tables for stable domain entities and relationships. Use JSONB only for fields that genuinely vary by adapter/scorer.

Do not place the entire run as one JSON blob.

### Initial relational schema

Recommended tables:

```text
benchmarks
benchmark_versions
tasks
task_versions
benchmark_version_tasks
repository_snapshots
agent_configurations
runs
run_artifacts
metric_definitions
metric_results
failure_labels
run_failure_labels
```

Later:

```text
experiments
experiment_runs
human_judgments
grader_calibrations
comparison_reports
```

### Database rules

- UUID/ULID primary keys are acceptable; choose one and document it.
- Stable human-readable keys must be unique separately from IDs.
- Immutable task versions are never edited after publication.
- Model configuration must be content-addressable or have a configuration digest.
- Every score records the scorer version.
- Every run records environment/config digests.
- Destructive deletes should be restricted; prefer archival where evidence integrity matters.
- DB migration files are mandatory for schema changes.
- Each migration requires upgrade/downgrade or an explicit irreversible-migration note.

---

# PART III — IMPLEMENTATION SEQUENCE

## Phase 4 — Vertical slice: one task, one local agent, one deterministic score

### Goal

Build the smallest complete evaluation flow.

### Scope

Implement:

```text
task manifest
→ validation
→ repository checkout/snapshot
→ sandbox
→ agent adapter
→ patch
→ tests
→ score
→ JSON report
```

Use a deliberately simple local/mock agent first.

### Acceptance criteria

- A valid task runs end to end.
- An invalid manifest is rejected before execution.
- Timeout behavior is deterministic.
- Patch is captured.
- Test outcomes are parsed structurally.
- A run result is serializable.
- Unit + integration tests cover success/failure paths.
- Same inputs produce equivalent deterministic evaluation outputs.

No web UI.

---

## Phase 5 — Task specification and benchmark authoring

### Goal

Make tasks reviewable and difficult to accidentally invalidate.

### Suggested task manifest

```yaml
schema_version: 1
task_key: py-validation-001
repository:
  source: fixture
  revision: ...
problem:
  statement: ...
setup:
  commands: [...]
evaluation:
  fail_to_pass: [...]
  pass_to_pass: [...]
resources:
  timeout_seconds: ...
  memory_mb: ...
network:
  mode: disabled
```

### Validation pipeline

Check:

- required fields,
- repository revision exists,
- setup succeeds,
- baseline tests have expected state,
- hidden evaluator assets are not exposed,
- target bug/feature is reproducible,
- gold/reference patch is not required by the runner,
- task can be reset and rerun,
- task does not depend on unstable network state.

### Dataset governance

Every task gets:

- author,
- reviewer,
- version,
- creation date,
- source/provenance,
- license note,
- category,
- difficulty notes,
- known limitations.

Do not use public benchmark examples as the only project dataset. Create original held-out tasks to reduce contamination risk.

---

## Phase 6 — Real coding-agent adapters

### Goal

Support multiple agents without leaking provider logic into the domain.

### Adapter contract

A coding agent receives:

- workspace reference,
- problem statement,
- allowed tools,
- execution budget,
- optional model settings.

It returns:

- completion status,
- final patch,
- structured tool trace,
- token/cost metadata where available,
- error information.

### Provider policy

Implement one adapter fully before adding the second.

Suggested order:

1. CLI/local adapter for deterministic test doubles.
2. First real hosted model/agent.
3. Second provider/model.
4. Optional Inspect-compatible adapter.

Never make provider response formats the persistence schema.

---

## Phase 7 — Scoring system

### Goal

Separate facts from judgments.

### Layer 1 — deterministic graders

Prioritize:

- FAIL_TO_PASS tests,
- PASS_TO_PASS/regression tests,
- lint/type checks,
- security static analysis,
- performance benchmark,
- patch/file scope,
- changed-lines count,
- mutation-test score where appropriate.

### Layer 2 — rubric graders

Possible dimensions:

- requirement completeness,
- unnecessary complexity,
- maintainability,
- explanation quality.

LLM graders are optional and must be calibrated.

### Score semantics

Avoid one magic score initially.

Produce a scorecard:

```text
functional_correctness
regression_safety
security
quality
efficiency
stability
cost
latency
```

Any aggregate score must document:
- normalization,
- weights,
- missing-value behavior,
- invalid-run behavior,
- statistical limitations.

---

## Phase 8 — Human evaluation and grader calibration

### Goal

Prove that automated qualitative grading corresponds to expert judgment.

### Workflow

1. Sample outputs across success/failure categories.
2. Create a blinded human-evaluation form.
3. Obtain independent judgments.
4. Measure inter-rater agreement.
5. Compare automated grader to human labels.
6. Analyze disagreements.
7. Revise rubric or grader.
8. Version the grader.

Useful statistics:

- accuracy,
- precision/recall/F1,
- Cohen’s kappa,
- rank correlation,
- confusion matrix.

Claims of grader validity require evidence from a completed calibration experiment.

---

## Phase 9 — Experiment runner and statistics

### Goal

Move from isolated runs to defensible model comparisons.

### Experiment configuration

```text
benchmark_version
agent configurations
number of attempts
randomization policy
resource policy
exclusion rules
```

### Report

At minimum:

- task count,
- valid/invalid/excluded counts,
- solve rate,
- per-category outcomes,
- confidence intervals,
- repeated-run variance,
- failure distribution,
- cost per successful task,
- latency per successful task,
- timeout/crash rates.

Avoid unsupported significance claims.

Keep raw results available so the report can be reproduced.

---

## Phase 10 — Async execution and scale

Only introduce queues when profiling proves synchronous execution is limiting.

Potential additions:

- worker process,
- Redis or PostgreSQL-backed queue,
- concurrency controls,
- retry policy,
- idempotency keys,
- leases/heartbeats,
- orphaned-run recovery.

Important distinction:

**Infrastructure retry ≠ additional model attempt.**

These must be stored differently.

---

## Phase 11 — API and authentication

### Goal

Expose a stable platform boundary after core semantics have matured.

Recommended API resources:

```text
/benchmarks
/tasks
/agent-configurations
/runs
/experiments
/reports
```

### Authentication

For a hosted multi-user version:

- OIDC/OAuth2 preferred over custom password auth.
- Separate identity from authorization.
- Roles:
  - admin
  - benchmark_author
  - evaluator
  - reviewer
  - viewer
- Server-side authorization checks are mandatory.
- Do not trust UI visibility as authorization.

Authentication is outside the first CLI-only milestone.

---

## Phase 12 — Dashboard and observability

### Dashboard should answer questions, not just look polished

Views:

- benchmark overview,
- experiment comparison,
- task/category breakdown,
- failure taxonomy,
- attempt variance,
- run trace,
- patch/test artifacts,
- cost/latency trends,
- grader disagreement.

### Observability

Collect:

- structured application logs,
- correlation/run IDs,
- state transitions,
- queue timings,
- sandbox resource use,
- model/tool event metadata,
- failure codes.

Never log credentials or secret-bearing environment variables.

---

## Phase 13 — Continuous evaluation

### Goal

Turn the project into regression infrastructure.

Examples:

- Platform PR → unit/integration/security tests.
- Scorer change → scorer regression suite.
- Task change → task validation suite.
- Agent/model update → controlled benchmark run.
- Report changes → snapshot/schema validation.

Do not run expensive model benchmarks on every small PR by default. Use manual/nightly/release workflows where appropriate.

---

# PART IV — TEST STRATEGY

## Test layers

### Unit
- value objects,
- state transitions,
- parsers,
- scoring functions,
- config validation.

### Contract
- agent adapter contract,
- sandbox interface,
- artifact store interface,
- provider response normalization.

### Integration
- PostgreSQL,
- Alembic migrations,
- Docker sandbox,
- repository checkout,
- test-report parsing.

### End-to-end
- one real fixture repository through the complete evaluation flow.

### Security
- command/path injection,
- hostile filenames,
- oversized artifacts,
- timeout/fork behavior,
- network policy,
- permission boundary.

### Property/fuzz testing
Suitable for:
- manifest parsers,
- score normalization,
- state transition inputs,
- artifact paths.

---

# PART V — DEVELOPMENT PRINCIPLES

- Changes focus on one primary engineering concern.
- Relevant tests and documentation evolve with behavior.
- Significant architectural decisions are recorded in ADRs.
- Working behavior is preserved across incremental milestones.
- Substantial refactoring is separated from unrelated feature work where practical.

---

# PART VI — CAPABILITY MILESTONES

The milestones below follow dependency order. Domain contracts and execution boundaries precede the end-to-end pipeline; persistence and single-agent evidence precede repeated experiments and cross-model evaluation. Each milestone can be delivered through small, independently reviewable changes.

### Product and Evaluation Foundations
- README skeleton.
- Vision.
- scope/non-goals.
- actors/use cases.
- terminology.
- initial roadmap.
- no production implementation.

### Architecture and Security Design
- System context.
- component boundaries.
- initial ADRs.
- threat-model draft.
- no provider integration.

### Engineering Quality Baseline
- Python packaging.
- formatter/linter/type checker.
- pytest.
- pre-commit.
- minimal CI.
- dependency update/security configuration.

### Core Domain Model
- Benchmark/task/run contracts.
- state-machine rules.
- tests.

### Task Specification
- task manifest schema v1,
- parser,
- validation,
- fixtures,
- tests.

### Repository Workspace
- fixture repository,
- checkout/reset,
- patch capture,
- tests.

### Sandboxed Execution
- container execution,
- resource policy,
- timeout,
- network disabled,
- security tests.

### Agent Abstraction
- adapter protocol,
- deterministic mock/local implementation,
- contract tests.

### Deterministic Evaluation
- structured test results,
- fail-to-pass/pass-to-pass semantics,
- tests.

### End-to-End Evaluation Pipeline
- CLI command,
- one sample task,
- JSON result,
- integration test.

### Persistence and Provenance
- PostgreSQL schema/migrations,
- repository interfaces,
- migration tests.

### Real Coding-Agent Integration
- provider isolation,
- configuration,
- sanitized tracing,
- mocked contract tests.

### Experimentation and Statistical Analysis
- repeated attempts,
- aggregation,
- statistical report.

### Failure Analysis
- documented labels,
- classifier workflow,
- report output.

### Cross-Model Evaluation
- second agent adapter,
- comparison report,
- provider-independent metrics.

### Advanced Platform Capabilities

Human calibration, queueing, API, authentication, and UI follow a credible evaluation core. Queueing additionally requires evidence that synchronous execution limits throughput.

---

# PART VII — PROJECT ACCEPTANCE CRITERIA

A working application alone is insufficient for credible evaluation. The project must provide inspectable evidence of:

- clear project scope and non-goals,
- ADRs,
- architecture diagrams or textual architecture,
- threat model,
- versioned schema/migrations,
- automated test layers,
- secure sandbox boundary,
- deterministic evaluation logic,
- model/provider adapters,
- failure taxonomy,
- reproducible benchmark manifest,
- experiment report,
- limitations,
- reproducible CI validation,
- independently reviewable changes.

Published evaluation results must be traceable to versioned inputs, execution evidence, and documented scoring semantics. Reports must disclose uncertainty, exclusions, and known limitations.

---

# PART VIII — DEFINITION OF DONE

A feature is done only when applicable items are complete:

- requirement/issue exists,
- acceptance criteria are explicit,
- design impact considered,
- code is typed and lint-clean,
- automated tests exist,
- negative/error paths tested,
- migration included if schema changed,
- security implications reviewed,
- docs updated,
- no secrets or private benchmark material exposed,
- CI passes,
- the change is independently reviewable,
- known limitations are recorded.

---

# PART IX — FUTURE RESEARCH DIRECTIONS

Potential extensions after a stable core:

- contamination-resistant/private benchmark generation,
- adversarial repository tasks,
- multilingual coding tasks,
- multi-agent software-engineering workflows,
- long-horizon issue resolution,
- test-generation quality metrics,
- semantic patch-equivalence analysis,
- flaky-test-aware scoring,
- performance regression attribution,
- reproducible remote sandboxes,
- benchmark drift analysis,
- model-routing experiments,
- evaluator uncertainty estimation.

The core architecture should make these extensions possible without requiring a rewrite.
