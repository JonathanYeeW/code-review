# SOP — Review a Pull Request

**Created:** 2026-11-06
**Updated:** 2026-11-24
**Purpose:** Review a pull request with three independent reads, walk the user through it until they can put their name on it, post it, and see it through to their final approval. What the user sees is `docs/cpf_reviewing-a-pr.md`.

---

## Trigger

The user says `review this PR <link>` or `review PR <number>`, in the repository of the product being worked on, or asked for if that isn't clear. Begin Phase 1.

The user says `re-review <pull request>`. Begin Phase 5.

The user says `review status`. Read `local/ledger.md`, and for each open review check `gh pr view <number> --json state,headRefOid,mergedAt`. Say in a line each what's waiting on whom: a new head is ready to re-review, a merge before the re-review goes to Phase 5's merged-first step, and an approved review whose harvest hasn't run gets it now.

**Every stop ends with a ball line,** saying who has the ball and what for, such as "Ball: the author. Waiting on their push. Say `re-review #212` when they've pushed." At each one, update the review's row in `local/ledger.md` (pull request, head reviewed, ball, next) and commit `local/`. The reviewer never messages another session. When the author is a session, the user tells it.

---

## Phase 1 — Get Ready

No input needed at this step.

1. Read `local/amendments/README.md`. Steps that name a default say which amendment replaces it.
2. Make one copy of the code per read, never using the checkout the user works in. Find the checkout in `local/product/README.md`, then `git -C <checkout> fetch origin`, `git -C <checkout> worktree add --detach ../<repo>-review-<number>-<session>-a origin/HEAD`, then `-b` and `-c`, where `<session>` is unique to this session. In each, `git fetch origin pull/<number>/head`, `git checkout --detach FETCH_HEAD`, install dependencies, and build what the checks depend on.
3. **What's changing.** `gh pr view <number> --json title,body,baseRefName,headRefName,files,commits`. If it names a ticket and the ticket's decisions are written down, read them.
4. **What's known.** Open each at its README and follow its routing to the areas the pull request touches: the product's docs, from its row in `local/product/README.md`; `local/coding-standards/long-term/README.md`, which names what every review reads; `local/system/README.md`; and every References row that covers this repository. Read `local/coding-standards/short-term.md` in full.
5. **Write the brief** for the reads in this session's temporary folder: the pull request, the ticket's decisions, and each doc opened with why, and each skipped with why. Phase 4 names them.

## Phase 2 — Three Reads

No input needed at this step.

Run three independent agents, each given this plugin's path, `local/`, the brief and its own worktree. None sees another's findings. `local/amendments/reads.md` says how on this machine. Default: subagents, if the client has them.

Each read starts from the brief, opening what it routes to, then reviews, and writes its findings to `read-<a|b|c>.md` in this session's temporary folder, one section per finding:

1. **Run the checks:** build, typecheck, lint and all tests, a package at a time if reads are running together on one machine. Undo anything a lint script changed. Rerun a failure in a part the pull request doesn't touch: if it passes alone, note both results; if it fails again, it's a finding. Note any check that can't run here.
2. **Read the diff in the order the code runs**, from where the person using the product or the caller starts, for what the standards and the product's docs expect, for whether the code does what the person needs, and for any document the change leaves wrong.
3. **Check every claim before writing it down.**
   - A claim about behavior is traced from where the person or caller starts, not from the function the comment sits on.
   - A claim that code can change safely, and any code or command a finding recommends, is tried in the worktree, then undone so `git status` is clean. One that fails is fixed or dropped. One that would write outside the worktree isn't run, and its How I checked says so.
   - Before a Question, run the read-only checks that bear on it, and say what they showed under What's happening. Never call anything that writes, never touch production, and never rebuild or restart what the user runs.
4. **Label each finding** as a Product call, a Code call, a Fix, a Nit or a Question, with what's wrong and how it was checked. Don't weigh importance.

## Phase 3 — One Review

No input needed at this step. Read the writing style and its examples first: `writing-styles/review-comments/ws_review-comments.md`, then `examples/`.

