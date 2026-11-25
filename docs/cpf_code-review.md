# Core Product Flow — Fresh Install to a Seasoned Reviewer

**Created:** 2026-11-06
**Updated:** 2026-11-24
**Context:** Starts when the user puts the plugin somewhere their LLM can read it. Ends when the user's corrections have become rare.

## Summary

This document is the whole path of Code Review, seen from the user's side. The flow has three phases: the user sets up an install, the reviewer reviews real pull requests, and it harvests what each one taught it, bringing the product's docs up to date as changes merge. Read it to place every other file: each phase names the CPF and procedure that hold its detail.

## Flow at a Glance

```
PHASE 1: SET UP (the user, once per install)

  1. The user puts the plugin where the LLM can read it
  2. They name the products, repositories and docs it covers
  3. They confirm what already exists, and adopt any rules worth keeping
  4. They record how this machine differs, if it does
  5. The user updates the wiki, so the first review starts from the product as it is

        ~~~~~~~~ handoff to a real pull request ~~~~~~~~

PHASE 2: REVIEW (the reviewer reads, the user understands, the author decides)

  6.  The user says "review this PR <link>"
  7.  Three independent reads, from one brief
  8.  One review written from the three
        → a claim that doesn't hold is dropped
  9.  The user processes it: what it does, then the Calls
  10. "Calls are right, post it"
        → Ball: the author
  11. The author pushes, and the reviewer checks it like a new diff
        → a thread resolves only when the code meets its comment
  12. The reviewer says it's ready, and the user gives the go or the no-go

        ~~~~~~~~ handoff to the harvest ~~~~~~~~

PHASE 3: HARVEST (the reviewer, on its own)

  13. What did this teach about how the user likes code written?
  14. The reviewer updates its standards, promoting what keeps coming back
  15. The product's docs, once the pull request has merged
  16. The user gets a short note

       → back to step 6 on the next pull request
```

**Every stop ends with a ball line,** saying who has the ball and what for, and `local/ledger.md` keeps every open review and every docs change waiting to push. `review status` reads it back.

---

## Phase 1: Set Up

The user tells the plugin where it's working, once per install. The detail is in `cpf_setting-up.md`, and the procedure is `sop_set-up.md`.

**Step 1: The user puts the plugin where the LLM can read it.** Then they tell the LLM `set up the reviewer`.

**Step 2: They name the products, repositories and docs it covers.** An install can start from another install's long-term standards, or empty.

**Step 3: They confirm what already exists, and adopt any rules worth keeping.** The reviewer finds the docs and standards in each repository and decides who controls each source. A source the user can't change is read, never changed.

**Step 4: They record how this machine differs, if it does.** Each difference is an amendment, which changes how a step is carried out and never what the step is, such as a reviewer account that posts. With none, the plain defaults apply.

**Step 5: The user updates the wiki.** `update the wiki` brings each product's docs up to date with what's merged, as `cpf_updating-the-wiki.md` says.

---

## Phase 2: Review

This is what the plugin is for. The detail is in `cpf_reviewing-a-pr.md`, and the procedure is `sop_review-a-pr.md`.

**Step 6: The user says "review this PR <link>".** A small pull request is a good first one.

**Step 7: Three independent reads, from one brief.** The reviewer writes a brief of what's changing and what it opened, and each read starts from it alone, because separate reads find different things.

**Step 8: One review written from the three.** An orchestrator checks each claim, holds each finding to one test, and writes one review. This step decides how long the review is.

- A claim that doesn't hold against the code is dropped, because a wrong comment costs the user a read.
- Each comment is a Call, a Fix, a Nit or a Question, written the way `writing-styles/review-comments/` shows.
- Every Call is left to the author, with the reviewer's recommendation labeled as the reviewer's.

**Step 9: The user processes it: what it does, then the Calls.** The user confirms they understand what the pull request does, then that each Call is real and correct. They don't decide the Calls.

**Step 10: "Calls are right, post it."** Nothing is posted without the user's word. An amendment says how posting works on this machine. The reviewer never messages another session, so the user tells the author.

**Step 11: The author pushes, and the reviewer checks it like a new diff.** One fresh read covers the push, every reply is checked as a claim, and the user reads the re-review, which opens with what the pull request is for, before it's posted. A thread resolves only when the code meets its comment. An answer alone never resolves one.

**Step 12: The reviewer says it's ready, and the user gives the go or the no-go.** The reviewer names anything an approval would leave out. Nothing merges on the reviewer's word alone.

---

## Phase 3: Harvest

The reviewer turns what happened into what the next review knows. It runs on its own once the user has decided, without waiting for the merge, and asks nothing, except, when the user starts one by hand on a pull request still open, whether it was a go, and, in a later session on the user's own sign-in, which replies under their name are theirs. The detail is in `cpf_harvesting.md`, and the procedure is `sop_harvest.md`.

**Step 13: What did this teach about how the user likes code written?** A correction is anything the user changed, closed or overruled, or had to ask about. A new one goes into short-term memory, in the user's words.

**Step 14: The reviewer updates its standards, promoting what keeps coming back.** A correction that has come back two or three times, by judgment rather than a count, becomes a long-term rule, with no question to the user. Entries that never came back drop off. Every change is a commit in `local/`, so git is the undo.

**Step 15: The product's docs, once the pull request has merged.** The harvest runs `update the wiki`, which writes what the merge made wrong in the docs and waits for `push the wiki` to push it.

**Step 16: The user gets a short note.** It says what changed, or that nothing did, and leaves out the thinking behind it.

The flow ends gradually rather than at a step. Corrections are common in the first reviews and should be rare by 20 or 30, which is how the user can tell the reviewer has learned them.

---

## When It Goes Wrong

| Symptom | Where it died | What it means |
|---|---|---|
| The review is too long to read | Step 8 | The orchestrator kept what it should have dropped, or the comments don't match the examples |
| A comment rests on something false about the product | Step 8 | The product's docs were used as evidence instead of checked against the code |
| The author credited Sam with a pick on a Call Sam never made | Step 8 | The recommendation wasn't labeled as the reviewer's |
| Nobody knew who had the ball after the review was posted | Step 10 | A stop ended without a ball line, or the ledger wasn't updated |
| A false reply resolved a thread, or a wrong doc line in a push went through | Step 11 | The push was checked against its threads, not like a new diff |
| A comment cites a rule the user can't find | Step 14 | The rule isn't written down where the comment says |
| A product-doc change never landed | Step 15 | It waited on something other than `push the wiki`, with no Waiting to push row |
| A plugin update overwrites something the user changed | Step 4 | The change was made to the plugin instead of an amendment |
