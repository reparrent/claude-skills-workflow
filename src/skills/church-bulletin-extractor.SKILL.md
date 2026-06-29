---
name: church-bulletin-extractor
description: Extract choir music and liturgical color from church bulletins or vacation-coverage memos and generate a formatted PDF report. Use this skill whenever you receive a church bulletin PDF, a scanned memo PDF, or a plain-text memo listing songs for upcoming services. The skill extracts all hymns (Blue Hymnal and CTK Yellow Songbook entries), offertory hymns, and anthems, formats seasonal liturgical elements with special indentation, and includes the altar garment color. Perfect for weekly bulletin processing AND multi-date vacation memos (typically sent ~3x/yr for Christmas, Easter, and summer). Handles BH S-xxx settings, CTK xxs variants, "Andi Smith" composed items, and produces a professional formatted PDF report — one page per service date, with page numbers.
---

# Church Bulletin Choir Extraction

## Overview

Extract all choir music from a church bulletin or vacation-coverage memo and generate a formatted PDF report that includes:
- Service date and liturgical color (altar garment color)
- All hymns, anthems, and offertory music with page numbers
- Special formatting for seasonal/routine liturgical settings (indented to distinguish from primary hymns)
- Professional PDF output with color-coded rows, gridlines, page breaks per date, and page numbers

---

## Input Modes

This skill handles two distinct input types. Identify which you have before proceeding.

### Mode 1: Weekly Bulletin PDF (CTK website)
A standard weekly bulletin PDF downloaded from the CTK website. Contains one service date with full liturgical details including the altar color.

### Mode 2: Vacation Coverage Memo (scanned PDF or plain text)
A memo sent by the music director (typically Andi Smith, pianomama64@gmail.com) listing songs for multiple upcoming Sundays while she is away. Sent approximately 3 times per year — Advent/Christmas, Lent/Easter, and summer (Ordinary Time).

**Key differences from a bulletin:**
- Lists **multiple service dates** on one document (typically 3)
- Uses shorthand: `hymn ###` instead of `Blue Hymnal ###`, `CTK ##` instead of `CTK Yellow Songbook ##`
- **No explicit liturgical color stated** — infer from the season label (e.g., "Pentecost 8/Proper 11" → Green)
- Some liturgical items are marked **"Andi Smith"** instead of a page number — these are pieces she composed
- May arrive as a scanned printout PDF (use visual reading rather than text extraction)
- The document may be duplicated across pages (two identical pages from scanning) — use only one copy

---

## When to Use This Skill

- Every week you receive a church bulletin and need to identify what the choir will sing
- When Andi Smith sends a vacation coverage memo (Advent, Easter, or summer)
- Processing bulletins across different liturgical seasons
- Building a record of seasonal music for planning or reference
- Sharing a clean, formatted list with choir members

---

## Formatting Rules

### Primary Songs (standard hymns and anthems)
- Listed at normal indentation
- Example: `All Glory, Laud and Honor | BH | 154`

