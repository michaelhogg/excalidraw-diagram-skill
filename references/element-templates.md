# Element Templates

**You're allowed, and indeed encouraged, to invent your own additional colors with additional meanings, if that'd be beneficial for the diagram that you're drawing.** For example, pink, purple or yellow might be useful in a particular diagram. The only important rule is to not use the three Meaning Colors in `color-palette.md` (green, red and strong orange) in other contexts with different meanings.

Copy-paste JSON templates for each Excalidraw element type. These templates contain the recommended fields, and not every field is mandatory. The other fields (such as `index`, `version`, `isDeleted` and `locked`) are omitted, because their default values are correct. See `json-schema.md` for a description of every property, including which fields are required and the default value of each optional field.

The `strokeColor` and `backgroundColor` values are placeholders — pull actual colors from `color-palette.md` based on the element's semantic purpose, or use an additional color as described above. The `fontFamily`, `lineHeight` and text `height` values are also placeholders, so replace them with numbers.

## Free-Floating Text (no container)
```json
{
  "type": "text",
  "id": "label1",
  "x": 100, "y": 100,
  "width": 200, "height": "<number: numberOfLines × fontSize × lineHeight (such as 1 × 20 × 1.2 = 24)>",
  "text": "Section Title",
  "fontSize": 20,
  "fontFamily": "<number: 3 (Cascadia, monospace) or 2 (Helvetica, sans-serif) or, in exceptional circumstances, one of the other FONT_FAMILY values in SKILL.md>",
  "lineHeight": "<number: 1.2 for fontFamily 3, or 1.15 for fontFamily 2 (see the Line Heights table in json-schema.md)>",
  "textAlign": "left",
  "verticalAlign": "top",
  "strokeColor": "<text color from palette>"
}
```

## Line (structural, not arrow)
```json
{
  "type": "line",
  "id": "line1",
  "x": 100, "y": 100,
  "width": 0, "height": 200,
  "strokeColor": "<structural line color from palette>",
  "strokeWidth": 2,
  "roughness": 0,
  "points": [[0, 0], [0, 200]]
}
```

## Small Marker Dot
```json
{
  "type": "ellipse",
  "id": "dot1",
  "x": 94, "y": 94,
  "width": 12, "height": 12,
  "strokeColor": "<marker dot color from palette>",
  "backgroundColor": "<marker dot color from palette>",
  "strokeWidth": 1,
  "roughness": 0
}
```

## Rectangle

The `boundElements` array must list the rectangle's text label, and also every arrow which is bound to the rectangle (whether the arrow starts or ends at the rectangle). Remove `boundElements` if nothing is bound to the rectangle.

```json
{
  "type": "rectangle",
  "id": "elem1",
  "x": 100, "y": 100, "width": 180, "height": 90,
  "strokeColor": "transparent",
  "backgroundColor": "<color from palette based on semantic purpose>",
  "strokeWidth": 2,
  "roughness": 0,
  "roundness": {"type": 3},
  "boundElements": [
    {"id": "text1", "type": "text"},
    {"id": "arrow1", "type": "arrow"}
  ]
}
```

## Text (centered in shape)

The text is centered in its container when `x = container.x + (container.width - width) / 2` and `y = container.y + (container.height - height) / 2`. The renderer doesn't center the text for you, so calculate `x` and `y` with these formulas. In this template, the container is the rectangle above, and the text is one line of `fontSize: 16`.

```json
{
  "type": "text",
  "id": "text1",
  "x": 130, "y": "<number: 100 + (90 - height) / 2>",
  "width": 120, "height": "<number: numberOfLines × fontSize × lineHeight (such as 1 × 16 × 1.2 = 19.2)>",
  "text": "Process",
  "fontSize": 16,
  "fontFamily": "<number: 3 (Cascadia, monospace) or 2 (Helvetica, sans-serif) or, in exceptional circumstances, one of the other FONT_FAMILY values in SKILL.md>",
  "lineHeight": "<number: 1.2 for fontFamily 3, or 1.15 for fontFamily 2 (see the Line Heights table in json-schema.md)>",
  "textAlign": "center",
  "verticalAlign": "middle",
  "containerId": "elem1",
  "strokeColor": "<text color from palette that is listed for the parent shape's color>"
}
```

## Arrow (straight)

This arrow connects the right side of `elem1` to the left side of `elem2`. Both `elem1` and `elem2` must list `{"id": "arrow1", "type": "arrow"}` in their `boundElements` arrays.

```json
{
  "type": "arrow",
  "id": "arrow1",
  "x": 282, "y": 145, "width": 118, "height": 0,
  "strokeColor": "<arrow color from palette: see Text and Line Colors>",
  "strokeWidth": 2,
  "roughness": 0,
  "points": [[0, 0], [118, 0]],
  "startBinding": {"elementId": "elem1", "mode": "orbit", "fixedPoint": [1, 0.5001]},
  "endBinding": {"elementId": "elem2", "mode": "orbit", "fixedPoint": [0, 0.5001]},
  "endArrowhead": "triangle"
}
```

For curves: use 3+ points in the `points` array, and set `"roundness": {"type": 2}`.

## Elbow Arrow

An elbow arrow only has horizontal and vertical segments. This arrow leaves the bottom side of `elem1` and enters the left side of `elem3` (a rectangle at `x: 400, y: 300, width: 180, height: 90`). Both `elem1` and `elem3` must list `{"id": "arrow2", "type": "arrow"}` in their `boundElements` arrays.

```json
{
  "type": "arrow",
  "id": "arrow2",
  "x": 190, "y": 192, "width": 208, "height": 153,
  "strokeColor": "<arrow color from palette: see Text and Line Colors>",
  "strokeWidth": 2,
  "roughness": 0,
  "points": [[0, 0], [0, 153], [208, 153]],
  "startBinding": {"elementId": "elem1", "mode": "orbit", "fixedPoint": [0.5001, 1]},
  "endBinding": {"elementId": "elem3", "mode": "orbit", "fixedPoint": [0, 0.5001]},
  "endArrowhead": "triangle",
  "elbowed": true
}
```

## Hand-Drawn Style

These templates use `roughness: 0`. If you set `roughness: 1` for a hand-drawn style, then also give each element a different `seed` number, because otherwise shapes of the same size are drawn identically.
