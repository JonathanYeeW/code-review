# Core Product Flow — A Fresh Clone to Ready for a Real Review

**Created:** 2026-11-06
**Updated:** 2026-11-24
**Context:** Starts when the user has put the plugin where their LLM can read it, and says `set up the reviewer`. Ends when the install is ready and the user is told what to run first.

## Summary

This flow is Phase 1 of `cpf_code-review.md`, in detail, seen from the user's side. The reviewer finds out which products it reviews and where their docs live, finds what's already written down, and records how this machine differs. Read it before setting up, or before bringing the reviewer to a new company. The steps are `../procedures/sop_set-up.md`.

## Flow at a Glance

```
PHASE 1: WHERE IT WORKS (the user answers, the reviewer records)

  1. The user names the products, repositories and docs
  2. A seed from another install, or start empty
  3. The install is created

        ~~~~~~~~ handoff to the search ~~~~~~~~

PHASE 2: FIND WHAT EXISTS (the reviewer, then the user)

  4. The reviewer finds the docs and standards that already exist
  5. It decides who controls each source
  6. The user confirms the list, and adopts any rules worth keeping
       → nothing written down anywhere: the install starts empty

        ~~~~~~~~ handoff to the machine ~~~~~~~~

PHASE 3: HOW THIS MACHINE DIFFERS (the user)

  7. Posting, approving and reads, each with a default

        ~~~~~~~~ handoff to the user ~~~~~~~~

PHASE 4: HAND OVER

  8. The install is ready: update the wiki, then a first review
```

---

## Phase 1: Where It Works

This runs once per install, and an install usually matches a company or a set of personal projects.

**Step 1: The user names the products, repositories and docs.** One install can cover many products, and a product can span more than one repository. The user says where each product's docs live, if they know.

**Step 2: A seed from another install, or start empty.** A seed, such as the user's install on another machine, brings its long-term coding standards and system knowledge. Short-term memory always starts empty, since what was in flight belongs to the other install.

**Step 3: The install is created.** `local/` is made at the plugin's root, with a README in each directory and an empty ledger, as its own git repository with no remote.

---

## Phase 2: Find What Exists

**Step 4: The reviewer finds the docs and standards that already exist.** It looks in each repository for a docs folder, contributing guides, a style guide and lint config, and follows anything they route to elsewhere, such as a team wiki.

**Step 5: It decides who controls each source.** A source is the user's when they own it or work in it alone, and not when a team reviews changes to it. Where it can't tell, it isn't.

**Step 6: The user confirms the list, and adopts any rules worth keeping.** They correct who controls what and add anything the search missed.

- Each product becomes a row in `local/product/README.md`: its repositories and checkouts, where its docs live, and how a docs change lands there.
- Each source the user can't change becomes a References row, read but never changed, with any rule adopted from it.

---

## Phase 3: How This Machine Differs

**Step 7: Posting, approving and reads, each with a default.** Each difference is an amendment, which changes how a step is carried out and never what the step is.

- **Posting:** a separate reviewer account, or by default the user's own GitHub sign-in.
- **Approving:** a reviewer account approves on the user's word, or by default the user approves.
- **Independent reads:** how this LLM client runs them, or by default subagents.

Making a reviewer account is the user's to do.

---

## Phase 4: Hand Over

**Step 8: The install is ready: update the wiki, then a first review.** The user is told to run `update the wiki` for a product whose docs haven't been synced, so the first review starts from the product as it is, then `review this PR <link>`. A small pull request is a good first one.

---

## When It Goes Wrong

| Symptom | Where it died | What it means |
|---|---|---|
| A harvest or wiki update tried to change a team's docs | Step 5 | A source was recorded as the user's when it isn't |
| A plugin update overwrites something the user changed | Step 7 | The change was made to the plugin instead of an amendment |
