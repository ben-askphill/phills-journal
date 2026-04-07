---
name: phills-journal
description: >
  Write and publish new editions of Phill's Journal, the Ask Phill ecommerce
  newsletter covering Shopify updates, AI features, app integrations, and
  merchant-focused insights. Use this skill whenever the task involves writing a
  journal edition, creating newsletter content for Ask Phill, publishing to the
  Phill's Journal Notion database, or gathering Shopify ecosystem news for the
  newsletter. Also use when someone mentions "Phill's Journal", "Ask Phill
  newsletter", "journal edition", or wants to draft ecommerce newsletter content
  in the Phill's Journal style.
---

# Phill's Journal

**Phill's Journal: Insights & Innovations** is a recurring Shopify ecosystem newsletter published by Ask Phill. Each edition covers 4-8 topics — platform updates, AI features, app integrations, and merchant-focused tips — written in a direct, first-person voice and published as a Notion page.

- **Authors:** Tijs Luitse (Lead AI & Innovation) and Ben Rosenberg (Senior Shopify Consultant)
- **Audience:** Shopify merchants and ecommerce teams, especially mid-market to enterprise, often B2B, often European
- **Cadence:** Every few weeks

## Setup

This skill uses the Notion API directly via `curl`. The API key is stored in `.env` in the skill directory (`.claude/skills/phills-journal/.env`). Source it before making API calls:

```bash
source ".claude/skills/phills-journal/.env"
```

The Phill's Journal Notion database ID is defined in `references/notion-api.md`.

## Workflow

Follow these steps in order for each new edition.

### 1. Research topics

Gather 4-8 timely, actionable topics. Good sources: Shopify changelog, app release notes, ecosystem blogs, and platform announcements. Mix it up — Shopify core features, integrations (Klaviyo, Flow, Gorgias), AI developments, and optionally an Ask Phill service spotlight as the last topic.

Each topic needs:

- A clear angle (what changed and why it matters)
- A source URL for the bookmark embed
- An emoji that matches the theme (see Emoji Reference below)

### 2. Write the edition

#### Title, subject, and summary

- **Title** (`Name` property): Punchy and topic-driven — not "Phill's updates from week X"
  - Good: *"Smarter Triggers & Analytics"*, *"Shopify Editions '26"*
- **Email subject**: Teases 2-3 topics as a newsletter subject line
  - Good: *"Metafields in Analytics, automated alerts with Flow, and Klaviyo tricks you didn't know"*
- **Summary**: 2-4 sentences summarising the edition for the gallery card

#### Page content

Structure the page body in this exact order:

1. **Cover image** at the top (full-width, no caption or a brief one)
2. **"What's included this week:"** as an H3 heading, followed by a divider
3. **TOC** — one line per topic: emoji + topic title (plain paragraphs, not bullet points)
4. **Topic sections** — each one gets:
   - H3 heading with emoji + title (same emoji as the TOC line)
   - Divider
   - 3-6 sentences of body text
   - Bookmark embed for the source URL (not an inline hyperlink)
   - Optional: image or video embed where relevant

### 3. Generate cover image

Use the `journal-cover` skill to generate the cover image. When sending the title:

**Use a shortened, concise version** of the journal edition title — not the full title. Strip creative phrasing and keep only the core topic.

Example: If the journal title is "Ship Without Fear: Shopify Rollouts", the cover image title should just be "Shopify Rollouts".

### 4. Publish to Notion

Read `references/notion-api.md` for the exact API calls, JSON payloads, and block schemas. The short version:

1. `POST /v1/pages` — create the page in the database with properties and empty children
2. `PATCH /v1/blocks/<page-id>/children` — append all content blocks
3. `PATCH /v1/pages/<page-id>` — set the cover banner, thumbnail, icon, and author:
   - Set the page icon to the custom emoji "journal-icon" (already saved in the workspace). Include in the PATCH body: `"icon": { "type": "custom_emoji", "custom_emoji": { "name": "journal-icon" } }`
   - Set the `Author(s)` people property to Tijs Luitse (`6d45bbd2-778d-4dc5-a695-0a390290b201`) and Ben Rosenberg (`2b7d872b-594c-817c-b416-000200f23fe1`). The Notion connection app appears as "created by" by default, so you must explicitly set this. It cannot be done at creation time — it must be a separate PATCH after the page exists.

Authentication uses `NOTION_API_KEY` environment variable. All API calls go through `curl` with `Authorization: Bearer $NOTION_API_KEY` and `Notion-Version: 2022-06-28`.

After publishing, report:
- Edition title
- Number of topics
- Notion page URL (`https://www.notion.so/<page-id-without-dashes>`)

---

## Writing Rules

Read `references/phills-journal-handover.md` before writing any edition. It contains the complete rules for writing voice, tone, page structure, emoji usage, topic selection, source handling, and the pre-publish checklist. Follow it exactly.

### Link handling

When linking to the Shopify changelog or any other external URL, always use Notion **bookmark blocks** — never inline hyperlinks. This applies to source links, references, and any external link embedded in the page content.

### Punctuation

Never use em dashes (—) or double dashes (--) in any written content. Use commas, semicolons, colons, or separate sentences instead.
