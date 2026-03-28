---
name: Lobster Brain
description: Use this skill to capture screenshots, links, podcasts, and long-form content into an Obsidian knowledge system. Extract metadata, summarize content, match it to the existing Lobster Brain taxonomy, ask for confirmation when needed, and save it into the correct subtopic page.
---

# Lobster Brain

## Purpose
Lobster Brain captures incoming knowledge items and stores them in Obsidian using a structured, evolving taxonomy.

Use this skill when the user sends:
- a screenshot
- a link to an article, post, blog, Substack, Medium page, or documentation page
- a podcast moment or podcast-related content
- long-form factual or reference text

The goal is to turn raw input into a searchable, well-organized knowledge entry with minimal manual effort from the user.

## Storage model
Use Obsidian only.

The storage structure is:
- `Lobster Brain.md`
- `Lobster Brain/<Category>/<Subtopic>.md`

`Lobster Brain.md` is the canonical overview page and routing index.

Do not rely on hardcoded categories or subtopics. Always read `Lobster Brain.md` first and use it as the source of truth.

Do not create separate category markdown pages unless the user explicitly requests them.

## Overview page behavior
`Lobster Brain.md` contains a markdown table of categories and subtopics.

Each row represents a category.

The subtopics column contains Obsidian links to subtopic pages.

When a new approved category or subtopic is created, update `Lobster Brain.md` to reflect it.

Do not invent structure outside what is present on the overview page unless the user approves a new category or subtopic.

## Input handling
### Screenshots
When given a screenshot:
1. Read the screenshot.
2. Extract visible text using OCR.
3. Detect the main platform from visible UI cues.
4. Extract the visible URL if present.
5. Extract the visible poster or author if present.
6. Determine the main content item.
7. Capture timestamp from the screenshot filename when possible.
8. If no filename timestamp is available, use ingestion timestamp.

If a screenshot contains multiple visible items, treat the main item as the most central or largest content block that appears to be the intended target of the capture.

Ignore sidebars, ads, recommendations, and surrounding interface chrome unless they are clearly the focus.

If two items are equally prominent, ask the user instead of guessing.

### Links
When given a link:
1. Fetch and parse the page.
2. Identify the source platform or site.
3. Extract title, author when available, and source URL.
4. Create a concise summary.
5. Preserve the full text whenever feasible.
6. Preserve code blocks and factual text as faithfully as possible.

### Podcasts
When given podcast content:
1. Capture playback timestamp when visible or provided.
2. Capture ingestion timestamp.
3. Store source metadata such as show, episode, or player context when available.
4. Summarize the captured content.

## Metadata rules
For each capture, gather and store as many of these fields as are relevant:
- category
- subcategory
- platform
- source_url
- poster_or_author
- screenshot_timestamp
- playback_timestamp
- ingestion_timestamp
- summary
- tags
- reminder_date
- completed
- confidence_notes

Never invent metadata without evidence.

If a field is unclear, use `unknown` or `needs_confirmation`.

Ask the user only when the uncertainty materially affects routing, retrieval, timestamps, reminders, or creation of new structure.

## Classification and routing
Before saving anything:
1. Read `Lobster Brain.md`.
2. Compare the new capture against existing categories and subtopics.
3. Normalize names before matching.
4. Prefer existing categories and subtopics whenever there is a strong match.

If no good category exists:
- Propose a new category.
- Ask for approval before adding it to `Lobster Brain.md`.

If a category exists but no good subtopic exists:
- Propose a new subtopic.
- Ask for approval before creating `Lobster Brain/<Category>/<Subtopic>.md`.
- Add the new subtopic link to `Lobster Brain.md` only after approval.

If two existing destinations are similarly strong matches, ask the user instead of guessing.

## Entry format
Append captures to the relevant subtopic page.

Always append new entries at the bottom so entries remain in oldest-first order.

Each entry should have this structure:
1. Metadata section.
2. Short summary.
3. Preserved source text.

Keep the visible portion compact.

Long OCR text, article text, transcript text, code examples, or other large source material should be preserved in a collapsible section when possible.

Use Obsidian-friendly collapsible formatting when available, such as a foldable callout or HTML details block.

## Confirmation flow
Before writing anything, show a compact draft to the user.

The draft should emphasize the most important fields:
- category
- subcategory
- platform
- source URL
- poster or author when relevant
- important timestamps
- reminder date if applicable
- any unknown or uncertain fields

Do not dump all raw text in the confirmation preview unless needed.

If the system wants to create a new category or subtopic, explicitly ask for approval before proceeding.

Only save after user confirmation.

## Timestamp rules
Use this timestamp priority:
1. Screenshot filename timestamp when available.
2. Source-specific timestamp when applicable.
3. Ingestion timestamp.

For podcasts, store both playback timestamp and ingestion timestamp.

If a timestamp is ambiguous, mark it as `needs_confirmation` and ask in the confirmation step.

## Reminder logic
If the user says they want to revisit something later, not forget it, follow up on it, or be reminded about it, prompt for or store a reminder date.

When a reminder is set:
- store `reminder_date`
- default `completed` to `false` unless explicitly marked finished

Treat reminder metadata as part of the saved knowledge entry.

## Risk control rules
### Prevent structure drift
- Normalize names before matching.
- Compare new names against existing categories and subtopics.
- If a proposed name is very similar to an existing one, ask whether the existing one should be used instead.

### Prevent wrong routing
- Always read `Lobster Brain.md` before routing.
- Ask when two matches are similarly plausible.

### Prevent hallucinated metadata
- Never fabricate poster, URL, category, timestamp, or source details.
- Use `unknown` or `needs_confirmation` when uncertain.

### Prevent note bloat
- Keep metadata and summary compact.
- Store long source text in a collapsible section.

### Preserve source fidelity
- Preserve full text whenever feasible.
- Preserve code blocks and factual text as faithfully as possible.

## Behavior boundaries
Do not silently create new taxonomy structure.

Do not hardcode the category system.

Do not treat side content as the main item unless it is clearly the intended capture target.

Do not create one page per screenshot unless the user explicitly asks for that behavior.

## Future compatibility
Keep the logic implementation-agnostic where possible so the same workflow can later be used in a standalone web app.
