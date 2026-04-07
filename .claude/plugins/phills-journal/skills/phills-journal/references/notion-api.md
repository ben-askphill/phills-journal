# Notion API Reference for Phill's Journal

All calls use `curl` with these headers:

```bash
-H "Authorization: Bearer $NOTION_API_KEY"
-H "Notion-Version: 2022-06-28"
-H "Content-Type: application/json"
```

## Database ID

The Phill's Journal database ID:

```
DATABASE_ID="2185f354-7352-8009-a878-de83966a3534"
```

---

## Step 1: Create the page

```bash
curl -X POST "https://api.notion.com/v1/pages" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2022-06-28" \
  -H "Content-Type: application/json" \
  -d '{
    "parent": { "database_id": "DATABASE_ID" },
    "properties": {
      "Name": {
        "title": [{ "text": { "content": "Edition Title Here" } }]
      },
      "Email subject": {
        "rich_text": [{ "text": { "content": "Subject line here" } }]
      },
      "Summary": {
        "rich_text": [{ "text": { "content": "2-4 sentence summary here" } }]
      }
    }
  }'
```

The response includes `"id": "page-id-with-dashes"`. Save this for subsequent calls.

---

## Step 2: Append content blocks

```bash
curl -X PATCH "https://api.notion.com/v1/blocks/PAGE_ID/children" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2022-06-28" \
  -H "Content-Type: application/json" \
  -d '{
    "children": [BLOCKS_ARRAY]
  }'
```

**Note:** Notion limits to 100 blocks per request. If you have more than 100 blocks, split into multiple PATCH calls.

### Block types used in journal editions

#### Image block (cover at top of page)

```json
{
  "type": "image",
  "image": {
    "type": "external",
    "external": { "url": "https://cdn.shopify.com/..." }
  }
}
```

#### Heading 3

```json
{
  "type": "heading_3",
  "heading_3": {
    "rich_text": [{ "type": "text", "text": { "content": "📊 Topic Title" } }]
  }
}
```

#### Divider

```json
{
  "type": "divider",
  "divider": {}
}
```

#### Paragraph

```json
{
  "type": "paragraph",
  "paragraph": {
    "rich_text": [{ "type": "text", "text": { "content": "Body text here..." } }]
  }
}
```

For text with links inline:

```json
{
  "type": "paragraph",
  "paragraph": {
    "rich_text": [
      { "type": "text", "text": { "content": "Check out " } },
      {
        "type": "text",
        "text": { "content": "this feature", "link": { "url": "https://..." } }
      },
      { "type": "text", "text": { "content": " for more details." } }
    ]
  }
}
```

#### Bookmark

Use this for source URLs — not inline hyperlinks:

```json
{
  "type": "bookmark",
  "bookmark": {
    "url": "https://shopify.dev/changelog/..."
  }
}
```

---

## Step 3: Set cover, icon, thumbnail, and author

This is a separate PATCH to the page (not blocks):

```bash
curl -X PATCH "https://api.notion.com/v1/pages/PAGE_ID" \
  -H "Authorization: Bearer $NOTION_API_KEY" \
  -H "Notion-Version: 2022-06-28" \
  -H "Content-Type: application/json" \
  -d '{
    "cover": {
      "type": "external",
      "external": { "url": "https://cdn.shopify.com/..." }
    },
    "icon": {
      "type": "custom_emoji",
      "custom_emoji": { "name": "journal-icon" }
    },
    "properties": {
      "Thumbnail": {
        "files": [{
          "type": "external",
          "name": "thumbnail.png",
          "external": { "url": "https://cdn.shopify.com/..." }
        }]
      },
      "Author(s)": {
        "people": [
          { "id": "6d45bbd2-778d-4dc5-a695-0a390290b201" },
          { "id": "2b7d872b-594c-817c-b416-000200f23fe1" }
        ]
      }
    }
  }'
```

**Important:** The Author property must be set in this separate PATCH after page creation — the Notion connection app shows as creator by default, and you need to explicitly override it.

---

## Building the page URL

Strip dashes from the page ID to form the URL:

```
https://www.notion.so/<page-id-without-dashes>
```

Example: page ID `3065f354-7352-8086-800c-db5e875b9d58` becomes `https://www.notion.so/3065f35473528086800cdb5e875b9d58`

---

## Typical block sequence for an edition

```json
[
  // 1. Cover image (full-width)
  { "type": "image", "image": { "type": "external", "external": { "url": "COVER_URL" } } },

  // 2. TOC heading
  { "type": "heading_3", "heading_3": { "rich_text": [{ "type": "text", "text": { "content": "What's included this week:" } }] } },
  { "type": "divider", "divider": {} },

  // 3. TOC lines (one paragraph per topic)
  { "type": "paragraph", "paragraph": { "rich_text": [{ "type": "text", "text": { "content": "📊 Analytics Deep Dive" } }] } },
  { "type": "paragraph", "paragraph": { "rich_text": [{ "type": "text", "text": { "content": "🤖 AI-Powered Recommendations" } }] } },

  // 4. Topic sections
  { "type": "heading_3", "heading_3": { "rich_text": [{ "type": "text", "text": { "content": "📊 Analytics Deep Dive" } }] } },
  { "type": "divider", "divider": {} },
  { "type": "paragraph", "paragraph": { "rich_text": [{ "type": "text", "text": { "content": "Body text for this topic..." } }] } },
  { "type": "bookmark", "bookmark": { "url": "https://source-url.com" } },

  { "type": "heading_3", "heading_3": { "rich_text": [{ "type": "text", "text": { "content": "🤖 AI-Powered Recommendations" } }] } },
  { "type": "divider", "divider": {} },
  { "type": "paragraph", "paragraph": { "rich_text": [{ "type": "text", "text": { "content": "Body text for this topic..." } }] } },
  { "type": "bookmark", "bookmark": { "url": "https://source-url.com" } }
]
```
