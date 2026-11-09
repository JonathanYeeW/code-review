# SOP — Harvest a Review

**Created:** 2026-11-06
**Purpose:** Turn one review, or one stack from setup, into what the next review starts from: updated coding standards and product memory, committed, and a short note to the user. What the user sees is `docs/cpf_harvesting.md`.

---

## Trigger

Phase 8 of `sop_review-a-pr.md` or the last step of a stack in `sop_set-up.md`, straight away. Don't wait for the merge. A stack is one harvest, with each layer's own decision.

Or the user says `harvest <pull request>`, for a review that wasn't harvested at the time. A merged pull request counts as a go, and the commit the review approved as the approved commit, or its merge commit if it wasn't approved. One closed without merging counts as a no-go. For one still open, ask the user which it is.

Begin Phase 1.

---

## Phase 1 — Learn

No input needed at this step. Read `local/amendments/README.md` first, and use the approved commit, or the merge commit if the author merged first, in worktree `-a` from the review, or a new worktree if there isn't one.

1. **Gather what was said.** Every reply on the pull request's threads and the author's own comments, with `gh api repos/<owner>/<repo>/pulls/<number>/comments` and `gh pr view <number> --comments`, any commit the user pushed, the ticket's notes, and what the user typed in the session, word for word. On the user's own sign-in, the reviewer's posts carry the user's name, so leave out the review itself and the replies this session posted.
2. **Find the corrections.** A correction is anything the user changed, closed or overruled, including a recommendation on a Call against the reviewer's pick, a pull request closed without a word, a dropped finding they brought back, and anything they had to ask that the review should have told them. Note any comment whose claim proved false, and any that fell short.
3. **Coding standards.** Search `local/coding-standards/short-term.md` and the long-term index for each correction, by principle and by the user's words.
   - Not there: propose a short-term entry.
   - In short-term: add this one to its entry. At two or three, by judgment and not a count, ask a promotion question. It should read as the same principle, not the same words.
   - Against a long-term standard: propose an exception to it.
   - Marked "no": add this one to its entry, and ask again only if it has come up several more times since the no.

   A Nit or Question raised from short-term that was accepted counts as seen too.
4. **Product.** Skip on a no-go. For what the review showed about the product, read `local/product/short-term.md` and the long-term index, and check each claim against the approved commit.
   - It holds, and memory doesn't say so or says otherwise: propose the long-term change.
   - It can't be checked, such as something the user said: propose a short-term entry.
   - A comment's false claim about the product: correct what led to it.
5. **Second agent.** Run a fresh agent that hasn't seen these proposals. `local/amendments/reads.md` says how on this machine. Give it what was gathered in step 1, the approved commit in its own worktree, `local/`, and whether this was a go or a no-go, and ask what, if anything, should change in long-term memory. Apply a long-term change only when both proposed it on their own, keeping whichever wording holds. One only one proposed stays a short-term entry. Short-term entries don't need its agreement.

## Phase 2 — Write

No input needed at this step.

1. **Short-term.** Update `local/coding-standards/short-term.md` and `local/product/short-term.md` in the shape the example shows. Remove any entry that hasn't been seen in about 20 harvests, counted by `git -C local log --grep '^harvest'` since it last changed. Keep a "no".
2. **Long-term.** Find where each change goes in `local/sources.md`.
   - **A source the user controls:** make the change in a new worktree, never the checkout the user works in: `git -C <source> worktree add --detach ../<source>-harvest-<session> origin/HEAD`. Write it the way that source's own contributing guide says, commit, and `git push origin HEAD:<default branch>`. If the push is behind, rebase and try once more. If it's refused, or the user has said not to push, keep the exact change in `local/held-back.md`, and say so in the note. It goes in the next time the source can take it. Then remove the worktree.
   - **A source someone else controls, or none:** write it under `local/`, and update the directory's README index. A product file is a CPF or a wiki, shaped against the type in `documentation-examples`. A coding standard is a rule in `local/coding-standards/long-term/<area>.md`, in the shape the example shows. Never change their source.
   - **This plugin:** never change it. A lesson about how a review runs goes in the note as a suggestion, even when it's the user's own preference, and how this machine differs goes in an amendment.
3. **Examples.** If a comment was accepted word for word and shows the style somewhere the examples don't, copy it whole into `local/examples/`, as `ws_review-comments.md` says. Replace an example it improves on.
4. **Commit `local/`,** in its own repository, with a message starting `harvest` and naming the pull request. It has no remote.

## Phase 3 — Tell

Send one message, in the shape the example shows: what changed under each heading, or that nothing did, with a link to any commit on a source the user controls, anything held back, any comment whose claim proved false or that fell short, and any suggestion for the plugin. Leave out the reasoning. Then any promotion questions, including any marked "asked".

> ⏸️ **GATE → User, only if there's a promotion question.** Say whether it's a rule, in a sentence. Nothing else waits on this.

- **A yes:** write the rule into long-term memory as Phase 2 step 2 says, remove its short-term entry, and commit.
- **A no:** keep the entry, marked "no", with the user's words, and commit.
- **No answer:** mark the entry "asked", and ask again next harvest.

---

## Example

A short-term entry, in `local/coding-standards/short-term.md`:

```
**A helper with one caller goes inline.** Seen 2. Asked.
- pbj-co/pbj#212: "put it back inline, it only has one caller"
- pbj-co/pbj#218: "this wrapper has one caller, just call it directly"
```

A long-term rule, in `local/coding-standards/long-term/general.md`:

````
## A helper with one caller goes inline

A helper with one caller makes the reader jump for nothing.

```ts
// Yes
const total = sandwiches.reduce((sum, s) => sum + s.price, 0);

// No
const total = sumPrices(sandwiches); // the only call
```

**Source:** pbj-co/pbj#212 and #218, "put it back inline, it only has one caller".
**Scope:** every repository in this install.
**Exceptions:** a helper with a name that says something the code can't.
````

A note, after #218 at the PB&J Machine Company:

```
Harvest of #218
Coding standards: 1 updated in short-term
Product: 1 confirmed against the code, committed to docs/product/wiki_orders.md (a1b2c3d)
Nothing removed

You've corrected this twice:
#212: "put it back inline, it only has one caller"
#218: "this wrapper has one caller, just call it directly"

Should "a helper with one caller goes inline" become a rule?
```
