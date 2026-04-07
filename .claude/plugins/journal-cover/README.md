# journal-cover

Creates Phill's Journal cover image variations in Figma.

## What it does

Given a new article title from the Shopify content writer, this skill:
1. Duplicates the master cover template in Figma (Brand Guidelines file, Journal page)
2. Updates the headline text with intelligent two-line splitting
3. Places the new cover below all existing covers on the Journal page

## Setup

Requires `FIGMA_ACCESS_TOKEN` in your environment (already configured via `.mcp.json`).

## Install

```
/plugin install journal-cover@ask-phill-tools
```

## Usage

Just tell the agent the new title:

> "Create a Journal cover for 'AI Tools & Shopify'"
> "Make a cover image for the new post about headless commerce"
