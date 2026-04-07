---
name: journal-cover
description: Creates a new Phill's Journal cover image and sets it as the Notion page cover. Use this skill whenever anyone suggests a new article title and needs a matching cover image, thumbnail, hero image, or blog header — even if they just say "make a cover for X", "create an image for this post", or "I need a header for this article". Generates the cover as a PNG entirely in code (no Figma required), uploads it to catbox.moe for hosting, and sets it as the cover photo on the relevant Notion page.
---

# Phill's Journal Cover Creator

Generates cover image variations for Phill's Journal entirely in code, uploads them to catbox.moe for hosting, and sets them as the Notion page cover.

## Cover design specs

| | |
|---|---|
| **Canvas** | 3840 x 2160 px |
| **Background** | Pre-made image — download from `https://raw.githubusercontent.com/ben-askphill/phills-journal/main/references/journal_cover_background_3840x2160.png` to `/tmp/journal_cover_bg.png` if not already cached. The generator composites text on top of this image — do **not** draw the circle/plus badge dynamically; it is already baked into the background. |
| **Font** | Inter Bold — download the variable font from `https://raw.githubusercontent.com/google/fonts/main/ofl/inter/Inter%5Bopsz%2Cwght%5D.ttf` to `/tmp/Inter.ttf` if not already cached. White `#FFFFFF`, up to 500px, -7% letter-spacing, 100% line-height. |
| **Text position** | Centred horizontally and vertically |

The cover is generated with a Python/Pillow script (see Step 2). It auto-scales the font down from 500px if a line would overflow the safe width.

## Workflow

### Step 1 — Determine the line break

The title text spans 1-2 lines separated by `\n`. Decide before generating:

- If the title contains ` & ` → line 2 starts with `& [rest]`
- If the title contains ` and ` (case-insensitive) → line 2 starts with `and [rest]`
- Multiple words, no conjunction → split roughly in half at the nearest word boundary before the midpoint, favouring a slightly shorter line 2
- Single word → no `\n`

**Examples:**

- `"AI Tools & Shopify"` → `"AI Tools\n& Shopify"`
- `"Building Better Product Pages"` → `"Building Better\nProduct Pages"`
- `"Speed"` → `"Speed"`

### Step 2 — Generate the PNG

Uses Python/Pillow to composite the title onto the background image. Download the Inter font if not already cached:

```bash
# Download Inter variable font (only if not cached)
[ -f /tmp/Inter.ttf ] || curl -sL -o /tmp/Inter.ttf \
  'https://raw.githubusercontent.com/google/fonts/main/ofl/inter/Inter%5Bopsz%2Cwght%5D.ttf'

# Download background image (only if not cached)
[ -f /tmp/journal_cover_bg.png ] || curl -sL -o /tmp/journal_cover_bg.png \
  'https://raw.githubusercontent.com/ben-askphill/phills-journal/main/references/journal_cover_background_3840x2160.png'
```

Then write and run the generator script:

```bash
cat > /tmp/gen_cover.py << 'SCRIPT'
import sys
from PIL import Image, ImageDraw, ImageFont

TITLE = sys.argv[1]
OUT = sys.argv[2]
BG_PATH = "/tmp/journal_cover_bg.png"

img = Image.open(BG_PATH).convert("RGBA")
draw = ImageDraw.Draw(img)
W, H = img.size

lines = TITLE.split("\n")

# Auto-scale font from 500px down until all lines fit within 80% canvas width
safe_w = int(W * 0.8)
size = 500
while size > 40:
    font = ImageFont.truetype("/tmp/Inter.ttf", size)
    # Set variable font axes: optical size 32, weight 700 (Bold)
    font.set_variation_by_axes([32, 700])
    if all(draw.textlength(line, font=font) <= safe_w for line in lines):
        break
    size -= 10

# Letter-spacing: -7% (applied by drawing char-by-char)
spacing_factor = -0.07

def draw_line_centered(y, text, font):
    # Measure total width with letter-spacing
    char_widths = [draw.textlength(c, font=font) for c in text]
    spacings = [w * spacing_factor for w in char_widths[:-1]] + [0]
    total_w = sum(w + s for w, s in zip(char_widths, spacings))
    x = (W - total_w) / 2
    for i, ch in enumerate(text):
        draw.text((x, y), ch, fill="#FFFFFF", font=font)
        x += char_widths[i] + spacings[i]

# Line height = 100% of font size
line_h = size
total_h = line_h * len(lines)
y_start = (H - total_h) / 2

for i, line in enumerate(lines):
    draw_line_centered(y_start + i * line_h, line, font)

img.convert("RGB").save(OUT)
print(f"Generated: {OUT}")
SCRIPT

python3 /tmp/gen_cover.py "TITLE_WITH_NEWLINE" /tmp/journal-cover.png
```

Replace `TITLE_WITH_NEWLINE` with the split title from Step 1, using a literal newline character in the string (e.g. `$'AI Tools\n& Shopify'`).

### Step 3 — Upload to catbox.moe

Upload the generated PNG to catbox.moe to get a publicly accessible URL:

```bash
curl -F "reqtype=fileupload" \
  -F "fileToUpload=@/tmp/journal-cover.png" \
  https://catbox.moe/user/api.php
```

The response body is the direct URL (e.g. `https://files.catbox.moe/abc123.png`). Save this as the hosted cover URL.

### Step 4 — Set as cover on the Notion page

Use the Notion MCP to update the page cover with the catbox.moe URL. The Notion page ID comes from the content writer's context (they will have provided a Notion page URL or ID):

```
notion-update-page(
  page_id: <notion_page_id>,
  cover: { type: "external", external: { url: "<catbox_url>" } }
)
```

Confirm back to the content writer with:

- The cover image URL
- Confirmation that the Notion page cover has been updated

## Notes

- Requires `Pillow` (`pip3 install Pillow` if not already installed)
- The generator handles font auto-scaling, so long titles still render correctly
- Anonymous catbox.moe uploads have no guaranteed retention policy — if a cover URL stops working, re-upload the image
- The background image and Inter variable font are downloaded once to `/tmp/` and reused across runs — do **not** regenerate or redraw the background