1. **Combine the same finding.** Keep the clearest, and carry over anything another read had that it lacks.
2. **Settle disagreements.** Check the claim again in a worktree. One the user has to settle becomes a Call.
3. **Check every kept claim again** as Phase 2 step 3 says, including any code or wording written while combining. A claim that doesn't hold is dropped.
4. **Set each kind:**
   - A finding that contradicts a recorded ticket decision is a Call that names the decision, never a Fix.
   - Anything held only in short-term memory is a Nit or a Question, never a Fix or a Call.
   - A rule from a References row that hasn't been adopted is at most a Question, quoting it. An adopted one that conflicts with a long-term standard is a Code call the first time it comes up, unless its row records the answer. The harvest writes the author's decision there.
   - A comment that applies a rule names it and quotes the line.
5. **Hold each Fix and Call to the test.** It goes out only if one of these is true. The number of reads that found it doesn't count.
   - Code this pull request wrote has a defect, however rare, for anyone who depends on it: the person using the product, a client or caller, whoever runs it, or its security. That holds even if something similar existed before.
   - It falls short of what the pull request's description says it does, including its summary.
   - A check the reads ran fails.
   - It breaks a long-term standard about what the code does.
   - It leaves a document the install points to wrong. A document that's only incomplete doesn't meet this.
   - It would lead the next person to change this code into a bug. A name that could be better isn't enough.
6. **Separate what this change didn't cause.** A problem in code this pull request didn't write, outside what its description promises, isn't a comment. Name it for the user in Phase 4.
7. **Keep the Questions that bear on the test,** including whether a check that couldn't run here passed. List every folder or suite a Question about checks covers, from the repository, not from memory. New behavior a long-term standard says should have a test, and has none that would fail without it, goes in one Fix per pull request, listing each case. Where the standards say that behavior doesn't get a test, it's nothing to raise.
8. **Keep the Nits to five per pull request.** A Nit is a preference, a standard about how code reads, or anything else that only tidies, such as a misplaced comment. It's never a defect. Keep the five most worth making: first what the most reads found, then what breaks an explicit long-term rule. Five counts the review as first posted.
9. **Write the body** as `examples/body.md` shows. It opens with what the pull request does, in product language: the problem the person has today, and how this changes it.
10. **Write each comment** as its example shows, to `review.json` in this session's temporary folder, in the shape GitHub's reviews API takes: `event: COMMENT`, the head as `commit_id`, the body, and each inline comment with `path`, `line` and `side: RIGHT` on a line in the diff, which can be an unchanged line inside a hunk. A finding outside the diff goes on the nearest changed line and names the file and lines it's about. Put comments in the order of the pull request's changes, from what the person does. Every Call is left to the author, with the reviewer's recommendation labeled as the reviewer's, as `examples/call-code.md` shows.

## Phase 4 — Process

> ⏸️ **GATE → User.** "The review of #<number> is ready. Process it?" The user may be away when it's ready.

On a yes, walk it in this order, one message each:

1. **What the pull request does:** the body's opening paragraph.

   > ⏸️ **GATE → User.** Say it's understood, or ask until it is.

2. **The Calls,** each with the problem as the reviewer sees it, the decision the author has to make, the reviewer's recommendation with its reasoning, and the alternative. The user isn't choosing; they're confirming each Call is real and correct, since a Call can block a pull request and their name is on it. After the Calls, in a line each: the counts of each kind, the path to `review.json`, what was opened and skipped, what this change didn't cause, and each dropped finding. With no Calls, this is one line and the counts.

   > ⏸️ **GATE → User.** "Calls are right, post it", or name a Call that's wrong, a dropped finding to bring back, or say not now.

A Call named wrong is rewritten or dropped, and shown again. A finding brought back is written as Phase 3 says. "Not now" leaves it unposted, removes every worktree this session made, and its ledger row says so; `review PR <number>` starts it again.

On "post it", post it as a comment review, never an approval: `gh api repos/<owner>/<repo>/pulls/<number>/reviews --input <file>`. `local/amendments/posting.md` says how on this machine, such as a reviewer account. Default: the user's own GitHub sign-in. Remove worktrees `-b` and `-c`. Keep `-a` for Phase 5. Ball: the author.

## Phase 5 — Re-Review

No input needed until the gate. A new session makes worktree `-a` and writes the brief again, as Phase 1 says. Post no new review.

