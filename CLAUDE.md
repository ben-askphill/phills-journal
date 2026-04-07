# Phill's Journal

This repo is the home of **Phill's Journal: Insights & Innovations** — Ask Phill's recurring ecommerce newsletter covering Shopify updates, AI features, app integrations, and merchant-focused insights.

## What is Phill's Journal?

A newsletter published every few weeks as a Notion page. Each edition covers 4-8 topics drawn from the Shopify ecosystem: platform changes, AI developments, app integrations (Klaviyo, Flow, Gorgias), and practical merchant tips. Written in a direct, first-person voice by Tijs Luitse and Ben Rosenberg.

**Audience:** Shopify merchants and ecommerce teams, especially mid-market to enterprise, often B2B, often European.

## How it works

The newsletter is produced end-to-end using two local Claude Code skills (in `.claude/plugins/`):

### `/phills-journal` skill

Handles the full writing and publishing workflow:

1. **Research** — Gather 4-8 timely topics from the Shopify changelog, app release notes, and ecosystem blogs
2. **Write** — Draft the edition with a punchy title, email subject, summary, TOC, and topic sections following a strict structure and voice guide
3. **Publish** — Create the page in the Phill's Journal Notion database via the Notion API, set cover image, icon, author properties, and append all content blocks

### `/journal-cover` skill

Generates a cover image for each edition:

1. Generates a PNG cover in code using the bundled background image and Inter Bold font
2. Splits the title text across 1-2 lines with intelligent line-break logic
3. Uploads the cover to catbox.moe and sets it as the Notion page cover

## Setup

- `NOTION_API_KEY` — required for publishing to Notion (stored in `.claude/plugins/phills-journal/.env`)
- Both skills live locally in `.claude/plugins/` within this repo

## Usage

```
"Write a new Phill's Journal edition"
"Create a journal edition about Shopify's latest updates"
"Create a Journal cover for 'AI Tools & Shopify'"
```
