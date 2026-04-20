# Figma Render Scripts

All Figma writing happens through the `use_figma` MCP tool, which runs JavaScript inside the Figma Plugin API. The Plugin API cannot copy nodes across files, so the skill replicates the canonical template structure in the output file rather than cloning it directly.

This file gives you the patterns. Adapt each one to the actual step count, lane set, and content the skill gathered. Feel free to combine scripts into one `use_figma` call when it makes the flow shorter.

## Shared constants

Every script starts with these. They come from `canonical-template.md`.

```js
const LABEL_W = 228.71;
const COL_W = 280;
const COL_GAP = 9;
const ROW_GAP = 11;

const C = {
  mustard:      "#FFD568",
  labelCream:   "#F7E2AA",
  bodyCream:    "#FEF2D2",
  emotionsBg:   "#2B2828",
  emotionsLine: "#465E47",
  pain:         "#F7C2AA",
  opp:          "#C9F8D1",
  black:        "#000000",
  white:        "#FFFFFF",
  screenSlot:   "#E8E3D4",
  storySlot:    "#F0E6D2",
  tagBg:        "#000000",
  tagText:      "#FFFFFF",
};

const solid = (hex) => {
  const r = parseInt(hex.slice(1,3), 16) / 255;
  const g = parseInt(hex.slice(3,5), 16) / 255;
  const b = parseInt(hex.slice(5,7), 16) / 255;
  return [{ type: "SOLID", color: { r, g, b } }];
};
```

Load fonts before creating any text:

```js
await figma.loadFontAsync({ family: "Inter", style: "Medium" });
await figma.loadFontAsync({ family: "Inter", style: "Semi Bold" });
await figma.loadFontAsync({ family: "Inter", style: "Regular" });
```

## Script 1: Build the journey map frame

Takes the full lane list (locked plus per-project, already ordered) and the step count. Creates the map in `figma.currentPage`.

```js
// Inputs the skill fills in at call time:
// - FLOW_NAME: string, the name of this flow
// - N_COLS: integer, step count
// - LANES: array of { key, name, icon, h, labelBg, cellBg, role }
//   role is one of: "header" | "body" | "screens" | "storyboard" | "emotions"

const totalH = LANES.reduce((s, r) => s + r.h, 0) + ROW_GAP * (LANES.length - 1);
const totalW = LABEL_W + COL_GAP + N_COLS * COL_W + (N_COLS - 1) * COL_GAP;

const root = figma.createFrame();
root.name = `journey:${FLOW_NAME}`;
root.resize(totalW, totalH);
root.x = 0; root.y = 0;
root.fills = [];
root.clipsContent = false;

let y = 0;
for (const row of LANES) {
  const rowFrame = figma.createFrame();
  rowFrame.name = `row:${row.key}`;
  rowFrame.resize(totalW, row.h);
  rowFrame.x = 0; rowFrame.y = y;
  rowFrame.fills = [];
  root.appendChild(rowFrame);

  // Label cell
  const label = figma.createFrame();
  label.name = `label:${row.key}`;
  label.resize(LABEL_W, row.h);
  label.x = 0; label.y = 0;
  label.fills = solid(row.labelBg);
  rowFrame.appendChild(label);

  if (row.icon) {
    const icon = figma.createText();
    icon.fontName = { family: "Inter", style: "Regular" };
    icon.fontSize = 40;
    icon.characters = row.icon;
    icon.fills = solid(C.black);
    icon.x = LABEL_W / 2 - 22;
    icon.y = Math.max(20, row.h / 2 - 48);
    icon.resize(44, 50);
    icon.textAlignHorizontal = "CENTER";
    label.appendChild(icon);
  }

  const nameText = figma.createText();
  nameText.fontName = { family: "Inter", style: "Semi Bold" };
  nameText.fontSize = 22;
  nameText.characters = row.name;
  nameText.fills = solid(C.black);
  nameText.x = 16;
  nameText.y = Math.min(row.h - 40, row.h / 2 + 8);
  nameText.resize(LABEL_W - 32, 32);
  nameText.textAlignHorizontal = "CENTER";
  label.appendChild(nameText);

  // Data cells
  for (let i = 0; i < N_COLS; i++) {
    const cell = figma.createFrame();
    cell.name = `cell:${row.key}:${i + 1}`;
    cell.resize(COL_W, row.h);
    cell.x = LABEL_W + COL_GAP + i * (COL_W + COL_GAP);
    cell.y = 0;
    cell.fills = solid(row.cellBg);
    cell.clipsContent = true;
    rowFrame.appendChild(cell);

    if (row.role === "screens") {
      const slot = figma.createRectangle();
      slot.name = "screenshot-slot";
      slot.resize(COL_W - 20, row.h - 40);
      slot.x = 10; slot.y = 20;
      slot.fills = solid(C.screenSlot);
      slot.cornerRadius = 8;
      cell.appendChild(slot);
    } else if (row.role === "storyboard") {
      const slot = figma.createRectangle();
      slot.name = "illustration-slot";
      slot.resize(COL_W - 20, row.h - 30);
      slot.x = 10; slot.y = 15;
      slot.fills = solid(C.storySlot);
      slot.cornerRadius = 8;
      cell.appendChild(slot);
    } else if (row.role === "emotions") {
      const gridTop = 30;
      const gridHeight = 107;
      const grid = figma.createFrame();
      grid.name = "emotion-grid";
      grid.resize(COL_W, gridHeight);
      grid.x = 0; grid.y = gridTop;
      grid.fills = [];
      cell.appendChild(grid);
      for (let li = 0; li < 5; li++) {
        const line = figma.createLine();
        line.name = `line-${li + 1}`;
        line.resize(COL_W, 0);
        line.x = 0;
        line.y = (gridHeight / 4) * li;
        line.strokes = solid(C.emotionsLine);
        line.strokeWeight = 1;
        grid.appendChild(line);
      }
    }
  }

  y += row.h + ROW_GAP;
}

figma.currentPage.appendChild(root);
figma.viewport.scrollAndZoomIntoView([root]);
return { rootId: root.id, width: totalW, height: totalH };
```

