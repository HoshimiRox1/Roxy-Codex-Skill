# Obsidian DataviewJS Patterns

Use this reference when the request clearly targets an Obsidian note and the result should be an embedded interactive component.

## Default Assumptions

- Dataview is installed and enabled
- The output should be a single self-contained `dataviewjs` block
- The block should run after direct insertion into a note
- The block should not require external CSS, remote scripts, or vault-wide setup unless the user explicitly asks for it
- Do not default to Mermaid inside notes; prefer inline `SVG` or styled DOM-based visual blocks unless the user explicitly asks for Mermaid

## Preferred Interaction Patterns

### Tabs

Use for:

- switching among concepts
- comparing related structures
- separating "definition", "shape", "commands", and "use cases"

### Step switcher

Use for:

- phased workflows
- protocol walkthroughs
- progressive learning paths

### Foldable sections

Use for:

- keeping large notes compact
- revealing details only when needed
- showing examples without overwhelming the note

### Flashcards

Use for:

- term recall
- command recall
- quick self-testing

### Mini quiz

Use for:

- checking conceptual understanding
- distinguishing similar structures or commands

### Timeline

Use for:

- request/response order
- lifecycle views
- staged system behavior

## Styling Rules

- Keep styles scoped to the generated root container
- Use mild borders, subtle fills, and theme-friendly text colors
- Avoid assuming a fixed page background color
- Prefer local CSS variables inside the block when that improves readability
- Keep spacing generous enough for Chinese content
- For diagrams, prefer card-like composition, consistent arrow styling, and muted semantic color coding over plain Mermaid-like rendering
- For inline `SVG`, prefer transparent backgrounds so the result blends with the current Obsidian theme or custom note background

## Integration Rules

- If the user gives an existing note, append or insert a new block without deleting their original content
- If the user only asks for content, return a standalone block ready to paste
- Default to self-contained sample data unless the user explicitly wants note fields, tags, or Dataview queries

## Pattern Selection Heuristic

1. Use tabs for multi-facet knowledge summaries
2. Use step switchers for ordered operations
3. Use foldable sections for dense reference material
4. Use flashcards or quizzes for review mode
5. Combine at most two interaction types by default to keep the block lightweight
