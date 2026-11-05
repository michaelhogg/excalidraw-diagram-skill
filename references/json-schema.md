# Excalidraw JSON Schema

This reference describes the schema which the current Excalidraw app writes, and which the bundled `@excalidraw/cli` renderer reads.

## File Structure

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [...],
  "appState": {
    "viewBackgroundColor": "#ffffff",
    "gridSize": 20
  },
  "files": {}
}
```

* `elements` are drawn in array order: later elements are drawn on top of earlier elements.
* `files` maps each `fileId` (which an `image` element references) to an object which holds the image data (`mimeType`, `id`, `dataURL`, `created`). It's an empty object when the diagram contains no images.

## Element Types

| Type | Use For |
|------|---------|
| `rectangle` | Processes, actions, components |
| `ellipse` | Entry/exit points, external systems |
| `diamond` | Decisions, conditionals |
| `arrow` | Connections between shapes |
| `text` | Labels inside shapes, or free-floating text |
| `line` | Non-arrow structural lines (which can't bind to shapes) |
| `frame` | Grouping containers (has an extra `name` property) |
| `image` | Embedded images (has extra `fileId`, `status`, `scale` and `crop` properties) |

## Common Properties

All elements share these:

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier |
| `type` | string | Element type |
| `x`, `y` | number | Position in pixels of the top-left corner of the element's bounding box |
| `width`, `height` | number | Size in pixels |
| `angle` | number | Rotation in radians (normally `0`) |
| `strokeColor` | string | Border color (hex or `"transparent"`) |
| `backgroundColor` | string | Fill color (hex or `"transparent"`) |
| `fillStyle` | string | `"solid"`, `"hachure"`, `"cross-hatch"` or `"zigzag"` |
| `strokeWidth` | number | `1` (thin), `2` (medium) or `4` (bold) |
| `strokeStyle` | string | `"solid"`, `"dashed"` or `"dotted"` |
| `roughness` | number | `0` (smooth), `1` (default) or `2` (rough) |
| `opacity` | number | 0-100 |
| `roundness` | object or null | Corner rounding (see "Roundness" below). `null` means sharp corners. |
| `seed` | number | Random seed for roughness |
| `version` | number | Edit counter (use `1` when generating) |
| `versionNonce` | number | Random number which changes on every edit |
| `index` | string or null | Fractional index for z-ordering (such as `"a0"`, `"a1"`). Use `null` when generating, and Excalidraw will assign indices based on array order. |
| `isDeleted` | boolean | Always `false` when generating |
| `groupIds` | string[] | IDs of the groups which the element belongs to (innermost group first) |
| `frameId` | string or null | ID of the `frame` which contains the element |
| `boundElements` | array | Elements which are bound to this element, as `{"id": "...", "type": "text"}` or `{"id": "...", "type": "arrow"}` objects. Use `[]` when there are none. |
| `updated` | number | Timestamp (in milliseconds) of the last edit |
| `link` | string or null | Hyperlink URL |
| `locked` | boolean | Whether the element is locked |

## Text-Specific Properties

| Property | Description |
|----------|-------------|
| `text` | The displayed text. For text inside a container, this includes the line breaks (`\n`) which Excalidraw inserts when it wraps the text to fit the container. |
| `originalText` | The text before wrapping. Same as `text` when the text doesn't need to wrap. |
| `fontSize` | Size in pixels (16-20 recommended) |
| `fontFamily` | 3 (`Cascadia`, monospace) or 2 (`Helvetica`, sans-serif) or, in exceptional circumstances, one of the other `FONT_FAMILY` values (see `SKILL.md`) |
| `lineHeight` | Line-height multiplier, which depends on `fontFamily` (see "Line Heights" below) |
| `textAlign` | `"left"`, `"center"` or `"right"` |
| `verticalAlign` | `"top"`, `"middle"` or `"bottom"` |
| `containerId` | ID of the parent shape (or arrow), or `null` for free-floating text |
| `autoResize` | `true` when the text box's width automatically fits the text. Use `true` when generating. |

### Line Heights

| `fontFamily` | Font | `lineHeight` |
|---|---|---|
| 2 | `Helvetica` | `1.15` |
| 3 | `Cascadia` | `1.2` |
| 5, 6, 8 | `Excalifont`, `Nunito`, `Comic Shanns` | `1.25` |

A text element's `height` must equal `numberOfLines × fontSize × lineHeight`. For example, two lines of `Cascadia` at `fontSize: 16` gives a `height` of `2 × 16 × 1.2 = 38.4`.

### Text Inside a Container

To place text inside a shape, the binding must be declared in both directions:

* The text element's `containerId` is the shape's `id`.
* The shape's `boundElements` includes `{"id": "<text id>", "type": "text"}`.

When `textAlign` is `"center"` and `verticalAlign` is `"middle"`, the text is centered in the container:

* `text.x = container.x + (container.width - text.width) / 2`
* `text.y = container.y + (container.height - text.height) / 2`

## Arrow-Specific Properties

| Property | Description |
|----------|-------------|
| `points` | Array of `[x, y]` coordinates, relative to the arrow's `x` and `y`. The first point is always `[0, 0]`. |
| `startBinding` | Connection to the start shape, or `null` (see "Binding Format" below) |
| `endBinding` | Connection to the end shape, or `null` |
| `startArrowhead` | Arrowhead at the start point (see "Arrowheads" below) |
| `endArrowhead` | Arrowhead at the end point |
| `elbowed` | `true` for an elbow arrow (which only has horizontal and vertical segments), or `false` for a straight/curved arrow |
| `fixedSegments` | Elbow arrows only: segments which the user has manually moved, or `null` |
| `startIsSpecial`, `endIsSpecial` | Elbow arrows only: internal routing flags. Use `null` when generating. |

* An arrow's `width` and `height` must equal the size of the bounding box of its `points`.
* An arrow with 2 points is straight. For a curved arrow, use 3+ points and `"roundness": {"type": 2}`. For an elbow arrow, use `"elbowed": true`, `"roundness": null` and only horizontal and vertical segments.
* An arrow can have a text label: the text's `containerId` is the arrow's `id`, and the arrow's `boundElements` includes the text.

### Arrowheads

`null` (no arrowhead), `"arrow"`, `"bar"`, `"triangle"`, `"triangle_outline"`, `"circle"`, `"circle_outline"`, `"diamond"`, `"diamond_outline"`, or one of the entity-relationship cardinality markers: `"cardinality_one"`, `"cardinality_many"`, `"cardinality_one_or_many"`, `"cardinality_exactly_one"`, `"cardinality_zero_or_one"`, `"cardinality_zero_or_many"`.

The legacy values `"dot"` and `"crowfoot_*"` are automatically converted to `"circle"` and `"cardinality_*"`, so don't use them.

## Line-Specific Properties

A `line` has the same `points`, `startArrowhead` and `endArrowhead` properties as an arrow, plus:

| Property | Description |
|----------|-------------|
| `polygon` | `true` when the line is a closed shape (its first and last points are the same), otherwise `false` |

A line can't bind to shapes: its `startBinding` and `endBinding` are always `null`.

## Binding Format

```json
{
  "elementId": "shapeId",
  "mode": "orbit",
  "fixedPoint": [1, 0.5001]
}
```

| Property | Description |
|----------|-------------|
| `elementId` | ID of the shape which the arrow's endpoint is bound to |
| `mode` | `"orbit"` when the endpoint sits just outside the shape's outline (this is the normal case), or `"inside"` when the endpoint is inside the shape |
| `fixedPoint` | `[fx, fy]` position, relative to the shape's bounding box, which the arrow points at. `[0, 0]` is the top-left corner and `[1, 1]` is the bottom-right corner. |

Useful `fixedPoint` values:

| Side of the shape | `fixedPoint` |
|---|---|
| Left | `[0, 0.5001]` |
| Right | `[1, 0.5001]` |
| Top | `[0.5001, 0]` |
| Bottom | `[0.5001, 1]` |

Excalidraw uses `0.5001` (instead of `0.5`) for the middle of a side.

The binding must be declared in both directions: the bound shape's `boundElements` must include `{"id": "<arrow id>", "type": "arrow"}`.

The legacy binding format (`{"elementId": "...", "focus": 0, "gap": 2}`) is still accepted, and is automatically converted to the format above, but don't use it when generating new diagrams.

## Roundness

| Value | Use For |
|-------|---------|
| `null` | Sharp corners, straight arrows/lines, and elbow arrows |
| `{"type": 3}` | Rounded corners on a `rectangle` (the radius adapts to the rectangle's size) |
| `{"type": 2}` | Rounded corners on a `diamond`, or curves on an `arrow` or `line` |
