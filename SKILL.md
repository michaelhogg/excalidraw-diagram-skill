---
name: excalidraw-diagram
description: Create Excalidraw diagram JSON files that make visual arguments. Use when the user wants to visualize workflows, architectures, or concepts.
---

# Excalidraw Diagram Creator

Generate `.excalidraw` JSON files that **argue visually**, not just display information.

**Setup:** If the user asks you to set up this skill (renderer, dependencies, etc.), see `README.md` for instructions.

## Customization

**The standard colors live in one file:** `references/color-palette.md`. Read it before generating any diagram and use it as the source of truth for the standard color choices — shape colors, line colors, text colors and evidence artifact colors.

To change the colors in the diagrams that this skill produces, edit `color-palette.md`. Everything else in this file is universal design methodology and Excalidraw best practices.

## Important guidelines for structure and styling

IMPORTANT: These guidelines override any other guidelines in this `SKILL.md` file.

* **Text**
  * **Font family (`fontFamily` parameter)**
    * Use a monospace font for technical terms (such as file names, function names, database table names, etc). `Cascadia` (`fontFamily: 3`) is good.
    * Use a sans-serif font for everything else. `Helvetica` (`fontFamily: 2`) is good.
    * In exceptional circumstances, you can use one of the other font families. If you need to see the full reference of all available font families, see `FONT_FAMILY` at https://github.com/excalidraw/excalidraw/blob/v0.18.1/packages/excalidraw/constants.ts#L130-L140
  * **"Fake bold" text styling**
    * Excalidraw doesn't support bold text. However, you can easily create a "fake bold" effect, by duplicating the `text` element and then shifting the duplicated element horizontally by a tiny offset (such as `0.5` units).
* **Lines and arrows**
  * When drawing a line with an arrowhead at the end, always use `endArrowhead: "triangle"`.
* **Shapes and borders**
  * **Don't leave content floating in empty unbounded space**
    * Always endeavor to enclose content within a suitable "container" rectangle.
  * **Use nested rectangles as much as possible (where relevant)**
    * These can represent hierarchies of entities (grandparent, parent, child, grandchild, etc). For example:
      * Filesystem structures (root directory, child directory, file within child directory, etc), especially where there are fewer files and folders, and where the diagram needs to show the contents of the files. If the diagram needs to show a lot of files and folders within a small space, the Tree pattern would be better.
      * Databases (database, table within the database, column within the table, etc).
  * **Never draw a border for any shape (rectangle, rounded rectangle, ellipse, etc)**
    * Instead, ensure that the shape's color has a strong contrast against the surrounding color.
      * For example, a light gray rectangle, surrounded by dark blue outside it.
    * This is particularly important for nested rectangles, where each level of nesting must use a color which strongly contrasts with both the previous (higher) level and the next (deeper) level.
      * For example, you could use two alternating colors for a set of nested rectangles:
        * Level 1 - dark blue
        * Level 2 - light gray
        * Level 3 - dark blue
        * Level 4 - light gray
        * Level 5 - dark blue
        * Level 6 - light gray
      * Alternatively, you could cycle through a set of three colors:
        * Level 1 - dark blue
        * Level 2 - light blue
        * Level 3 - white
        * Level 4 - dark blue
        * Level 5 - light blue
        * Level 6 - white

---

## Core Philosophy

**Diagrams should ARGUE, not DISPLAY.**

A diagram isn't formatted text. It's a visual argument that shows relationships, causality, and flow that words alone can't express. The shape should BE the meaning.

**The Isomorphism Test**: If you removed all text, would the structure alone communicate the concept? If not, redesign.

**The Education Test**: Could someone learn something concrete from this diagram, or does it just label boxes? A good diagram teaches—it shows actual formats, real event names, concrete examples.

---

## Depth Assessment (Do This First)

Before designing, determine what level of detail this diagram needs:

### Simple/Conceptual Diagrams
Use abstract shapes when:
- Explaining a mental model or philosophy
- The audience doesn't need technical specifics
- The concept IS the abstraction (e.g., "separation of concerns")

