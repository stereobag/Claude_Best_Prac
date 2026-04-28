# Claude Code Command Reference

Synthesized from the Academind Claude Code Cheat Sheet and the DailyDoseofDS 150+ Claude Code Command Cheat Sheet.

---

## CLI Launch Flags

| Flag / Command | What it does |
|---|---|
| `claude` / `claude "prompt"` | Start interactive session, optionally with an initial prompt |
| `claude -p "prompt"` | One-shot query — print result and quit, no ongoing session |
| `claude -c` | Continue most recent session |
| `claude --agent <name>` | Start session with a custom agent |
| `claude --allowedTools "Read" "Write"` | Skip permission confirmation for specified tools |
| `claude --disallowedTools "Write"` | Disallow certain tools from context |
| `claude --dangerously-skip-permissions` | Skip ALL permission confirmations — use with caution |
| `claude --append-system-prompt "..."` | Append instruction to default system prompt |
| `claude --system-prompt "..."` | Replace entire default system prompt |
| `claude --model opus` | Set default model for current session |
| `claude --permission-mode plan` | Start in plan mode |
| `claude --remote "message"` | Start a remote (web) session |
| `claude --agents` | Define sub-agents via JSON at startup |
| `claude --worktree` | Run session in an isolated git worktree |
| `claude --debug` | Enable verbose debug/context logging |
| `resume <session>` | Resume a specific session by name or ID |

---

## Slash Commands

### Session & Context
| Command | What it does |
|---|---|
| `/help` | List available commands and get usage help |
| `/model` | Choose active model for current session |
| `/clear` | Clear session context window |
| `/compact` | Compact and clear current session context history |
| `/context` | View statistics about current context window & usage |
| `/usage` | View current usage for active plan |
| `/resume` | Resume session linked to a specific pull request |
| `/fork` | Create a separate conversation branch for risk-free exploration |
| `/alias` | Auto-generate a session name from your initial message |
| `/title` | Set a prompt bar title to visually identify the session |
| `/status` | Show token consumption and dollar spend for current session |
| `/export` | Export the conversation as searchable HTML + JSON |
| `/report` | Generate an HTML report: sessions, tokens, cost, top commands |
| `/version` | Show detailed Claude version info |

### Project Setup & Memory
| Command | What it does |
|---|---|
| `/init` | Analyze project and create initial CLAUDE.md file |
| `/add` | Open CLAUDE.md for in-session editing |
| `#add-dir <path>` | Add additional working directories to current conversation |
| `#` (inline) | Append a one-liner to CLAUDE.md without opening the editor |
| `#remember` | Create a note to CLAUDE.md |

### Code Review & Quality
| Command | What it does |
|---|---|
| `/diff` | Show git diff of all changes Claude made in current session |
| `/review` | Run three parallel agents for quality, security, and test coverage |
| `/inspect` | Review a specific pull request (requires PR URL) |
| `/security` | Run a security-focused audit on specified files or folder |
| `/tests` | Run current branch's commits against the test suite |
| `/workflow` | Bundle an LLM for systematic debugging workflows |

### Configuration & Tooling
| Command | What it does |
|---|---|
| `/config` | Open interactive settings menu |
| `/mcp` | View and manage installed MCP servers |
| `/permissions` | View and update/change permissions |
| `/rewind` | Undo to an earlier point — same as ESC + ESC |
| `/statusline` | Configure the Claude Code status line |
| `/teleport` | Resume a remote Claude Code session |
| `/ide` | Manage connections to VS Code and JetBrains |
| `#plugin` | Install, remove, and manage third-party plugins |
| `/diagnose` | Diagnose installation issues |
| `/bug` | Send a bug report directly to Anthropic |
| `/upgrade` | Upgrade Claude packages and plugins immediately |

### Multi-Agent & Automation
| Command | What it does |
|---|---|
| `/agents` | Create and manage specialized agents (builders, researchers, reviewers) |
| `/loop` | Run repetitive changes across multiple files automatically |
| `/schedule` | Schedule Claude to run a task on a time interval |
| `/background` | List and manage background shell processes |

---

## Inline Prefixes (Mid-Conversation)

| Prefix | What it does |
|---|---|
| `#why` | Ask a question without interrupting the current task flow |
| `#autorun` / `#auto` | Automate injecting file content into your prompt |
| `#bash` | Run a bash command directly; output feeds into Claude |
| `#think` | Toggle speed-optimized (extended thinking) API settings |
| `#propose` | Propose code changes as a plan without making them |
| `#output-style` | Switch Claude's response style (Concise, Detailed, Technical, Creative) |
| `#model` | Switch between low/medium/high reasoning tier; "Auto" resets |

---

## Keyboard Shortcuts

| Shortcut | What it does |
|---|---|
| `SHIFT + ENTER` / `OPTION + ENTER` / `CTRL + J` | Enter new line without submitting |
| `SHIFT + TAB` | Switch between modes / Accept All in plan mode |
| `CTRL + C` | Cancel current input or generation |
| `ESC + ESC` | Restore code prior to last action (undo) |
| `OPTION + P` / `ALT + P` | Switch model |
| `↑ / ↓ Arrow keys` | Cycle through options, questions, or past messages |
| `CTRL + O` | Toggle verbose output / open prompt in system editor |
| `CTRL + B` | Move task to background |
| `CTRL + V` / `CMD + V` / `ALT + V` | Insert text or image |
| `CTRL + SHIFT + /` | Search through command history |
| `CTRL + Z` | Send current job to background |
| `ALT + .` | Show/hide Claude's thinking mode |

---

## Settings (settings.json)

```json
{
  "permissions": { ... },
  "model": "opus",
  "alwaysThinkingEnabled": true,
  "hooks": { },
  "env": { "IS_DEMO": 1 }
}
```

| Key | Purpose |
|---|---|
| `permissions` | Manage tool permissions for all sessions |
| `model` | Set default AI model for new sessions |
| `alwaysThinkingEnabled` | Enable/disable extended ("advanced") thinking mode |
| `hooks` | Automate shell commands on events (pre/post tool calls, session start/stop) |
| `env` | Environment variables applied to every session |
