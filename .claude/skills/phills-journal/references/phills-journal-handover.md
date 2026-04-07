# Phill's Journal – Writing Process Handover

> **Purpose:** This document tells a Claude Code agent everything it needs to know to write new editions of *Phill's Journal: Insights & Innovations* — from structure and tone to Notion setup and required properties.

---

## 1. What is Phill's Journal?

**Phill's Journal: Insights & Innovations** is a recurring ecommerce newsletter published by Ask Phill. It covers the Shopify ecosystem: AI features, platform updates, new app functionality, migrations, and hidden gems. The audience is merchants and ecommerce teams who want actionable, up-to-date insights.

- **Authors:** Tijs Luitse (Lead AI & Innovation @ Ask Phill) and Benjamin van der Weerdt
- **Cadence:** Every few weeks
- **Published in:** Notion (and sent as email)
- **Notion database:** [Phill's Journal Database](https://www.notion.so/2185f35473528009a878de83966a3534)
- **Journal homepage:** [Phill's Journal: Insights & Innovations](https://www.notion.so/2185f354735280f9a6c5f5cebee77f98)

---

## 2. Notion Structure

Each edition is a **page inside the `Phill's Journal Database`** (collection ID: `2185f354-7352-801a-b88a-000b0eb8a946`).

### Database Properties

| Property | Type | Notes |
|---|---|---|
| `Name` | Title | The edition title (e.g. "Smarter Triggers & Analytics") |
| `Summary` | Text | 2–4 sentence summary of the full edition |
| `Email subject` | Text | The subject line used when sending as email |
| `Author(s)` | Person | Tijs Luitse (`6d45bbd2-778d-4dc5-a695-0a390290b201`) and Benjamin van der Weerdt (`ad473aa2-be95-448f-a58f-d6bfde9c807e`) |
| `Thumbnail` | File | Cover image for the gallery card |
| `Created` | Created time | Auto-set |

### Creating a New Entry

Use the **default template** (template ID: `22b5f354-7352-8036-99d0-c5b74f5e471e`, named "Phill's updates from week") when creating a new page in the database. You can override properties alongside the template.

---

## 3. Page Structure

Each journal page follows this exact structure:

```
[Cover / header image]
[Optional: image caption]

### What's included this week:
---
[emoji] [Topic 1 title]
[emoji] [Topic 2 title]
[emoji] [Topic 3 title]
... (typically 4–8 topics)

### [emoji] [Topic 1 title]
---
[Body text]
[Optional: bookmark link to source]

### [emoji] [Topic 2 title]
---
[Body text]
[Optional: bookmark / image / video embed]

...
```

### Structure Rules

- **Cover image** at the very top (full-width image block, no caption or a brief one)
- **"What's included this week:"** section is a bullet-style table of contents using emojis — no H3, just plain lines with emoji + topic title
- Each **topic gets its own H3 heading** with a matching emoji, followed by a horizontal rule (`---`)
- Source links appear as **bookmark embeds** (not inline hyperlinks) below the relevant section
- Images or videos can be included inside topic sections where relevant

---

## 4. Writing Style & Tone

These are the core rules that define the voice of the journal. Study the examples below carefully.

### Voice
- **First-person, direct, knowledgeable** — written as Tijs, an ecommerce expert talking to peers
- Not academic, not salesy. Confident but not arrogant.
- Conversational but substantive — it should feel like a smart colleague explaining something useful

### Sentences
- Short to medium-length sentences
- Lead with the *so what* — explain why a feature matters before explaining how it works
- No fluff, no filler phrases like "exciting new update" or "game-changer"
- Occasional personal opinion is fine and encouraged (e.g. *"This is 100% my favorite feature"*, *"This is a big one"*)

### Per-topic body text
- Typically **3–6 sentences** per topic
- Start with context (what changed / what this is)
- Move to impact (why it matters for merchants)
- End with a practical tip or how-to where relevant
- If there's a how-to, keep it brief: one or two concrete steps

### Good example (from "Smarter Triggers & Analytics"):
> *"Most people use Klaviyo's built-in Shopify triggers or segments to start email flows. But there's a hidden method that gives you way more flexibility: the 'Track an Event' action in Shopify Flow. This lets you send any Flow event directly to Klaviyo as a custom metric... Super useful for things Shopify doesn't send emails for by default..."*

### Things to avoid
- Do NOT use bullet points inside topic body text (the TOC at the top is the exception)
- Do NOT repeat the topic title in the body text
- Do NOT use passive voice if active works
- Do NOT use corporate jargon ("leverage", "utilize", "seamlessly")
- Keep opinions grounded — back them up with a reason

---

## 5. Topics Selection

Topics should be:
- **Timely** — recent Shopify releases, new app features, platform updates
- **Actionable** — things merchants or agencies can do something with
- **Relevant to the Ask Phill audience** — Shopify merchants, especially mid-market to enterprise, often B2B, often in Europe
- **Varied** — mix of Shopify core features, apps/integrations (Klaviyo, Flow, Gorgias, etc.), AI developments, and Ask Phill service spotlights

Typical topic count per edition: **4–8 topics**

The last topic in some editions is an **Ask Phill service spotlight** (e.g. "Year-End Klaviyo Refresh") — these describe a service package or offering and include a CTA callout block.

---

## 6. Required Properties to Fill Per Edition

When creating a new Notion entry, always populate:

1. **`Name`** — a punchy, topic-driven title (not "Phill's updates from week X")
   - Good: *"Smarter Triggers & Analytics"*, *"Shopify Editions '26"*
   - Bad: *"Phill's updates from week 12"*

2. **`Email subject`** — written as a newsletter subject line; should tease 2–3 of the topics
   - Good: *"Metafields in Analytics, automated alerts with Flow, and Klaviyo tricks you didn't know"*

3. **`Summary`** — 2–4 sentence paragraph summarising the whole edition; used as the gallery card description
   - Good: *"This week is all about working smarter with your data and automations. Metafields now work as filters and dimensions in Analytics, Flow can send automated performance alerts and now emails from your own address, plus a hidden way to trigger Klaviyo flows directly from Shopify Flow."*

4. **`Thumbnail`** — a cover image uploaded as a file attachment on the page

---

## 7. Emoji Usage

Each topic in the TOC and the corresponding H3 heading uses the **same emoji**. The emoji should match the theme of the topic. Common ones seen in past editions:

| Emoji | Usage |
|---|---|
| 📊 | Analytics, data, metrics |
| 🤖 | AI, automation |
| 📬 | Email, triggers, notifications |
| ✉️ | Email-related features |
| 💬 | Chat, conversational features |
| ⏰ | Scheduling, timing |
| 🛠️ | Building, apps, dev tools |
| 🌍 | Internationalisation, markets |
| 💳 | Payments |
| ❄️ | Seasonal / Editions releases |
| 🔮 | AI-driven prediction/simulation |
| 🤝 | Partnerships, collaborations |
| 🐉 | End-of-year / special features |

---

## 8. Source Handling

- Every topic should ideally reference a **source URL** (Shopify changelog, blog post, app documentation, etc.)
- In Notion, these appear as **bookmark blocks** (embedded link cards) — not inline hyperlinks
- Place the bookmark at the **end of the topic section**, after the body text
- If a video is relevant (e.g. official Shopify feature video), embed it as a video block

---

## 9. Reference Examples

| Edition | Notion URL | Topics |
|---|---|---|
| Smarter Triggers & Analytics | https://www.notion.so/3065f35473528086800cdb5e875b9d58 | Metafields in Analytics, Flow alerts, Klaviyo triggers, Sidekick, Flow email sender |
| Shopify Editions '26 | https://www.notion.so/2c65f354735280729e3cef74166274d7 | Winter '26 overview, Rollouts, Product Network, SimGym, Forms translation, Sidekick app builder, Flow from chat, Klarna EU, Klaviyo year-end |

---

## 10. Quick Checklist for Each Edition

- [ ] Page created in the correct database using the default template
- [ ] `Name` is punchy and topic-driven (not "week X")
- [ ] `Email subject` written as a teaser with 2–3 topic references
- [ ] `Summary` is 2–4 sentences, fills in the gallery card
- [ ] `Thumbnail` image uploaded
- [ ] Cover image at top of page
- [ ] "What's included this week:" TOC with emoji list
- [ ] Each topic has H3 heading + emoji + horizontal rule + body text
- [ ] Source bookmarks embedded (not hyperlinks) at end of each section
- [ ] No bullet points inside body text
- [ ] Tone is direct, first-person, practical — no fluff
