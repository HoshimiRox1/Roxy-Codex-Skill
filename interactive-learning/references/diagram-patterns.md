# Diagram Patterns

Use this reference when the task is clearly visual and the main decision is which diagram to generate and how to keep it readable.

## Default Rendering Preference

Prefer handcrafted `SVG` over Mermaid for diagram output.

Use Mermaid only when:

- the user explicitly asks for Mermaid syntax as the deliverable
- the environment only supports Mermaid syntax
- the goal is fast editable syntax rather than polished presentation

In normal learning-note workflows, especially for Obsidian, aim for a polished custom diagram rather than a generic Mermaid chart. For flowcharts and note workflow diagrams, treat `SVG` as the default output path.

## Core Types

### Framework diagram

Use when the user wants to understand a system made of stable parts, roles, layers, or class relationships.

Signals:

- "relationship"
- "framework"
- "architecture"
- "components"
- "how these classes connect"

Layout guidance:

- Group related nodes into areas or soft containers
- Keep the main reading direction left-to-right or top-to-bottom
- Limit each group to a manageable number of nodes
- Use color families to distinguish roles without high saturation

### Flowchart

Use when the user wants to understand steps, branching, decision points, or operational flow.

Signals:

- "process"
- "flow"
- "what happens first"
- "how does it work"
- "operation sequence"

Layout guidance:

- Keep the main path visually dominant
- Use diamonds sparingly for branch points
- Keep label text short
- Avoid crossing lines whenever possible
- Prefer curved or gently rounded connectors when that makes the route easier to read
- Default to a card-based node style rather than a raw engineering-diagram appearance

### Sequence diagram

Use when the topic is primarily time-ordered interactions between participants.

Signals:

- "client and server"
- "request and response"
- "interaction order"
- "who talks to whom"
- "timeline"

Layout guidance:

- Put actors in stable vertical lanes
- Keep arrows horizontal and chronological from top to bottom
- Prefer a few clear interactions over exhaustive protocol detail
- Annotate only the critical steps

### Comparison diagram

Use when the user wants to contrast items, tradeoffs, use cases, or properties.

Signals:

- "difference"
- "compare"
- "versus"
- "what is each used for"
- "which case fits which structure"

Layout guidance:

- Align compared items on a shared axis or card grid
- Use consistent fields across items
- Highlight the most decision-relevant differences first

### Concept relationship diagram

Use when the user needs to understand links among concepts but not a strict process or strict hierarchy.

Signals:

- "what is related to what"
- "how these ideas connect"
- "knowledge map"
- "concept map"

Layout guidance:

- Keep one or two central concepts near the visual center
- Use directional arrows only when the relationship has direction
- Use lines without arrows for non-directional association

## Density Rules

- Prefer one main message per diagram
- Compress large topics into 5-9 primary nodes before adding details
- If the topic still feels crowded, generate one main diagram and one short supplement instead of one overloaded diagram
- If the content cannot fit comfortably, increase canvas size or split the view before allowing overlap or clipping

## Polished SVG Style

When generating custom `SVG`, prefer this visual language:

- Rounded rectangles or soft containers instead of sharp boxes
- Muted semantic color groups rather than loud saturation
- Thin, consistent borders and connectors
- One strong title line plus one smaller support line inside important nodes
- Spacious alignment and generous padding for Chinese text
- Clear left-to-right or top-to-bottom hierarchy
- Dashed lines only for feedback loops, broadcasts, optional links, or weak coupling

This style should feel closer to a designed architecture card layout than to a raw diagramming-tool export.

## Canvas Rules

- Default to a transparent root canvas
- Do not place a full-page background rectangle by default
- If section grouping is helpful, use local containers or panels around related nodes instead of a global backdrop
- Use a responsive `viewBox` and let the canvas breathe rather than cropping tightly around dense content
- For note-oriented diagrams, prefer internal coordinates that stay fixed while the outer host scales the figure responsively

## Layout Safety Rules

- Treat overlap prevention as a hard requirement
- Treat text clipping as a hard requirement
- Choose larger spacing over tighter composition when there is any doubt
- Keep consistent horizontal and vertical rhythm between sibling nodes
- Leave explicit breathing room for arrowheads, labels, and second-line support text
- If a row becomes too dense, wrap into another row or switch to a vertical composition
- For Chinese labels, assume text can visually occupy more width than expected and reserve extra padding
- Do not treat container scaling as a substitute for layout work; a smaller rendered size can hide clutter but does not fix geometry errors

Before finalizing a diagram, mentally validate:

1. No two node boxes overlap
2. No text escapes its intended box
3. No arrowhead lands inside unrelated content
4. The main reading order is obvious at a glance
5. The diagram still works if rendered slightly narrower in Obsidian preview

## Connector Rules

- Prioritize clear routing over decorative curves
- Keep arrowheads visible and consistent
- Use dashed connectors only for optional, indirect, or weaker relationships
- Avoid line crossings unless the alternative is materially worse
- Use direct solid connectors for the main path so the visual story can be read without extra legend
- Anchor connectors to sensible node edges or centers rather than approximate freehand points
- Keep arrow endpoints outside text regions and inside reserved connector gutters
- If connectors and labels compete for space, move the label or enlarge spacing before accepting a collision
- For flowcharts, gentle curves are preferred over sharp elbows when both are equally clear

## Chinese Layout Rules

- Prefer concise labels over full-sentence node text
- Use multi-line labels only when they improve readability
- Leave enough node padding so Chinese text does not feel cramped
- Avoid depending on uncommon external fonts
- Wrap long primary labels across two lines when needed instead of shrinking them excessively
- Keep secondary text shorter and lighter than the primary label
- Center text and chips explicitly when visual centering matters; do not rely on implicit browser defaults

## Theme-Neutral Styling

- Avoid hardcoded white page backgrounds unless the figure needs a panel
- Prefer transparent or softly tinted containers
- Use medium-contrast strokes that survive both light and dark themes
- Keep palette restrained: muted blue, teal, green, amber, slate, or red accents are usually enough
- If a background panel is needed, keep it subtle enough to blend into both light and dark note themes

## Practical SVG Authoring Heuristics

- Start from node content, then derive box width, height, and spacing
- Use repeated sizing rules for similar nodes within the same lane or section
- Prefer a small set of stable x/y alignment columns instead of many ad hoc coordinates
- Reserve dedicated lanes or gutters for arrows in process diagrams
- When adding badges or chips, center them deliberately and verify they do not collide with nearby connectors
- Prefer one of two strategies and stay consistent within the same figure:
  1. fully handcrafted coordinates for small polished diagrams
  2. a semantic wide-layout or narrow-layout template with repeated spacing tokens
- Avoid pseudo-precise text measurement logic unless the environment is known and deterministic; generous padding is usually more robust than tight auto-fit
- If responsiveness is required, prepare a second composition for narrow containers instead of compressing the original layout until it breaks

## Node Content Rules

- Put the primary concept or object name on the first line
- Put behavior, role, delegate, event, or method hints on a second lighter line when useful
- Keep secondary text short and supportive rather than explanatory paragraphs
- Prefer concise labels that make the diagram scannable at a glance

## Output Heuristic

If the user does not explicitly specify a diagram type:

1. Choose sequence diagram for ordered interactions across actors
2. Choose flowchart for operational steps and branching
3. Choose framework diagram for stable structures and class or module roles
4. Choose comparison diagram for side-by-side evaluation
5. Choose concept relationship diagram for knowledge maps and non-linear explanation
