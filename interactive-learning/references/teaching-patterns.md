# Teaching Patterns

Use this reference when the skill needs to stay instructional without becoming too large.

## Default Teaching Scaffold

When useful, break the topic into these four parts:

- Concept: what the thing is
- Relationship: what it connects to
- Step: how it behaves or unfolds
- Misconception: what learners usually confuse

Keep each part compact. The goal is orientation, not a full chapter.

## Light Learning Unit

Default to a small learning unit:

- Topic title
- A brief why-this-view explanation
- One main visual or interactive artifact
- One short supplement such as misconceptions, key uses, or review notes

Do not generate every possible explanation mode unless the user explicitly asks for more depth.

## Choosing the Main Artifact

- Choose one strong artifact first
- Add one small supplement only if it materially improves comprehension
- Avoid generating multiple competing diagrams for the same request unless comparison is the point

Examples:

- Redis data structures: one comparison-oriented visual plus a short "typical use cases" block
- Unreal Gameplay class relationships: one framework diagram plus a short note on common misunderstandings
- Client-server Redis flow: one sequence or flow view plus a short operational summary

## Compression Rules

- Replace exhaustive detail with representative detail
- Prefer canonical cases over edge cases
- Summarize similar items under one grouped label when the diagram gets crowded
- If the response starts to look like a course module, trim back to the essential path

## When to Escalate to HTML

Prefer embedded `HTML` instead of `SVG` when the learner benefits from interaction such as:

- progressively revealed steps
- switching among multiple perspectives
- flashcard recall
- quick self-test
- timeline stepping

Do not escalate to `HTML` just because the topic is large. Use it when interaction improves understanding.

## Supporting Block Ideas

Use at most one small support block by default. Good options:

- common misconception list
- key use-case summary
- mini review checklist
- short "when to use which" comparison

Keep the support block short enough that the main artifact remains the focal point.
