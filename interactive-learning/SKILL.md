---
name: interactive-learning
description: Generate learning-oriented diagrams and interactive study content for concept explanation, structure building, process walkthroughs, learning roadmaps, phased plans, comparisons, and Obsidian note enhancement. Use whenever the user is learning, planning how to learn, organizing knowledge, asking for a roadmap, a progression path, a phased breakdown, or any explanation that would be easier to grasp visually or interactively. Trigger aggressively even when the user does not explicitly ask for SVG, HTML, dataviewjs, an image, or a file.
---

# Interactive Learning

Interpret the user's request as a learning task first, not just a rendering task.

Default to artifact-first teaching, not prose-first teaching.

If this skill is explicitly invoked, assume the user wants at least one visual or interactive learning artifact unless they explicitly ask for text-only output.

Do not spend multiple turns offering output options before generating the first useful artifact. Choose a best-fit format and produce it immediately unless a missing host constraint makes that unsafe.

Trigger this skill aggressively when the user is asking to learn, plan learning, or understand structure. Strong trigger signals include:

- roadmap
- learning path
- study plan
- growth path
- phased plan
- step-by-step learning
- from zero to X
- beginner to advanced
- concept breakdown
- framework understanding
- process explanation
- misconception cleanup

Do not wait for the user to explicitly ask for a diagram, image, page, widget, or interactivity if the topic is obviously easier to learn through one.

Start by extracting four elements from the topic when useful:

- Concept
- Relationship
- Step
- Misconception

Keep this teaching scaffold light. Do not expand the response into a full lesson unless the user explicitly asks for it.

Choose the output format proactively.

When the user asks for a learning route, roadmap, phased growth plan, or "how to become X" style topic, do not default to a long textual outline. Default to a roadmap-style visual or interactive artifact first, with only a compact supporting explanation.

For roadmap tasks, decide the number of phases from the topic itself. Do not force a fixed 4, 5, 6, or 7-stage structure just because a template example happens to use that count.

When constructing a roadmap, first infer the real phase shape:

- how much prerequisite buildup the topic needs
- whether the topic is mostly linear or has branches
- whether practice/project phases need to be separate from theory phases
- whether review, correction, or specialization deserves its own phase

Then choose the phase count that best fits the topic. Prefer a smaller number of stronger phases over padding the roadmap with filler stages.

Detect the host context before choosing the format.

Treat the task as Obsidian-targeted when any of these signals are present:

- the user explicitly mentions Obsidian, Dataview, DataviewJS, vault, note, markdown note, or frontmatter
- the user provides a `.md` note path that appears to live inside an Obsidian vault
- the visible conversation path or working context clearly points at an Obsidian vault or note workflow

If the task is Obsidian-targeted and the requested output is interactive, default to `dataviewjs`. Do not choose standalone `HTML` as the final deliverable for interactive Obsidian note content unless the user explicitly asks for raw `HTML`.

If the user wants interactive knowledge points inside Obsidian, default to one of these `dataviewjs` patterns:

- accordion roadmap
- tabbed concept explorer
- stepwise explainer

Use `SVG` when the task is primarily static structure or explanation. Prefer custom-authored `SVG` for framework diagrams, flowcharts, sequence diagrams, concept relationship maps, comparison views, and other diagrammatic explanations that should be directly saveable and easy to read in Obsidian.

Use inline `SVG` inside `HTML` or `dataviewjs` when the note container width matters more than standalone export. This hybrid mode is preferred in Obsidian when the same diagram must stay visually stable across editor width changes, preview width changes, or callout/tab containers.

Use embedded single-file `HTML + CSS + JS` when the task benefits from interaction such as tab switching, staged reveal, folding sections, flashcards, quizzes, timelines, or step-by-step demonstrations, and the target is not clearly Obsidian.

When the target is not clearly Obsidian and the user asks for a roadmap, study plan, staged progression, or learning path, strongly prefer one of these defaults:

- single-file `HTML` accordion roadmap when the phases contain expandable details, examples, checkpoints, or optional branches
- polished standalone `SVG` roadmap when the value is mostly in one glance, structural overview, or printable/static clarity

For overview-style roadmap requests, prefer a one-page roadmap where all phases are visible at once. Do not hide later phases behind completion gates unless the user explicitly asks for gamified progression.

Use `dataviewjs` when the task clearly happens inside an Obsidian note and the user wants interactive content embedded directly into the note. Default to a single self-contained `dataviewjs` block that can run as inserted, without requiring external CSS, external scripts, or additional vault structure unless the user explicitly asks for integration with note fields, tags, tasks, or other data.

If the user asks for exercises, self-tests, quiz cards, or practice notes inside Obsidian, treat that as a `dataviewjs` quiz-board task by default rather than a static note or a raw `HTML` widget.

