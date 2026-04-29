# Stop Hitting Claude Code Limits

*Source: Product Compass — https://www.productcompass.pm/p/stop-hitting-claude-code-limits*

A user cut Claude Code costs from ~€1,185/month to €180/month by fixing four controllable root causes. Here's the playbook.

---

## 1. Cache Misses

Prompt cache pricing:

| Cache type | Price multiplier |
|---|---|
| Cache read | 0.1× input price |
| Cache write (5-min TTL) | 1.25× |
| Cache write (1-hour TTL) | 2× |
| Cache refresh on hit | Free |

**Rules:**
- Lock your tools at session start — adding or removing an MCP server mid-session invalidates the cached prefix and forces full re-reads
- Never switch models mid-session (`/model`) — this also clears the cache
- Target ~90% cache hit rate on the default 5-min TTL; ~97–99% on 1-hour TTL (API only)
- Monitor cache hit rates via `platform.claude.com/usage/cache`

---

## 2. Context Bloat

**Disable the 1M default context window; use 200K instead:**

```json
"env": {
  "CLAUDE_CODE_DISABLE_1M_CONTEXT": "1",
  "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "80"
}
```

**Five session moves:**

| Command | When to use |
|---|---|
| `/compact` | At 50% context or after completing a task — don't wait for auto-trigger |
| `/clear` | Between unrelated pieces of work |
| `/rewind` | When a turn goes sideways |
| Subagents | Mechanical, research, or parallelizable work |
| Load lean | Disable unused MCP servers and tools before starting |

**Write tighter prompts:**
- Spec-style prompts with explicit file paths and constraints
- Tag known files directly instead of asking Claude to search for them

**Token compression tools:**
- [`rtk-ai/rtk`](https://github.com/rtk-ai/rtk) — 60–90% whitespace compression, ~20–30% token reduction
- [`juliusbrussee/caveman`](https://github.com/juliusbrussee/caveman) — strips conversational filler from responses

---

## 3. Wrong Model or Effort Level

**Set effort explicitly per task:**

```
/effort low       # Quick fixes, mechanical tasks
/effort medium    # Most prompts (big savings vs default)
/effort high      # Demanding reasoning
/effort xhigh     # Default for agentic coding (Opus 4.7)
/effort max       # Rarely worth ~2× the xhigh cost
```

**Model routing strategy:**
- Sonnet sessions: cheaper, covers most in-scope work
- Opus sessions + delegation: pay for Opus on planning and tradeoffs, delegate everything else to cheaper models
- Alternative: use OpenRouter for Pro/Max/Team limit relief — GLM-5.1 ≈ Opus at ~1/12× the cost

**Subagent routing by complexity:**
- Haiku: bulk mechanical work, no judgment needed
- Sonnet: scoped research, code exploration, in-scope synthesis
- Opus: tasks requiring real planning or tradeoffs

---

## 4. Wrong Input Format

**Screenshots / web content:**
Use [`vercel-labs/agent-browser`](https://github.com/vercel-labs/agent-browser) (browses via accessibility tree) instead of screenshots — reduces tokens by ~90% versus screenshot-based approaches.

**PDFs:**
Use `pdftotext` CLI instead of Claude's Read tool, which loads PDFs as images. Reserve Read for charts or images embedded in documents.

**Large repositories:**
[`tirth8205/code-review-graph`](https://github.com/tirth8205/code-review-graph) builds a codebase knowledge graph — reduces tokens by 6.8× on reviews, up to 49× on daily coding tasks.

---

## 5. Monitoring Your Usage

| Tool | Purpose |
|---|---|
| [`phuryn/claude-usage`](https://github.com/phuryn/claude-usage) | Historical breakdown by session, day, week, all-time (Pro/Max/Team) |
| [`Gronsten/claude-usage-monitor`](https://github.com/Gronsten/claude-usage-monitor) | Real-time: current 5-hour window and active session tokens with color thresholds |
| `platform.claude.com/usage/cache` | Cache hit rate dashboard (API users) |
