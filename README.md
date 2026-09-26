# RepoAgentEval

**A Repository-Level Evaluation and Benchmarking Platform for LLM-Based Software Engineering Agents**

**Xây dựng nền tảng đánh giá và benchmark tác nhân lập trình sử dụng mô hình ngôn ngữ lớn trên các tác vụ kỹ nghệ phần mềm ở mức kho mã nguồn**

RepoAgentEval will measure how coding agents change repositories: whether they satisfy requirements, preserve existing behavior, and produce reproducible evidence at an acceptable cost. Its intended output is a reviewable scorecard with patches, test results, configuration provenance, and explicit limitations.

## Current status

Project framing only (roadmap PR 1), pending review. This repository contains documentation; there is no runnable evaluator, dataset, package, API, or benchmark result yet. There are no installation or execution commands at this stage.

The [roadmap](docs/ROADMAP.md) is the primary source of truth. Its Part VI defines the PR sequence; the numbered phases organize the broader subject areas. Work proceeds one independently reviewable slice at a time. The next slice is **PR 2 — Architecture and ADR baseline**.

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

A versioned fixture task will run through manifest validation, repository preparation, isolated execution with a deterministic local agent, grading, and a JSON report. This vertical slice is scheduled for PR 10 after its prerequisites. It will establish evidence handling before hosted agents, repeated experiments, or database persistence are added.

The larger credible benchmark milestone requires 20–30 original tasks, multiple attempts, at least two adapters, and statistical reporting. Those are future acceptance targets, not current capabilities.

## Engineering approach

Start with a modular monolith and CLI evaluation core. Keep provider types behind adapters, prefer deterministic graders, version experimental inputs, and retain evidence for failures as well as successes. Treat repositories, generated patches, setup scripts, and emitted artifacts as untrusted. Docker is the roadmap's initial execution direction, with residual risks to be documented and tested.

Runtime and boundary decisions belong in PR 2 ADRs; tooling and CI belong in PR 3. No dependencies are required to review these documents. Review should focus on the product assumptions, milestone boundaries, measurement limits, and unresolved risks. Final design approval and merge remain with the maintainer.
