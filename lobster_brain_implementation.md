# Lobster Brain Implementation Log

## What was built

### Claude Code Skill
- **Location:** `~/.claude/skills/lobster-brain/SKILL.md`
- **Trigger:** `/lobsterbrain`, sharing screenshots/links, or mentioning "lobster brain"
- **Type:** Global Claude Code skill (available from any project)

### Obsidian Vault Structure
- **Vault path:** `/Users/michieljanssen/Library/Mobile Documents/iCloud~md~obsidian/Documents/Michiel/`
- **Synced via:** iCloud (accessible on Mac and iPhone via Obsidian mobile)

#### Files created in vault

| File | Purpose |
|------|---------|
| `Lobster Brain.md` | Overview page with category/subtopic routing table |
| `Lobster Brain/AI/Claude Tips and Tricks.md` | Subtopic page |
| `Lobster Brain/AI/Claude Code.md` | Subtopic page |
| `Lobster Brain/AI/Claude MD Files.md` | Subtopic page |
| `Lobster Brain/AI/OpenClaw Tips.md` | Subtopic page |
| `Lobster Brain/AI/Prompting Resources.md` | Subtopic page |
| `Lobster Brain/Health/Peptides.md` | Subtopic page |
| `Lobster Brain/Health/Supplements.md` | Subtopic page |
| `Lobster Brain/Tech/Mac.md` | Subtopic page |
| `Lobster Brain/Tech/Networks.md` | Subtopic page |
| `Lobster Brain/Business/Start-up Stories.md` | Subtopic page |
| `Lobster Brain/Business/Business Ideas.md` | Subtopic page |
| `Lobster Brain/Business/Business Inspiration.md` | Subtopic page |
| `Lobster Brain/Fitness/Exercise Ideas.md` | Subtopic page |

## Skill behavior summary

### Supported inputs
- Screenshots (OCR + platform detection + metadata extraction)
- Links (fetch, summarize, preserve full text)
- Podcasts (playback timestamp + ingestion timestamp + source metadata)
- Long-form text (summarize + preserve)

### Workflow
1. Receive input from user
2. Read `Lobster Brain.md` to get current taxonomy
3. Extract metadata from input
4. Classify and match to existing category/subtopic
5. Show compact confirmation draft to user
6. Write entry to subtopic page only after confirmation

### Key rules
- Never creates new categories or subtopics without user approval
- Never fabricates metadata — uses `unknown` for unclear fields
- Always reads `Lobster Brain.md` before routing (no hardcoded categories)
- Entries appended oldest-first with collapsible full text sections
- Supports reminders with `reminder_date` and `completed` fields

## Design decisions

| Decision | Choice | Reason |
|----------|--------|--------|
| Platform | Claude Code skill (not OpenClaw) | User's current setup |
| Mobile access | Desktop only for now | Claude mobile app lacks file system access |
| Vault sync | iCloud | Already configured, syncs Mac + iPhone |
| Initial categories | Pre-seeded (5 categories, 13 subtopics) | User-specified starter taxonomy |
| Entry format | Compact metadata + collapsible full text | Prevents note bloat while preserving source |
| Taxonomy source of truth | `Lobster Brain.md` | Dynamic routing, no hardcoded categories |

## Future considerations
- Mobile capture workflow (via Dispatch or inbox file)
- Standalone web app (product logic is implementation-agnostic)
- Notion support (explicitly excluded from v1)
