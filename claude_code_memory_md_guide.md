# Claude Code — CLAUDE.md & MEMORY.md: Complete Guide

**Source:** [To Data & Beyond — Claude Code MEMORY.md: Everything You Need to Know](https://todatabeyond.substack.com/p/claude-code-memorymd-everything-you)
**Author:** Youssef Hosni (Mar 16, 2026)

> "CLAUDE.md is for your instructions. MEMORY.md is Claude's own scratchpad."

Claude Code ships with two complementary context files that together form its persistent memory system. Understanding the split is the difference between a session that "remembers" your conventions and one that asks the same questions every time.

---

## The Core Distinction

| File | Owner | Purpose | Lifecycle |
|---|---|---|---|
| **CLAUDE.md** | You (manually authored) | Instructions, rules, preferences, and policies Claude must follow | Edited deliberately, committed to source control |
| **MEMORY.md** | Claude (auto-managed) | Accumulated learnings: build commands, debugging fixes, architectural patterns, coding preferences | Updated automatically as Claude works |

CLAUDE.md is **prescriptive** — what you want. MEMORY.md is **observational** — what Claude has learned.

---

## What Auto-Memory Captures

Claude automatically writes to MEMORY.md when it encounters facts worth keeping for future sessions:

- **Build & test commands** — the exact incantations that work in this repo
- **Debugging solutions** — fixes for recurring issues and gotchas
- **Architecture patterns** — module relationships and design decisions inferred from the code
- **User preferences** — coding style, communication tone, review priorities
- **Tooling quirks** — CI behavior, deploy steps, environment setup

The feature activates by default after updating Claude Code — no configuration needed.

---

## Storage & File Layout

Memory files live at:

```
~/.claude/projects/<project-slug>/memory/
├── MEMORY.md            # The index — first 200 lines load at session start
├── user_*.md            # User-profile memories
├── feedback_*.md        # Behavioral guidance from past corrections
├── project_*.md         # Project facts, decisions, deadlines
└── reference_*.md       # Pointers to external systems
```

Projects are identified by **Git repository root** — meaning the directory must be a git repo (`git init` if not) for memory to scope correctly.

---

## The Context Hierarchy

When Claude starts a session, multiple context layers stack in priority order (highest wins):

1. **Organization policies** — enterprise-wide CLAUDE.md (centrally managed paths)
2. **Project CLAUDE.md** — team-shared, committed to the repo
3. **User ~/.claude/CLAUDE.md** — personal preferences across all projects
4. **MEMORY.md (first 200 lines)** — auto-learned context for this project

Project-level instructions override user-level preferences, which override auto-memory. Manual rules always beat learned habits.

---

## The 200-Line Constraint

Only the **first 200 lines (or 25 KB)** of MEMORY.md load at session start. This is deliberate: full memory dumps would eat the context window before any real work begins.

**How Claude manages overflow:**
- MEMORY.md itself stays as a lean **index** — one line per topic
- Detail lives in topic-specific files (`debugging.md`, `api-conventions.md`, `incident-2026-03-12.md`)
- Those files load **on demand** when their topic becomes relevant

Treat MEMORY.md as a table of contents, not a journal.

---

## CLAUDE.md Mechanics

### File Discovery
Claude walks up the directory tree from the working location, loading every `CLAUDE.md` it finds. Multiple files **stack** — they don't override.

```
/repo/CLAUDE.md              # global repo rules
/repo/backend/CLAUDE.md      # backend-specific additions
/repo/backend/api/CLAUDE.md  # api-specific additions
```

When working in `/repo/backend/api/`, all three load together.

### Imports
CLAUDE.md supports nested imports via `@path/to/file` syntax, up to **5 levels deep**:

```markdown
# Coding standards
@docs/style-guide.md

# Review checklist
@.claude/review-checklist.md
```

### Sizing
Shorter CLAUDE.md files lead to better adherence. Long files dilute attention — the model can hold ~5 strong rules far better than 50 weak ones. Use Skills for procedural content; reserve CLAUDE.md for invariants.

---

## What Belongs Where

### Put in CLAUDE.md
- Coding standards and naming conventions
- Architectural boundaries ("never call X from Y")
- Required tools or libraries
- Review checklists and PR rules
- Communication style preferences
- Forbidden patterns

### Let MEMORY.md capture
- Build/test/lint commands for this repo
- Debugging recipes that worked
- Names of long-running processes and how to restart them
- The user's role, expertise level, and tone preferences
- Project deadlines, incident history, ongoing initiatives

### Use Skills (not CLAUDE.md) for
- Multi-step procedures
- Domain-specific workflows
- Anything used occasionally rather than every session

---

## Setup & Activation

```bash
# 1. Update Claude Code (memory requires 2.1.76+)
claude update

# 2. Make sure your project is a git repo
git init

# 3. Launch a session — memory populates automatically as you work
claude
```

No flags, no config files. The system kicks in on first use.

---

## Useful Commands

| Command | Purpose |
|---|---|
| `/init` | Bootstrap a new CLAUDE.md from your existing codebase |
| `/memory` | Inspect, edit, or clear current memory state |
| `/doctor` | Diagnose memory loading and context issues |

---

## Best Practices

1. **Start small** — A 20-line CLAUDE.md with sharp rules beats a 200-line dump of every preference you've ever had.
2. **Commit CLAUDE.md** — It's team context, not personal config. Treat changes as PR-reviewable.
3. **Keep MEMORY.md as an index** — One line per topic with `[[links]]` to detailed files.
4. **Convert relative dates to absolute** when writing memory ("Thursday" → "2026-03-05") so context survives time.
5. **Audit memory periodically** — Stale facts cause worse outputs than missing ones. Delete what's no longer true.
6. **Use the hierarchy** — Org → Project → User → Memory. Push rules to the highest layer they apply to.
7. **Don't duplicate code structure** — File paths and call graphs come from reading the repo, not from memory.

---

## What NOT to Memorize

The article (and Anthropic's guidance) is explicit about non-goals:

- Code structure, file paths, framework conventions → derivable from the repo
- Git history or recent changes → `git log` / `git blame`
- Debugging fixes already encoded in the commit → the diff is the record
- Anything documented in CLAUDE.md already
- Ephemeral task state → use tasks or plans, not memory

These exclusions hold even when the user explicitly asks to "save this." Ask what was *surprising* — that's the part worth keeping.

---

## Why This Architecture Matters

The MEMORY.md / CLAUDE.md split mirrors a deeper principle:

- **Prescriptive context** belongs in version control, reviewed by humans, applies broadly.
- **Observational context** belongs in machine-local storage, updated continuously, scoped to a single user's working environment.

Conflating them produces either a CLAUDE.md that's too noisy to follow or a MEMORY.md that drifts from team reality. Keep them separated and each does its job well.
