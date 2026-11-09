# SOP — Set Up the Reviewer

**Created:** 2026-11-06
**Purpose:** Take a fresh clone to an install that knows where it works and has learned the user from stacks of small pull requests, ready for a first real review. What the user sees is `docs/cpf_setting-up.md`.

---

## Trigger

The user says `set up the reviewer`. Begin Phase 1.

The user says `teach the reviewer`, on an install that already exists, for another stack. Begin Phase 4.

Every `local/` path is under this plugin's own root, never a repository being reviewed.

---

## Phase 1 — Where It Works

Ask the user, in one message: which repositories this install covers, where each one's checkout is, and where its documentation and coding standards live, if they know.

> ⏸️ **GATE → User.** Name the repositories and checkouts, and anything you know about where the docs and standards are.

Then create the install, with no further input:

1. Create `local/` at this plugin's root, as the directory tree in `README.md` shows, with a README in each directory, and `git init` inside it. It never gets a remote.
2. Commit it: `git -C <plugin>/local add -A && git -C <plugin>/local commit -m "start the install"`.

## Phase 2 — Find What Already Exists

No input needed until the gate.

1. In each repository, look for a docs or standards folder, CONTRIBUTING files at any level, a CLAUDE.md or similar, a style guide, and lint or formatter config.
2. Follow anything they route to outside the repository, such as a team wiki or a standards repository.
3. For each source, decide who controls it. Check `gh api repos/<owner>/<repo> --jq '{owner: .owner.login, permissions: .permissions, default_branch: .default_branch}'`, and whether the default branch takes a direct push. It's the user's when the user owns it or works in it alone, and it isn't when a team reviews changes to it. Where you can't tell, it isn't.
4. Write one short list: each source, its scope, what it covers, who controls it, and what nothing covers yet.

> ⏸️ **GATE → User.** Confirm the list, correct who controls what, add anything the search missed, and name any rule worth adopting now.

Record each repository in `local/sources.md`, with its checkout and default branch. Record each source as a row there: scope, where, what it covers, who controls it, and rules adopted or declined, with an empty table for conflict answers.

- **Each `long-term/README.md`:** an index with one entry per area. An area a source the user controls covers points to that source. Any other area lists its file in `local/`, once it has one. It says which files every review reads, whatever the pull request touches.
- **Each adopted rule:** write it as `sop_harvest.md` Phase 2 step 2 says, and index it.

Commit `local/`.

## Phase 3 — How This Machine Differs

Ask the user, in one message, each with its default:

- **Posting and opening pull requests:** is there a separate reviewer account? Default: the user's own GitHub sign-in.
- **Approving:** should a reviewer account approve on the user's word? Default: the user approves.
- **Independent reads:** can this LLM client run independent agents? Default: subagents.
- **The session that built a change:** should the reviewer be able to message it? Default: the author reads the review on GitHub.
- **Teaching stacks:** should they go somewhere other than the shared repository? Default: the repository itself.

> ⏸️ **GATE → User.** Answer any that differ from the default, or say none do.

Write each difference to `local/amendments/<name>.md`: `posting.md`, `approving.md`, `reads.md`, `build-session.md` or `teaching.md`, saying how this machine carries out that step, and list it in `local/amendments/README.md`. Making a reviewer account is the user's to do. Commit `local/`.

## Phase 4 — Teach

No input needed until the gate. Ask which repository if the install covers more than one. Read `local/README.md`, both `short-term.md` files and both long-term indexes, and the coding standards they point to. Make a worktree: `git -C <checkout> fetch origin`, then `git -C <checkout> worktree add --detach ../<repo>-teach-<session> origin/HEAD`, and install its dependencies. `<session>` is unique to this session. Never use the checkout the user works in.

1. **Read the code in the order it runs,** from where `local/teaching.md` says this repository's last stack stopped, or from the app's entry point. Not file by file.
2. **Stop at each thing you'd change, and note it as one guess.** One small change, which can touch more than one file, such as a utility and its test. One change per pull request.
3. **Change how code is arranged, nothing else.** A change the person using the product, or a client of the server, would notice is written up in `local/teaching.md` for a real pull request, not made. So is one where a standard and a primary source disagree.

Stop at five guesses, or fewer on a short day.

> ⏸️ **GATE → User.** Here are the guesses, a line each, and the repository they'd open in. These are real pull requests, so teammates can see them, reviewers may be notified, and merging may deploy. Say to open them, drop any, or name somewhere quieter.

Then for each guess, bottom up:

1. **Make the change, and run the tests, typecheck and lint.** `git status` holds only the change. If a check fails on the change, or there are none to run, say so in its description.
2. **Commit and push under the user's git identity,** on a branch named as the repository's standard says, or `teach-<date>-1` and on, and `teach-<date>b-1` for a second stack that day.
3. **Write the description** to the repository's own pull request standard, if it has one, and open with the guess: "I'd make this change, what do you think?"
4. **Open it:** `gh pr create --base <previous layer or the default branch> --head <branch> --title <title> --body-file <file>`. The first targets the default branch, and each next one the one before it. The title follows the repository's standard, or `review: <what changed>`. `local/amendments/posting.md` says how on this machine, and `local/amendments/teaching.md` where, if either exists.

> ⏸️ **GATE → User.** Read them together. Correct it, close it or say to merge it, and say why if it isn't obvious.

A correction is made on its branch, the layers above are rebased onto it, and it's handed back, without asking first. Once the user says to merge, bottom up, one layer at a time, as far as the repository allows. If it requires a teammate's review, that's for the user to get:

1. Write down each layer's tip first: `git rev-parse <branch>`.
2. Retarget the next pull request to the default branch.
3. `git fetch origin`, then rebase this layer onto it, `git rebase --onto origin/<default branch> <previous layer's old tip>`, and force-push under the user's git identity. The bottom layer skips this.
4. Merge it the way the repository does.
5. `git fetch origin`, then check `git diff <reviewed tip> origin/<default branch>`. If it isn't empty, stop and name the difference for the user.

Then:

1. Delete the merged branches and the worktree.
2. Update `local/teaching.md`: where this stack stopped, and any findings written up instead of made.
3. Name each written-up finding for the user in a line.
4. Run `sop_harvest.md`, on the stack.

## Phase 5 — Hand Over

> ⏸️ **GATE → User.** Run another stack with `teach the reviewer`, or say the reviewer is ready. The harvest notes show how many new promotion questions each stack raised, and that's the sign to go on.

When they say it's ready, tell them the next step is `review this PR <link>`, and a small pull request is a good first one.

---

## Example

A teaching pull request's description, at the PB&J Machine Company:

```
I'd make this change, what do you think?

`SpreadProvider` publishes `onRefresh`, and nothing calls it. The order flow reloads
spreads through `AppStartup` instead. This removes it.

Nothing on screen changes. Typecheck, lint and all 640 tests pass.
```
