# Core Product Flow — A Pull Request Handed Over to a Final Approval

**Created:** 2026-11-06
**Updated:** 2026-11-24
**Context:** Starts when the user hands the reviewer a pull request. Ends when the user gives the go or the no-go, and the pull request moves on to its harvest.

## Summary

This flow is Phase 2 of `cpf_code-review.md`, in detail, seen from the user's side. The reviewer reads the pull request three times and writes one review, the user processes it and says to post it, and the author responds until the user approves. Read it before handing over a pull request. The steps are `../procedures/sop_review-a-pr.md`.

## Flow at a Glance

```
PHASE 1: READ (the reviewer, alone)

  1. The user hands over a pull request
  2. The reviewer loads what it knows, and writes a brief
  3. Three independent reads
  4. Every claim checked before it's written down
       → a check that can't run here: a Question, or named for the user

        ~~~~~~~~ handoff to the orchestrator ~~~~~~~~

PHASE 2: WRITE (the orchestrator, alone)

  5. Each finding held to one test
       → a problem this change didn't cause: named for the user
       → a claim that doesn't hold, or a finding that fails the test: dropped
  6. One review written, every Call left to the author

        ~~~~~~~~ handoff to the user ~~~~~~~~

PHASE 3: PROCESS (the user)

  7. "The review is ready. Process it?"
  8. What the pull request does, until the user understands it
  9. The Calls, acknowledged as real and correct
  10. "Calls are right, post it"
       → not now: nothing is posted
       → Ball: the author

        ~~~~~~~~ handoff to the pull request's author ~~~~~~~~

PHASE 4: RE-REVIEW (the author, then the reviewer, then the user)

  11. The author pushes, and the user says re-review
  12. The push is checked like a new diff
  13. The threads are worked through
        → the code meets its comment: resolved
        → it doesn't, or only an answer came: a reply, and it stays open
  14. The user processes the re-review and says to post it
       → loops 11–14 until the reviewer thinks it's good to go

        ~~~~~~~~ handoff to the user ~~~~~~~~

PHASE 5: APPROVE (the user)

  15. The user gives the go or the no-go
        → merged first: the reviewer checks what merged, and says what it leaves out
```

**Every stop ends with a ball line,** saying who has the ball and what for, and updates the review's row in `local/ledger.md`. `review status` reads the ledger back and says what's waiting on whom. The reviewer never messages another session. When the author is a session, the user tells it.

---

## Phase 1: Read

The reviewer works alone, and changes nothing that anyone else can see.

**Step 1: The user hands over a pull request.** They say `review this PR <link>`.

**Step 2: The reviewer loads what it knows, and writes a brief.** It starts from what's changing and works outward, using each README as an index and opening only what it needs.

- **What's changing:** the pull request and its ticket, since a recorded decision is a product call already made.
- **What's known:** the product's docs, the coding standards and the system knowledge for the areas the pull request touches, and any References row that covers this repository.
- **The brief** names the ticket and where its decisions are written, and each document opened and each skipped, with why. Every read starts from it, the user sees it at step 9, and each re-review adds the decisions made since.

**Step 3: Three independent reads.** Each read has its own copy of the code and sees nothing of the others' work, because separate reads find different things. Each runs the checks itself, and reads the diff in the order the code runs, from where the person using the product starts.

**Step 4: Every claim checked before it's written down.** A wrong comment costs the user a read, and it can be applied by someone who trusts it.

- A claim about what the product does is traced from where the person starts, not from the line it sits on.
- A claim that code can change safely, and any code or command a finding recommends, is tried in the read's copy, then undone. One that fails is fixed or dropped.
- What can't be checked here, such as an end-to-end test that needs the user's device, becomes a Question, or is named for the user.

---

## Phase 2: Write

This phase decides how long the review is. Reads find everything they can, and the orchestrator decides what's worth the author's time.

**Step 5: Each finding is held to one test.** Its kind is set first: a finding that contradicts a recorded decision is a Call that names the decision, never a Fix, and anything held only in short-term memory is a Nit or a Question, never a Fix or a Call. Then it goes out only if its claim still holds when checked again, and one of these is true.

- Code this pull request wrote has a defect, however rare, for anyone who depends on it: the person using the product, a client or caller, whoever runs it, or its security. That holds even if something similar existed before.
- It falls short of what the pull request's description says it does, including its summary.
- A check the reads ran fails.
- It breaks a long-term standard about what the code does.
- It leaves a document the install points to wrong. A document that's only incomplete doesn't meet this.
- It would lead the next person to change this code into a bug. A name that could be better isn't enough.

The number of reads that found a Fix or Call doesn't decide whether it goes out. Everything else is dropped, with three exceptions: up to five Nits per pull request, a Question that bears on the test, and one Fix listing the tests a standard says new behavior should have. Dropped findings, and problems this change didn't cause, are listed for the user at step 9.

