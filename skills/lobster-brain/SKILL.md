---
name: lobsterbrain
description: >
  Capture screenshots, links, podcasts, and long-form content into the Lobster Brain
  knowledge system in Obsidian. Use this skill whenever the user wants to save, capture,
  or store knowledge — including when they share a screenshot, paste a link, mention a
  podcast moment, or provide reference text they want to keep. Also triggers on
  /lobsterbrain or any mention of "lobster brain".
---

# Lobster Brain

## What this skill does

Lobster Brain captures incoming knowledge and stores it in Obsidian using a structured,
evolving taxonomy. It extracts metadata, summarizes content, matches it to the existing
taxonomy, confirms with the user, and saves it to the correct subtopic page.

## Obsidian vault location

The vault root is:

```
/Users/michieljanssen/Library/Mobile Documents/iCloud~md~obsidian/Documents/Michiel/
```

All paths below are relative to this vault root.

## First-run initialization

Before doing anything, check whether `Lobster Brain.md` exists in the vault root.

If it does not exist, create it along with the full starter structure. See the
"Initial structure" section below for the exact content and folders to create.

If it already exists, read it and use it as the source of truth — never rely on
hardcoded categories.

## Initial structure

On first run, create `Lobster Brain.md` with this content:

```markdown
# Lobster Brain

| Category | Subtopics |
|----------|-----------|
| AI | [[Lobster Brain/AI/Claude Tips and Tricks\|Claude Tips and Tricks]], [[Lobster Brain/AI/Claude Code\|Claude Code]], [[Lobster Brain/AI/Claude MD Files\|Claude MD Files]], [[Lobster Brain/AI/OpenClaw Tips\|OpenClaw Tips]], [[Lobster Brain/AI/Prompting Resources\|Prompting Resources]] |
| Health | [[Lobster Brain/Health/Peptides\|Peptides]], [[Lobster Brain/Health/Supplements\|Supplements]] |
| Tech | [[Lobster Brain/Tech/Mac\|Mac]], [[Lobster Brain/Tech/Networks\|Networks]] |
| Business | [[Lobster Brain/Business/Start-up Stories\|Start-up Stories]], [[Lobster Brain/Business/Business Ideas\|Business Ideas]], [[Lobster Brain/Business/Business Inspiration\|Business Inspiration]] |
| Fitness | [[Lobster Brain/Fitness/Exercise Ideas\|Exercise Ideas]] |
```

Also create the folder structure and empty subtopic pages:

- `Lobster Brain/AI/Claude Tips and Tricks.md`
- `Lobster Brain/AI/Claude Code.md`
- `Lobster Brain/AI/Claude MD Files.md`
- `Lobster Brain/AI/OpenClaw Tips.md`
- `Lobster Brain/AI/Prompting Resources.md`
- `Lobster Brain/Health/Peptides.md`
- `Lobster Brain/Health/Supplements.md`
- `Lobster Brain/Tech/Mac.md`
- `Lobster Brain/Tech/Networks.md`
- `Lobster Brain/Business/Start-up Stories.md`
- `Lobster Brain/Business/Business Ideas.md`
- `Lobster Brain/Business/Business Inspiration.md`
- `Lobster Brain/Fitness/Exercise Ideas.md`

Each subtopic page starts with a heading matching its name:

```markdown
# <Subtopic Name>
```

## Supported inputs

Handle these input types:

### Screenshots
1. Read the screenshot image.
2. Extract visible text via OCR.
3. Detect the main platform from visible UI cues.
4. Extract the visible URL if present.
5. Extract the visible poster or author if present.
6. Capture timestamp from the screenshot filename when possible.
7. If no filename timestamp, use ingestion timestamp.

If the screenshot contains multiple items, treat the most central or largest content
block as the main item. Ignore sidebars, ads, and chrome. If two items are equally
prominent, ask the user.

### Links
1. Fetch and parse the page.
2. Identify the source platform or site.
3. Extract title, author, and source URL.
4. Create a concise summary.
5. Preserve the full text whenever feasible.
6. Preserve code blocks and factual text faithfully.

