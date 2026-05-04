# Harbor Framework

*Source: https://www.harborframework.com*

---

## What It Is

Harbor is a framework for **specifying, evaluating, and optimizing AI agents in sandboxed container environments**. It evolved from Terminal-Bench after the team identified a core gap: defining and managing containerized tasks at scale is hard. Harbor solves this with standardized interfaces and integrations so you don't build custom evaluation infrastructure from scratch.

---

## Installation

```bash
uv tool install harbor
harbor --help
```

---

## Core Concepts

| Concept | Description |
|---|---|
| **Sandboxed tasks** | Agent tasks run in isolated containers — safe, reproducible, parallelizable |
| **Modular interfaces** | Separate, composable interfaces for environments, agents, and tasks |
| **Centralized registry** | Pre-built benchmarks and datasets available via `harbor dataset list` |
| **Cloud sandbox providers** | Scale horizontally via Daytona, Modal, E2B, or Runloop |
| **Optimization integration** | Native hooks into SkyRL and GEPA for RL and prompt optimization |

---

## Running Evaluations

**Against a registered dataset:**
```bash
harbor run -d "<org/name>" -m "<model>" -a "<agent>"
```

**Against a local dataset:**
```bash
harbor run -p "<path/to/dataset>" -m "<model>" -a "<agent>"
```

**With cloud sandbox + horizontal scaling (recommended for speed):**
```bash
harbor run -d "<org/name>" -m "<model>" -a "<agent>" --env "daytona" -n 32
```

Parallelization matters here — sandboxed agent evals are slow because each task runs multiple turns. Scaling to 32 parallel workers cuts wall-clock time significantly.

---

## Use Cases

- **Custom eval suites** — define your own tasks and run them at scale
- **Prompt optimization** — integrate with GEPA for automated prompt tuning
- **Reinforcement learning** — generate reward signals from sandboxed task outcomes
- **SFT data generation** — produce supervised fine-tuning traces from agent trajectories
- **CI/CD agent testing** — gate deployments on eval pass rates

---

## Cloud Sandbox Providers

Harbor connects to four providers for distributed execution:

| Provider | Notes |
|---|---|
| **Daytona** | Dev environments in the cloud |
| **Modal** | Serverless compute, good for burst parallelism |
| **E2B** | Sandboxed code execution |
| **Runloop** | Dev sandbox infrastructure |

---

## Optimization Integrations

| Framework | What it enables |
|---|---|
| **SkyRL** | Reinforcement learning from agent trajectories |
| **GEPA** | Automated prompt optimization using eval signals |

---

## Why It Matters for Agent Development

The shift Harbor represents: from one-off benchmark runs toward a **composable, scalable evaluation platform**. Pre-integrated CLI agent support and a centralized task registry mean you can start evaluating against real tasks in minutes rather than building container scaffolding yourself.

**Key insight from the creators:** sandboxed evals benefit heavily from parallelization — design your eval pipelines to run tasks concurrently from the start.
