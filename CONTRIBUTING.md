# Contributing to Code Review

This document covers how the plugin is laid out and what a change to it has to satisfy: what's published and what isn't, what may be named, where a file goes, how a flow and its procedure stay in step, and how a change is tested. What the plugin does, and how to start it, is in [`README.md`](README.md).

## Architecture

The plugin has two parts, split by who they belong to. A change belongs to exactly one.

| Part | Where | Holds | Changes by |
|---|---|---|---|
| The plugin | `docs/`, `procedures/`, `writing-styles/` | How a review runs and how it reads. The same for everyone | A pull request on this repository |
| The install | `local/` | What the reviewer knows in one place, and how this machine differs. Never in this repository | A harvest, or the user |

**An opinion belongs in the plugin.** The plugin has a point of view on how a review runs and reads, so it doesn't offer settings for it. A preference about how a review reads is a change to the writing style, not a slot in a procedure.

**What a place taught belongs in the install.** A coding standard or a fact about a product is never added to the plugin, however general it looks.

**An amendment can change how a step is carried out, never what the step is.** A step that varies by machine, such as posting, names its amendment and gives a plain default. Add a new one only when a machine can't do a step the default way.

**A harvest never changes the plugin.** A lesson about how a review should run goes in the harvest's note as a suggestion, and becomes a pull request here if the owner agrees.

**Nothing published names a real person, company, repository or project,** with one exception: [`documentation-examples`](https://github.com/JonathanYeeW/documentation-examples), the public collection whose document types this plugin follows. Examples are set in its PB&J Machine Company. A real name makes a stranger's reviewer follow someone else's rule.

### Directory Structure

```
code-review/
├── README.md             # What it is and how to start it
├── CONTRIBUTING.md       # This file
├── docs/                 # For a person to read: what happens and why
├── procedures/           # For an LLM to follow: what to do, in order
├── writing-styles/       # How a review reads. The examples are the style
└── local/                # Git-ignored, and its own git repository: one install
```

**`docs/` explains, `procedures/` instructs.** A CPF is written in product language for a person. An SOP is written for an LLM and carries the commands. A file that does both is split.

**Every directory has a README that indexes it,** so an LLM can find its way through the tree from the READMEs alone.

## Writing a File

**Each file follows its type in `documentation-examples`.** CPFs: `docs/product/core-product-flows/`. SOPs: `docs/llm/sops/`. Writing styles: `writing-styles/`.

**The examples are the style.** To change how a comment reads, change its example first, and keep the style file to what the examples have in common. A new kind of comment gets a new whole example.

**A fact has one home.** The README glosses, a CPF tells the story, and an SOP gives the steps. A rule the SOP and CPF both state is kept word for word alike.

## Keeping It Small

**A change to a procedure removes about as much as it adds.** Every line in a procedure is read on every review, and a reviewer with no limit grows until nobody reads what it writes. Before adding a line, find one it replaces. A change that can't has to say why it's worth the length.

**A rule answers something that happened.** Don't add a line for a case that hasn't come up, or a guess an LLM made that changed nothing.

**A review's length is part of what a change is tested on.** A change that makes reviews longer has to be worth what they cost to read.

## Keeping a Flow and Its Procedure in Step

Each SOP has a CPF that tells the same story: `sop_set-up.md` and `cpf_setting-up.md`, `sop_review-a-pr.md` and `cpf_reviewing-a-pr.md`, `sop_harvest.md` and `cpf_harvesting.md`. `cpf_code-review.md` is the whole flow.

- **A change to a gate, a step or a rule lands in the SOP, its CPF and the whole flow,** in the same change.
- **A change to what a procedure writes into `local/`** updates the README's tree, and the rule or entry shape in `sop_harvest.md`'s example.
- **A CPF's symptom table gets a row** when a change exists because something went wrong.

## Testing a Change

**A cold read.** Start a fresh agent that reads only a clone of this repository, starting at the README. Ask it to carry out the procedure you changed. It reports where it guessed and every question it would ask. A question it can only answer by asking you is a gap.

**One real run.** A change to a procedure runs on a real pull request before it's relied on, with its own `local/`, posting nothing until the user says so. Report the comment count and length beside the last review's. A cold read shows whether the words are clear. Only a run shows whether a review is any good.

**A blind rerun for what reads find.** To test whether reads still catch what they should, rerun on a commit that already has a review, from a description and a memory with that review kept out. Whoever writes the review must not have seen the earlier one.

## Review and Acceptance

**A change is accepted by the plugin's owner, not merged on its own.** The procedures are what every review after it follows, so a wrong step repeats.

## Commit Messages

Conventional commit prefixes, all lowercase: `feat:`, `bug:`, `refactor:`, `docs:`. A change to a procedure's behavior is `feat:`. A change to wording only is `docs:`.
