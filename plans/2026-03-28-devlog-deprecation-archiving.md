> Source: `~/.claude/plans/mighty-greeting-kay.md`
> Archived: 2026-03-28 · Project: claude-devlog-skill
> Maintained by [claude-plans-skill](https://github.com/code-katz/claude-plans-skill).

---

# Plan: Add Deprecation & Archiving to claude-devlog-skill

## Context

As DEVLOGs grow, two problems compound:

1. **Decision clarity**: Earlier entries may contain decisions that were later reversed or implemented differently. Claude reads the full DEVLOG at session start and may act on stale decisions with no signal that they're outdated.
2. **Context window pressure**: The d20Mob DEVLOG is already 702 lines after one week. At this rate, 3,000+ lines within months. Stale entries consume context budget without providing current value.

The current skill has no mechanism to mark entries as superseded or to reduce file size over time. Entries are write-once, append-only.

**Target repo**: `/Users/willcurran/d20m-development/code-katz/claude-devlog-skill/`
**File to modify**: `SKILL.md` (259 lines, the entire skill definition)

## Design Decisions

- **Inline markers + archiving** (both required, per user confirmation)
- **Deprecation review happens automatically** during entry creation (not a separate manual step)
- **Only deprecated entries get archived** (age alone does not make an entry stale)
- **Archive file is `DEVLOG-ARCHIVE.md`** (neutral name, alphabetically adjacent to DEVLOG.md)
- **No tombstones** in DEVLOG.md after archiving (the superseding entry already provides current context)

## Changes to SKILL.md

### 1. Entry Format (line ~46): Minor edit

Add a comment showing where the optional deprecation marker goes:

```markdown
## [YYYY-MM-DD] Brief descriptive title
<!-- If superseded, a SUPERSEDED marker goes here (see Entry Deprecation) -->

**Category:** ...
```

### 2. New Section: "Entry Deprecation" (insert after "Fields Explained", before "When to Create Entries" ~line 105)

**Marker format:**
```markdown
## [2026-03-22] Original decision title
> **SUPERSEDED [2026-03-26]:** Replaced by [2026-03-26] d20Mob ruleset adopted. Reason: custom ruleset replaces SRD 5e as mechanical foundation.
```

**Rules to define:**
- Blockquote with bold `SUPERSEDED [date]:` placed immediately after the heading, before `**Category:**`
- Include: replacement entry date + title, one-phrase reason
- One marker per entry max
- For partial supersession: `> **PARTIALLY SUPERSEDED [date]:** [which decisions are stale]. [which still hold].`
- Entry body below the marker is untouched (history is preserved)

### 3. Updated Workflow (modify existing Steps 1-4, lines 133-179)

Renumber to 6 steps:

1. **Draft the Entry** (unchanged)
2. **Review for Superseded Entries** (NEW)
   - Scan existing entries: headings + Summary + Decisions Made sections only (not full Detail)
   - For each conflict, draft a SUPERSEDED marker
   - Lightweight scan: ~30-40 lines for a 15-entry DEVLOG, not 700
3. **Show the User** (renamed, now includes proposed deprecations alongside new entry)
4. **Update DEVLOG.md** (renamed, now also inserts deprecation markers on stale entries)
5. **Archive if Needed** (NEW, see below)
6. **Commit and Push** (renamed, commit message notes deprecations/archiving if applicable)

### 4. New Section: "Archiving" (insert after Workflow)

**Trigger**: Both conditions must be true:
- DEVLOG.md exceeds ~50 entries or ~1,500 lines
- Deprecated (SUPERSEDED) entries exist to archive

**What gets archived**: Only entries with a SUPERSEDED marker. Current entries are never archived regardless of age.

**Archive file format** (`DEVLOG-ARCHIVE.md`):
```markdown
# [Project Name] -- Development Log Archive

Superseded entries moved from DEVLOG.md. Each entry's SUPERSEDED marker points to its replacement.
Auto-maintained via claude-devlog-skill. Entries are reverse-chronological.

---

## [2026-03-22] Original decision title
> **SUPERSEDED [2026-03-26]:** Replaced by ...

[full original entry preserved]

---
```

**No tombstones**: When an entry moves to the archive, it is removed from DEVLOG.md entirely. The superseding entry already exists in DEVLOG.md.

**Commit message**: `devlog: archive N superseded entries`

### 5. Updated "Reading the Devlog" (modify lines 219-239)

**At session start:**
- Read DEVLOG.md in full (all entries are current or explicitly marked SUPERSEDED)
- When encountering a SUPERSEDED marker, note the supersession and skip the entry body. Do not act on decisions from superseded entries. Follow the pointer to the replacement.
- Do NOT read DEVLOG-ARCHIVE.md at session start (historical only)

**New subsection: "Historical research"**
- When user asks about decision history or "why did we change X": read DEVLOG-ARCHIVE.md
- Present the evolution: original decision -> supersession -> current decision

## Execution

Single file edit to `SKILL.md`:
1. Add deprecation marker comment to Entry Format template
2. Add "Entry Deprecation" section after "Fields Explained"
3. Modify Workflow section (renumber steps, add Steps 2 and 5)
4. Add "Archiving" section after Workflow
5. Modify "Reading the Devlog" section

## Verification

1. Read the updated SKILL.md end-to-end to confirm flow and internal consistency
2. Manually test by running `/devlog` on a project with an existing DEVLOG.md and confirming the deprecation review step fires
3. Verify that a new Claude session on a project with SUPERSEDED entries correctly skips stale decisions
