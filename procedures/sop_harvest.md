# SOP — Harvest a Review

**Created:** 2026-11-06
**Updated:** 2026-11-24
**Purpose:** Turn one review into what the next review starts from: updated coding standards, the product's docs brought up to date if it merged, committed, and a short note to the user with no questions. What the user sees is `docs/cpf_harvesting.md`.

---

## Trigger

Phase 7 of `sop_review-a-pr.md`, straight away. Don't wait for the merge.

Or the user says `harvest <pull request>`, for a review that wasn't harvested at the time. A merged pull request counts as a go, and the commit the review approved as the approved commit, or its merge commit if it wasn't approved. One closed without merging counts as a no-go, at the last head the review checked. For one still open, ask the user which it is.

Begin Phase 1.

---

## Phase 1 — Learn

No input needed at this step. Read `local/amendments/README.md` first, and use the approved commit, or the merge commit if the author merged first, in worktree `-a` from the review, or a new worktree if there isn't one.

1. **Gather what was said.** Every reply on the pull request's threads and the author's own comments, with `gh api repos/<owner>/<repo>/pulls/<number>/comments` and `gh pr view <number> --comments`, any commit the user pushed, the ticket's notes, and what the user typed in the session, word for word. On the user's own sign-in, the reviewer's posts carry the user's name, so leave out the review itself and the replies this session posted. When an earlier session posted some of them, show the user the replies under their name and ask which are theirs; that's the one question a harvest may ask.
2. **Find the corrections.** A correction is anything the user changed, closed or overruled, including a Call they said was wrong, a pull request closed without a word, a dropped finding they brought back, and anything they had to ask that the review should have told them. Note any comment whose claim proved false, and any that fell short.
3. **Coding standards.** Search `local/coding-standards/short-term.md` and the long-term README for each correction, by principle and by the user's words.
   - Not there: a new short-term entry.
   - In short-term: add this one to its entry. When it has come back two or three times, by judgment and not a count, promote it. It should read as the same principle, not the same words.
   - Against a long-term standard: an exception to it.
   - A Code call on a References rule that conflicts with a long-term standard: the author's decision, written in that reference's row.

   A Nit or Question raised from short-term that was accepted counts as seen too. Recurrence across reviews is the evidence; no second agent votes.

## Phase 2 — Write

No input needed at this step.

1. **Short-term.** Update `local/coding-standards/short-term.md` in the shape the example shows. Remove any entry that hasn't been seen in about 20 harvests, counted by `git -C local log --grep '^harvest'` since it last changed.
2. **Long-term.** Write a promotion or an exception into `local/coding-standards/long-term/`, in the folder and shape its `CONTRIBUTING.md` says, or, without one, as the example shows, and remove the short-term entry. Update the folder's README. Never change `local/system/` or a References row's source: a lesson about how a system is built goes in the note.
3. **The product's docs.** If the pull request has merged, run `sop_update-the-wiki.md` for its product. If not, the next update reaches it.
4. **The ledger.** Remove the review's row from `local/ledger.md`. A lesson about how a review runs, or a comment that shows the style somewhere the plugin's examples don't, goes under Plugin suggestions, never into the plugin.
5. **Commit `local/`,** in its own repository, with a message starting `harvest` and naming the pull request. It has no remote.

## Phase 3 — Tell

Send one message, in the shape the example shows: what changed under each heading, or that nothing did, any rule promoted, in a line, what `update the wiki` changed or what it's waiting on, any comment whose claim proved false or that fell short, and any plugin suggestion. Leave out the reasoning. Ask nothing; git is the undo, and the user can say to take a promotion back.

---

## Example

A short-term entry, in `local/coding-standards/short-term.md`:

```
**A helper with one caller goes inline.** Seen 2.
- pbj-co/pbj#212: "put it back inline, it only has one caller"
- pbj-co/pbj#218: "this wrapper has one caller, just call it directly"
```

A promoted rule, in `local/coding-standards/long-term/writing/functions.md`:

````
## A Helper Earns Its Name

- **A helper earns its own name by being used in more than one place.** With one caller it's a step of its caller.

```ts
// Yes
const total = sandwiches.reduce((sum, s) => sum + s.price, 0);

// No
const total = sumPrices(sandwiches); // the only call
```
````

A note, after #221 at the PB&J Machine Company:

```
Harvest of #221
Coding standards: 1 updated in short-term, 1 promoted: "a helper with one caller goes inline" (writing/functions.md)
Product docs: wiki_orders.md now says a removed sandwich lowers the total; waiting on `push the wiki`
Nothing removed
Plugin suggestion: a re-review should grep every copy of a reason it disproves
```
