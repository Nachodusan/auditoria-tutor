# Hallucination audit — blind double annotation

Human evaluation protocol used to measure how often
[Tutor IA](https://github.com/Nachodusan/tutor-ia) states things that are not in
the student's official coursebook.

The tutor answers student questions grounded in their textbook. Automated
grounding checks (reranker abstention gate + per-sentence NLI) catch a lot, but
they are themselves models — so the ground truth has to come from people who know
the material.

## Design

Two teachers rate **the same 81 cases**, independently:

| | |
|---|---|
| **Annotators** | 2, working in isolation |
| **Cases** | 81, identical set for both |
| **Order** | different per annotator, to break position and fatigue effects |
| **Contact** | none until both finish — no discussing cases, no looking at the other's page |
| **Session** | 1½–2½ hours, resumable; progress is saved locally |

Each annotator opens only their own page (`persona-A.html`, `persona-B.html`) on
their own machine. Instructions live inside the page, so there is no shared
briefing to drift between the two.

## Why it is built this way

- **Blind and independent** — if annotators can see each other, agreement is
  inflated and the number stops meaning anything.
- **Different case order** — the same list in the same order lets fatigue and
  anchoring land on the same items for both raters.
- **Resumable, saved locally** — a 2-hour forced sitting produces rushed ratings
  near the end.
- **Two raters, not one** — a single annotator gives you an opinion. Two give you
  a disagreement rate, and the disagreements are where the rubric is unclear.

The output is the per-case agreement between A and B, plus the set of cases they
disagreed on, which is what gets fed back into the abstention threshold and the
rubric.

## Files

```
index.html       → entry page, assigns each teacher their own link
persona-A.html   → 81 cases, order A
persona-B.html   → same 81 cases, order B
```

Static HTML, no backend. Progress persists in the browser.
