# Memory

OpenClaw remembers things by writing plain Markdown files in the agent workspace. The model only "remembers" what gets saved to disk — there is no hidden state.

## Memory Files

All files live in `~/.openclaw/workspace/` (or your configured workspace):

| File | Purpose | Loaded when |
|------|---------|------------|
| `MEMORY.md` | Long-term durable facts, preferences, decisions | Every DM session start |
| `memory/YYYY-MM-DD.md` | Daily notes, running context | Today + yesterday auto-loaded |
| `DREAMS.md` | Dream diary summaries (optional) | On request |

## Usage

Just ask the agent naturally:
- "Remember that I prefer TypeScript"
- "Note that the deploy target is production"
- "Forget that I said X"

The agent uses `memory_search` and `memory_get` tools to recall relevant notes.

## Memory Tools

- `memory_search` — semantic search across notes (works even when wording differs)
- `memory_get` — read a specific memory file or line range

Both are provided by the active memory plugin (default: `memory-core`).

## Memory Wiki (optional)

The `memory-wiki` plugin adds a provenance-rich knowledge layer:
- Deterministic page structure with structured claims and evidence
- Contradiction and freshness tracking
- Generated dashboards and compiled digests
- Tools: `wiki_search`, `wiki_get`, `wiki_apply`, `wiki_lint`

Enable via plugins config. Does not replace the active memory plugin — they work alongside each other.

## Dreaming

OpenClaw can run "dreaming" sweeps — background agent runs that review and consolidate memory. See `DREAMS.md` for output. Enable via config.

Docs: https://docs.openclaw.ai/concepts/memory
Memory wiki: https://docs.openclaw.ai/concepts/memory-builtin
Dreaming: https://docs.openclaw.ai/concepts/dreaming
