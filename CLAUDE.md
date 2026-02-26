# ROK Memory - Cross-Platform Memory Bridge

## What This Is

Git-based shared memory bridge that syncs context between Claude Desktop (research/planning) and Claude Code (implementation). Part of the ROK 3.0+ ecosystem.

**No build system** - pure markdown + git. No dependencies beyond git, bash, curl.

## Structure

```
CURRENT_CONTEXT.md          # Single source of truth - read this first
handoffs/                   # Session summaries when switching platforms
decisions/                  # Architectural decisions (planned)
learnings/                  # Reusable patterns (planned)
projects/                   # Per-project context (planned)
threads/                    # Long-running discussions (planned)
```

## Conventions

**Git commits**: `[MEMORY] Platform: Brief description`
- Platform: `desktop` or `code`
- Example: `[MEMORY] code: Implemented OAuth flow, tests passing`

**File naming**: `YYYY-MM-DD-HHmm-platform.md` or `YYYY-MM-DD-platform.md`

**Update frequency**: `CURRENT_CONTEXT.md` updated every session end; handoffs written when switching platforms.

## Workflow

```bash
# Read context
git pull origin main
cat CURRENT_CONTEXT.md

# Write handoff
git add . && git commit -m "[MEMORY] code: Description"
git push origin main
```

## ROK Integration

- `/sync-context` - pulls latest, displays CURRENT_CONTEXT.md
- `/sync-context push` - commits current state
- `/memory-write` + `/memory-query` - separate Supabase backend (not this repo)
- `/session-start` auto-loads both git context + Supabase memory

## Remote

- **Repo**: github.com/klatt42/rok-memory (public)
- **Branch**: main
- **No ports** - not a running application