## Script 2: Populate text cells

After the frame exists, set cell content. Pass in a `CONTENT` object keyed by lane, then by column index.

```js
// CONTENT example:
// {
//   steps: { 1: { num: 1, title: "Lands on dashboard" }, 2: {...} },
//   userActions: { 1: "Logs in. Lands on Background Checks dashboard.", 2: "..." },
//   painPoints: { 1: "User cannot find profile menu.", 2: "..." },
//   ...
// }

const setBodyText = async (cellName, text) => {
  const cell = figma.currentPage.findOne(n => n.name === cellName);
  if (!cell) return;
  const t = figma.createText();
  t.fontName = { family: "Inter", style: "Medium" };
  t.fontSize = 16;
  t.characters = text;
  t.fills = solid(C.black);
  t.x = 16; t.y = 14;
  t.resize(COL_W - 32, cell.height - 28);
  cell.appendChild(t);
};

const setStepHeader = async (cellName, num, title) => {
  const cell = figma.currentPage.findOne(n => n.name === cellName);
  if (!cell) return;
  const numText = figma.createText();
  numText.fontName = { family: "Inter", style: "Semi Bold" };
  numText.fontSize = 22;
  numText.characters = `${num}.`;
  numText.fills = solid(C.black);
  numText.x = 16; numText.y = 20;
  numText.resize(40, 28);
  cell.appendChild(numText);

  const titleText = figma.createText();
  titleText.fontName = { family: "Inter", style: "Semi Bold" };
  titleText.fontSize = 22;
  titleText.characters = title;
  titleText.fills = solid(C.black);
  titleText.x = 52; titleText.y = 20;
  titleText.resize(COL_W - 68, cell.height - 40);
  cell.appendChild(titleText);
};

// Example usage:
for (const col of Object.keys(CONTENT.steps)) {
  const s = CONTENT.steps[col];
  await setStepHeader(`cell:steps:${col}`, s.num, s.title);
}
for (const lane of ["userActions","painPoints","opportunities", ...PER_PROJECT_LANE_KEYS]) {
  const col = CONTENT[lane] || {};
  for (const idx of Object.keys(col)) {
    await setBodyText(`cell:${lane}:${idx}`, col[idx]);
  }
}
```

