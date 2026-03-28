# Lobster Brain Design Questions and Answers

## Purpose
This document records the key design questions, clarifications, and agreed answers used to shape the Lobster Brain product spec. It is intended as a reference for future prompt design, OpenClaw skill implementation, or standalone app development.

## Naming
### What is the system called?
**Answer:** The product and main page name is `Lobster Brain`.

## Scope
### What kinds of inputs should the system support?
**Answer:** Screenshots, direct links such as Substack or Medium posts, podcasts, and long-form factual or reference text.

### Is Notion included?
**Answer:** No for v1. The architecture is Obsidian-only for clarity and simplicity.

### Is this only an OpenClaw skill?
**Answer:** No. It should work as an OpenClaw skill first, but the logic should also be clean enough to become a standalone web app later.

## Storage architecture
### What is the main storage target?
**Answer:** Obsidian only.

### What is the folder structure?
**Answer:**
- `Lobster Brain.md`
- `Lobster Brain/<Category>/<Subtopic>.md`

### Are there separate category pages?
**Answer:** No, not by default. The overview page plus subtopic pages are the core structure. Separate category markdown pages should not be created unless requested later.

### What is the role of `Lobster Brain.md`?
**Answer:** It is the canonical overview page and routing index. The system must read it dynamically before deciding where content belongs.

### What format should the overview page use?
**Answer:** A markdown table listing categories and subtopics, with subtopics linked to their Obsidian pages.

## Knowledge organization
### Is content stored as one page per screenshot?
**Answer:** No. Content is appended to topic or subtopic pages over time.

### How should entries be ordered on a page?
**Answer:** Oldest first, with new items appended at the bottom.

### What should happen if content fits an existing category and subtopic?
**Answer:** Use the existing structure.

### What should happen if it does not fit an existing category?
**Answer:** Propose a new category and ask for approval before adding it.

### What should happen if it fits a category but not an existing subtopic?
**Answer:** Propose a new subtopic and ask for approval before creating it and linking it from the overview page.

## Entry layout
### What should a stored entry look like?
**Answer:** Metadata first, then a short summary, then preserved full or searchable text below.

### Should full text always be visible?
**Answer:** No. The visible part should stay compact, while the full text should be stored in a collapsible section when possible.

### Should long OCR or article text be preserved?
**Answer:** Yes. Preserve the full text whenever feasible, including factual text and code examples, so it can be searched and reused later.

## Metadata
### Which metadata fields matter most?
**Answer:** Platform, source URL when available, poster or author, timestamps, category, subcategory, summary, tags, reminder date, completion state, and confidence notes.

### How should timestamps work?
**Answer:** Prefer screenshot filename timestamp first, then source-specific timestamp where applicable, then ingestion time. Podcasts should store both playback timestamp and ingestion timestamp.

### How should missing poster or source information be handled?
**Answer:** Use `unknown` or ask for clarification when the missing field materially affects retrieval or routing.

## Input handling
### How should screenshots be interpreted?
**Answer:** OCR the screenshot, detect the main platform from the UI, extract visible metadata, and treat the main item as the most central or largest intended content block.

### What if a screenshot contains multiple items?
**Answer:** Use the main item. If two items are equally prominent, ask the user.

### How should links be handled?
**Answer:** Fetch the page, summarize it, preserve the full text when feasible, and route it like any other capture.

### How should podcasts be handled?
**Answer:** Store playback timestamp and ingestion timestamp, plus source metadata when available.

## Confirmation behavior
### What should be shown for confirmation?
**Answer:** A compact draft showing the most important metadata and routing fields, especially anything affecting storage location, retrieval, or follow-up.

### Should the system ask about every uncertainty?
**Answer:** No. It should only ask about uncertainties that materially affect routing, retrieval, timestamps, reminders, or structure creation.

## Reminders
### Should the system support reminder dates?
**Answer:** Yes. Reminder dates are stored as metadata so the user can revisit interesting items later.

### How should reminder completion be tracked?
**Answer:** Store a completion field such as `completed`, so reminder items can later be filtered into open and finished states.

## Risk management
### How should duplicate or near-duplicate categories be prevented?
**Answer:** Normalize names, compare against the existing overview page, and ask when similar names already exist.

### How should wrong routing be reduced?
**Answer:** Always read the overview page first and ask when two candidates are similarly strong.

### How should note bloat be reduced?
**Answer:** Keep visible entries compact and move long source text into a collapsible section.

### How should hallucinated metadata be prevented?
**Answer:** Never invent metadata. Use `unknown` or `needs_confirmation` instead.

## Future direction
### Could this become a standalone app later?
**Answer:** Yes. The product logic is being designed separately from the OpenClaw-specific wrapper so it can later be implemented as a web app with the same workflow and storage model.
