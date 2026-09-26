# Actors and trust assumptions

These are workflow roles, not implemented authentication roles. One person may occupy several roles during local development, but authorship, review, and operational responsibility should remain distinguishable in evidence.

| Actor | Primary need | Produces or changes | Reads | Trust concern |
| --- | --- | --- | --- | --- |
| Benchmark author | Define a meaningful, reproducible task | Problem statement, snapshot reference, visible/hidden checks, provenance and license notes | Validation evidence | An author can accidentally leak answers or define an invalid task; review is required |
| Evaluator/researcher | Compare agent configurations fairly | Experiment inputs, budgets, attempt schedule | Permitted artifacts and results | Changing budgets or excluding failures after seeing outcomes can bias comparisons |
| Reviewer | Assess task validity and interpretation | Review findings, approval decisions, exclusion rationale | Task definitions, validation results, scoring evidence | Must be able to challenge authors and inspect changes to evaluation assets |
| Platform operator | Keep execution bounded and evidence available | Sandbox policy, trusted runtime configuration, cleanup and retention controls | Operational logs and resource evidence | Powerful host access must not be delegated to evaluated code; queues/storage are later responsibilities |
| Read-only consumer | Understand an experiment's relevance and limits | No benchmark or run mutations | Published reports and approved evidence | A published report must not expose secrets or held-out material |

## Initial trust boundaries

The operator-controlled runner and evaluation configuration form a trusted control boundary. Task repositories, setup commands, agent output, patches, filenames, and process output cross it as untrusted inputs. Review reduces mistakes but does not make executable repository contents safe.

The candidate workspace must not contain hidden tests, reference solutions, provider credentials, or host control sockets during agent execution. Grading assets require a separate access boundary and integrity checks; the mechanism must be defined during architecture design and validated during sandbox implementation.

Reports cross a publication boundary. The reviewer checks redaction and disclosure scope before evidence is shared. Access to a public report does not imply access to all underlying logs or held-out tasks.

No authentication system, permission database, or hosted tenancy model is introduced here. See [sandbox research](../research/sandbox-options.md) for the unresolved execution risks.
