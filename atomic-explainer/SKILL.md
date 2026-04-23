---
name: atomic-explainer
description: Explain new concepts, technical jargon, and specialized terms at the smallest practical conceptual granularity. Use when the user is new to a topic, says a term is confusing, asks for a more detailed or step-by-step explanation, or when a concise answer would otherwise assume background knowledge the user may not have.
---

# Atomic Explainer

## Overview

Explain unfamiliar terms without assuming the user already knows adjacent jargon.
Prefer small, direct meaning units over compressed expert summaries.

## Core Rules

Follow these defaults whenever the skill is active:

1. Detect likely unknown terms early.
If an explanation depends on words like `offset`, `quorum`, `backlog`, `runid`, protocol names, acronyms, or domain terms, do not assume the user knows them just because they appear in documentation or code.

2. Explain each term in one meaning at a time.
Prefer:
- what it is
- what role it plays
- what it is not

Do not fuse all three into one dense sentence unless the user explicitly asks for a compressed summary.

3. Start from the operation, not the formal definition.
Good default pattern:
- "It means..."
- "In this flow it is used for..."
- "If X happens, it lets the system do Y."

4. Use the smallest practical scope.
If a sentence contains two unfamiliar ideas, split it.
If a term requires another term, define the dependency first or inline it immediately.

5. Prefer causal language over taxonomy.
Use forms like:
- "If this is missing, the system falls back to..."
- "If the gap is still there, it can resume..."
- "If not, it must restart from scratch."

6. Avoid premature abstraction.
Do not begin with broad textbook phrasing when a concrete mechanical description would be clearer.
Only give the broader abstraction after the user can track the mechanics.

7. Respect the user's requested depth.
If the user asks for "one sentence" or "a very short answer", still avoid hidden assumptions, but compress carefully.
If the user shows confusion after a short answer, immediately switch to a more atomic explanation.

## Default Explanation Ladder

Use this ladder and stop as soon as the user is satisfied:

### Level 1: Single-sentence meaning

Give the term's direct meaning in one sentence with no extra jargon.

### Level 2: Flow position

State where the term sits in the mechanism or sequence.

### Level 3: Failure branch

State what happens if it is missing, too small, invalid, stale, or unavailable.

### Level 4: Contrast

Briefly distinguish it from nearby concepts the user is likely to confuse it with.

Examples:
- `backlog` vs backup
- `offset` vs data content
- `Sentinel` vs Redis data node

## Preferred Patterns

### Pattern A: New term

Use:
"`X` means ..."

Then optionally:
"Here it is used to ..."

### Pattern B: Command with strange arguments

Split argument-by-argument.

Example shape:
- "`?` means ..."
- "`-1` means ..."
- "Together this command is saying ..."

### Pattern C: Mechanism confusion

Turn the mechanism into small branches:
- "If the missing part is still available ..."
- "If it has already been overwritten ..."
- "Then the system must ..."

## Anti-Patterns

Do not do these when the skill is active:

- Do not answer with a compressed expert summary that presumes prerequisite vocabulary.
- Do not define a term using another undefined term unless you immediately unpack it.
- Do not jump straight to analogy when a direct mechanical explanation is enough.
- Do not overuse category labels like "a high-availability component" without saying what action it actually performs.
- Do not say "basically" and then leave the core action implicit.

## Response Style

Keep explanations plain and compact.
When multiple new terms appear together, prefer short flat bullets over one dense paragraph.
If the user is actively learning, optimize for "immediately understandable" rather than "formally complete."