Do not default to Mermaid. Mermaid is a fallback only when the user explicitly requests Mermaid syntax or when the environment strictly requires Mermaid. For learning-note workflows, and especially for note flowcharts or process diagrams, prefer handcrafted `SVG` rather than Mermaid.

Select the diagram type before generating output.

Prefer these core diagram classes:

- Framework diagram
- Flowchart
- Sequence diagram
- Comparison diagram
- Concept relationship diagram

Prefer these core interactive knowledge-block classes when the request is not primarily a diagram:

- Accordion roadmap
- Tabbed concept explorer
- Stepwise explainer

Selection hints:

- use Accordion Roadmap for learning plans, phased growth, and "from zero to production" requests
- use Tabbed Concept Explorer for one-topic-many-variants or side-by-side distinctions
- use Stepwise Explainer for time-ordered or causal processes

Roadmap decomposition hints:

- simple topic: often 3-4 phases are enough
- moderate topic: often 4-6 phases are enough
- broad or career-scale topic: often 5-8 phases are enough

These are heuristics, not quotas. Never pad a roadmap to hit a preferred number.

If the user does not specify a format but the topic is clearly better explained visually, choose the most suitable format automatically and say so briefly.

If the user does not specify a format and the topic is clearly a learning artifact task, generate the artifact instead of merely proposing formats.

Default output behavior:

- Give a short explanation of why the chosen representation fits the topic
- Produce one main output artifact
- Optionally add one small supporting block if it improves learning clarity without making the result bulky

When the environment allows writing local files and the artifact would be more useful as a runnable or viewable file, prefer creating the file in the current working directory or the user's target directory rather than only pasting a large artifact inline.

Typical local-file defaults outside Obsidian:

- `.html` for interactive roadmap, stepper, quiz, or concept explorer artifacts
- `.svg` for static framework, comparison, flow, or roadmap diagrams

Do not wait several turns for the user to ask "can you make this a picture" if the skill already indicates that a visual artifact is the better teaching medium.

If a local file would be useful but there is genuine ambiguity about location or host context, ask one concise question. Otherwise, generate the artifact directly.

For exercise-oriented tasks, the main artifact should usually be the exercise component itself rather than a long prose note.

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
- For flowcharts, prefer smooth or orthogonal curved connectors with clear arrowheads instead of rigid diagram-tool defaults when that improves readability
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
- Use this path for interactive quiz boards only when the target is outside Obsidian or the user explicitly wants an `.html` file

For `dataviewjs`, follow these rules:

- Assume Dataview is installed and enabled
- Produce a single block that the user can paste into a note directly
- Keep styling local and lightweight
- Use DOM manipulation only as needed
- Prefer self-contained interaction over vault-wide coupling unless the user explicitly requests note-driven data behavior
- When a note needs a diagram but not a full interaction model, prefer embedding inline `SVG` or generating diagram-like DOM blocks rather than using Mermaid by default
- For note flowcharts, output inline `SVG` by default rather than Mermaid, even when the note is otherwise plain and non-interactive
- When the main risk is note-width adaptation, prefer a local wrapper element plus inline `SVG` with `width: 100%`, `height: auto`, and a stable `viewBox`
- If responsiveness is needed, use `ResizeObserver` only to switch layout classes or swap between a wide and narrow diagram variant; do not use it to micromanage every node position
- If the diagram is primarily static, keep the `SVG` geometry deterministic and let the outer DOM control sizing
- For quiz or practice-note tasks, prefer a card-like board with answer buttons, immediate feedback, expandable explanation, progress state, and next-question controls
- Do not default to a static Markdown question list plus `<details>` answers for practice-note tasks unless the user explicitly asks for a static printable format

When the user provides an existing Obsidian note or asks to modify a note, preserve the original content. Add new interactive or visual content as an insertion or append-only block rather than rewriting or deleting the user's existing material unless the user explicitly requests full restructuring.

When the user does not provide an existing note, generate fresh content from scratch.

When the topic is complex, compress aggressively. Prefer one strong central view plus one small supplement over multiple parallel diagrams or oversized study interfaces.

If more detailed diagram rules are needed, read `references/diagram-patterns.md`.

If the task is a flowchart, note workflow diagram, or reusable SVG diagram generation pattern, read `references/svg-flowchart-template.md`.

If more detailed teaching-structure rules are needed, read `references/teaching-patterns.md`.

If the task is specifically about embedding interactive study components into Obsidian notes, read `references/obsidian-dataviewjs-patterns.md`.

If the task is a quiz, exercise note, or practice board, read `references/quiz-patterns.md`.

If the task is an interactive knowledge note or concept block inside Obsidian, read `references/knowledge-point-patterns.md`.

If the task explicitly asks for a reusable unified shell for interactive knowledge blocks, prefer a single `dataviewjs` shell with `mode + data` instead of three unrelated standalone implementations.
