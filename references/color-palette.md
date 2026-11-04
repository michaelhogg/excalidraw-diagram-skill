# Color Palette

**This is the single source of truth for the standard colors.** To change the standard colors in your diagrams, edit this file. Additional colors can also be invented for a particular diagram, as described in `element-templates.md`.

Each entry defines one color. Use that color for whichever element needs it (a shape, a line, an arrow, a marker or text).

---

## Meaning Colors

These colors communicate a specific meaning, so the diagram's reader can understand them at a glance.

| Color         | Hex       | Text on this color | Meaning                                                                                                           |
|---------------|-----------|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Green         | `#15803d` | `#ffffff`          | Positive, good, acceptable, recommended, preferred, successful, safe, and other such positive meanings            |
| Red           | `#d92d20` | `#ffffff`          | Negative, bad, unacceptable, not recommended, not preferred, failure, dangerous, and other such negative meanings |
| Strong orange | `#f97316` | `#000000`          | Warning, caution, potentially risky, and other similar meanings                                                   |

**Rules:**

- **Green must always be used to indicate a positive meaning, and must not be used for anything else.**
- **Red must always be used to indicate a negative meaning, and must not be used for anything else.**
- **Strong orange can be used to indicate a warning or a similar meaning, and must not be used for anything else.** Don't use it for small text on a white background, because the contrast is too low. Instead, use a strong orange shape which contains `#000000` text.

---

## Neutral Colors

These colors have no special meaning. Use them for anything which doesn't need a particular color.

| Name                        | Hex       | Text on this color |
|-----------------------------|-----------|--------------------|
| Primary/Neutral (dark blue) | `#1b3a6b` | `#ffffff`          |
| Secondary (medium blue)     | `#2563eb` | `#ffffff`          |
| Light blue                  | `#7fb0f5` | `#000000`          |
| Light gray                  | `#d1d5db` | `#000000`          |
| White                       | `#ffffff` | `#000000`          |

---

## Text and Line Colors

Use these colors for text, lines, arrows and marker dots: black on light colors, and white on dark colors.

| Background                                                     | Hex       |
|----------------------------------------------------------------|-----------|
| Light colors (light blue, light gray, white and strong orange) | `#000000` |
| Dark colors (dark blue, medium blue, green and red)            | `#ffffff` |

Other colors can be used if they're beneficial. For example, use red for a line which has a negative meaning, and for the text which labels that line. Place colored text on a white background, because it has the strongest contrast there.

---

## Evidence Artifact Colors

Use these colors for code snippets, data examples and other concrete evidence inside technical diagrams. Place evidence artifacts on a white or light gray background, so that they contrast with their surroundings.

| Element            | Hex       |
|--------------------|-----------|
| Background         | `#0f172a` |
| Default code text  | `#f3f4f6` |
| Code keywords      | `#7dd3fc` |
| Code strings       | `#c4b5fd` |
| Code comments      | `#9ca3af` |
| JSON and data text | `#7dd3fc` |

Don't use green or red for syntax coloring, because those colors are reserved for the meanings which are defined above.

---

## Background

| Property          | Hex       |
|-------------------|-----------|
| Canvas background | `#ffffff` |

---

## Nested Rectangles

Any color scheme can be used for nested rectangles, as long as each rectangle strongly contrasts with its parent and its child, and the Meaning Colors aren't used for other meanings. For example, you could alternate between dark blue and light gray.
