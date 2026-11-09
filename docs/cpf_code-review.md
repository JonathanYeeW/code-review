# Core Product Flow — Fresh Install to a Seasoned Reviewer

**Created:** 2026-11-06
**Updated:** 2026-11-06
**Context:** Starts when the user puts the plugin somewhere their LLM can read it. Ends when reviews have stopped teaching the reviewer much, and the user's corrections and promotion questions have become rare.

## Summary

This document is the whole path of Code Review, seen from the user's side. The flow has four phases: the user sets up an install, the reviewer learns the user on small pull requests, it reviews real pull requests, and it harvests what each one taught it. Read it to place every other file: each phase names the CPF and procedure that hold its detail.

## Flow at a Glance

```
PHASE 1: SET UP (the user, once per install)

  1. The user puts the plugin where the LLM can read it
  2. They say which repositories and which documentation it covers
  3. They adopt any existing rules worth keeping
  4. They record how this machine differs, if it does

        ~~~~~~~~ handoff to the reviewer ~~~~~~~~

PHASE 2: TEACH (the reviewer guesses, the user answers)

  5. The reviewer opens a stack of five one-change pull requests
       → more than one change in a pull request: the answer can't be read
  6. The user answers each one
  7. The stack is harvested (steps 16–19)

       → loops 5–7 until the user is comfortable

        ~~~~~~~~ handoff to a real pull request ~~~~~~~~

PHASE 3: REVIEW (the reviewer reads, the user decides)

  8.  The user says "review this PR <link>"
  9.  The reviewer loads what it knows
  10. Three independent reads
  11. One review written from the three
        → a claim that doesn't hold is dropped
  12. The Calls are addressed
  13. The user sees the review and says to post it
  14. The pull request's author responds, and the reviewer checks the result
        → a change that doesn't match its comment: back to the author
        → a wrong claim behind a change: the change stops, and the user is asked
  15. The reviewer says it's ready, and the user gives the go or the no-go

        ~~~~~~~~ handoff to the harvest ~~~~~~~~

PHASE 4: HARVEST (the reviewer, on its own)

  16. What did this teach about how the user likes code written?
  17. What did this teach about how the product works?
  18. The reviewer updates its memory
  19. The user gets a short note, and any promotion questions

       → back to step 8 on the next pull request
```

---

## Phase 1: Set Up

The user tells the plugin where it is working, once per install. The detail is in `cpf_setting-up.md`, and the procedure is `sop_set-up.md`.

**Step 1: The user puts the plugin where the LLM can read it.** Then they tell the LLM to follow the setup procedure. How the LLM finds the plugin depends on the user's own workflow.

**Step 2: They say which repositories and which documentation it covers.** The reviewer needs to know where the code is and where its primary documentation and coding standards live.

- The reviewer records these as pointers and reads them. It doesn't copy them.
- A company's standards usually apply across its repositories, so they're named once for the install.
- For each source, the LLM decides whether the user controls it and records that. It decides where long-term memory lives: in the source if the user controls it, and in the install if someone else does or none exists.

**Step 3: They adopt any existing rules worth keeping.** The reviewer lists what it found, and the user names what they want.

- An adopted rule becomes the reviewer's own, and the rest stay as context it reads.
- When a primary source later disagrees with an adopted rule, the reviewer asks the user once and remembers the answer.

**Step 4: They record how this machine differs, if it does.** Each difference is an amendment, which changes how a step is carried out and never what the step is.

- A machine might have a reviewer account that can post, or run a different LLM client.
- With no amendment, the plain defaults apply.

---

## Phase 2: Teach

The reviewer learns the user from their reactions, and small pull requests are the cheapest place to get them. This is still setup, so the detail is in the same `cpf_setting-up.md` and `sop_set-up.md`.

**Step 5: The reviewer opens a stack of five one-change pull requests.** They're stacked on each other, and each is one small guess on code that's already merged, such as "I'd make this change, what do you think?" One change per pull request, because the user's reaction has to say which change it was about. A wrong guess is cheap, because the code already works without it.

**Step 6: The user answers each one.** They correct it, close it or say to merge it, and say why if it isn't obvious.

**Step 7: The stack is harvested.** It runs the same harvest as Phase 4, so corrections become short-term memory and ones that come back become promotion questions. The sign it's working is fewer new promotion questions from one stack to the next.

---

## Phase 3: Review

This is what the plugin is for: the reviewer reads and the user decides. The detail is in `cpf_reviewing-a-pr.md`, and the procedure is `sop_review-a-pr.md`.

