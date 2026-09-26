# RepoAgentEval

**A Repository-Level Evaluation and Benchmarking Platform for LLM-Based Software Engineering Agents**

**Xây dựng nền tảng đánh giá và benchmark tác nhân lập trình sử dụng mô hình ngôn ngữ lớn trên các tác vụ kỹ nghệ phần mềm ở mức kho mã nguồn**

RepoAgentEval will measure how coding agents change repositories: whether they satisfy requirements, preserve existing behavior, and produce reproducible evidence at an acceptable cost. Its intended output is a reviewable scorecard with patches, test results, configuration provenance, and explicit limitations.

## Current status

The product and evaluation foundations are documented. Executable evaluation infrastructure has not yet been implemented; there is no runnable evaluator, dataset, package, API, or benchmark result. There are no installation or execution commands at this stage.

The [roadmap](docs/ROADMAP.md) describes capability milestones and their dependencies. Architecture, security boundaries, and execution contracts are planned next.

## Start here

| Document | Question it answers |
| --- | --- |
| [Vision](docs/product/vision.md) | What decision should this platform improve? |
| [Scope](docs/product/scope.md) | What belongs in each milestone, and what is excluded? |
| [Personas](docs/product/personas.md) | Who produces, operates, reviews, and consumes evidence? |
| [Use cases](docs/product/use-cases.md) | What must the MVP enable, including failure paths? |
| [Success metrics](docs/product/success-metrics.md) | What evidence will demonstrate progress? |
| [Benchmark landscape](docs/research/benchmark-landscape.md) | What can existing benchmarks and frameworks teach us? |
| [Evaluation methods](docs/research/evaluation-methods.md) | What can tests measure, and where is judgment needed? |
| [Sandbox options](docs/research/sandbox-options.md) | What isolation assumptions need validation? |
| [Data contamination](docs/research/data-contamination-notes.md) | How could task exposure compromise conclusions? |
| [Glossary](docs/glossary.md) | What do the project terms mean? |

## Intended first runnable milestone

A versioned fixture task will run through manifest validation, repository preparation, isolated execution with a deterministic local agent, grading, and a JSON report. This pipeline depends on validated task specifications, workspace management, sandboxing, an agent abstraction, and deterministic grading. It will establish evidence handling before hosted agents, repeated experiments, or database persistence are added.

The larger credible benchmark milestone requires 20–30 original tasks, multiple attempts, at least two adapters, and statistical reporting. Those are future acceptance targets, not current capabilities.

## Engineering approach

Start with a modular monolith and CLI evaluation core. Keep provider types behind adapters, prefer deterministic graders, version experimental inputs, and retain evidence for failures as well as successes. Treat repositories, generated patches, setup scripts, and emitted artifacts as untrusted. Docker is the roadmap's initial execution direction, with residual risks to be documented and tested.

Runtime and boundary decisions will be documented in ADRs. Tooling and CI will establish the engineering quality baseline before evaluation infrastructure is implemented. The documentation identifies product assumptions, milestone boundaries, measurement limits, and unresolved risks.