## Script 3: Upload screenshots

Screenshots come in as base64 or a URL. `figma.createImage` accepts a `Uint8Array`. When you have a URL, fetch the bytes first. When you have base64, decode.

```js
// IMAGE_BYTES: Uint8Array of the PNG
// cellName: "cell:screens:<col>"

const slotInCell = (cell) => cell.findOne(n => n.name === "screenshot-slot");

const uploadToSlot = async (cellName, imageBytes) => {
  const cell = figma.currentPage.findOne(n => n.name === cellName);
  if (!cell) return;
  const image = figma.createImage(imageBytes);
  const slot = slotInCell(cell);
  if (!slot) return;
  slot.fills = [{ type: "IMAGE", scaleMode: "FIT", imageHash: image.hash }];
};
```

## Script 4: Place emotion markers

```js
// EMOTIONS example: { 1: 3, 2: 2, 3: 4, ... } where value 1..5 matches canonical-template.md scale

const EMOJI = { 1: "😠", 2: "🙁", 3: "😐", 4: "🙂", 5: "😄" };
const Y_BY_LEVEL = { 5: 8, 4: 34.75, 3: 61.5, 2: 88.25, 1: 115 };

for (const [col, level] of Object.entries(EMOTIONS)) {
  const cell = figma.currentPage.findOne(n => n.name === `cell:emotions:${col}`);
  if (!cell) continue;
  const marker = figma.createText();
  marker.name = "emotion-marker";
  marker.fontName = { family: "Inter", style: "Regular" };
  marker.fontSize = 32;
  marker.characters = EMOJI[level];
  marker.fills = solid(C.white);
  marker.x = COL_W / 2 - 20;
  marker.y = Y_BY_LEVEL[level];
  marker.resize(40, 45);
  marker.textAlignHorizontal = "CENTER";
  cell.appendChild(marker);
}
```

## Script 5: Add "Needs clarification" tag pills

```js
// TAGS example: [{ lane: "userActions", col: 3, text: "Needs clarification" }, ...]

for (const tag of TAGS) {
  const cell = figma.currentPage.findOne(n => n.name === `cell:${tag.lane}:${tag.col}`);
  if (!cell) continue;
  const pill = figma.createFrame();
  pill.name = "tag:needs-clarification";
  pill.resize(217.5, 28.2);
  pill.x = 31.2; pill.y = 14;
  pill.cornerRadius = 30;
  pill.fills = solid(C.tagBg);
  cell.appendChild(pill);

  const t = figma.createText();
  t.fontName = { family: "Inter", style: "Medium" };
  t.fontSize = 17;
  t.characters = tag.text;
  t.fills = solid(C.tagText);
  t.textAlignHorizontal = "CENTER";
  t.resize(217.5, 22);
  t.x = 0; t.y = 3;
  pill.appendChild(t);
}
```

## Script 6: Upload storyboard illustration

Same as Script 3 but targets `illustration-slot` and uses `scaleMode: "FILL"` for a painterly image.

```js
const uploadStoryboard = async (cellName, imageBytes) => {
  const cell = figma.currentPage.findOne(n => n.name === cellName);
  if (!cell) return;
  const slot = cell.findOne(n => n.name === "illustration-slot");
  if (!slot) return;
  const image = figma.createImage(imageBytes);
  slot.fills = [{ type: "IMAGE", scaleMode: "FILL", imageHash: image.hash }];
};
```

## Creating new pages for multi-flow output

When the user chose "one file, each flow as a page":

```js
const page = figma.createPage();
page.name = FLOW_NAME;
await figma.setCurrentPageAsync(page);
// Then run Script 1 and friends.
```

Note: do not set `figma.currentPage` directly. Use `setCurrentPageAsync` (the Plugin API requires this).

## Error handling

If any `use_figma` call throws, stop and report the full error to the user. Common traps:

- Font not loaded before creating text. Always `loadFontAsync` first.
- `cell.findOne` returns `null` when the name is wrong. Check the lane key spelling.
- `LINE` nodes do not have `cornerRadius`. Do not set it.
- Setting `figma.currentPage = x` throws. Use `await figma.setCurrentPageAsync(x)`.
