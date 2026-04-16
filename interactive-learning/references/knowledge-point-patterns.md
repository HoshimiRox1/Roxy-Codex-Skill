# Knowledge Point Patterns

Use this reference when the user wants interactive knowledge blocks inside an Obsidian note rather than a static explanation.

## Default Position

- If the target is Obsidian and the user wants interactive knowledge presentation, prefer `dataviewjs`
- Do not treat these as generic HTML widgets when they are meant to live in notes
- Choose one primary interaction pattern instead of mixing several unrelated behaviors into one block

## Core Templates

### 1. Accordion Roadmap

Use for:

- learning roadmaps
- phased plans
- staged system overviews
- "from 0 to production" style progression

Default structure:

- title and short framing paragraph
- ordered phases
- clickable phase header
- expandable phase body
- short summary per phase
- optional connector text between phases

Strength:

- keeps long content compact
- preserves reading order
- makes progression obvious

### 2. Tabbed Concept Explorer

Use for:

- comparing related concepts
- switching among data structures, commands, classes, APIs, or components
- one-topic-many-variants knowledge notes

Default structure:

- tab row
- one active concept panel
- visual or structural mini-diagram
- short use-case explanation
- expandable command or example list
- one supporting note

Strength:

- prevents vertical sprawl
- good for side-by-side mental distinction
- lets the note stay focused on one concept at a time

### 3. Stepwise Explainer

Use for:

- processes that unfold over time
- lifecycle explanation
- persistence, replication, request flow, recovery flow
- "how A and B work together" topics

Default structure:

- title and framing statement
- optional role summary cards
- step buttons or pills
- active scene panel
- diagram side
- explanation side
- one-line takeaway summary

Strength:

- ideal for causal or time-ordered understanding
- reduces overload by revealing one stage at a time

## Selection Heuristic

1. If the main question is "what are the stages", use Accordion Roadmap
2. If the main question is "what are the variants and how do they differ", use Tabbed Concept Explorer
3. If the main question is "how does the process unfold", use Stepwise Explainer

## Obsidian Rules

- Output a single self-contained `dataviewjs` block
- Scope styles locally with a unique class prefix
- Keep sample data inside one array or object near the top of the block
- Separate data definition from render logic so the template can be reused
- Avoid vault-wide dependencies unless explicitly requested
- Prefer a unified high-level shell with `mode + data` when the block is meant to be reused across topics

## Authoring Rules

- Prefer data arrays plus a single render function over hardcoding repeated DOM fragments everywhere
- Put all visual tokens near the top of the CSS block
- Use one event delegation hook when the interaction pattern supports it
- Default to Chinese copy and note-friendly density
- Prefer theme-friendly colors and Obsidian variables over hardcoded white backgrounds
- Reuse one shell for roadmap, tabs, and stepper when their surrounding frame is substantially the same

## Anti-Patterns

- Do not mix roadmap, tabs, and multi-step playback into one oversized component by default
- Do not bury answers, examples, or key explanations in distant appendices
- Do not generate static long prose when the user explicitly asked for interactive knowledge points
- Do not let one-off Redis wording leak into the template if the block is meant to be generic
