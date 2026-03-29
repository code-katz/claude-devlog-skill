# devlog — Development Log

A living record of architectural decisions, milestones, key insights, and strategic direction.
Auto-maintained via Claude devlog skill. Entries are reverse-chronological.

---

## [2026-03-29] Added entry deprecation markers and DEVLOG-ARCHIVE.md archiving

**Category:** `feature`
**Tags:** `deprecation`, `archiving`, `superseded`, `context-management`, `skill-behavior`
**Risk Level:** `med`
**Breaking Change:** `yes`

### Summary
Added SUPERSEDED/PARTIALLY SUPERSEDED markers for stale devlog entries, automatic deprecation review during entry creation, threshold-based archiving to DEVLOG-ARCHIVE.md, and updated reading instructions that skip superseded entries at session start.

### Detail
- New "Entry Deprecation" section: defines `> **SUPERSEDED [date]:**` blockquote marker format, placement rules, partial supersession, user-approval requirement
- Updated workflow from 4 steps to 6: added Step 2 (Review for Superseded Entries) and Step 5 (Archive if Needed)
- Step 2 is a lightweight scan: headings + Summary + Decisions Made sections only, not full Detail bodies
- New "Archiving" section: triggers when DEVLOG.md exceeds ~50 entries or ~1,500 lines AND superseded entries exist. Only SUPERSEDED entries are archived. PARTIALLY SUPERSEDED entries stay. No tombstones.
- Archive file is `DEVLOG-ARCHIVE.md` (neutral name, alphabetically adjacent)
- Updated "Reading the Devlog": skip superseded entry bodies at session start, do NOT load DEVLOG-ARCHIVE.md proactively
- New "Historical research" subsection: read archive when user asks about decision evolution
- SKILL.md grew from 259 to 368 lines (+109)

### Decisions Made
- **Inline markers + archiving (both):** Inline markers alone solve decision clarity but not context window pressure. Archiving alone loses the signal for entries not yet archived. Both are needed.
- **DEVLOG-ARCHIVE.md over DEPRECATED-DEVLOG.md:** "Archive" is neutral; "deprecated" has negative connotations. The file contains valid historical context, just superseded.
- **No tombstones:** Removing archived entries entirely keeps DEVLOG.md lean. The superseding entry already exists in the active file.
- **Threshold-based archiving (50 entries or 1,500 lines):** Based on current data (d20Mob DEVLOG hit 702 lines in one week). Conservative enough to avoid premature archiving on small projects.
- **Only SUPERSEDED entries get archived, never by age alone:** A 3-month-old architectural decision still in effect is more valuable than a 1-week-old reversed decision.

### Related
- Plan: `code-katz/claude-devlog-skill/plans/2026-03-28-devlog-deprecation-archiving.md`
- Companion change: claude-plans-skill status markers (same session)

---

## [2026-03-26] Added lint check requirement to SKILL.md

**Category:** `feature`
**Tags:** `lint`, `code-quality`, `skill-behavior`, `cross-tool`
**Risk Level:** `low`
**Breaking Change:** `no`

### Summary
Added a "Lint Check" subsection to the Project Context section, requiring the skill to verify the target project has a linter configured on first use per session.

### Detail
- New subsection placed after "Project Context (First Use Per Session)", before "Entry Format"
- Checks for stack-appropriate lint config: Ruff (Python), ESLint/Biome (JS/TS), SwiftLint (Swift), golangci-lint (Go), clippy (Rust), pre-commit (general)
- Flags missing linter config to the user and recommends a tool before proceeding
- Mirrors the same requirement added to `claude-team-cli` coordinator profile, adapted for skill context
- Part of a cross-tool effort to standardize lint checks across all code-katz tools

### Decisions Made
- **Placed inside Project Context rather than as a standalone section** — the lint check triggers at the same time as project context setup, so grouping them keeps the workflow linear
- **Identical linter list across all code-katz tools** — consistency matters more than per-tool customization; all tools operate in the same project environments

---

## [2026-03-10] Added attribution link to generated DEVLOG.md headers

**Category:** `feature`
**Tags:** `branding`, `attribution`, `skill`, `template`
**Risk Level:** `low`
**Breaking Change:** `no`

### Summary
Updated the DEVLOG.md header template in SKILL.md to include a hyperlink back to the `claude-devlog-skill` GitHub repo, providing attribution and discoverability on every project that uses the skill.

### Detail
- Header template now includes: `Auto-maintained via [claude-devlog-skill](https://github.com/code-katz/claude-devlog-skill)`
- Applies to all new DEVLOG.md files created going forward — existing files are unchanged
- Installed copy at `~/.claude/skills/devlog/SKILL.md` synced with repo

