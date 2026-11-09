# Core Product Flow — A Pull Request Handed Over to a Final Approval

**Created:** 2026-11-06
**Updated:** 2026-11-06
**Context:** Starts when the user hands the reviewer a pull request. Ends when the user gives the go or the no-go, and the pull request moves on to its harvest.

## Summary

This flow is Phase 3 of `cpf_code-review.md`, in detail, seen from the user's side. The reviewer reads the pull request three times and writes one review, its Calls are addressed, the user says to post it, and the author responds until the user approves. Read it before handing over a pull request. The steps are `../procedures/sop_review-a-pr.md`.

## Flow at a Glance

```
PHASE 1: READ (the reviewer, alone)

  1. The user hands over a pull request
  2. The reviewer loads what it knows
  3. Three independent reads
  4. Every claim checked before it's written down
       → a check that can't run here: a Question, or named for the user

        ~~~~~~~~ handoff to the orchestrator ~~~~~~~~

PHASE 2: WRITE (the orchestrator, alone)

  5. Each finding held to one test
       → a problem this change didn't cause: named for the user
       → a claim that doesn't hold, or a finding that fails the test: dropped
  6. One review written

        ~~~~~~~~ handoff to the user ~~~~~~~~

PHASE 3: ADDRESS AND POST (the user)

  7. The Calls are addressed
  8. The user says to post it
       → not now: nothing is posted

        ~~~~~~~~ handoff to the pull request's author ~~~~~~~~

PHASE 4: RESPOND AND APPROVE (the author, then the user)

  9.  The author responds on the pull request
  10. The reviewer checks the result, on the threads
        → addressed: the thread is resolved
        → not addressed: a reply, and the thread stays open
        → something new: a new thread
        → a wrong claim behind a change: it stops, and the user is asked
       → loops 9–10 until the reviewer thinks it's good to go

  11. The user gives the go or the no-go
        → merged first: the reviewer checks what merged, and says what it leaves out
```

---

## Phase 1: Read

The reviewer works alone, and changes nothing that anyone else can see.

**Step 1: The user hands over a pull request.** They say `review this PR <link>`.

**Step 2: The reviewer loads what it knows.** It starts from what's changing and works outward, using each README as an index and opening only what it needs.

- **What's changing:** the pull request and its ticket, since a recorded decision is a product call already made.
- **The product:** what it already knows about this part of the product, and anything related this change could affect.
- **The coding standards:** what it holds for the areas the pull request touches.
- **The primary documentation:** anything those didn't reach, entered at the source's own README.

**Step 3: Three independent reads.** Each read has its own copy of the code, loads what it knows for itself, and sees nothing of the others' work, because separate reads find different things.

- Each runs the build, type checks, lint and tests itself.
- Each reads the diff in the order the code runs, from where the person using the product starts.

**Step 4: Every claim checked before it's written down.** A wrong comment costs the user a read, and it can be applied by someone who trusts it.

- A claim about what the product does is traced from where the person starts, not from the line it sits on.
- A claim that code can change safely, and any code or command a finding recommends, is tried in the read's copy, then undone. One that fails is fixed or dropped. One that would write outside the read's copy isn't run, and its How I checked says so.
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

The number of reads that found something doesn't decide. Everything else is dropped, with three exceptions: a Nit can go out, up to the five most worth making per pull request, favoring what the most reads found, then what breaks an explicit long-term rule, a Question stays if it bears on the test, and new behavior a standard says should have a test goes in one Fix per pull request. A Nit is a preference, a standard about how code reads, or anything else that only tidies, such as a misplaced comment. It's never a defect. Dropped findings, and problems this change didn't cause, are listed for the user in step 8, where any can be brought back.

**Step 6: One review written.** It follows `writing-styles/review-comments/`: a body restating the pull request as the reviewer understood it, and one comment per finding.

- Where reads found the same thing, the clearest comment stays.
- A disagreement the code can settle is settled in the code. One the user has to settle becomes a Call.

---

## Phase 3: Address and Post

Only the Calls and the word to post ask anything of the user. Fixes, Nits and Questions don't.

**Step 7: The Calls are addressed.** The reviewer brings the Calls in a message of their own, since they get skipped beside anything else, and each goes out carrying one of three.

- **The user's recommendation:** the user says what they'd do, in their own words. A letter alone gets asked for once more as a sentence, because someone who can't say it hasn't read it.
- **The reviewer's recommendation:** the user doesn't have time, so the LLM's pick goes out, marked as not reviewed by them.
- **No recommendation:** the options go out as they are, for the author to decide.

**Step 8: The user says to post it.** They see the review, with what it left out beside it.

- The ask gives the review, the counts, what the reads opened, what couldn't be checked, what this change didn't cause, and each dropped finding.
- "Not now" posts nothing, and the user can also wait for a push the author still has to make.
- An amendment says how posting works on this machine. With none, it's posted from the user's own GitHub sign-in. The session the user is talking to makes the post, since an agent's post on a relayed word can be refused.

---

## Phase 4: Respond and Approve

The author owns the code. The reviewer owns its review, and the user owns the approval.

**Step 9: The author responds on the pull request.** They apply the Fixes and Nits, decide each Call, and reply to the Questions. The reviewer never edits the code, commits or pushes on someone's behalf.

**Step 10: The reviewer checks the result, on the threads.** It starts when the author reports they're done, or when the user says `re-review <pull request>`. The checks run again at the pushed commit, and each thread gets one of three.

- **Addressed:** the change meets the comment, or the author answered it, so the reviewer resolves the thread.
- **Not addressed:** the reviewer replies on the thread, and it stays open.
- **Something new:** a new defect gets a new thread. A new Nit gets one only if the review posted fewer than five.

When a change turns out to rest on a wrong claim, it stops and the user is asked, so nobody builds around a false comment. The author and the reviewer loop through steps 9 and 10 until the reviewer thinks it's good to go.

**Step 11: The user gives the go or the no-go.** The reviewer asks once it thinks the pull request is good to go, with the commit it checked, what an approval would leave out, and any Question still open. The user reads the code and decides.

- The commit on GitHub is compared with what was checked before anything is approved.
- An amendment says whether the reviewer's account approves on the user's word. With none, the user approves it themselves.
- If the author merged first, nothing is approved. The reviewer checks what merged and says what it leaves out.

Once the user has decided, the harvest in `cpf_harvesting.md` runs.

---

## When It Goes Wrong

| Symptom | Where it died | What it means |
|---|---|---|
| The review is too long to read | Step 5 | Findings went out that fail the test, or Nits weren't held to five |
| A finding the user wanted never went out | Step 5 | The orchestrator dropped it. Bringing it back at step 8 teaches the next harvest what was missed |
| The product gained behavior nobody chose | Step 4, then 10 | A claim wasn't traced from where the person starts, and someone built around it |
| A comment fought a decision already made | Step 2 or 5 | The ticket's decisions weren't read, or the finding went out as a Fix |
| The author took the reviewer's recommendation as the user's | Step 7 | The Call went out unmarked |
| The Calls were never addressed | Step 7 | They were sent beside something else, such as the ask to post |
| The approval covers code nobody checked | Step 11 | The commit on GitHub wasn't compared with what was checked |
| The user asked whether their docs were read | Step 2 or 8 | The reads went to documents by name instead of a source's README, or the ask to post didn't say what they opened |