### Indented Items
The following are indented to show they are liturgical constants or composed/performed pieces, not congregational hymns:
- **BH S-### entries** (liturgical settings, e.g., Memorial Acclamation S-136)
- **CTK entries ending in "s"** (seasonal variants, e.g., 5s)
- **"Andi Smith" items** (Sanctus, Lord's Prayer, Fraction Anthem when listed as "Andi Smith") — include "Andi Smith" in the Page column as a reminder; leave Source blank

Example indented rows:
```
    Sanctus                  |        | Andi Smith
    Memorial Acclamation     | BH     | S-136
    Lord's Prayer            |        | Andi Smith
    Fraction Anthem          |        | Andi Smith
```

### Prelude
Include as a row with blank Source and Page — the prelude is noted in the memo but no song is specified.

### Liturgical Color (Memo Mode)
When the memo does not state a color, infer from the season label:
- Advent → Blue or Violet
- Christmas / Easter → White
- Pentecost (Ordinary Time, Proper ##) → Green
- Red letter feasts / Pentecost Sunday → Red

Display as: `Pentecost 8 / Proper 11  ·  Liturgical Color: Green`

---

## Output Format

A formatted PDF report containing:
- **Title:** "Church Songs for Service" (bulletin) or "Church Songs — [Name] Vacation Coverage" (memo)
- **Subtitle:** Source attribution (e.g., "Prepared from memo by Andi Smith (pianomama64@gmail.com)")
- **One page per service date** (PageBreak between dates)
- **Page numbers** centered at the bottom of each page
- 3-column table per date: Title | Source | Page
- Header row: blue background (#1F4E79), white text
- Alternating row colors: white / light blue (#EAF1FB)
- Gridlines for clarity
- 8.5×11 portrait, 1" margins (0.85" bottom to accommodate page number)

---

## Implementation Notes

### Mode 1 — Bulletin PDF Extraction (pdfplumber)

```python
1. Extract full text from PDF with pdfplumber
2. Find service date via regex (flexible comma handling)
3. Find liturgical color (color names near date)
4. Parse all hymn/anthem lines:
   - Split by "Hymnal" or "Songbook" keywords
   - Extract title, source abbreviation, page number
5. Separate seasonal items (S- or ending in s) → indent
6. Generate formatted PDF using reportlab
```

**Finding the service date:**
- Look in the first few lines
- Handles: `April 12, 2026` | `April, 12 2026` | `Apr 12, 2026`

**Finding the liturgical color:**
- Usually appears near the service date, parenthetically
- Common colors: Blue/Violet (Advent/Lent), White (Christmas/Easter), Red (Pentecost/Martyrs), Green (Ordinary Time)

**Catching all hymns:**
- Blue Hymnal: `Blue Hymnal ###` or `Blue Hymnal S-###`
- CTK: `CTK Yellow Songbook ###` or variants with `#s`
- Offertory: look for "Offertory Hymn" or "Offertory Anthem"
- Anthems: "Anthem", "Fraction Anthem"

### Mode 2 — Vacation Memo (visual read or plain text)

When the input is a scanned PDF, read it visually — do not attempt pdfplumber text extraction on scanned images. Parse the content directly from what you see.

Structure each service as a data list:
```python
# Each entry: (title, source, page, indented)
("Prelude", "", "", False),
("Open: Judge Eternal, Throned in Splendor", "BH", "596", False),
("Song of Praise: He is Exalted", "CTK", "56", False),
("Sequence: All That We Have", "CTK", "3", False),
("Offertory: Lord, Make Us Servants of Your Peace", "BH", "593", False),
("Sanctus", "", "Andi Smith", True),
("Memorial Acclamation", "BH", "S-136", True),
("Lord's Prayer", "", "Andi Smith", True),
("Fraction Anthem", "", "Andi Smith", True),
("Communion: Lord of All Hopefulness", "BH", "482", False),
("Communion: Holy Ground", "CTK", "62", False),
("Close: Lord, Dismiss Us With Thy Blessing", "BH", "344", False),
```

Shorthand decoding:
- `hymn ###` → Source: BH, Page: ###
- `CTK ##` → Source: CTK, Page: ##
- `S-###` → Source: BH, Page: S-###, indented: True
- `"Andi Smith"` → Source: "", Page: "Andi Smith", indented: True

### reportlab PDF Generation

```python
from reportlab.lib.pagesizes import letter
from reportlab.lib import colors
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.units import inch
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle, PageBreak

HEADER_BG = colors.HexColor("#1F4E79")
ROW_ALT   = colors.HexColor("#EAF1FB")
GRID      = colors.HexColor("#BBBBBB")

def add_page_number(canvas, doc):
    canvas.saveState()
    canvas.setFont("Helvetica", 8)
    canvas.setFillColor(colors.HexColor("#666666"))
    canvas.drawCentredString(letter[0] / 2, 0.5 * inch, f"Page {canvas.getPageNumber()}")
    canvas.restoreState()

# Insert PageBreak() between services (not after the last one)
# Pass onFirstPage=add_page_number, onLaterPages=add_page_number to doc.build()
```

---

## Edge Cases

- **Scanned memo has duplicate pages:** Both pages are identical scans — use only the first.
- **Date format variations:** Flexible regex; handle comma typos.
- **Missing color:** Infer from season label; if unknown, display date only.
- **Offertory not labeled:** Search for offertory position (after sequence, before sanctus).
- **Anthem without source:** Leave Source blank.
- **Multiple communion hymns:** List each as a separate row.

---

## Real-World Example — Summer Vacation Memo (June 2026)

Memo from Andi Smith covering July 19, July 26, and August 2, 2026 (Ordinary Time — Green).
Output file: `Andi_Smith_Vacation_Songs.pdf`

| Date | Season | Color |
|------|--------|-------|
| July 19, 2026 | Pentecost 8 / Proper 11 | Green |
| July 26, 2026 | Pentecost 9 / Proper 12 | Green |
| August 2, 2026 | Pentecost 10 / Proper 13 | Green |

The working Python script for this memo is saved at:
`outputs/generate_bulletin_report.py`
It serves as a reference implementation for Mode 2.

---

## Testing the Skill

**Mode 1 — Bulletin:**
1. Palm Sunday bulletin (Lent — Purple)
2. Easter Sunday bulletin (Easter — White)
3. Summer bulletin (Ordinary Time — Green)

**Mode 2 — Memo:**
1. Advent/Christmas memo (multiple dates, White)
2. Lent/Easter memo (multiple dates, Purple → White)
3. Summer memo (multiple dates, Green) ← proven June 2026

Verify:
- All dates parsed and on separate pages
- Page numbers present
- All hymns/anthems captured (no missing Offertory)
- S- entries, xxs entries, and "Andi Smith" items properly indented
- Liturgical color correct (inferred if not stated)
- PDF formatting clean and readable

---

## Changelog

<!-- Version notes: add new entries at the top of this list -->

- **V3 — June 28, 2026:** Added Mode 2 (vacation coverage memo / scanned PDF input). Documented "Andi Smith" composed items as indented category, page-break-per-date output, page numbering, liturgical color inference, and shorthand decoding. Reference implementation: `outputs/generate_bulletin_report.py`.
- **V2 ** Original weekly bulletin PDF extraction (Mode 1 only) Claude.ai/Claude Chat.  Scheduled Tas.
- **V1:** Original weekly bulletin PDF extraction, OpenAI, ChatGPT
