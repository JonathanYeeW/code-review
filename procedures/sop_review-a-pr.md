# SOP — Review a Pull Request

**Created:** 2026-11-06
**Purpose:** Review a pull request with three independent reads, post one review in the comment style on the user's word, and see it through to the user's final approval. What the user sees is `docs/cpf_reviewing-a-pr.md`.

---

## Trigger

The user says `review this PR <link>` or `review PR <number>`. Begin Phase 1.

The user says `re-review <pull request>`, or the author reports they're done. Begin Phase 6.

---

## Phase 1 — Get Ready

No input needed at this step.

1. Read `local/amendments/README.md`. Steps that name a default say which amendment replaces it.
2. Make one copy of the code per read, never using the checkout the user works in: `git -C <checkout> fetch origin`, then `git -C <checkout> worktree add --detach ../<repo>-review-<session>-a origin/HEAD`, then `-b` and `-c`, where `<session>` is unique to this session. In each, `git fetch origin pull/<number>/head`, `git checkout --detach FETCH_HEAD`, install dependencies, and build what the checks depend on.
3. **What's changing.** Read the pull request: `gh pr view <number> --json title,body,baseRefName,headRefName,files,commits`. If it names a ticket and the ticket's decisions are written down, read them.
4. **The product.** Read `local/README.md` and `local/product/short-term.md` in full. Use `local/product/long-term/README.md` as a search index, following it to a source the user controls where it points to one. Open what's known about this part of the product, and anything related that this change could affect.
5. **The coding standards.** Read `local/coding-standards/short-term.md` in full. Use `local/coding-standards/long-term/README.md` the same way, and open the files for the areas the pull request touches, and any the index says every review reads.
6. **The primary documentation.** Read the rows in `local/sources.md` whose scope covers this repository that steps 4 and 5 didn't reach. Open each source at its own README, in worktree `-a` when it's in this repository, and follow its routing to those areas. Note what was opened and what was skipped, with why, for Phase 5.

## Phase 2 — Three Reads

No input needed at this step.

Run three independent agents, each given this plugin's path, `local/`, the pull request and its own worktree. None sees another's findings. `local/amendments/reads.md` says how on this machine. Default: subagents, if the client has them.

Each read first loads what it knows for itself, as Phase 1 steps 3–6 say. Then it reviews, and writes its findings to `read-<a|b|c>.md` in this session's temporary folder, one section per finding:

1. **Run the checks:** build, typecheck, lint and all tests, a package at a time if reads are running together on one machine. Undo anything a lint script changed. Rerun a failure in a part the pull request doesn't touch: if it passes alone, note both results; if it fails again, it's a finding. Note any check that can't run here.
2. **Read the diff in the order the code runs**, from where the person using the product or the caller starts. Read for what the long-term standards and primary documentation expect, for whether the code does what the person needs, and for any document the install points to that the change leaves wrong.
3. **Check every claim before writing it down.**
   - A claim about behavior is traced from where the person or caller starts, not from the function the comment sits on.
   - A claim that code can change safely, and any code or command a finding recommends, is tried in the worktree, then undone so `git status` is clean. One that fails is fixed or dropped. One that would write outside the worktree isn't run, and its How I checked says so.
   - Before a Question, run the read-only checks that bear on it, and say what they showed under What's happening. Never call anything that writes, never touch production, and never rebuild or restart what the user runs.
4. **Label each finding** as a Product call, a Code call, a Fix, a Nit or a Question, with what's wrong and how it was checked. Don't weigh importance.

## Phase 3 — One Review

No input needed at this step. Read the writing style and its examples first: `writing-styles/review-comments/ws_review-comments.md`, then `examples/`, then `local/examples/`.

1. **Combine the same finding.** Keep the clearest, and carry over anything another read had that it lacks.
2. **Settle disagreements.** Check the claim again in a worktree. One the user has to settle becomes a Call.
3. **Check every kept claim again** as Phase 2 step 3 says. A claim that doesn't hold is dropped.
4. **Set each kind:**
   - A finding that contradicts a recorded ticket decision is a Call that names the decision, never a Fix.
   - Anything held only in short-term memory is a Nit or a Question, never a Fix or a Call.
   - A primary source's rule that hasn't been adopted is at most a Question, quoting it. A source the user controls counts as adopted. One that conflicts with a long-term standard is a Code call the first time it comes up within that source's scope, unless `local/sources.md` records the answer. Write the answer there.
   - A comment that applies a rule names it and quotes the line.
5. **Hold each Fix and Call to the test.** It goes out only if one of these is true. The number of reads that found it doesn't count.
   - Code this pull request wrote has a defect, however rare, for anyone who depends on it: the person using the product, a client or caller, whoever runs it, or its security. That holds even if something similar existed before.
   - It falls short of what the pull request's description says it does, including its summary.
   - A check the reads ran fails.
   - It breaks a long-term standard about what the code does.
   - It leaves a document the install points to wrong. A document that's only incomplete doesn't meet this.
   - It would lead the next person to change this code into a bug. A name that could be better isn't enough.
