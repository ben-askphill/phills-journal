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

This skill uses the Notion API directly via `curl`. The API key is stored in `.env` in the plugin root (`${CLAUDE_PLUGIN_ROOT}/.env`). Source it before making API calls:

```bash
source "${CLAUDE_PLUGIN_ROOT}/.env"
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

## Writing Voice

Write as Tijs — a knowledgeable ecommerce expert talking to peers. Conversational but substantive, like a smart colleague explaining something useful. Not academic, not salesy. Confident but not arrogant.

**Sentence style:**
- Short to medium-length sentences
- Lead with the *so what* — why a feature matters before how it works
- No fluff or filler ("exciting new update", "game-changer")
- Personal opinions are encouraged when grounded (*"This is 100% my favorite feature"*)

**Per-topic body text (3-6 sentences):**
1. Context — what changed or what this is
2. Impact — why it matters for merchants
3. Practical tip — a concrete how-to if relevant (keep it to 1-2 steps)

**Example** (from "Smarter Triggers & Analytics"):

> *"Most people use Klaviyo's built-in Shopify triggers or segments to start email flows. But there's a hidden method that gives you way more flexibility: the 'Track an Event' action in Shopify Flow. This lets you send any Flow event directly to Klaviyo as a custom metric... Super useful for things Shopify doesn't send emails for by default..."*

**Avoid:**
- Bullet points inside topic body text (the TOC is the only exception)
- Repeating the topic title in the body text
- Passive voice when active works
- Corporate jargon ("leverage", "utilize", "seamlessly")

---

## Emoji Reference

Match each topic's emoji to its theme. Use the same emoji in both the TOC line and the H3 heading.

| Emoji | Theme |
|---|---|
| :bar_chart: | Analytics, data, metrics |
| :robot: | AI, automation |
| :mailbox_with_mail: | Email, triggers, notifications |
| :envelope: | Email features |
| :speech_balloon: | Chat, conversational |
| :alarm_clock: | Scheduling, timing |
| :hammer_and_wrench: | Dev tools, apps, building |
| :globe_with_meridians: | Internationalisation, markets |
| :credit_card: | Payments |
| :snowflake: | Seasonal / Editions releases |
| :crystal_ball: | AI prediction, simulation |
| :handshake: | Partnerships |
| :dragon: | End-of-year / special |

---

## Reference Examples

Study these past editions for tone, structure, and formatting:

| Edition | URL | Topics |
|---|---|---|
| Smarter Triggers & Analytics | https://www.notion.so/3065f35473528086800cdb5e875b9d58 | Metafields in Analytics, Flow alerts, Klaviyo triggers, Sidekick, Flow email sender |
| Shopify Editions '26 | https://www.notion.so/2c65f354735280729e3cef74166274d7 | Winter '26 overview, Rollouts, Product Network, SimGym, Forms translation, Sidekick app builder, Flow from chat, Klarna EU, Klaviyo year-end |

---

## Pre-publish Checklist

Before considering an edition complete, verify:

- [ ] Page created in the correct database with all properties filled
- [ ] `Name` is punchy and topic-driven
- [ ] `Email subject` teases 2-3 topics
- [ ] `Summary` is 2-4 sentences
- [ ] Cover image at top of page + set as page cover
- [ ] `Thumbnail` property set
- [ ] Page icon set to custom emoji "journal-icon"
- [ ] Author property set to Tijs and Ben (not the connection app)
- [ ] "What's included this week:" TOC with emoji lines
- [ ] Each topic: H3 + emoji + divider + body text + bookmark
- [ ] No bullet points inside body text
- [ ] Source bookmarks embedded (not inline hyperlinks)
- [ ] Tone is direct, first-person, practical — no fluff
