# Awesome-Agent-Sandbox-Security-Frameworks

# Top Agent Sandbox Security Frameworks Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on MicroVM Isolation, Syscall Interception & Deterministic Execution Boundaries*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Agent Sandbox Security Frameworks**. These tools provide the execution boundary that keeps rogue recursive script executions, prompt-injected code, and runaway agent loops safely trapped within isolated environments—preventing kernel exploits, credential theft, and lateral movement.

**Examples** include E2B, Modal, Daytona, Docker Sandboxes, Fly.io Machines, Isolated VM, Sphere Engine, CodeSandbox, Replit Containers, and Wasmer (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, custom sandbox runtimes, and open isolation layers—ideal for platform engineers, security architects, and developers seeking transparent, vendor-independent agent execution containment.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **[E2B](https://e2b.dev/)**  
  Agent sandbox platform running each sandbox in a **Firecracker microVM** with its own Linux kernel, dedicated memory, and no shared state. Backed by $35M funding, used by roughly half the Fortune 500. SDKs for Python, TypeScript, and Java. ~150ms cold boot, custom templates for pre-configured environments. Pro plan adds $150/month base fee. Ephemeral only—no persistent state without external storage. CPU-only, no GPU support .

- **[Modal](https://modal.com/)**  
  The only major agent sandbox platform with **native GPU support** (NVIDIA A10G, A100, H100). Sandboxes run inside **gVisor containers**—weaker isolation than Firecracker microVMs, but stronger than plain Docker. Scales to 50,000+ concurrent containers. Python-first API. Sandbox workloads priced at ~$0.14/vCPU-hour (roughly 3x general Modal serverless functions). Cold starts of 300-500ms .

- **[Daytona](https://daytona.io/)**  
  Agent sandbox platform with ~$0.05/vCPU-hour pricing and no base fee. $200 free credit. Per-second billing. No native GPU support. No environment branching—sandboxes created from scratch each time. CPU-only, best for standard code execution workflows without GPU dependencies .

- **[Docker Sandboxes](https://www.docker.com/)**  
  Each agent gets its own **isolated microVM and kernel**, separate from the developer's host environment. Available through free standalone CLI. Developers define sandbox access to files, networks, and secrets—policies stay outside the agent's control. Docker Cloud Sandboxes extend the same microVM isolation to cloud compute, allowing work to continue after laptop closure. Sandbox Kits (standard OCI images) package agent, tools, and guardrails as versioned, shareable artifacts .

- **[Fly.io Machines](https://fly.io/)**  
  Lightweight VMs with fast startup and global edge deployment. Used for running agent workloads with hardware-enforced isolation.

- **[Isolated VM](https://isolated-vm.io/)**  
  Node.js library for running JavaScript in isolated V8 contexts. Provides memory isolation and resource limits within a single process—not a full VM boundary.

- **[Sphere Engine](https://sphere-engine.com/)**  
  Code execution platform with sandboxed environments for running untrusted code. Used for competitive programming, education, and agent code evaluation.

- **[CodeSandbox](https://codesandbox.io/)**  
  Cloud development environment with sandboxed execution for web applications. Uses microVMs for isolation on the server side.

- **[Replit Containers](https://replit.com/)**  
  Cloud development environment with containerized execution. Provides isolated environments for running agent-generated code.

- **[Wasmer](https://wasmer.io/)**  
  WebAssembly-based runtime with **secure sandboxed execution of untrusted code**—no containers or VMs required. Users define tightly scoped, fine-grained permissions to limit attack surface. WebAssembly enables much more resource-effective startup, scaling, and operation than traditional container and virtualization technologies. Runs tens of thousands of applications per server with minimal overhead .

## Open-Source GitHub Projects

- **[agentbox](https://pypi.org/project/agentbox-sandbox/)**  
  Self-hosted code execution sandbox for AI agents (open-source). `docker compose up` gives you an HTTP API for running untrusted code in isolated environments. Features **Docker backend** (ephemeral `docker run --rm`, `--network=none`, optional `--memory`), **default-deny egress** with allowlists, **per-request timeouts** and **memory limits**, **workspace snapshots** (`"snapshot": true` then `"snapshot_id"`), and **credential stripping** when backend is not unrestricted. Python SDK + TypeScript client. Not gVisor/Firecracker (yet)—stronger than raw `docker exec`, weaker than microVMs .

- **[Mitos](https://pkg.go.dev/mitos.run/mitos)**  
  KVM-backed snapshot-fork sandbox runtime (Go). Features **snapshot-fork sandboxes** via CLI, **gRPC runtime path** (`sandbox.v1.Sandbox`) over vsock to real guest VMs, **per-fork networking** with distinct guest MAC addresses and IPs (fork-correctness validation), **MCP server** exposing sandbox lifecycle (create, exec, file IO, fork, terminate) as Model Context Protocol tools, and **reproducible benchmark harness** (`claim-bench`) measuring fork and exec data path against real KVM engine .

- **[deepagents-docker](https://pypi.org/project/deepagents-docker/)**  
  Docker sandbox backend for Deep Agents (open-source). Runs agents in isolated Docker containers without compromising host machine. Features **long-lived container** with shell commands executed inside, **bind-mounted workspace directory** for file sharing, **resource limits** (memory, CPU, PIDs), **read-only root filesystem** with tmpfs mounts for `/tmp` and `/var/tmp`, **outbound traffic control**, and **automatic container cleanup** via `atexit` or context manager. Explicitly marked: "Use this for trusted workloads and development, not as a hard multi-tenant boundary" .

- **[agent-sandbox-runtime](https://pypi.org/project/agent-sandbox-runtime/)**  
  Docker-isolated code execution with self-correction loop (open-source). Features **Reflexion Loop** with LangGraph state machine, **5 specialist agents** (Architect, Coder, Critic, Optimizer, Security), **Docker isolation** with memory/CPU limits and no network by default, **6 LLM providers** (Groq, OpenRouter, Anthropic, Google Gemini, OpenAI, Ollama), and **FastAPI server + CLI**. Benchmark: 92% success rate, 743ms average response .

- **[ipybox](https://pypi.org/project/jarvis-ipybox/)**  
  Lightweight, stateful, secure Python code execution sandbox (Apache-2.0) built with IPython and Docker. Designed for AI agents that interact with their environment through code execution. Stateful—maintains IPython kernel state across executions. Fork available as `jarvis-ipybox` .

- **[Microsoft Execution Containers (MXC)](https://blogs.windows.com/windowsdeveloper/2026/10/07/microsoft-execution-containers-policy-driven-containment-for-ai-agents/)**  
  Policy-driven execution layer for untrusted code or dynamically generated workloads (open-source). Developers declare required resources (files, network destinations); MXC enforces the boundary. Policy remains outside agent's control—agent cannot grant itself additional access. **Spectrum of containment options**: Process container (Windows AppContainer, macOS Seatbelt, Linux Bubblewrap), Session container (Windows-only, separate OS session), WSL container (Linux-first toolchains), and MicroVM (experimental, hardware-enforced isolation). Unified JSON config schema and multi-language SDK. Maps to backends on Windows, macOS, or Linux .

- **[gVisor](https://github.com/google/gvisor)**  
  Application kernel (Apache-2.0) providing sandboxed container execution. Intercepts syscalls in userspace via the Sentry process—host kernel is never invoked directly by guest code. **Systrap** uses seccomp for better performance; **KVM** mode uses virtualization for syscall isolation (fastest on bare-metal). **Directfs** for high-performance file operations. Integrates with Docker, containerd, and Kubernetes as a runtime class. Overhead: 2-5% CPU-bound, 15-30% filesystem-heavy, 30-50% for high-bandwidth UDP. Used in production at Google (App Engine, Cloud Run) .

- **[Firecracker](https://github.com/firecracker-microvm/firecracker)**  
  KVM-based microVM monitor (Apache-2.0) by AWS. Powers AWS Lambda and Fargate. Each microVM runs its own Linux kernel with hardware-enforced isolation. **~125ms boot**, **<5 MiB memory overhead per VM**, up to 150 microVMs per second per host. Minimal device emulation (virtio-net, virtio-block, virtio-vsock, serial console, keyboard controller). ~50,000 lines of Rust vs QEMU's ~2M lines of C. Requires VM orchestration control plane—operationally more involved than gVisor .

- **[Kata Containers](https://github.com/kata-containers/kata-containers)**  
  Open-source secure container runtime (Apache-2.0) running containers inside lightweight VMs. Combines container speed with VM-level isolation. Kubernetes-native control plane on top of Firecracker or other hypervisors .

### Additional Strong Open-Source Options

- **[Bubblewrap](https://github.com/containers/bubblewrap)** — Unprivileged sandboxing tool (LGPL-2.0) used by Flatpak. Linux-only, uses user namespaces for isolation.
- **[nsjail](https://github.com/google/nsjail)** — Process isolation tool (Apache-2.0) using Linux namespaces, seccomp-bpf, and cgroups.
- **[isolate](https://github.com/ioi/isolate)** — Sandbox for running programs with restricted resources (GPL-2.0). Used in competitive programming.
- **[Jailer](https://github.com/google/jailer)** — Sandboxing tool (Apache-2.0) from Google for running untrusted code with seccomp and namespace restrictions.
- Community **n8n** workflows for sandbox lifecycle automation and security alerting.

**Frameworks for building custom systems**: Combine **Firecracker** or **Kata Containers** for hardware-enforced microVM isolation with **gVisor** for syscall-level containment, **agentbox** for Docker-based HTTP API execution, **MXC** for policy-driven containment across platforms, and **Mitos** for KVM-backed snapshot-fork sandboxes—creating a fully self-hosted, enterprise-grade agent sandbox security platform.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Agent sandbox frameworks handling untrusted code must follow security best practices and comply with applicable regulations (SOC 2, GDPR, etc.).
- Self-hosted open-source solutions require proper security hardening, kernel management, and ongoing maintenance. **Standard Docker containers share the host kernel**—if an attacker breaks out through a kernel exploit, they gain access to the host and every other container on that host . For production AI agents executing untrusted code, use **Firecracker microVMs or Kata Containers** for hardware-enforced boundaries. gVisor is acceptable for compute-heavy workloads with limited I/O where you control the code. Containers alone are insufficient for untrusted code. NVIDIA guidance: "run agentic tools within a fully virtualized environment isolated from the host kernel at all times, including VMs, unikernels, or Kata containers" .

---

**Made for platform engineers, security architects, and developers building transparent agent sandbox solutions.**  
Let's make Agent Sandbox Security more open, data-driven, and contained.
