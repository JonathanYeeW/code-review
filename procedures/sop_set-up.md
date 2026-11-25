# SOP — Set Up the Reviewer

**Created:** 2026-11-06
**Updated:** 2026-11-24
**Purpose:** Take a fresh clone to an install that knows how the user writes code and builds systems, which products it reviews and where their docs live, and how this machine differs, ready for a first real review. What the user sees is `docs/cpf_setting-up.md`.

---

## Trigger

The user says `set up the reviewer`. Begin Phase 1.

Every `local/` path is under this plugin's own root, never a repository being reviewed.

---

## Phase 1 — Where It Works

Ask the user, in one message:

- Which products this install reviews, each one's repositories and checkouts, and where its docs live, if they know.
- Whether to seed coding standards and system from another install, such as this user's install on another machine, by a folder path, or start them empty.

> ⏸️ **GATE → User.** Name the products, repositories and checkouts, anything known about their docs, and a seed or "start empty".

Then create the install, with no further input:

1. Create `local/` at this plugin's root, as the directory tree in `README.md` shows, with a README in each directory, and `git init` inside it. It never gets a remote. `local/ledger.md` starts with four empty sections: Open reviews (pull request, head reviewed, ball, next), Docs behind (pull request, product), Waiting to push (repository, branch, what) and Plugin suggestions.
2. With a seed, copy its `coding-standards/long-term/` and `system/` in whole. Starting empty, `coding-standards/long-term/` gets `writing/`, `checking/` and `languages/`, each with a README, and the harvest's example shows a rule's shape. `coding-standards/short-term.md` starts empty either way, since what was in flight belongs to the other install.
3. Commit it: `git -C <plugin>/local add -A && git -C <plugin>/local commit -m "start the install"`.

## Phase 2 — Find What Already Exists

No input needed until the gate.

1. In each repository, look for a docs folder, CONTRIBUTING files at any level, a CLAUDE.md or similar, a style guide, and lint or formatter config.
2. Follow anything they route to outside the repository, such as a team wiki or a standards repository.
3. For each source, decide whether the user can change it. Check `gh api repos/<owner>/<repo> --jq '{owner: .owner.login, permissions: .permissions, default_branch: .default_branch}'`, and whether the default branch takes a direct push. It's the user's when they own it or work in it alone, and it isn't when a team reviews changes to it. Where you can't tell, it isn't.
4. Write one short list: each product's docs and how a docs change lands there, and each source the user can't change, with what it covers.

> ⏸️ **GATE → User.** Confirm the list, correct who controls what, add anything the search missed, and name any rule worth adopting now.

Record each product as a row in `local/product/README.md`: its repositories, checkouts and default branches, where its docs live, and how a docs change lands. A product with no docs of its own keeps them in `local/product/<product>/`, committed in `local/`. Record each source the user can't change as a References row in the README it belongs to (`coding-standards/`, `system/` or `product/`), with any rule adopted. Commit `local/`.

## Phase 3 — How This Machine Differs

Ask the user, in one message, each with its default:

- **Posting:** is there a separate reviewer account? Default: the user's own GitHub sign-in.
- **Approving:** should a reviewer account approve on the user's word? Default: the user approves.
- **Independent reads:** can this LLM client run independent agents? Default: subagents.

> ⏸️ **GATE → User.** Answer any that differ from the default, or say none do.

Write each difference to `local/amendments/<name>.md`: `posting.md`, `approving.md` or `reads.md`, saying how this machine carries out that step, and list it in `local/amendments/README.md`. Making a reviewer account is the user's to do. Commit `local/`.

## Phase 4 — Hand Over

Tell the user the install is ready, and the next steps: `update the wiki` for a product whose docs haven't been synced, so the first review starts from the product as it is, then `review this PR <link>`. A small pull request is a good first one.
