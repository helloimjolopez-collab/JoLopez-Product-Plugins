# Canonical Template Reference

The MB User Flows Mapper skill replicates this template as the starting point for every journey map.

## Figma file

- File key: `tr2RVfYUO41bZ3s2kZWReC`
- URL: https://www.figma.com/design/tr2RVfYUO41bZ3s2kZWReC
- Plan: MB Product, MB Design Team (`team::940693506887137461`)

## Node structure

- Wrapper frame (title and subtitle plus the template): node `1:2`, name `Canonical Journey Map Template`.
- Template frame (the thing to replicate): node `1:5`, name `journey-map-template`.

The template frame contains 7 rows (locked lanes) and 4 columns (1 label column plus 3 example step columns).

## Exact dimensions

| Measurement | Value |
|---|---|
| Label column width | 228.71 |
| Step column width | 280 |
| Column gap | 9 |
| Row gap | 11 |
| Total template width (3 cols) | 1095.71 |
| Total template height | 1535 |

## Row specs

| Key | Name | Height | Label bg | Cell bg | Role |
|---|---|---|---|---|---|
| `steps` | Steps | 175 | #FFD568 | #FFD568 | header (numbered step plus title) |
| `userActions` | User Actions | 205 | #F7E2AA | #FEF2D2 | body text |
| `screens` | Screens | 307 | #F7E2AA | #FEF2D2 | screenshot slot (rect, corner radius 8, #E8E3D4) |
| `storyboard` | Storyboard | 262 | #F7E2AA | #FEF2D2 | illustration slot (rect, corner radius 8, #F0E6D2) |
| `emotions` | Emotions | 170 | #F7E2AA | #2B2828 | 5-line grid plus emoji marker |
| `painPoints` | Pain Points | 175 | #F7C2AA | #F7C2AA | body text |
| `opportunities` | Opportunities | 175 | #C9F8D1 | #C9F8D1 | body text |

## Palette (exact hex)

- Mustard header: `#FFD568`
- Label column cream: `#F7E2AA`
- Body cell cream: `#FEF2D2`
- Emotions background: `#2B2828`
- Emotions grid lines: `#465E47` at 1px weight
- Pain Points: `#F7C2AA`
- Opportunities: `#C9F8D1`
- Screenshot slot: `#E8E3D4`
- Illustration slot: `#F0E6D2`
- Tag pill (Needs clarification): bg `#000000`, text `#FFFFFF`, corner radius ~30 (pill shape)

## Typography

- Font family: Inter.
- Header text (step titles, lane names): Inter Semi Bold, 22pt, black.
- Body text: Inter Medium, 16pt, black.
- Tag pill text: Inter Medium, about 17pt, white.

## Node naming conventions inside the template

Use these exact names when generating nodes. The render scripts rely on them.

- Row frame: `row:<key>` (for example `row:userActions`).
- Label cell: `label:<key>` (for example `label:steps`).
- Data cell: `cell:<key>:<colIndex>` where `colIndex` is 1-indexed (for example `cell:emotions:3`).
- Screenshot rectangle inside a Screens cell: `screenshot-slot`.
- Illustration rectangle inside a Storyboard cell: `illustration-slot`.
- Emotion grid container: `emotion-grid`. Inner lines: `line-1` through `line-5`.
- Emotion marker: `emotion-marker`.

## Emotion marker Y-offsets

The emotion grid sits at `y=30` inside the cell, with height 107. The 5 horizontal lines are at `y=0, 26.75, 53.5, 80.25, 107` relative to the grid top.

Marker Y inside the cell (absolute, top-left origin):

| Level | Emoji | Marker Y |
|---|---|---|
| 5 (delighted) | 😄 | 8 |
| 4 (happy) | 🙂 | 34.75 |
| 3 (neutral) | 😐 | 61.5 |
| 2 (unhappy) | 🙁 | 88.25 |
| 1 (frustrated) | 😠 | 115 |

Marker size: 40 by 45. Font size 32. Horizontal position `COL_W / 2 - 20` (centered).

## How to extend columns beyond 3

For each row, do this for every step column beyond the third:

1. Duplicate the last existing data cell (`cell:<key>:<lastIndex>`).
2. Shift the duplicate's X by `COL_W + COL_GAP` (which is 289).
3. Rename to `cell:<key>:<newIndex>`.
4. Clear content, then populate with new data.

Then resize the row frame width to `LABEL_W + COL_GAP + N * COL_W + (N - 1) * COL_GAP`.
