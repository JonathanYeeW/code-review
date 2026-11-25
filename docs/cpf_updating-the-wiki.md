# Core Product Flow — Merged Changes to Up-to-Date Product Docs

**Created:** 2026-11-24
**Updated:** 2026-11-24
**Context:** Starts when the user says `update the wiki`, or a harvest runs for a merged pull request. Ends when the product's docs are committed where they live, and pushed on `push the wiki`.

## Summary

This flow is how a product's docs keep up with its code, seen from the user's side. The reviewer lists everything merged since the docs were last synced, reviewed or not, finds what each change made wrong, checks it against the code, and writes it. The user answers only questions, and says when to push. Read it to understand what the reviewer writes into the product's docs on its own. The steps are `../procedures/sop_update-the-wiki.md`.

## Flow at a Glance

```
PHASE 1: WHAT MERGED (the reviewer)

  1. The product's docs, and the commit they're synced through
  2. Everything merged since then

        ~~~~~~~~ handoff to the agents ~~~~~~~~

PHASE 2: WHAT IT MADE WRONG (agents, on their own)

  3. One agent per change finds what's now false or missing
       → a claim that doesn't hold against the code: dropped
  4. What's kept is sorted
       → a procedure, or a disagreement with the system: a question

        ~~~~~~~~ handoff to the docs ~~~~~~~~

PHASE 3: WRITE (the reviewer, then the user only if asked)

  5. Each fact is written, and the synced commit moves forward
  6. A commit where the docs live, and a ledger row
  7. One message, and any questions

        ~~~~~~~~ handoff to the user ~~~~~~~~

PHASE 4: PUSH (on the user's word)

  8. "Push the wiki"
```

---

## Phase 1: What Merged

**Step 1: The product's docs, and the commit they're synced through.** The product's row in `local/product/README.md` says where its docs live and how a change lands there. The docs' own README names the last commit they're synced through. With none yet, the user says where to start.

**Step 2: Everything merged since then.** On each of the product's repositories, reviewed or not. A harvest's call reaches only its own product, and still covers everything since the synced commit, not only that pull request.

---

## Phase 2: What It Made Wrong

**Step 3: One agent per change finds what's now false or missing.** Each looks at what the person can do, how the product's code is put together, and where it departs from the system knowledge. Each claim is checked against the code at the commit being synced to, and one that doesn't hold is dropped. Nothing is voted on.

**Step 4: What's kept is sorted.** Only a product fact is written without asking.

- **A product fact** is written.
- **A procedure** that disagrees with the code is a question, never an edit, since a procedure is how people work, and only partly what the code does.
- **A disagreement with the system knowledge** is a question: did the system change, or is this product an exception?

---

## Phase 3: Write

**Step 5: Each fact is written, and the synced commit moves forward.** Each goes into the doc it belongs in, shaped against its type, and each README it touches is updated.

**Step 6: A commit where the docs live, and a ledger row.** Docs in a repository are committed on a branch, and get a Waiting to push row in `local/ledger.md`. Any Docs behind row the update now covers comes off. Docs kept in `local/` are committed there. Docs with no git, such as a hosted wiki, are written in place.

**Step 7: One message, and any questions.** What changed in a line each, the branch, and each question. The user answers each in a sentence, and nothing else waits on it.

---

## Phase 4: Push

**Step 8: "Push the wiki."** For each Waiting to push row, the branch is pushed, or opened as a docs pull request, as the product's row says that repository takes a docs change. Then the row comes off the ledger. Ball: nobody.

---

## When It Goes Wrong

| Symptom | Where it died | What it means |
|---|---|---|
| A product-doc change waited on two agents agreeing and never landed | Step 3 | A change was held for a vote. Each claim is checked against the code instead |
| A push to a docs source was refused, and the change sat in a file nobody looked at | Step 6 or 8 | It wasn't on a branch with a Waiting to push row, so nothing said it was still owed |
