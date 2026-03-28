# Lobster Brain Product Spec

## Overview
Lobster Brain is an Obsidian-first capture and knowledge organization system for screenshots, links, podcasts, and long-form text. It uses AI-assisted extraction, classification, summarization, and confirmation to help route new information into a structured personal knowledge base.

The product is being designed so it can first be implemented as an OpenClaw skill and later as a standalone web app without changing the underlying information architecture.

## Goals
- Capture content from screenshots, shared links, podcasts, and text-heavy sources.
- Extract and preserve metadata such as source platform, source URL when visible, poster or author, timestamps, and reminder state.
- Route captures into existing Obsidian categories and subtopics using a dynamic overview page instead of hardcoded taxonomy.
- Propose new categories or subtopics only when no strong existing match exists, and require approval before creating them.
- Store compact, readable entries while preserving full source text for later search and reuse.
- Support future implementation as a web app using the same product logic.

## Non-goals
- Notion support in v1.
- Automatic category or subtopic creation without approval.
- One-page-per-screenshot storage.

## Inputs
### Supported inputs
- Screenshots.
- Direct links such as Substack, Medium, blogs, docs, or article pages.
- Podcast captures, including playback timestamp when available.
- Long-form factual or reference text.

### Input behavior
#### Screenshots
- Run OCR on the screenshot.
- Detect the main platform from visible UI cues.
- Capture the screenshot timestamp from the filename when possible; otherwise use ingestion time.
- Extract the visible URL if present.
- Extract the visible poster or author if present; otherwise use `unknown` or ask for clarification when important.

#### Links
- Fetch and parse the linked content.
- Generate a concise summary.
- Preserve full text whenever feasible so the content can be searched or reused later.
- Preserve code blocks and factual text as faithfully as possible.

#### Podcasts
- Capture the playback timestamp when visible.
- Also store ingestion timestamp.
- Treat show, episode, or player source as metadata when available.

## Storage model
### Obsidian only
Version 1 targets Obsidian only.

### Folder structure
Use this pattern:
- `Lobster Brain.md`
- `Lobster Brain/<Category>/<Subtopic>.md`

Do not create separate category markdown pages unless requested later.

### Overview page
`Lobster Brain.md` is the canonical taxonomy index and source of truth for routing.

It contains a markdown table listing categories and subtopics, where subtopics are Obsidian links to their pages.

The system must read this page dynamically before routing content. It must not rely on hardcoded category or subtopic lists.

### Subtopic pages
Each subtopic page stores multiple capture entries over time.

New entries are appended at the bottom so ordering remains oldest first.

## Entry format
Each saved capture is a compact block with:
1. Metadata first.
2. Short summary second.
3. Main text and preserved source text stored below in searchable form.

### Visible section
The visible section should stay compact and readable. It should include the most important metadata fields plus the short summary.

### Full text section
Full OCR text, article text, transcript text, code examples, or other long content should be preserved in a collapsed section when possible so the note remains compact while the source remains searchable.

Recommended Obsidian-compatible options:
- Foldable callout.
- HTML details block.

## Metadata schema
Recommended metadata fields for each capture:
- `category`
- `subcategory`
- `platform`
- `source_url`
- `poster_or_author`
- `screenshot_timestamp`
- `playback_timestamp`
- `ingestion_timestamp`
- `summary`
- `tags`
- `reminder_date`
- `completed`
- `confidence_notes`

Fields that are not applicable may be omitted or set to `unknown` depending on the capture type.

## Classification and routing
### Taxonomy matching
Before saving, the system must read `Lobster Brain.md` and compare the new capture against existing categories and subtopics.

It should normalize names before matching and prefer existing topics whenever there is a strong match.

### New structure proposals
If no good category exists, propose a new category and ask for approval before adding it to the overview page.

If a category exists but no good subtopic exists, propose a new subtopic page and ask for approval before creating it and linking it from the overview page.

Do not create new structure automatically.

### Main item detection
For screenshots containing multiple visible items, treat the main item as the most central or largest content block that appears to be the intended target of the capture.

Ignore sidebars, ads, recommended content, or surrounding chrome unless they are clearly the focus.

If two items are equally prominent, ask the user instead of guessing.

## Confirmation flow
Before writing anything, present a compact confirmation draft with the most important fields.

The confirmation should focus on fields that affect routing, retrieval, or future action, such as category, subcategory, platform, source URL, poster or author when important, timestamps when ambiguous, and reminder fields.

Use `unknown` or `needs_confirmation` for uncertain metadata rather than inventing values.

Ask questions only for fields that materially affect routing, retrieval, or reminders.

## Timestamp rules
### General priority
Use this priority order for timestamps:
1. Screenshot filename timestamp when available.
2. Source-specific timestamp when applicable.
3. Ingestion timestamp.

### Podcasts
For podcasts, store both playback timestamp and ingestion timestamp.

### Ambiguous timestamps
If timestamp parsing is unclear, mark it as `needs_confirmation` and ask in the confirmation step.

## Reminder logic
If the user indicates they want to revisit something later or avoid forgetting it, the system should prompt for or store a reminder date.

When a reminder is set, store `reminder_date` and default `completed` to `false` unless explicitly marked finished.

The reminder is stored as metadata in v1. Later automation or review logic can use `reminder_date` plus `completed` to identify items needing follow-up.

## Risk controls
### Structure drift
- Normalize names before matching.
- Compare proposed names to existing categories and subtopics before creating anything new.
- If close matches exist, ask whether one of them should be used instead of creating a new one.

### Wrong routing
- Always read `Lobster Brain.md` before routing.
- Ask instead of guessing when two matches are similarly strong.

### Metadata hallucination
- Never invent poster, URL, category, timestamp, or source details without evidence.
- Use `unknown` or `needs_confirmation` for unclear fields.

### Note bloat
- Keep the visible section compact.
- Put long OCR or source text in a collapsed section.

### Source fidelity loss
- Preserve full text whenever feasible.
- Preserve code blocks and factual text as faithfully as possible.

## Future app alignment
The product logic should remain separate from the OpenClaw-specific implementation.

### Core product behavior
- Ingest content.
- Extract metadata.
- Classify and route.
- Confirm with the user.
- Save to Obsidian.
- Track reminders.

### Potential future web app responsibilities
- Upload screenshots or paste links.
- Display confirmation UI.
- Manage reminders and review queues.
- Sync with the Obsidian vault or an ingestion layer.
