# Devlog Skill for Claude Code

A custom Claude Code skill that maintains a structured development changelog (`DEVLOG.md`) in your Git repository. It captures architectural decisions, progress milestones, key conversation takeaways, and strategic decisions — so critical context from your development conversations doesn't evaporate between sessions.

## What It Does

- **Proactively suggests** devlog entries when significant decisions, milestones, or insights come up in conversation
- **Accepts ad-hoc entries** when you say "log this" or "add to devlog"
- **Writes detailed entries** with category tags, summaries, full context, and rationale
- **Auto-pushes to your Git remote** after you approve each entry
- **Works with any project** — not tied to a specific repo or product
- **Supports GitHub and GitLab** — uses your system's existing git credentials

## Entry Categories

| Category | What It Captures |
|---|---|
| `architecture` | Technical design decisions, data model changes, stack choices |
| `milestone` | Completed work, version releases, phase transitions |
| `takeaway` | Key insights, lessons learned, important context |
| `strategy` | Business decisions, market positioning, go-to-market pivots |

## Installation

This skill is for [Claude Code](https://claude.ai/code) (the CLI). To install as a personal skill available across all your projects:

```bash
mkdir -p ~/.claude/skills/devlog
curl -o ~/.claude/skills/devlog/SKILL.md \
  https://gitlab-master.nvidia.com/wcurran/devlog/-/raw/main/SKILL.md
```

## Usage

Once installed, the skill activates automatically in Claude Code. You can:

- **Let Claude suggest entries** — it will offer to log significant decisions at natural pause points
- **Request entries directly** — say "log this", "devlog", or "add this to the devlog"
- **Invoke directly** — type `/devlog` in Claude Code
- **Review before pushing** — Claude always shows you the draft entry before committing

Authentication is handled via your system's git credential helper (e.g., macOS Keychain) — no token prompts needed per session.

## DEVLOG.md Format

Entries are reverse-chronological (newest first) and follow this structure:

```markdown
## [2025-03-04] Brief descriptive title

**Category:** `architecture`
**Tags:** `relevant`, `topic`, `tags`

### Summary
A 1-2 sentence overview of what happened or was decided.

### Detail
Full context including what was decided, why, what alternatives
were considered, and implications for future work.

### Related
- Links to related entries or documents
```

## Repository Contents

| File | Purpose |
|---|---|
| `SKILL.md` | The skill source file — Claude's instructions for how the devlog works |
| `README.md` | This file — repo documentation |

## License

See [LICENSE](LICENSE) for details.
