---
name: interactive-learning
description: Generate learning-oriented diagrams and interactive study content for concept explanation, structure building, process walkthroughs, comparisons, and Obsidian note enhancement. Use when the user is learning or organizing knowledge and would benefit from visual or interactive presentation, including framework diagrams, flowcharts, sequence diagrams, concept relationship maps, comparison views, embedded HTML study widgets, or dataviewjs components inside Obsidian notes. Trigger broadly whenever the request implies interactive content, diagrammatic explanation, structured learning aids, or insertion of study components into existing notes, even if the user does not explicitly ask for SVG, HTML, or dataviewjs.
---

# Interactive Learning

Interpret the user's request as a learning task first, not just a rendering task.

Start by extracting four elements from the topic when useful:

- Concept
- Relationship
- Step
- Misconception

Keep this teaching scaffold light. Do not expand the response into a full lesson unless the user explicitly asks for it.

Choose the output format proactively.

Use `SVG` when the task is primarily static structure or explanation. Prefer custom-authored `SVG` for framework diagrams, flowcharts, sequence diagrams, concept relationship maps, comparison views, and other diagrammatic explanations that should be directly saveable and easy to read in Obsidian.

Use inline `SVG` inside `HTML` or `dataviewjs` when the note container width matters more than standalone export. This hybrid mode is preferred in Obsidian when the same diagram must stay visually stable across editor width changes, preview width changes, or callout/tab containers.

Use embedded single-file `HTML + CSS + JS` when the task benefits from interaction such as tab switching, staged reveal, folding sections, flashcards, quizzes, timelines, or step-by-step demonstrations.

Use `dataviewjs` when the task clearly happens inside an Obsidian note and the user wants interactive content embedded directly into the note. Default to a single self-contained `dataviewjs` block that can run as inserted, without requiring external CSS, external scripts, or additional vault structure unless the user explicitly asks for integration with note fields, tags, tasks, or other data.

Do not default to Mermaid. Mermaid is acceptable only when the user explicitly requests Mermaid or when the environment strongly requires Mermaid syntax. In normal cases, prefer a handcrafted `SVG` or an embedded `HTML`/`dataviewjs` component that visually reads like a polished diagram rather than a generic Mermaid chart.

Select the diagram type before generating output.

Prefer these core diagram classes:

- Framework diagram
- Flowchart
- Sequence diagram
- Comparison diagram
- Concept relationship diagram

If the user does not specify a format but the topic is clearly better explained visually, choose the most suitable format automatically and say so briefly.

Default output behavior:

- Give a short explanation of why the chosen representation fits the topic
- Produce one main output artifact
- Optionally add one small supporting block if it improves learning clarity without making the result bulky

Treat the output as a light learning unit rather than a large course page. When useful, organize it as:

- Topic title
- Compact breakdown
- Main visual or interactive artifact
- Short misconception or review notes

For `SVG`, follow these rules:

- Default to Chinese text layout unless the user explicitly requests another language
- Use restrained, academic or note-friendly colors rather than highly saturated palettes
- Prefer theme-neutral presentation that can blend into both light and dark Obsidian themes
- Default to a transparent canvas background so the diagram can adapt to the host note or page background
- Do not add a full-canvas backdrop, gradient wash, or framed poster background unless the user explicitly asks for a self-contained poster-like card
- Avoid relying on external fonts, external scripts, or remote assets
- Keep the file as self-contained as practical
- Prioritize layout clarity, readable spacing, clear arrows and connectors, and non-garbled Chinese text
- Prefer hand-composed card-based diagrams with rounded rectangles, soft fills, subtle borders, and short secondary labels when that improves readability
- Use solid arrows for main flow and dashed connectors only for secondary, feedback, or broadcast relationships
- Favor a visually polished custom layout over Mermaid-like default chart aesthetics
- Size nodes from their content rather than forcing long labels into fixed boxes when that risks overlap
- Prefer line breaking, larger containers, or a larger canvas over crowded placement
- Avoid node overlap and text clipping entirely; if a perfect arrow anchor is hard, a slightly imperfect arrow is acceptable, but node collisions are not
- Assume editability is not required unless the user explicitly asks for a more editable structure
- Always define a stable `viewBox`; do not rely on outer `width` and `height` alone for layout correctness
- Prefer explicit `text-anchor`, `dominant-baseline`, `tspan`, and line spacing values over browser defaults when text centering matters
- Avoid `foreignObject`, external CSS inheritance, and font-dependent measurement tricks unless the user explicitly wants rich HTML inside SVG
- Treat host-container scaling problems and internal coordinate problems as different issues: wrapping an `SVG` in `HTML` or `dataviewjs` can fix responsive sizing, but it does not repair bad node coordinates, bad text wrapping, or insufficient spacing inside the diagram itself

For embedded `HTML`, follow these rules:

- Use plain `HTML + CSS + JS`
- Keep everything in a single file unless the user asks otherwise
- Avoid frameworks and CDN dependencies
- Default to desktop-oriented presentation
- Do not optimize for mobile unless the user explicitly requests it
- Make the result feel like an embedded study widget, not a full website

For `dataviewjs`, follow these rules:

- Assume Dataview is installed and enabled
- Produce a single block that the user can paste into a note directly
- Keep styling local and lightweight
- Use DOM manipulation only as needed
- Prefer self-contained interaction over vault-wide coupling unless the user explicitly requests note-driven data behavior
- When a note needs a diagram but not a full interaction model, prefer embedding inline `SVG` or generating diagram-like DOM blocks rather than using Mermaid by default
- When the main risk is note-width adaptation, prefer a local wrapper element plus inline `SVG` with `width: 100%`, `height: auto`, and a stable `viewBox`
- If responsiveness is needed, use `ResizeObserver` only to switch layout classes or swap between a wide and narrow diagram variant; do not use it to micromanage every node position
- If the diagram is primarily static, keep the `SVG` geometry deterministic and let the outer DOM control sizing

When the user provides an existing Obsidian note or asks to modify a note, preserve the original content. Add new interactive or visual content as an insertion or append-only block rather than rewriting or deleting the user's existing material unless the user explicitly requests full restructuring.

When the user does not provide an existing note, generate fresh content from scratch.

When the topic is complex, compress aggressively. Prefer one strong central view plus one small supplement over multiple parallel diagrams or oversized study interfaces.

If more detailed diagram rules are needed, read `references/diagram-patterns.md`.

If more detailed teaching-structure rules are needed, read `references/teaching-patterns.md`.

If the task is specifically about embedding interactive study components into Obsidian notes, read `references/obsidian-dataviewjs-patterns.md`.