**Step 8: The user says "review this PR <link>".** A small pull request is a good first one.

**Step 9: The reviewer loads what it knows.** It starts from the pull request and its ticket, then what it knows about this part of the product and anything related, then the coding standards, using each README as a search index.

**Step 10: Three independent reads.** Each read reviews the pull request alone, and none sees another's findings, because separate reads find different things.

**Step 11: One review written from the three.** An orchestrator checks each claim, holds each finding to one test, and writes one review. This step decides how long the review is.

- A claim that doesn't hold against the code is dropped, because a wrong comment costs the user a read.
- Each comment is a Call, a Fix, a Nit or a Question, written the way `writing-styles/review-comments/` shows.
- Anything held only in short-term memory is a Nit or a Question, never a Fix or a Call.

**Step 12: The Calls are addressed.** The reviewer brings each Call to the user before anything is posted, and it goes out carrying the user's recommendation, the reviewer's, or none, as `cpf_reviewing-a-pr.md` says.

**Step 13: The user sees the review and says to post it.** Nothing is posted without the user's word, and the reviewer tells them here what it couldn't check and what it chose not to raise.

- An amendment says how posting works on this machine.
- With none, it's posted from the user's own GitHub sign-in.

**Step 14: The pull request's author responds, and the reviewer checks the result.** The author applies the Fixes and Nits, decides each Call, and replies to the Questions. The reviewer works the threads until it thinks it's good to go.

**Step 15: The reviewer says it's ready, and the user gives the go or the no-go.** The reviewer names anything an approval would leave out, such as a check it couldn't run. Nothing merges on the reviewer's word alone.

---

## Phase 4: Harvest

The reviewer turns what happened into what the next review knows. It runs on its own once the user has decided, without waiting for the merge, and the user hears only the result and any promotion question. The review finishes there, so nothing stays open to check back on later. The accepted risk is that the author changes something after the approval, or never merges it, and memory is then a little wrong until a later harvest of that area corrects it. The detail is in `cpf_harvesting.md`, and the procedure is `sop_harvest.md`.

**Step 16: What did this teach about how the user likes code written?** Each correction is compared against what the reviewer already holds.

- A correction is anything the user changed, closed or overruled, or had to ask about.
- A new correction goes into short-term memory, in the user's words.
- One that has come back two or three times becomes a promotion question.

**Step 17: What did this teach about how the product works?** The approved code changes what's true about the product.

- A fact the reviewer can check against the approved code, and a second agent also proposes, goes to long-term memory, with no question to the user.
- One it can't check yet, such as something the user said, stays in short-term memory.
- On a no-go, nothing is learned about the product, since the change may never exist.

**Step 18: The reviewer updates its memory.** It decides and makes the change on its own, with a second agent agreeing on anything long-term, and short-term entries that never came back drop off. A long-term product file is written as a CPF or a wiki, shaped against `documentation-examples`, a coding standard as a rule, and the directory's README index is updated with it. Where each change is committed is in `cpf_harvesting.md`.

**Step 19: The user gets a short note, and any promotion questions.** The note says what changed, or that nothing did, and leaves out the thinking behind it.

- A promotion question shows the user their own words and the rule it would write, and asks whether it's a rule.
- A yes writes the rule into long-term memory. A "no" is remembered, and the reviewer asks again only if the pattern keeps coming up.

A note might read:

```
Harvest of #212
Coding standards: 2 added to short-term, 1 promotion question below
Product: 3 added to short-term, 2 confirmed against the code
Nothing removed
```

The flow ends gradually rather than at a step. Promotion questions are common in the first stacks and should be rare by 20 or 30 reviews, which is how the user can tell the reviewer has learned them.

---

## When It Goes Wrong

| Symptom | Where it died | What it means |
|---|---|---|
| The review is too long to read | Step 11 | The orchestrator kept what it should have dropped, or the comments don't match the examples |
| A comment rests on something false about the product | Step 11 | Product memory was used as evidence instead of checked against the code |
| A comment cites a rule the user can't find | Step 18 | The rule isn't written down where the comment says |
| A rule was promoted that the user never agreed to | Step 18 | A preference skipped the promotion question |
| Promotion questions aren't becoming rarer | Step 7 or 19 | The reviewer isn't holding what it learned, or the code is changing faster than it learns |
| A plugin update overwrites something the user changed | Step 4 | The change was made to the plugin instead of an amendment |