1. **Update.** In worktree `-a`, `git fetch origin pull/<number>/head` and `git checkout --detach FETCH_HEAD`, reinstall dependencies if the lockfile changed, and run the local checks. A check only the author can run, such as an end-to-end test on a device, is reported as the author's report.
2. **One fresh read of what changed.** Run one agent, as Phase 2 does, on the diff since the last checked head and every reply since, with the brief but not the earlier review. It reads the push like a new diff.
3. **Check it like a new diff.** Every reply is a claim: open each place it says changed. A reason disproved anywhere is grepped for every copy, docs and descriptions included. Every document the push writes or leaves is read against the code. Every result the author reports is held to the long-term standards.
4. **Work through the threads.** List them with `gh api graphql -F o=<owner> -F r=<repo> -F n=<number> -f query='query($o:String!,$r:String!,$n:Int!){repository(owner:$o,name:$r){pullRequest(number:$n){reviewThreads(first:100){nodes{id isResolved path line comments(first:50){nodes{databaseId author{login} body}}}}}}}'`. Reply with `gh api repos/<owner>/<repo>/pulls/<number>/comments/<databaseId>/replies -f body=<text>`; open a new thread with `gh api repos/<owner>/<repo>/pulls/<number>/comments -f body=<text> -f commit_id=<head> -f path=<path> -F line=<line> -f side=RIGHT`; resolve with `gh api graphql -F id=<thread id> -f query='mutation($id:ID!){resolveReviewThread(input:{threadId:$id}){thread{isResolved}}}'`. A thread resolves only when the code meets its comment, or a Question's thread says what happened, including a check that wasn't run and why. An answer alone never resolves one. A change that doesn't match its comment gets a reply, and stays open. A new defect opens a new thread, written as Phase 3 says; a new Nit only if the review posted fewer than five. A change marked as not changing behavior that now does is named.

> ⏸️ **GATE → User.** "#<number> has a new push. Process the re-review?"

On a yes, one message: what the pull request does now, only where it changed; how each Call landed, what the author decided and whether the code does it; any new Calls, as Phase 4 shows them; then in a line each, threads resolved, threads left open and why, and new Fixes and Nits.

> ⏸️ **GATE → User.** "Right, post it", or name what's wrong. With new Calls, it's their acknowledgment too.

Post the replies, new threads and resolves. When a change turns out to rest on a false claim of the review's, correct the comment with a reply on its thread. Repeat this phase each time the author pushes, until every thread is resolved or left open on the user's word. Ball: the author again, or the user for the go, as Phase 6 says.

**If the author merged first,** Phase 6 doesn't run. Compare what was checked with what merged, post nothing, tell the user what the merge covers and leaves out, and go to Phase 7 at the merge commit.

## Phase 6 — Approve

Once the reviewer thinks it's good to go, tell the user the pull request is ready, in one message: the commit checked, what an approval would leave out, and each open Question with what it checks and how to try it.

> ⏸️ **GATE → User.** Read the code and give the go or the no-go, say what still needs changing, or say not now. What still needs changing goes to the author, and Phase 5 runs again. A no-go goes straight to Phase 7. "Not now" waits.

On a go:

1. Check it hasn't merged, and the pushed head matches what was checked: `git diff <checked commit> <pushed head>` is empty, or each difference is named and accepted.
2. `local/amendments/approving.md` says whether the reviewer's account approves, at that commit, with a body saying what it covers and doesn't. Default: the user approves it themselves.

## Phase 7 — Hand to the Harvest

Once the user gives the go or the no-go, run `sop_harvest.md` straight away, in worktree `-a` at the approved commit. Then remove every worktree named `<repo>-review-<number>-*`, whichever session made it, since the review is done. No input needed at this step.

## Notes

- **The reviewer never edits the code, commits or pushes on the author's behalf.** Its posts are the review, its replies, new threads, resolving threads, and any approval an amendment allows.
- **A user who defers a check only they can run** ("I'll run the end-to-end tests later") also defers the Questions only that check can answer, as a deferral, never a pass. It doesn't cover a Question the code could answer. Each counted Question's thread says it isn't checked yet, and the approval names it.
- **Never remove a worktree another session made,** except this pull request's own once it's harvested.
- **Every write to GitHub in Phases 4 to 6 is made by the session the user is talking to.** A read can be an agent, and writes can't be delegated to one.

The output of this procedure is a review, and its examples are `writing-styles/review-comments/examples/`.
