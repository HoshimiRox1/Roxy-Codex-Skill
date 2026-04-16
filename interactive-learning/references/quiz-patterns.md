# Quiz Patterns

Use this reference when the task is a quiz, exercise sheet, self-test, review board, or practice note.

## Default Position

- Treat the task as assessment-first rather than explanation-first
- The main artifact should usually be the quiz component itself
- In Obsidian, prefer `dataviewjs`
- Outside Obsidian, prefer single-file `HTML`
- Do not default to a static Markdown worksheet with answers hidden at the bottom

## Core Interaction Model

Prefer a card-board pattern with one active question at a time.

Minimum behavior:

- show the current question
- render answer choices as explicit buttons
- allow one active selection
- show immediate correctness feedback after submission
- show explanation below the card after answering
- offer `next question`
- offer `reset`

Useful optional behavior:

- progress indicator
- score counter
- shuffle questions
- retry current question
- reveal reference answer for non-multiple-choice prompts

## Question Types

Default to the simplest type that fits the topic.

- single choice: best default for most technical practice notes
- true/false: good for misconception checks
- short answer reveal: acceptable when grading free text is unrealistic

Avoid mixing too many question types in one lightweight practice block.

## Feedback Rules

- Correct feedback should be concise and confirm the key reason
- Incorrect feedback should point to the decisive concept, not just say "wrong"
- Explanations should appear near the current card, not in a distant answer appendix
- If the learner answers correctly, it is acceptable to auto-advance only when the interface still feels readable and calm

## Obsidian Rules

When the host is Obsidian:

- deliver a single `dataviewjs` block
- keep styles local to the root container
- keep sample data self-contained unless note data is explicitly requested
- avoid raw `<details>` answer dumps as the main solution

## HTML Rules

When the host is not Obsidian and the user wants a file they can open directly:

- deliver a single self-contained `.html` file
- use local CSS and vanilla JS only
- avoid external dependencies
- keep the card width and typography desktop-friendly by default

## Layout Heuristic

The default quiz board should include:

- title area
- short instruction line
- progress row
- question card
- answer button stack
- feedback / explanation panel
- footer controls

## Output Heuristic

- Obsidian practice note: `dataviewjs` quiz board
- Standalone preview file: single-file `HTML` quiz board
- Printable worksheet: static Markdown only when the user explicitly asks for it
