# Claude Code's 5-Layer Agent Development Kit

**Source:** [To Data & Beyond — Claude Code's 5-Layer Agent Development Kit](https://todatabeyond.substack.com/p/claude-codes-5-layer-agent-development)

> "Most agent failures are architectural failures, not model failures."

Claude Code exposes a structured architecture of five distinct layers that work together to create reliable, scalable agent systems. Each layer solves a specific category of production failure independently, making the overall system more debuggable and maintainable than monolithic prompt-based approaches.

---

## Layer 1: CLAUDE.md — The Memory Layer

**Purpose:** Persistent project instructions and policy storage.

- Acts as the agent's "working constitution" — loaded at every session start.
- Hierarchical loading via directory traversal (walks up the tree).
- Scoped instructions via nested `CLAUDE.md` and `CLAUDE.local.md` files.
- Supports file imports up to five hops using `[@path/to/import]` syntax.
- Contains durable context: coding standards, architectural decisions, review checklists.
- Complements auto-memory (`MEMORY.md`), which stores Claude's own accumulated notes.

**Key insight:** CLAUDE.md is not context you paste once — it's the agent's working constitution.

---

## Layer 2: Skills — The Knowledge Layer

**Purpose:** Modular, on-demand expertise packages.

- Structured as `SKILL.md` files inside `.claude/skills/`.
- Only loaded into context when needed (unlike persistent CLAUDE.md).
- Support directory structures with supporting reference files.
- Invokable via slash commands or auto-triggered by Claude.
- Bundled capabilities include `/debug`, `/loop`, `/simplify`.
- Contain procedures, checklists, and domain-specific guidance.
- Reference material stored efficiently — minimal tokens until used.

**When to use:** Repeatedly-used instructions, specialized workflows, or procedures that shouldn't always consume memory space.

---

## Layer 3: Hooks — The Guardrail Layer

**Purpose:** Deterministic, event-driven workflow automation.

- React to lifecycle events and enforce non-negotiable behavior.
- Common event types: `PreToolUse`, `PostToolUse`, lifecycle transitions.
- Can intercept, transform, or block actions deterministically.
- Use cases: auto-formatting code, blocking edits to protected files, auditing config changes.
- Operate at the runtime level — outside model discretion.
- Can preprocess large inputs before Claude sees them, reducing token waste.

**Example:** A `PreToolUse` hook can filter huge test logs down to only failures, cutting thousands of tokens to a manageable size.

---

## Layer 4: Subagents — The Delegation Layer

**Purpose:** Bounded, specialized task execution in isolated contexts.

- Dedicated AI assistants handling specific task types independently.
- Preserve main context by keeping exploration and logs separate.
- Built-in options: **Explore** (read-only), **Plan** (planning), **General-purpose**.
- Inherit parent permissions with additional restrictions.
- **Critical limitation:** Cannot spawn nested subagents (no infinite recursion).
- Configuration scopes:
  - User-level: `~/.claude/agents/`
  - Project-level: `.claude/agents/`
  - Session-only: via CLI
- Each subagent is a Markdown file with YAML frontmatter.

**Design principle:** Delegation is bounded by design — not unrestricted.

---

## Layer 5: Plugins — The Distribution Layer

**Purpose:** Package and share agent behavior across teams and projects.

- Distributes packaged expertise, workflows, and governance across scopes.
- Supports plugin configuration sections, allowlists, and permission controls.
- Can carry approved subagents, scoped tool permissions, and standard hooks.
- Enables organization-wide standardization (macOS, Linux, WSL, Windows).
- Treated as bounded units with their own behavior surface — not passive extensions.

---

## Architectural Principles

When systems break down, each failure maps to a missing layer:

| Failure mode | Missing layer |
|---|---|
| Forgetting conventions | Memory layer (CLAUDE.md) |
| Repeating setup instructions | Modular knowledge layer (Skills) |
| Inconsistent behavior around risky actions | Guardrail layer (Hooks) |
| Overloaded main context | Delegation layer (Subagents) |
| Non-standardized behavior across teams | Distribution layer (Plugins) |

---

## Best Practices Summary

1. **CLAUDE.md** — Durable project rules, coding standards, architectural boundaries that apply universally.
2. **Skills** — Reusable operational procedures, repeated instruction sets, multi-step workflows.
3. **Hooks** — Enforce zero-exception policies, preprocess inputs, audit critical changes.
4. **Subagents** — Isolate focused work, specialize by task type, preserve main context cleanliness.
5. **Plugins** — Distribute and standardize behavior at team or organization level.

---

## Key Commands & CLI Flags

| Command / Flag | Purpose |
|---|---|
| `/doctor` | Diagnoses hook, MCP server, and context usage issues |
| `/agents` | Create or manage subagents interactively |
| `--append-system-prompt` | CLI argument (does **not** propagate into plugins) |
| `--allowedTools` | CLI argument (does **not** propagate into plugins) |

---

## Operational Insight

Route processing to **hooks and skills** because they preprocess data *before* Claude encounters it — reducing token waste while improving reliability. This is not a convenience feature; it's an efficiency mechanism built into the layered design.

The overall model prioritizes **composability**: each layer addresses one category of production failure, so the system is more debuggable and maintainable than monolithic prompt-based agents.
