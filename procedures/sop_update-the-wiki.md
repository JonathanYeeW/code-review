# SOP — Update the Wiki

**Created:** 2026-11-24
**Purpose:** Bring a product's docs up to date with everything merged since they were last synced, reviewed or not, checked against the code, and committed where the docs live. What the user sees is `docs/cpf_updating-the-wiki.md`.

---

## Trigger

The user says `update the wiki`, or "these went in, update the wiki", naming a product or a release. Begin Phase 1.

Phase 2 step 3 of `sop_harvest.md`, for a merged pull request. Begin Phase 1, for its product.

The user says `push the wiki`. Begin Phase 4.

---

## Phase 1 — What Merged

No input needed at this step, unless the product is unclear.

1. **Find the docs.** The product's row in `local/product/README.md` names its repositories, where its docs live and how a docs change lands there. The docs' own README names the commit they're synced through, or, for docs with no README such as Notion, the product's row does.
2. **Make a place to write,** never the checkout the user works in:
   - Docs in a git repository: `git -C <checkout> fetch origin`. If `local/ledger.md` has a Waiting to push row for this product, continue on its branch; otherwise `git -C <checkout> worktree add -b wiki/<date> ../<repo>-wiki-<session> origin/HEAD`.
   - Docs in `local/product/<product>/`: write there.
   - Docs somewhere with no git, such as Notion: write there, with no history forced on it.
3. **List what merged** since the synced commit, on each of the product's repositories: `gh pr list --state merged --base <default branch> --search "merged:>=<the synced commit's date>" --json number,title,body,mergeCommit`, keeping those whose merge commit isn't already in the synced commit's history. With no synced commit yet, ask the user where to start.
4. Read any notes for this product its row names, such as findings waiting for an update. Each is a claim to check like any other.

## Phase 2 — What It Made Wrong

No input needed at this step.

Run agents, one per merged change or a few small ones together, each given the docs, the change and a worktree at the commit being synced to. Each finds what in the docs the change made false, or a fact it added that the docs are missing: what the person can do, how this product's code is put together, and where it departs from `local/system/`. Each claim is checked against the code at that commit before it's kept. A claim that doesn't hold is dropped. Nothing is voted on.

Sort what's kept:

- **A product fact** is written, without asking.
- **A procedure** that disagrees with the code is a question, never an edit: a procedure is how people work, and only partly what the code does.
- **A disagreement with `local/system/`** is a question: did the system change, or is this product an exception? An exception, once the user says so, is written in the product's docs with its reason.

## Phase 3 — Write

No input needed at this step.

1. Write each fact into the doc it belongs in, shaped against its type in `documentation-examples`, or as the install's own docs standard says if `local/system/` has one. Update each README the change touches.
2. Move the synced-through commit in the docs' README forward.
3. Commit where the docs live:
   - In a git repository: on the branch, with a `docs:` message naming what merged. Add or update its row under Waiting to push in `local/ledger.md`.
   - In `local/`: commit `local/` with a message starting `wiki`.
4. Remove each note it settled from the product's notes, and the file once it's empty.
5. Send one message: what changed in a line each, the branch, and each question.

> ⏸️ **GATE → User, only if there's a question.** Answer each in a sentence. Nothing else waits on this.

An answer that changes the docs is written, committed, and reported in a line. One that changes `local/system/` is written there too, as the user says.

## Phase 4 — Push

On `push the wiki`, for each Waiting to push row: push its branch, or open a docs pull request, as the product's row says that repository takes a docs change. Then remove the row, and the worktree that has its branch checked out (`git -C <checkout> worktree list`), whichever session made it, and commit `local/`. Ball: nobody.

## Notes

- **The update never edits the code.** It writes docs only.
- **A harvest's call reaches only the merged pull request's product,** and the update covers everything merged since the synced commit, not only that pull request.
