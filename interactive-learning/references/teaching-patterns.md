# Teaching Patterns

Use this reference when the skill needs to stay instructional without becoming too large.

## Default Teaching Scaffold

When useful, break the topic into these four parts:

- Concept: what the thing is
- Relationship: what it connects to
- Step: how it behaves or unfolds
- Misconception: what learners usually confuse

Keep each part compact. The goal is orientation, not a full chapter.

## Artifact-First Default

For this skill, the default unit of value is a learning artifact, not a long text explanation.

- If the request sounds like a roadmap, learning path, phased plan, or "how do I become X" question, default to a visual or interactive roadmap
- If the request sounds like a process, default to a stepwise visual
- If the request sounds like a comparison, default to a comparison visual
- If the request sounds like concept organization, default to a framework or relationship visual

Do not spend extra turns listing format options unless the host constraint is genuinely unclear.

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
- If the user did not specify a format but the topic is obviously easier visually, generate the artifact instead of asking for permission to be visual

Examples:

- Redis data structures: one comparison-oriented visual plus a short "typical use cases" block
- Unreal Gameplay class relationships: one framework diagram plus a short note on common misunderstandings
- Client-server Redis flow: one sequence or flow view plus a short operational summary

## Roadmap Decomposition

When the task is a roadmap, do not start from a fixed number of stages. Start from the structure of the topic.

Ask these questions implicitly before generating the roadmap:

- How many prerequisite layers are actually needed?
- Does the topic have a clean linear progression, or does it split into branches?
- Should practice, project work, review, or specialization be separate phases?
- Would combining two adjacent phases improve clarity, or hide an important transition?

Useful heuristics:

- simple topic or short study plan: often 3-4 phases
- medium topic with foundation plus application: often 4-6 phases
- broad domain, career path, or long learning route: often 5-8 phases

These are only heuristics. Do not pad the roadmap to reach a visually pleasing number.

Prefer fewer meaningful phases to many thin phases with low teaching value.

## Compression Rules

- Replace exhaustive detail with representative detail
- Prefer canonical cases over edge cases
- Summarize similar items under one grouped label when the diagram gets crowded
- If the response starts to look like a course module, trim back to the essential path

## Interactive Format Routing

Before escalating to an interactive format, decide whether the host is Obsidian.

- If the user is working inside Obsidian, prefer `dataviewjs` for the final deliverable
- If the user is outside Obsidian and wants a runnable artifact, prefer single-file `HTML`
- Do not let generic interactivity requirements override the Obsidian boundary

## When to Escalate to HTML

Prefer embedded `HTML` instead of `SVG` when the learner benefits from interaction such as:

- progressively revealed steps
- switching among multiple perspectives
- flashcard recall
- quick self-test
- timeline stepping

Do not escalate to `HTML` just because the topic is large. Use it when interaction improves understanding and the target is not clearly Obsidian.

Roadmap and learning-plan tasks often deserve `HTML` even when they are not quizzes, especially when the phases contain expandable details, milestones, branching options, or review checkpoints.

If the roadmap is mostly about one-glance structure rather than staged interaction, a polished `SVG` roadmap is usually better.

If the learner needs an overview first, do not hide later phases behind stateful interactions, locks, or progression gates by default.

## Practice Mode

If the request is a quiz, exercise sheet, self-test, or practice note:

- treat the task as assessment-first rather than explanation-first
- default the main artifact to an interactive question component
- do not use a static question list with answers hidden at the bottom unless the user explicitly asks for a static format
- in Obsidian, use `dataviewjs`
- outside Obsidian, use single-file `HTML`

## Supporting Block Ideas

Use at most one small support block by default. Good options:

- common misconception list
- key use-case summary
- mini review checklist
- short "when to use which" comparison

Keep the support block short enough that the main artifact remains the focal point.

## Local File Bias

When the environment supports file creation and the artifact would be materially easier to use as a file:

- create a single-file `.html` or `.svg` artifact directly
- prefer the current working directory unless the user names a different target
- mention the chosen file briefly instead of delaying until the user explicitly asks for a file or picture