**Step 6: One review written, every Call left to the author.** It follows `writing-styles/review-comments/`: a body that opens with a whiteboard paragraph in the reviewer's own words, from the code it read, never the description's, and one comment per finding. Every Call is left to the author, with the reviewer's recommendation labeled as the reviewer's, such as "The reviewer's recommendation, not decided by Sam."

---

## Phase 3: Process

The user's name goes on the review, so they understand it before it's posted. They aren't asked to decide anything in it.

**Step 7: "The review is ready. Process it?"** The review waits for the user, who may be away when it's done.

**Step 8: What the pull request is for, until the user understands it.** The reviewer shows the body's whiteboard paragraph, and the user says it's understood, or asks until it is. Every walk, in a re-review and at the approval too, opens with that paragraph, so a user coming back cold knows what the review is about.

**Step 9: The Calls, acknowledged as real and correct.** Each Call comes with the problem as the reviewer sees it, the decision the author has to make, the reviewer's recommendation and the alternative. The user isn't choosing, since the author decides. They're confirming each Call is real and correct, because a Call can block a pull request. Then, in a line each: the counts, what was opened and skipped, what this change didn't cause, and each dropped finding.

**Step 10: "Calls are right, post it."** Or the user names a Call that's wrong, which is rewritten or dropped and shown again, a dropped finding to bring back, or says not now, which posts nothing. It posts as a comment review, never an approval, from the user's own GitHub sign-in unless an amendment says otherwise. Ball: the author, and the user tells them the review is up.

---

## Phase 4: Re-Review

The author owns the code. The reviewer owns its review, and never edits the code, commits or pushes on someone's behalf.

**Step 11: The author pushes, and the user says re-review.** The author applies the Fixes and Nits, decides each Call and replies to the Questions. The user says `re-review <pull request>`.

**Step 12: The push is checked like a new diff.** A response commit can be wrong in the same ways the first one was. The reviewer runs the checks, then one fresh read covers what changed and every reply, from the brief, without seeing the earlier review.

- Every reply is a claim, and each place it says changed is opened.
- A reason disproved anywhere is searched for in every copy, docs and descriptions included.
- Every document the push writes or leaves is read against the code, and every result the author reports is held to the standards.

**Step 13: The threads are worked through.** A thread resolves only when the code meets its comment, or a Question's thread says what happened. An answer alone never resolves one, and a reply that relays the user's decision, or a Question the author can't answer, waits for the user's own word.

- A change that doesn't match its comment gets a reply, and stays open. A new defect gets a new thread, and a new Nit only if the review posted fewer than five.
- When a change rests on a false claim of the review's, or the review's own suggestion has a defect, it's corrected on its thread, whatever the Nit count.

**Step 14: The user reads the re-review and says to post it.** One message, ready when the user comes back: the whiteboard paragraph, what the pull request does now where it changed, how each Call landed and whether the code does what the author decided, any new Calls, anything waiting on the user's word, the count of threads resolved, and a line for each thread left open. The loop runs until every thread is resolved or left open on the user's word.

---

## Phase 5: Approve

**Step 15: The user gives the go or the no-go.** The reviewer asks once it thinks the pull request is good to go, with the whiteboard paragraph, the commit it checked, what an approval would leave out, and any Question still open. The user reads the code and decides, or sends what still needs changing back through Phase 4.

- The commit on GitHub is compared with what was checked before anything is approved. The user approves it themselves, unless an amendment lets the reviewer's account approve on their word.
- If the author merged first, nothing is approved. The reviewer checks what merged and says what it leaves out.

Once the user has decided, the harvest in `cpf_harvesting.md` runs.

---

## When It Goes Wrong

| Symptom | Where it died | What it means |
|---|---|---|
| The review is too long to read | Step 5 | Findings went out that fail the test, or Nits weren't held to five |
| A finding the user wanted never went out | Step 5 | The orchestrator dropped it. Bringing it back at step 10 teaches the next harvest what was missed |
| A comment fought a decision already made | Step 2 or 5 | The ticket's decisions weren't read, or the finding went out as a Fix |
| The author's reply said Sam picked an option on a Call Sam never decided | Step 6 | The recommendation wasn't labeled as the reviewer's, not decided by Sam |
| Nobody knew who had the ball after the review was posted | Step 10 | A stop ended without a ball line, or the ledger wasn't updated |
| A thread was resolved on the author's reply, and the reply was false | Step 13 | An answer was taken as the fix. Only code that meets the comment resolves a thread |
| A wrong line a response commit wrote in a doc went through | Step 12 | The push was checked against its threads, not read like a new diff |
| A reason the review disproved was still in a doc | Step 12 | It was fixed where the thread pointed, and no one searched for its other copies |
| The approval covers code nobody checked | Step 15 | The commit on GitHub wasn't compared with what was checked |
| The user asked whether their docs were read | Step 2 or 9 | The brief didn't say what was opened and skipped |
