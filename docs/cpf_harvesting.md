# Core Product Flow — A Decided Pull Request to a Short Note

**Created:** 2026-11-06
**Updated:** 2026-11-06
**Context:** Starts when the user gives the go or the no-go on a pull request, or finishes answering a stack during setup. Ends when the user has a short note saying what changed, and any promotion question has been asked.

## Summary

This flow is Phase 4 of `cpf_code-review.md`, in detail, seen from the user's side. The reviewer works out what the review taught it about the user and about the product, makes the changes itself, and tells the user what changed. The user's time goes only on promotion questions. Read it to understand what the reviewer changes without asking. The steps are `../procedures/sop_harvest.md`.

## Flow at a Glance

```
PHASE 1: LEARN (the reviewer, on its own)

  1. Everything the user said is gathered
  2. What did this teach about how the user likes code written?
  3. What did this teach about how the product works?
       → a no-go: nothing is learned about the product
  4. A second agent answers the same questions, cold
       → only one of them proposed a long-term change: it isn't made

        ~~~~~~~~ handoff to memory ~~~~~~~~

PHASE 2: WRITE (the reviewer, on its own)

  5. Short-term memory is updated, and what stopped coming back drops off
  6. Long-term memory is updated, and each change is a commit
       → a source someone else controls: never changed

        ~~~~~~~~ handoff to the user ~~~~~~~~

PHASE 3: TELL (the reviewer, then the user)

  7. The user gets a short note
  8. The user answers any promotion question
       → no answer: it's asked again next harvest
```

---

## Phase 1: Learn

The reviewer works out what changed in what it knows, without asking. It starts at once and doesn't wait for the merge.

**Step 1: Everything the user said is gathered.** From the pull request's threads and the session, in the user's words. A correction is anything the user changed, closed or overruled, including a finding they brought back after it was dropped, and anything they had to ask that the review should have told them.

**Step 2: What did this teach about how the user likes code written?** Each correction is compared with what the reviewer already holds.

- A new one is a short-term entry.
- One that's come back two or three times becomes a promotion question, which the user decides.
- One that goes against a long-term standard becomes an exception to it.

**Step 3: What did this teach about how the product works?** Each thing the review showed about the product is checked against the approved code.

- A fact that holds goes to long-term memory, with no question to the user.
- One it can't check, such as something the user said, is a short-term entry.
- On a no-go, this step is skipped, since the change may never exist.

**Step 4: A second agent answers the same questions, cold.** It sees what was said and what's already written, but not the reviewer's answers. A long-term change is made only when both proposed it on their own, because a wrong change is carried by every review after it, and a missed one just comes back.

---

## Phase 2: Write

The reviewer makes the changes itself, and the history is what keeps them reversible.

**Step 5: Short-term memory is updated, and what stopped coming back drops off.** An entry that hasn't been seen in about 20 harvests is removed. A no to promoting one stays, so it's asked again only if it keeps coming up.

**Step 6: Long-term memory is updated, and each change is a commit.** Where it goes depends on who controls the source, as `sources.md` records.

- A source the user controls gets a direct commit, pushed to its default branch, written the way that source's own contributing guide says.
- Knowledge of a source someone else controls, or of no source, goes in `local/`, as a commit in its own history, and its README index is updated. Their source is never changed.
- The plugin is never changed. A lesson about how it should work goes in the note as a suggestion.

---

## Phase 3: Tell

The note is the whole report. It says what changed and leaves out the reasoning, which the commits and memory already hold.

**Step 7: The user gets a short note.** What changed under coding standards and under the product, or that nothing did, any comment whose claim proved false, and any suggestion for the plugin.

**Step 8: The user answers any promotion question.** Each shows the user's own words and the rule it would write.

- A yes writes the rule into long-term memory.
- A no is kept on the entry.
- An unanswered one waits for the next harvest, and holds nothing up.

---

## When It Goes Wrong

| Symptom | Where it died | What it means |
|---|---|---|
| The same correction keeps coming back | Step 2 | It was never compared with short-term memory, or a promotion question was never asked |
| A rule appeared that the user never agreed to | Step 6 | A preference skipped the promotion question |
| Docs describe something the author changed after the approval | Step 3 | The accepted risk of harvesting at the approval. A later harvest corrects it |
| Long-term memory fills with one-offs | Step 4 | A change went in on one agent's word |
| The user is reading long reports | Step 7 | The note carried the reasoning, not just what changed |
| A wrong change can't be found or undone | Step 6 | It was made without a commit |
| A team's docs changed | Step 6 | The source was recorded as the user's when it isn't |