### Decisions Made
- Used a Markdown hyperlink rather than a plain URL so it renders cleanly on GitHub and GitLab
- Chose not to retrofit existing DEVLOG.md files — the change is low-stakes and not worth touching projects retroactively

---

## [2026-03-10] Renamed project to claude-devlog-skill

**Category:** `strategy`
**Tags:** `branding`, `rename`, `github`, `gitlab`
**Risk Level:** `low`
**Breaking Change:** `no`

### Summary
Renamed the project from `devlog-skill` to `claude-devlog-skill` across GitHub, GitLab, README, and local git remotes to align with the `claude-` namespace convention used by sibling project `claude-dev-team`.

### Detail
- GitHub repo renamed: `code-katz/devlog-skill` → `code-katz/claude-devlog-skill`
- GitLab repo renamed: `wcurran/devlog-skill` → `wcurran/claude-devlog-skill`
- Local git remotes updated to new URLs
- README install URL updated to reference new GitHub raw path
- GitHub preserves old URL as a redirect — existing installs are unaffected

### Decisions Made
- Chose `claude-devlog-skill` over `devlog-skill` to match the `claude-` prefix convention and signal that this is part of a Claude Code tooling ecosystem alongside `claude-dev-team`
- No breaking change to existing installs because GitHub redirects the old repo URL transparently

---

## [2026-03-10] README marketing overhaul

**Category:** `milestone`
**Tags:** `readme`, `positioning`, `documentation`, `branding`
**Risk Level:** `low`
**Breaking Change:** `no`

### Summary
Full README rewrite replacing flat feature documentation with conversion-focused content: a problem statement, ICP section, before/after exchange, and badges.

### Detail
- **"The Problem"** section added — leads with the context-loss pain before explaining the solution. Previous README assumed the reader already understood why this mattered.
- **"Who This Is For"** added — explicitly names solo developers and small teams on long-running projects. Also calls out `claude-dev-team` users as a natural second audience.
- **"See the Difference"** before/after exchange added — contrasts generic Claude advice with a devlog-aware response that references prior architectural decisions, roadmap direction, and known bugs. Auth/HIPAA scenario used as the example.
- **Badges** added: MIT license, Works with Claude Code, type: skill.
- **Tagline** replaced the wordy description with: *"Stop rebuilding context. Start every Claude session where the last one left off."*
- Existing sections (categories, format, installation, usage) preserved and reordered so the hook comes before the technical detail.

### Decisions Made
- Led with the problem (context loss) rather than the solution (a DEVLOG.md file) — the feature is only compelling once the reader has felt the pain
- Before/after scenario used auth + HIPAA compliance context to demonstrate architectural memory, roadmap awareness, and known-bug carryover in a single exchange — chosen because it's concrete and recognizable to the target audience
- "Who This Is For" kept short (two paragraphs) — naming the audience without over-segmenting

---

## [2026-03-10] Devlog skill schema v2 — richer entry format

**Category:** `feature`
**Tags:** `devlog`, `schema`, `skill`, `adr`
**Risk Level:** `low`
**Breaking Change:** `yes`

### Summary
Updated SKILL.md with a richer entry schema: two new metadata fields (`Risk Level`, `Breaking Change`), a dedicated `Decisions Made` section, and a revised category taxonomy.

### Detail
- Added `Risk Level: low | med | high` metadata to every entry — describes blast radius and reversibility
- Added `Breaking Change: yes | no` metadata — flags when public contracts, APIs, or CLI behavior changes
- Added `### Decisions Made` section to the entry template — a lightweight inline ADR for trade-off decisions
- Replaced old categories (`architecture`, `milestone`, `takeaway`, `strategy`) with: `feature`, `bugfix`, `refactor`, `infrastructure`, `security`, `milestone`, `strategy`
- Added new `## Fields Explained` section to document the new fields with definitions and value options
- Updated Style Guidelines with two new rules: flag `high` risk on auth/secrets/data model changes; always populate `Decisions Made` when alternatives were considered
- `strategy` category retained — preserved for purely non-technical/business entries

### Decisions Made
- **Kept `strategy` category**: Initial proposal dropped it in favor of a code-centric taxonomy. Restored after recognizing business/non-technical decisions need a home in the log.
- **Removed `takeaway` and `architecture`**: Both were vague and overlapped with other categories. `architecture` maps cleanly to `refactor`/`infrastructure`; `takeaway` knowledge now belongs in `Decisions Made` of the relevant entry.
- **`Decisions Made` as a section, not inline in `Detail`**: Separating ADR content from general context makes it easier to scan entries for trade-off rationale specifically.

### Related
- Breaking change: existing DEVLOG.md entries using `architecture`, `takeaway` categories are not retroactively updated — new entries use the new taxonomy
