# Core Product Flow — A Decided Pull Request to a Short Note

**Created:** 2026-11-06
**Updated:** 2026-11-24
**Context:** Starts when the user gives the go or the no-go on a pull request. Ends when the user has a short note saying what changed, with nothing to answer.

## Summary

This flow is Phase 3 of `cpf_code-review.md`, in detail, seen from the user's side. The reviewer works out what the review taught it about how the user writes code, makes the changes itself, brings the product's docs up to date if the pull request merged, and tells the user what changed. It asks nothing. Read it to understand what the reviewer changes on its own. The steps are `../procedures/sop_harvest.md`.

## Flow at a Glance

```
PHASE 1: LEARN (the reviewer, on its own)

  1. Everything the user said is gathered
  2. The corrections are found
  3. Each is compared with the coding standards
       → come back two or three times: promoted

        ~~~~~~~~ handoff to memory ~~~~~~~~

PHASE 2: WRITE (the reviewer, on its own)

  4. Short-term and long-term standards are updated
  5. The product's docs, if the pull request merged
       → not merged: the next update reaches it
  6. The ledger, and a commit

        ~~~~~~~~ handoff to the user ~~~~~~~~

PHASE 3: TELL (the reviewer)

  7. The user gets a short note
```

---

## Phase 1: Learn

The reviewer works out what changed in what it knows, without asking. The one exception is a harvest in a later session on the user's own sign-in, where the reviewer's earlier replies carry the user's name: it shows them and asks which are the user's. It starts at once and doesn't wait for the merge.

**Step 1: Everything the user said is gathered.** From the pull request's threads, the author's comments, the ticket and the session, in the user's words. The reviewer's own posts are left out, since on the user's sign-in they carry the user's name.

**Step 2: The corrections are found.** A correction is anything the user changed, closed or overruled, including a Call they said was wrong, a dropped finding they brought back, and anything they had to ask that the review should have told them. A comment whose claim proved false, or that fell short, is noted too.

**Step 3: Each is compared with the coding standards.** By principle and by the user's words, not by exact match.

- A new one is a short-term entry.
- One that's come back two or three times, by judgment rather than a count, is promoted to a long-term rule. Recurrence across reviews is the evidence, and nothing is voted on.
- One that goes against a long-term standard becomes an exception to it.

---

## Phase 2: Write

The reviewer makes the changes itself, and the history in `local/` is what keeps them reversible.

**Step 4: Short-term and long-term standards are updated.** A promotion is written as a rule in the long-term folder, its README index is updated, and the short-term entry is removed. An entry that hasn't been seen in about 20 harvests drops off. A source the user doesn't control, and the system knowledge, are never changed: a lesson about how a system is built goes in the note.

**Step 5: The product's docs, if the pull request merged.** The harvest runs `update the wiki` for the product, as `cpf_updating-the-wiki.md` says. If it hasn't merged, the next update reaches it.

**Step 6: The ledger, and a commit.** The review's row comes off `local/ledger.md`. A lesson about how a review runs, or a comment that shows the style somewhere the plugin's examples don't, goes under Plugin suggestions, never into the plugin. Then `local/` is committed.

---

## Phase 3: Tell

**Step 7: The user gets a short note.** What changed in the standards, or that nothing did, any rule promoted, in a line, what the wiki update changed or what it's waiting on, any comment whose claim proved false or fell short, and any plugin suggestion. It leaves out the reasoning and asks nothing. Git is the undo, and the user can say to take a promotion back.

A note might read:

```
Harvest of #221
Coding standards: 1 updated in short-term, 1 promoted: "a helper with one caller goes inline" (writing/functions.md)
Product docs: wiki_orders.md now says a removed sandwich lowers the total; waiting on `push the wiki`
Nothing removed
Plugin suggestion: a re-review should grep every copy of a reason it disproves
```

---

## When It Goes Wrong

| Symptom | Where it died | What it means |
|---|---|---|
| The same correction keeps coming back | Step 3 | It was never compared with short-term memory, or it was compared by words instead of principle |
| A rule appeared that the user didn't want | Step 4 | It was promoted on too little. The user says to take it back, and git undoes it |
| Docs describe something the author changed after the approval | Step 5 | The accepted risk of harvesting at the approval. The next wiki update corrects it |
| The user is reading long reports | Step 7 | The note carried the reasoning, not just what changed |
| A wrong change can't be found or undone | Step 6 | It was made without a commit |
| The plugin changed after a harvest | Step 6 | A lesson went into the plugin instead of Plugin suggestions |