### Podcasts
1. Capture playback timestamp when visible or provided.
2. Capture ingestion timestamp.
3. Store source metadata (show, episode, player context) when available.
4. Summarize the captured content.

### Long-form text
1. Identify the topic and any source metadata.
2. Summarize the content.
3. Preserve the full text.

## Metadata

For each capture, gather and store as many of these fields as are relevant:

- **category** — which top-level category
- **subcategory** — which subtopic page
- **platform** — where the content came from
- **source_url** — link to the original
- **poster_or_author** — who created it
- **screenshot_timestamp** — from filename if available
- **playback_timestamp** — for podcasts
- **ingestion_timestamp** — when captured (use current date/time)
- **summary** — concise description
- **tags** — relevant keywords
- **reminder_date** — if user wants to revisit later
- **completed** — `false` by default when reminder is set

Never invent metadata. Use `unknown` or `needs_confirmation` for unclear fields.
Only ask the user when uncertainty affects routing, retrieval, or reminders.

## Classification and routing

Before saving anything:

1. Read `Lobster Brain.md` from the vault.
2. Compare the capture against existing categories and subtopics.
3. Normalize names before matching (case-insensitive, trim whitespace).
4. Prefer existing categories and subtopics whenever there is a strong match.

**No good category exists:**
- Propose a new category.
- Ask for approval before adding it to `Lobster Brain.md`.

**Category exists but no good subtopic:**
- Propose a new subtopic.
- Ask for approval before creating the page and updating `Lobster Brain.md`.

**Two destinations are equally strong:**
- Ask the user instead of guessing.

**Similar name already exists:**
- Ask whether the existing one should be used instead of creating a new one.

## Confirmation flow

Before writing anything to the vault, show a compact draft:

```
Category: AI
Subtopic: Claude Code
Platform: Twitter
Source: https://...
Author: @someone
Timestamp: 2026-04-06 14:30
Summary: How to use Claude Code projects for...
Tags: #claude-code #workflow
```

Include `reminder_date` if applicable. Flag any `unknown` or `needs_confirmation` fields.

Do not dump all raw text in the preview — just the routing-critical fields and summary.

If creating a new category or subtopic, explicitly ask for approval.

Only save after the user confirms.

## Entry format

Append each capture to the bottom of the relevant subtopic page (oldest-first order).

Use this structure:

```markdown
---

**Platform:** Twitter
**Source:** [Link](https://...)
**Author:** @someone
**Captured:** 2026-04-06 14:30
**Tags:** #claude-code #workflow

**Summary:** How to use Claude Code projects for organizing work.

> [!info]- Full text
> The complete extracted text goes here.
> Code blocks, quotes, and formatting are preserved faithfully.
> This section stays collapsed by default to keep the page compact.
```

Rules for entries:
- Use `---` as separator between entries.
- Keep the visible portion compact (metadata + summary).
- Put long OCR text, article text, transcripts, or code in a collapsible Obsidian
  callout (`> [!info]- Full text`).
- Preserve code blocks and factual text as faithfully as possible.
- If a reminder is set, add `**Reminder:** 2026-05-01` and `**Completed:** false`.

## Timestamp priority

1. Screenshot filename timestamp (when available).
2. Source-specific timestamp (publication date, etc.).
3. Ingestion timestamp (now).

For podcasts, store both playback and ingestion timestamps.
If ambiguous, mark as `needs_confirmation` and ask during confirmation.

## Reminder logic

If the user says they want to revisit, not forget, follow up, or be reminded:
- Prompt for a reminder date if not provided.
- Store `reminder_date` and set `completed` to `false`.
- Include reminder fields in the saved entry metadata.

## Rules

- Never silently create new taxonomy structure — always ask first.
- Never hardcode the category system — always read `Lobster Brain.md`.
- Never fabricate metadata — use `unknown` when unsure.
- Never treat sidebar/ad content as the main item.
- Never create one page per screenshot.
- Always confirm before writing to the vault.
- Keep entries compact; collapse long content.
- Preserve source text faithfully.
