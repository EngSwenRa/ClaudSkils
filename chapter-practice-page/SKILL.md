---
name: "chapter-practice-page"
description: "When Rayan uploads a course chapter, lecture slides or PDF, turn it into an interactive randomized practice page (quiz, worked solutions, cheat sheet, progress) published as an artifact."
---

# Chapter practice page

Goal: Rayan wants to master a chapter by practice. For every chapter he uploads, build one self-contained interactive page like the "CSC 220 Unit 1 Number Systems Practice" page, and publish it as an artifact.

## Steps

1. **Convert and read the file.** Use the markitdown-convert skill if listed (otherwise `markitdown file > chapter.md`), then Read the markdown fully. Slide PDFs convert messily (tables split, formulas garbled); rely on the worked examples and exercise answer tables, and cross-check them against your own calculation.
2. **Map the chapter into practice topics.** List every skill or procedure the chapter teaches (conversions, calculations, trace-throughs, definitions, formulas). Group them into 4-7 topics (for example: To decimal, From decimal, Shortcuts, Fractions, Coding). Note the exact technique and wording the instructor uses so worked solutions match the course.
3. **Choose question types per topic.**
   - Computational or procedural content: write a random *generator* per question type that returns `{sub, q, ans, steps, hint}`. `steps` is a worked solution in the instructor's method. Make 10-20 types, covering every example and exercise in the slides.
   - Conceptual or definition content (no calculation): use multiple-choice generators from a fact bank with a short explanation per answer, shuffled each time.
   - Mixed: do both.
4. **Write the page** as ONE self-contained HTML file (no external fetches; system fonts; light and dark via `prefers-color-scheme`). Include:
   - Practice tab: topic chips (All plus each topic), stats (correct/total, streak, accuracy), question card, answer input (Enter checks, Enter again goes to next), Check/Next, Hint button, Show solution (counts as missed), feedback plus worked steps after every answer.
   - Cheat sheet tab: the chapter's rules, formulas and reference tables, short.
   - My progress tab: per-question-type attempts and accuracy, weakest first, with a reset button.
   - Answer normalisation: uppercase, strip spaces, ignore leading zeros (and trailing fraction zeros) unless the type needs them.
   - Persist stats in `localStorage` inside try/catch; the page must work without it.
   - No alert/confirm/prompt, no downloads.
5. **Verify before publishing.** Extract the script into a Node harness with a stubbed `document`/`localStorage`, run each generator ~400 times, and check every answer against an independent computation (for example reparsing the question and recomputing). Fix any mismatch. Also spot-check that the worked steps read correctly. Do not publish unverified answers; wrong answers in a study tool are worse than none.
6. **Publish.** Write the file to the working folder, then call `mcp__claude-artifacts__list_types` once, and since no type fits an interactive practice tool, publish with `publish_files` (title `<COURSE CODE> – <Chapter title> Practice`, icon like `calculator`). Read the artifacts `guide` (pages) before the first publish of a session.
7. **Reply briefly:** one sentence on what the page covers (topics), anything in the chapter you could not turn into questions and why (for example repeating fractions with no exact answer), and one next step. Mention that the markdown was produced with markitdown. Do not repeat the URL if the app already shows a card.

## Quality rules

- Questions come from this chapter only; match its notation, base conventions and terminology.
- Keep numbers in sizes appropriate for exam-style hand work.
- Every generator must be able to produce an exact, unambiguous answer. Avoid questions with several valid answers unless the checker accepts all.
- If the chapter has exercise tables with answers, make sure at least those exact problem styles appear.
- If a chapter is mostly theory, say so and build the multiple-choice version rather than forcing calculations.
- Reuse the same visual design and tab structure for every chapter so they feel like one set.