### Comprehensive/Technical Diagrams
Use concrete examples when:
- Diagramming a real system, protocol, or architecture
- The diagram will be used to teach or explain (e.g., YouTube video)
- The audience needs to understand what things actually look like
- You're showing how multiple technologies integrate

**For technical diagrams, you MUST include evidence artifacts** (see below).

---

## Research Mandate (For Technical Diagrams)

**Before drawing anything technical, research the actual specifications.**

If you're diagramming a protocol, API, or framework:
1. Look up the actual JSON/data formats
2. Find the real event names, method names, or API endpoints
3. Understand how the pieces actually connect
4. Use real terminology, not generic placeholders

Bad: "Protocol" → "Frontend"
Good: "AG-UI streams events (RUN_STARTED, STATE_DELTA, A2UI_UPDATE)" → "CopilotKit renders via createA2UIMessageRenderer()"

**Research makes diagrams accurate AND educational.**

---

## Evidence Artifacts

Evidence artifacts are concrete examples that prove your diagram is accurate and help viewers learn. Include them in technical diagrams.

**Types of evidence artifacts** (choose what's relevant to your diagram):

| Artifact Type | When to Use | How to Render |
|---------------|-------------|---------------|
| **Code snippets** | APIs, integrations, implementation details | Dark rectangle + syntax-colored text (see color palette for evidence artifact colors) |
| **Data/JSON examples** | Data formats, schemas, payloads | Dark rectangle + colored text (see color palette) |
| **Event/step sequences** | Protocols, workflows, lifecycles | Timeline pattern (line + dots + labels) |
| **UI mockups** | Showing actual output/results | Nested rectangles mimicking real UI |
| **Real input content** | Showing what goes IN to a system | Rectangle with sample content visible |
| **API/method names** | Real function calls, endpoints | Use actual names from docs, not placeholders |

**Example**: For a diagram about a streaming protocol, you might show:
- The actual event names from the spec (not just "Event 1", "Event 2")
- A code snippet showing how to connect
- What the streamed data actually looks like

**Example**: For a diagram about a data transformation pipeline:
- Show sample input data (actual format, not "Input")
- Show sample output data (actual format, not "Output")
- Show intermediate states if relevant

The key principle: **show what things actually look like**, not just what they're called.

---

## Multi-Zoom Architecture

Comprehensive diagrams operate at multiple zoom levels simultaneously. Think of it like a map that shows both the country borders AND the street names.

### Level 1: Summary Flow
A simplified overview showing the full pipeline or process at a glance. Often placed at the top or bottom of the diagram.

*Example*: `Input → Processing → Output` or `Client → Server → Database`

### Level 2: Section Boundaries
Labeled regions that group related components. These create visual "rooms" that help viewers understand what belongs together.

*Example*: Grouping by responsibility (Backend / Frontend), by phase (Setup / Execution / Cleanup), or by team (User / System / External)

### Level 3: Detail Inside Sections
Evidence artifacts, code snippets, and concrete examples within each section. This is where the educational value lives.

*Example*: Inside a "Backend" section, you might show the actual API response format, not just a box labeled "API Response"

**For comprehensive diagrams, aim to include all three levels.** The summary gives context, the sections organize, and the details teach.

### Bad vs Good

| Bad (Displaying) | Good (Arguing) |
|------------------|----------------|
| 5 equal boxes with labels | Each concept has a shape that mirrors its behavior |
| Card grid layout | Visual structure matches conceptual structure |
| Icons decorating text | Shapes that ARE the meaning |
| Same container for everything | Distinct visual vocabulary per concept |
| Flat, disconnected boxes | Nested rectangles that mirror how the content is structured |

### Simple vs Comprehensive (Know Which You Need)

| Simple Diagram | Comprehensive Diagram |
|----------------|----------------------|
| Generic labels: "Input" → "Process" → "Output" | Specific: shows what the input/output actually looks like |
| Named boxes: "API", "Database", "Client" | Named boxes + examples of actual requests/responses |
| "Events" or "Messages" label | Timeline with real event/message names from the spec |
| "UI" or "Dashboard" rectangle | Mockup showing actual UI elements and content |
| ~30 seconds to explain | ~2-3 minutes of teaching content |
| Viewer learns the structure | Viewer learns the structure AND the details |

**Simple diagrams** are fine for abstract concepts, quick overviews, or when the audience already knows the details. **Comprehensive diagrams** are needed for technical architectures, tutorials, educational content, or when you want the diagram itself to teach.

---

## Design Process (Do This BEFORE Generating JSON)

### Step 0: Assess Depth Required
Before anything else, determine if this needs to be:
- **Simple/Conceptual**: Abstract shapes, labels, relationships (mental models, philosophies)
- **Comprehensive/Technical**: Concrete examples, code snippets, real data (systems, architectures, tutorials)

**If comprehensive**: Do research first. Look up actual specs, formats, event names, APIs.

### Step 1: Understand Deeply
Read the content. For each concept, ask:
- What does this concept **DO**? (not what IS it)
- What relationships exist between concepts?
- What's the core transformation or flow?
- **What would someone need to SEE to understand this?** (not just read about)

### Step 2: Map Concepts to Patterns
For each concept, find the visual pattern that mirrors its behavior:

| If the concept... | Use this pattern |
|-------------------|------------------|
| Spawns multiple outputs | **Fan-out** (radial arrows from center) |
| Combines inputs into one | **Convergence** (funnel, arrows merging) |
| Has hierarchy/nesting | **Tree** (lines + text, inside a container rectangle) |
| Is a sequence of steps | **Timeline** (line + dots + labels, inside a container rectangle) |
| Loops or improves continuously | **Spiral/Cycle** (arrow returning to start) |
| Is an abstract state or context | **Cloud** (overlapping ellipses) |
| Transforms input to output | **Assembly line** (before → process → after) |
| Compares two things | **Side-by-side** (parallel with contrast) |
| Separates into phases | **Gap/Break** (visual separation between sections) |

### Step 3: Ensure Variety
For multi-concept diagrams: **each major concept must use a different visual pattern**. No uniform cards or grids.

### Step 4: Sketch the Flow
Before JSON, mentally trace how the eye moves through the diagram. There should be a clear visual story.

### Step 5: Generate JSON
Only now create the Excalidraw elements. **See below for how to handle large diagrams.**

### Step 6: Render & Validate (MANDATORY)
After generating the JSON, you MUST run the render-view-fix loop until the diagram looks right. This is not optional — see the **Render & Validate** section below for the full process.

---

## Large / Comprehensive Diagram Strategy

**For comprehensive or technical diagrams, you MUST build the JSON one section at a time.** Do NOT attempt to generate the entire file in a single pass. This is a hard constraint — Claude Code has a ~32,000 token output limit per response, and a comprehensive diagram easily exceeds that in one shot. Even if it didn't, generating everything at once leads to worse quality. Section-by-section is better in every way.

### The Section-by-Section Workflow

**Phase 1: Build each section**

1. **Create the base file** with the JSON wrapper (`type`, `version`, `appState`, `files`) and the first section of elements.
2. **Add one section per edit.** Each section gets its own dedicated pass — take your time with it. Think carefully about the layout, spacing, and how this section connects to what's already there.
3. **Use descriptive string IDs** (e.g., `"trigger_rect"`, `"arrow_fan_left"`) so cross-section references are readable.
4. **If the diagram uses `seed` values, namespace them by section** (e.g., section 1 uses 100xxx, section 2 uses 200xxx) to avoid collisions. Seeds are only needed for hand-drawn diagrams (`roughness` above 0).
5. **Update cross-section bindings** as you go. When a new section's element needs to bind to an element from a previous section (e.g., an arrow connecting sections), edit the earlier element's `boundElements` array at the same time.

**Phase 2: Review the whole**

After all sections are in place, read through the complete JSON and check:
- Are cross-section arrows bound correctly on both ends?
- Is the overall spacing balanced, or are some sections cramped while others have too much whitespace?
- Do IDs and bindings all reference elements that actually exist?

Fix any alignment or binding issues before rendering.

**Phase 3: Render & validate**

Now run the render-view-fix loop from the Render & Validate section. This is where you'll catch visual issues that aren't obvious from JSON — overlaps, clipping, imbalanced composition.

### Section Boundaries

Plan your sections around natural visual groupings from the diagram plan. A typical large diagram might split into:

- **Section 1**: Entry point / trigger
- **Section 2**: First decision or routing
- **Section 3**: Main content (hero section — may be the largest single section)
- **Section 4-N**: Remaining phases, outputs, etc.

Each section should be independently understandable: its elements, internal arrows, and any cross-references to adjacent sections.

### What NOT to Do

- **Don't generate the entire diagram in one response.** You will hit the output token limit and produce truncated, broken JSON. Even if the diagram is small enough to fit, splitting into sections produces better results.
- **Don't use a coding agent** to generate the JSON. The agent won't have sufficient context about the skill's rules, and the coordination overhead negates any benefit.
- **Don't write a Python generator script.** The templating and coordinate math seem helpful but introduce a layer of indirection that makes debugging harder. Hand-crafted JSON with descriptive IDs is more maintainable.

---

## Visual Pattern Library

### Fan-Out (One-to-Many)
Central element with arrows radiating to multiple targets. Use for: sources, PRDs, root causes, central hubs.
```
        ○
       ↗
  □ → ○
       ↘
        ○
```

### Convergence (Many-to-One)
Multiple inputs merging through arrows to single output. Use for: aggregation, funnels, synthesis.
```
  ○ ↘
  ○ → □
  ○ ↗
```

### Tree (Hierarchy)
Parent-child branching with connecting lines and text labels, inside a container rectangle. Use for: org charts, taxonomies, and file systems where the diagram needs to show a lot of files and folders within a small space (if there are fewer files and folders, and the diagram needs to show the contents of the files, use nested rectangles instead).
```
  label
  ├── label
  │   ├── label
  │   └── label
  └── label
```
Use `line` elements for the trunk and branches, and `text` elements for the labels.

### Spiral/Cycle (Continuous Loop)
Elements in sequence with arrow returning to start. Use for: feedback loops, iterative processes, evolution.
```
  □ → □
  ↑     ↓
  □ ← □
```

### Cloud (Abstract State)
Overlapping ellipses with varied sizes. Use for: context, memory, conversations, mental states.

### Assembly Line (Transformation)
Input → Process Box → Output with clear before/after. Use for: transformations, processing, conversion.
```
  ○○○ → [PROCESS] → □□□
  chaos              order
```

### Side-by-Side (Comparison)
Two parallel structures with visual contrast. Use for: before/after, options, trade-offs.

### Gap/Break (Separation)
Visual whitespace or barrier between sections. Use for: phase changes, context resets, boundaries.

### Lines as Structure
Use lines (type: `line`, not arrows) as primary structural elements inside a container rectangle:
- **Timelines**: Vertical or horizontal line with small dots (10-20px ellipses) at intervals, text labels beside each dot
- **Tree structures**: Vertical trunk line + horizontal branch lines, with text labels
- **Dividers**: Thin dashed lines to separate sections
- **Flow spines**: A central line that elements relate to, rather than connecting boxes

```
Timeline:           Tree:
  ●─── Label 1        │
  │                   ├── item
  ●─── Label 2        │   ├── sub
  │                   │   └── sub
  ●─── Label 3        └── item
```

Enclose each of these patterns in a container rectangle, so that its labels aren't left floating in empty space.

---

## Shape Meaning

Choose shape based on what it represents:

| Concept Type | Shape | Why |
|--------------|-------|-----|
| Labels, descriptions, details | no shape of its own, placed inside a container rectangle | The container groups the text with the content that it describes |
| Section titles, annotations | no shape of its own, placed inside the section's container rectangle | Font size/weight makes the text stand out within its container |
| Markers on a timeline | small `ellipse` (10-20px) | Visual anchor, not container |
| Start, trigger, input | `ellipse` | Soft, origin-like |
| End, output, result | `ellipse` | Completion, destination |
| Decision, condition | `diamond` | Classic decision symbol |
| Process, action, step | `rectangle` | Contained action |
| Abstract state, context | overlapping `ellipse` | Fuzzy, cloud-like |
| Hierarchy node | lines + text, inside a container rectangle | Structure through lines |

---

## Color as Meaning

Colors encode information, not decoration. Start with the colors in `references/color-palette.md` — the meaning colors, neutral colors, text and line colors, and evidence artifact colors are all defined there.

**Key principles:**
- Green always indicates a positive meaning and red always indicates a negative meaning, and neither color is used for anything else
- Strong orange can indicate a warning and isn't used for anything else, and neutral colors have no special meaning
- Text, lines, arrows and marker dots are black on light colors and white on dark colors, and other colors can be used if they're beneficial (such as red for a line which has a negative meaning, and for the text which labels that line)
- Evidence artifacts (code snippets, JSON examples) use their own dark background + colored text scheme
- Each shape uses one color with no border, and that color must contrast strongly with its surroundings

**You're allowed, and indeed encouraged, to invent your own additional colors with additional meanings, if that'd be beneficial for the diagram that you're drawing** (such as pink, purple or yellow). The only important rule is to not use the three Meaning Colors (green, red and strong orange) in other contexts with different meanings. If a concept has no particular meaning, use Primary/Neutral or Secondary.

---

## Modern Aesthetics

For clean, professional diagrams:

### Roughness
- `roughness: 0` — Clean, crisp edges. Use for modern/technical diagrams.
- `roughness: 1` — Hand-drawn, organic feel. Use for brainstorming/informal diagrams.

**Default to 0** for most professional use cases.

### Stroke Width
- `strokeWidth: 1` — Thin, elegant. Good for lines, dividers, subtle connections.
- `strokeWidth: 2` — Standard. Good for shapes and primary arrows.
- `strokeWidth: 3` — Bold. Use sparingly for emphasis (main flow line, key connections).

### Opacity
**Keep all elements fully opaque.** Either omit `opacity` (which defaults to 100) or set `opacity: 100`. Use color, size, and stroke width to create hierarchy instead of transparency.

### Small Markers Instead of Shapes
Instead of full shapes, use small dots (10-20px ellipses) as:
- Timeline markers
- Bullet points
- Connection nodes
- Visual anchors for text labels

---

## Layout Principles

### Hierarchy Through Scale
- **Hero**: 300×150 - visual anchor, most important
- **Primary**: 180×90
- **Secondary**: 120×60
- **Small**: 60×40

### Whitespace = Importance
The most important element has the most empty space around it (200px+).

### Flow Direction
Guide the eye: typically left→right or top→bottom for sequences, radial for hub-and-spoke.

### Connections Required
Position alone doesn't show relationships. If A relates to B, there must be an arrow.

---

## Text Rules

**CRITICAL**: The JSON `text` property contains ONLY readable words.

```json
{
  "id": "myElement1",
  "text": "Start",
  "originalText": "Start"
}
```

Settings: `fontSize: 16`, `fontFamily: 3` (`Cascadia`, monospace) or `fontFamily: 2` (`Helvetica`, sans-serif) or, in exceptional circumstances, one of the other `FONT_FAMILY` values, `textAlign: "center"`, `verticalAlign: "middle"`

---

## JSON Structure

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

## Element Templates

See `references/element-templates.md` for copy-paste JSON templates for each element type (text, line, dot, rectangle, arrow). Pull colors from `references/color-palette.md` based on each element's semantic purpose.

---

## Render & Validate (MANDATORY)

You cannot judge a diagram from JSON alone. After generating or editing the Excalidraw JSON, you MUST render it to PNG, view the image, and fix what you see — in a loop until it's right. This is a core part of the workflow, not a final check.

### How to Render

To render the `/path/to/diagram.excalidraw` file to a new PNG image at `/path/to/diagram.png`:

```bash
cd .claude/skills/excalidraw-diagram/references && ./node_modules/.bin/excalidraw render /path/to/diagram.excalidraw --scale 1 --max-width 2048 --max-height 2048 --out /path/to/diagram.png
```

Then use the **Read tool** on the PNG to actually view it.

For very large diagrams, you may need to increase the `--scale`, `--max-width` and `--max-height` parameters in order to render the PNG with sufficient detail. For example, you could successively double the value of each parameter until the rendered PNG has sufficient detail:

```
--scale 2 --max-width 4096  --max-height 4096
--scale 4 --max-width 8192  --max-height 8192
--scale 8 --max-width 16384 --max-height 16384
```

The maximum supported values are:

* `--scale 16`
* `--max-width 16384`
* `--max-height 16384`

### The Loop

After generating the initial JSON, run this cycle:

**1. Render & View** — Run the render script, then Read the PNG.

**2. Audit against your original vision** — Before looking for bugs, compare the rendered result to what you designed in Steps 1-4. Ask:
- Does the visual structure match the conceptual structure you planned?
- Does each section use the pattern you intended (fan-out, convergence, timeline, etc.)?
- Does the eye flow through the diagram in the order you designed?
- Is the visual hierarchy correct — hero elements dominant, supporting elements smaller?
- For technical diagrams: are the evidence artifacts (code snippets, data examples) readable and properly placed?

**3. Check for visual defects:**
- Text clipped by or overflowing its container
- Text or shapes overlapping other elements
- Arrows crossing through elements instead of routing around them
- Arrows landing on the wrong element or pointing into empty space
- Labels floating ambiguously (not clearly anchored to what they describe)
- Uneven spacing between elements that should be evenly spaced
- Sections with too much whitespace next to sections that are too cramped
- Text too small to read at the rendered size
- Overall composition feels lopsided or unbalanced

**4. Fix** — Edit the JSON to address everything you found. Common fixes:
- Widen containers when text is clipped
- Adjust `x`/`y` coordinates to fix spacing and alignment
- Add intermediate waypoints to arrow `points` arrays to route around elements
- Reposition labels closer to the element they describe
- Resize elements to rebalance visual weight across sections

**5. Re-render & re-view** — Run the render script again and Read the new PNG.

**6. Repeat** — Keep cycling until the diagram passes both the vision check (Step 2) and the defect check (Step 3). Typically takes 2-4 iterations. Don't stop after one pass just because there are no critical bugs — if the composition could be better, improve it.

### When to Stop

The loop is done when:
- The rendered diagram matches the conceptual design from your planning steps
- No text is clipped, overlapping, or unreadable
- Arrows route cleanly and connect to the right elements
- Spacing is consistent and the composition is balanced
- You'd be comfortable showing it to someone without caveats

### First-Time Setup

If the render script hasn't been set up yet:

```bash
cd .claude/skills/excalidraw-diagram/references
npm install
```

---

## Quality Checklist

### Depth & Evidence (Check First for Technical Diagrams)
1. **Research done**: Did you look up actual specs, formats, event names?
2. **Evidence artifacts**: Are there code snippets, JSON examples, or real data?
3. **Multi-zoom**: Does it have summary flow + section boundaries + detail?
4. **Concrete over abstract**: Real content shown, not just labeled boxes?
5. **Educational value**: Could someone learn something concrete from this?

### Conceptual
6. **Isomorphism**: Does each visual structure mirror its concept's behavior?
7. **Argument**: Does the diagram SHOW something text alone couldn't?
8. **Variety**: Does each major concept use a different visual pattern?
9. **No uniform containers**: Avoided card grids and equal boxes?

### Structural
10. **Connections**: Every relationship has an arrow or line
11. **Flow**: Clear visual path for the eye to follow
12. **Hierarchy**: Important elements are larger/more isolated

### Technical
13. **Text clean**: `text` contains only readable words
14. **Font**: `fontFamily: 3` (`Cascadia`, monospace) for technical terms, `fontFamily: 2` (`Helvetica`, sans-serif) for everything else or, in exceptional circumstances, one of the other `FONT_FAMILY` values
15. **Roughness**: `roughness: 0` for clean/modern (unless hand-drawn style requested)
16. **Opacity**: All elements are fully opaque (`opacity` is omitted or set to 100)

### Visual Validation (Render Required)
17. **Rendered to PNG**: Diagram has been rendered and visually inspected
18. **No text overflow**: All text fits within its container
19. **No overlapping elements**: Shapes and text don't overlap unintentionally
20. **Even spacing**: Similar elements have consistent spacing
21. **Arrows land correctly**: Arrows connect to intended elements without crossing others
22. **Readable at export size**: Text is legible in the rendered PNG
23. **Balanced composition**: No large empty voids or overcrowded regions
