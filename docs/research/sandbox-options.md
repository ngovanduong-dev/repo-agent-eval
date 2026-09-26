# Sandbox feasibility and residual risk

Initial desk research, consulted 2026-09-26. No isolation, performance, or compatibility experiment has been run. The roadmap specifies Docker for the initial milestone; the boundary and its supported host environment must be justified in PR 2 and validated in PR 7.

| Option | Mechanism described by primary source | Project assessment and unanswered question |
| --- | --- | --- |
| [Docker Engine](https://docs.docker.com/engine/security/) | Namespaces, control groups, capabilities and daemon controls | Practical roadmap baseline, but shared-kernel and daemon exposure risks remain. Which host configuration can enforce every required limit? |
| [gVisor](https://gvisor.dev/docs/) | A userspace application kernel and OCI runtime mediate application system calls | Potential additional boundary later; compatibility and syscall overhead need measurement on representative tasks |
| [Firecracker](https://firecracker-microvm.github.io/) | Linux KVM-based microVMs with a separate virtualization boundary | A later isolation option; image lifecycle, host virtualization support and operations would need separate design |

The project assessments are inferences from these mechanisms and the roadmap's small-scope constraint. They are not comparative benchmark findings or security certifications.

## Required initial policy

- Disposable container per attempt, non-root execution, no privileged mode and no mounted host Docker socket.
- Explicit CPU, memory, PID and wall-clock limits; bounded logs and artifacts; cleanup on success, error, timeout and cancellation.
- Writes confined to the task workspace; infrastructure/configuration mounts read-only; no broad host mounts or production secrets.
- Network disabled by default. Any justified exception must have a reviewed allowlist and reproducibility rationale.
- Setup scripts and dependency installation are also untrusted execution; they do not bypass the sandbox.

Docker is not equivalent to a hardened VM or a proof that hostile code cannot affect the host. Shared-kernel vulnerabilities, unsafe mounts, daemon access, dependency supply-chain problems, and configuration mistakes remain risks. A dedicated evaluation environment should be assessed before running genuinely hostile tasks.

## Questions and validation work for later slices

PR 2 must describe the host trust boundary, supported environment, credential placement, hidden-grader isolation, and artifact handling. Hosted provider access will need a design that does not silently grant candidate code general network access or provider credentials.

PR 7 security checks must address malicious setup scripts, resource exhaustion/fork bombs, filesystem and symlink escapes, network exfiltration, secret discovery, command injection from metadata, path traversal, oversized output, and cleanup failure. Grader and workspace designs must also consider poisoned fixtures, malicious patches, protected-test tampering, and fabricated reports. PR 3's CI design must address compromised dependencies and separate trusted platform checks from untrusted evaluation execution.

These are requirements to demonstrate later. There is no executable sandbox configuration in PR 1.
