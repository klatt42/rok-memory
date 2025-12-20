# ROK Memory Bridge

**Cross-platform shared memory for Claude Desktop and Claude Code sessions.**

This repository serves as a persistent, version-controlled memory layer that bridges context between AI assistant platforms.

## Purpose

Eliminates the need for humans to manually relay context between:
- **Claude Desktop** (research, planning, high-level decisions)
- **Claude Code** (implementation, file editing, terminal operations)

## Directory Structure

```
rok-memory/
├── README.md                    # This file
├── CURRENT_CONTEXT.md           # Active working context (always read first)
├── handoffs/                    # Session handoff notes
│   └── YYYY-MM-DD-platform.md   # Dated handoff files
├── decisions/                   # Architectural decisions across projects
│   └── project-name/            # Per-project decisions
├── learnings/                   # Captured insights and patterns
│   └── topic.md                 # Topic-specific learnings
├── projects/                    # Project-specific memory
│   └── project-name/
│       ├── context.md           # Current project context
│       ├── blockers.md          # Active blockers
│       └── next-steps.md        # Prioritized next actions
└── threads/                     # Conversation continuity
    └── thread-id.md             # Long-running discussion threads
```

## How It Works

### Writing Context (End of Session)

```bash
# Claude Desktop or Claude Code
git pull origin main
# Update relevant files
git add .
git commit -m "[MEMORY] Platform: Summary of context"
git push origin main
```

### Reading Context (Start of Session)

```bash
# First thing in new session
git pull origin main
cat CURRENT_CONTEXT.md
```

## Key Files

| File | Purpose | When to Update |
|------|---------|----------------|
| `CURRENT_CONTEXT.md` | What to know RIGHT NOW | Every session end |
| `handoffs/*.md` | Detailed session summaries | When switching platforms |
| `decisions/**/*.md` | Why we chose X over Y | When making architectural choices |
| `learnings/*.md` | Patterns and insights | When discovering reusable knowledge |
| `projects/**/context.md` | Project-specific state | When project context changes |

## Sync Commands

### From Claude Code
```
/sync-context              # Pull and display CURRENT_CONTEXT.md
/sync-context push         # Commit and push current state
/sync-context project:X    # Load project-specific context
```

### From Claude Desktop

**Setup (per-session, container resets between conversations):**
```bash
# Clone with token authentication for push access
git clone https://<GH_TOKEN>@github.com/klatt42/rok-memory.git /home/claude/rok-memory
cd /home/claude/rok-memory
git config user.email "claude-desktop@anthropic.com"
git config user.name "Claude Desktop"
```

**After research/work - push handoff:**
```bash
cd /home/claude/rok-memory
git pull origin main

# Write handoff file (include time for multiple same-day handoffs)
cat > handoffs/$(date +%Y-%m-%d-%H%M)-desktop.md << 'EOF'
# Handoff from Claude Desktop
**Date**: $(date +%Y-%m-%d)
**Topic**: [What was researched/decided]

## Summary
[Key findings]

## For Claude Code
[Specific next steps for implementation]

## Files/URLs Referenced
- [List resources]
EOF

# Commit and push
git add .
git commit -m "[MEMORY] desktop: <brief description>"
git push origin main
```

**Tell user:** "Handoff pushed to rok-memory. Tell CC to sync."

### From Claude Code

**Pick up Desktop handoff:**
```bash
cd ~/projects/rok-memory && git pull
cat handoffs/$(date +%Y-%m-%d)-desktop.md
```

**Or use the slash command:**
```
/sync-context
```

## Commit Convention

```
[MEMORY] Platform: Brief description

Platform: desktop | code
Examples:
- [MEMORY] desktop: Research complete for auth implementation
- [MEMORY] code: Implemented OAuth flow, tests passing
- [MEMORY] code: Blocked on API rate limiting
```

## Best Practices

1. **Always pull before reading** - Get latest context
2. **Always push after writing** - Share context immediately
3. **Keep CURRENT_CONTEXT.md focused** - Only active, relevant info
4. **Archive to handoffs/** - Move completed work out of current context
5. **Be specific in commits** - Future sessions depend on clear messages

## Integration with ROK System

This repository is part of the ROK 3.0 ecosystem:

```
~/.claude/
├── CLAUDE.md              # Global ROK configuration
├── commands/
│   └── sync-context.md    # Sync command definition
└── agents/
    └── *.md               # Specialized agents

~/projects/rok-memory/     # THIS REPOSITORY
├── CURRENT_CONTEXT.md     # Shared working memory
└── ...

~/projects/*/              # Individual projects
├── claude-progress.txt    # Project-specific progress
└── feature_list.json      # Project-specific features
```

## Setup

```bash
# Clone the repository
git clone https://github.com/klatt42/rok-memory.git ~/projects/rok-memory

# Verify access
cd ~/projects/rok-memory
git pull origin main
```

---

**ROK Memory Bridge v1.0** | Cross-Platform Context Continuity
