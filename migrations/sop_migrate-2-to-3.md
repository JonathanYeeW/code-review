# SOP — Migrate an Install From 2.x to 3.0

**Created:** 2026-11-24
**Purpose:** Move an install's `local/` from 2.x's layout to 3.0's, losing nothing it knows: each file goes to its new home, or is removed only once what it held has one. Produces a 3.0 `local/`, committed, and a report of what moved where.

---

## Trigger

The user pulls 3.0 onto an install that was set up with 2.x, and says `migrate the install`. Begin Phase 1.

A 2.x install has a `local/sources.md`. A 3.0 install has `local/product/README.md` and `local/system/`. Run this once; a review on 3.0's procedures reads only 3.0's layout.

---

## What Changed

| 2.x | 3.0 |
|---|---|
| `local/sources.md`: repositories, sources, conflict answers | `local/product/README.md`, one row per product with its repositories, checkouts, where its docs live and how a docs change lands; and a References section in `coding-standards/`, `system/` and `product/` for sources the user can't change, with any conflict answer in its row |
| A coding-standards source the user controls, pointed to from `long-term/README.md` | Its content in `local/`: how code reads in `coding-standards/long-term/`, how systems are built in `local/system/` |
| `local/coding-standards/long-term/<area>.md` | `coding-standards/long-term/writing/`, `checking/` and `languages/`, one file per concern; anything about building a repo, an app or its infrastructure in `local/system/` |
| `what-a-review-reads-for.md` | `coding-standards/long-term/checking/reviewing.md` |
| `local/product/short-term.md` | Gone. A fact that can be checked goes in the product's notes for the next `update the wiki`; the rest is dropped |
| `local/product/long-term/` | The product's own docs, or `local/product/<product>/` for a product with none |
| `local/held-back.md` | Gone. A coding-standards change is applied in `long-term/`; a docs change goes in the product's notes, or under Waiting to push in the ledger |
| `local/teaching.md`, `local/amendments/teaching.md` | Gone, with teaching stacks |
| `local/amendments/calls.md` | Gone. Every Call is left to the author, by default |
| `local/amendments/build-session.md` | Gone. The reviewer never messages another session |
| `local/examples/` | Gone. Each example worth keeping is a plugin suggestion in the ledger |
| — | `local/ledger.md`: open reviews, waiting to push, plugin suggestions |

---

## Phase 1 — Plan

No input needed until the gate.

1. **Commit `local/` as it is,** so git holds the 2.x install whole: `git -C <plugin>/local add -A && git -C <plugin>/local commit -m "before 3.0"`. Git is the undo for everything after.
2. **Read every file in `local/`,** and every source `sources.md` names that the user controls.
3. **Map each one** to its 3.0 home with the table above. A long-term rule goes to the file for its concern: how code is structured, named, split into functions, commented, or fails is `writing/`; what gets a test, what's logged and the lint gate is `checking/`; what's true of one language is `languages/`; how a repo, an app type, an integration, the database or infrastructure is set up is `local/system/`.
4. **Sort `product/short-term.md` and `held-back.md`** entry by entry: a coding-standards change to apply, a checkable fact for the product's notes, or a leftover to drop.
5. **List the open reviews,** from the user or `gh pr list`, for the ledger.

> ⏸️ **GATE → User.** Here's where each file goes, the entries dropped with why, and the open reviews. Correct anything. A source the user controls that others also use, such as a team's standards, can stay where it is as a References row instead of moving in.

## Phase 2 — Move

No input needed at this step.

1. **Create 3.0's folders,** as the README's tree shows, each with a README: `system/`, `product/`, and `coding-standards/long-term/writing/`, `checking/` and `languages/`. Create `local/ledger.md` with its three sections, and the open reviews as rows.
2. **Move each file** as Phase 1 mapped it. A rule keeps its words; only where it lives changes. Rewrite each link it holds to its new path.
3. **Write each README:** one row per file or folder, and the References section in `coding-standards/`, `system/` and `product/`.
4. **Remove what's gone** only once everything it held has a home: `sources.md`, `product/short-term.md`, `product/long-term/`, `held-back.md`, `teaching.md`, `examples/`, and the retired amendments.

## Phase 3 — Check

No input needed until the gate.

1. **Every link in `local/` resolves,** and nothing still names `sources.md`, `held-back.md` or `product/short-term.md`.
2. **A fresh agent reads `local/` cold,** from its README, and says for one product: where its docs are, which coding standards and system docs a review of it would open, and what's open in the ledger. A question it can't answer from the READMEs is a gap.
3. **Commit `local/`** with the message `3.0`.

> ⏸️ **GATE → User.** What moved where, in a line per folder; what was dropped; and the fresh agent's answers. The install is ready for `update the wiki`, then `review this PR <link>`.
