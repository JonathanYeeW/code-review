# Core Product Flow — A Fresh Clone to Ready for a Real Review

**Created:** 2026-11-06
**Updated:** 2026-11-06
**Context:** Starts when the user has put the plugin where their LLM can read it, and says `set up the reviewer`. Ends when the user says the reviewer is ready for a real pull request.

## Summary

This flow is Phases 1 and 2 of `cpf_code-review.md`, in detail, seen from the user's side. The reviewer finds out where it's working and what's already written down, then learns the user from stacks of small pull requests until the user is comfortable handing it a real one. Read it before setting up, or before bringing the reviewer to a new company. The steps are `../procedures/sop_set-up.md`.

## Flow at a Glance

```
PHASE 1: SET UP (the user answers, the reviewer records)

  1. The user says which repositories this install covers
  2. The reviewer finds the documentation and standards that already exist
  3. It decides who controls each source
  4. The user adopts any rules worth keeping
       → nothing written down anywhere: the install builds its own
  5. The user says how this machine differs, if it does

        ~~~~~~~~ handoff to teaching ~~~~~~~~

PHASE 2: TEACH (the reviewer guesses, the user answers)

  6. The reviewer reads the code in the order it runs
  7. It opens a stack of five one-change pull requests, on the user's word
       → a change the person using the product would notice: written up, not made
  8. The user answers each one
  9. The answers are harvested
       → loops 6–9 until the user is comfortable
  10. The user says the reviewer is ready
```

---

## Phase 1: Set Up

The reviewer works out where it is. This runs once per install, and an install usually matches a company or a set of personal projects.

**Step 1: The user says which repositories this install covers.** One install can cover many, since a company's standards usually apply across all of them.

**Step 2: The reviewer finds the documentation and standards that already exist.** It looks in each repository for a docs folder, contributing guides, a style guide and lint config, and follows anything they route to elsewhere, such as a team wiki. The user adds anything it missed.

**Step 3: It decides who controls each source.** That decides where long-term memory lives, and it depends on the place.

- A source the user controls becomes the long-term memory, and harvests commit to it directly.
- A source someone else controls is read but never changed, and the install keeps its own memory beside it.
- The reviewer records each decision in `sources.md`, where the user can overrule it.

**Step 4: The user adopts any rules worth keeping.** The reviewer lists what it found, and the user names what they want, so an adopted rule becomes the reviewer's own. With nothing written down anywhere, the install starts empty and grows from the user's answers.

**Step 5: The user says how this machine differs, if it does.** Each difference is an amendment, such as a reviewer account for posting, or an LLM client that can't run independent agents. With none, the plain defaults apply, and reviews post from the user's own GitHub sign-in.

---

## Phase 2: Teach

The reviewer learns the user from their reactions, and small pull requests are the cheapest place to get them. Each one is a guess the user can judge in a minute.

**Step 6: The reviewer reads the code in the order it runs.** It starts at the entry point and stops at each thing it would change, keeping its place so the next stack carries on from there.

**Step 7: It opens a stack of five one-change pull requests, on the user's word.** The reviewer lists its guesses first, since opening them is public. They're stacked on each other, on code that's already merged, and each is one small guess: "I'd make this change, what do you think?"

- One change can touch more than one file, such as a utility and its test, or a renamed term everywhere it appears.
- One change per pull request, because the user's reaction has to say which change it was about.
- Only how code is arranged changes. A change the person using the product, or a client of the server, would notice is written up and named for the user, not made, because it needs a real pull request. So is one where a standard and a primary source disagree.
- These are real pull requests in a real repository, so teammates can see them, reviewers may be notified, and merging may deploy. An amendment can keep them somewhere quieter.

**Step 8: The user answers each one.** They correct it, close it or say to merge it, and say why if it isn't obvious. A correction is made on its branch and handed back.

**Step 9: The answers are harvested.** `cpf_harvesting.md`. Corrections become short-term memory, and ones that come back become promotion questions. The sign it's working is fewer new promotion questions from one stack to the next.

**Step 10: The user says the reviewer is ready.** It's their call, with those questions to go on.

---

## When It Goes Wrong

| Symptom | Where it died | What it means |
|---|---|---|
| The user can't say which change they're reacting to | Step 7 | More than one change went in a pull request |
| A setup pull request changed what's on screen | Step 7 | It should have been written up, not made |
| A harvest changed a team's docs | Step 3 | A source was recorded as the user's when it isn't |
| Teammates were surprised by a stack | Step 7 | The stack went in a shared repository with no word to them, or no amendment to keep it quieter |
| Stacks keep coming back with the same corrections | Step 9 | The answers weren't harvested |
| The first real review reads like a linter | Step 10 | The reviewer was handed a real pull request before it knew the user |