6. **Separate what this change didn't cause.** A problem in code this pull request didn't write, outside what its description promises, isn't a comment. Name it for the user in Phase 5.
7. **Keep the Questions that bear on the test,** including whether a check that couldn't run here passed. List every folder or suite a Question about checks covers, from the repository, not from memory. New behavior a long-term standard says should have a test, and has none that would fail without it, goes in one Fix per pull request, listing each case. Where the standards say that behavior doesn't get a test, it's nothing to raise.
8. **Keep the Nits to five per pull request.** A Nit is a preference, a standard about how code reads, or anything else that only tidies, such as a misplaced comment. It's never a defect. Keep the five most worth making: first what the most reads found, then what breaks an explicit long-term rule. Five counts the review as first posted.
9. **Write the body** as `examples/body.md` shows.
10. **Write each comment** as its example shows, to `review.json` in this session's temporary folder, in the shape GitHub's reviews API takes: `event: COMMENT`, the head as `commit_id`, the body, and each inline comment with `path`, `line` and `side: RIGHT` on a line in the diff, which can be an unchanged line inside a hunk. A finding outside the diff goes on the nearest changed line and names the file and lines it's about. Put comments in the order of the pull request's changes, from what the person does.

## Phase 4 — Address the Calls

Send the Calls in a message of their own, with nothing else in it. With none, say so in one line and go to Phase 5. Each block gives its pull request, its question, what changes under each option with an example, and the reviewer's pick last.

> ⏸️ **GATE → User.** For each Call, give your recommendation in a sentence, say to use the reviewer's, or say to leave it open for the author. If the answer is only a letter, ask once for the sentence, then use what they give.

Write the answer into the comment, in the form its example shows: `examples/call-product.md` for the user's, `examples/call-code.md` for the reviewer's, marked as not reviewed by the user, and `examples/call-left-to-the-author.md` for none.

## Phase 5 — Post

Send one message: the path to `review.json` and the body as it will read, the counts of each kind, one line naming what the reads opened and skipped, what couldn't be checked, what this change didn't cause, each dropped finding in a line, and anything the author still has to push. Then ask to post.

> ⏸️ **GATE → User.** Say to post it, bring back any dropped finding, wait for the author's push, or say not now.

A finding brought back is written as Phase 3 says, a Call goes through Phase 4, and the counts are asked again. Waiting for a push reruns Phase 3 at the new head, then asks again. "Not now" leaves it unposted, and removes every worktree this session made.

On the user's word, post it as a comment review, never an approval: `gh api repos/<owner>/<repo>/pulls/<number>/reviews --input <file>`. `local/amendments/posting.md` says how on this machine, such as a reviewer account. Default: the user's own GitHub sign-in.

Remove worktrees `-b` and `-c`. Keep `-a` for Phase 6.

## Phase 6 — Check the Result

No input needed at this step, unless a check below finds something. A new session makes worktree `-a` only, as Phase 1 step 2 says. `local/amendments/build-session.md` says how to reach the session that built the change, if the user wants it told. Default: the author reads the review on GitHub.

Post no new review. In worktree `-a`, `git fetch origin pull/<number>/head` and `git checkout --detach FETCH_HEAD`, reinstall dependencies if the lockfile changed, and run the local checks. A check only the author can run, such as an end-to-end test on a device, is reported as the author's report. Then work through the threads with `gh api graphql`:

- **Addressed:** a comment the change meets, or a thread the author has answered, is resolved with `resolveReviewThread`.
- **Not addressed:** a change that doesn't match its comment gets a reply on its thread, which stays open.
- **New:** a new defect in the change opens a new thread, written as Phase 3 says. A new Nit opens one only if the review posted fewer than five. Name a change no thread asked for in the report, and tell the user about any change marked as not changing behavior that now does.
- **A Question** is answered when its thread says what happened, including a check that wasn't run and why. One the author says is still open or running stays open, and the user can defer it.

When a change turns out to rest on a false claim, bring the user the question, and on their word correct the comment with a reply on its thread.

Report the commit it covers, each change in a line, and the checks. Repeat this phase each time the author pushes, until every thread is resolved or left open on the user's word.

**If the author merged first,** Phase 7 doesn't run. Compare what was checked with what merged, post nothing, tell the user what the merge covers and leaves out, and go to Phase 8 at the merge commit.

## Phase 7 — Approve

Once the reviewer thinks it's good to go, tell the user the pull request is ready, in one message: the commit checked, what an approval would leave out, and each open Question with what it checks and how to try it.

> ⏸️ **GATE → User.** Read the code and give the go or the no-go, say what still needs changing, or say not now. What still needs changing goes to the author, and Phase 6 runs again. A no-go goes straight to Phase 8. "Not now" waits, and `harvest <pull request>` can run later.

On a go:

1. Check it hasn't merged, and the pushed head matches what was checked: `git diff <checked commit> <pushed head>` is empty, or each difference is named and accepted.
2. `local/amendments/approving.md` says whether the reviewer's account approves, at that commit, with a body saying what it covers and doesn't. Default: the user approves it themselves.

## Phase 8 — Hand to the Harvest

Once the user gives the go or the no-go, run `sop_harvest.md` straight away, in worktree `-a` at the approved commit. Then remove this session's worktree. No input needed at this step.

## Notes

- **The reviewer never edits the code, commits or pushes on the author's behalf.** Its posts are the review, its replies, new threads, resolving threads, and any approval an amendment allows.
- **A user who defers a check only they can run** ("I'll run the end-to-end tests later") also defers the Questions only that check can answer, as a deferral, never a pass. It doesn't cover a Question the code could answer. Each counted Question's thread says it isn't checked yet, and the approval names it.
- **Never remove a worktree another session made.**
- **Every write to GitHub in Phases 5 to 7 is made by the session the user is talking to.** A read can be an agent, and writes can't be delegated to one.

The output of this procedure is a review, and its examples are `writing-styles/review-comments/examples/`.
