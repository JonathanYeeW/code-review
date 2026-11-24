# Code Review

## What Is Code Review?

Code Review is a set of plain-text procedures an LLM follows to review pull requests, so the user can honestly put their name on a review they didn't write. The reviewer, the LLM following these procedures, does the reading. The user does the understanding and the judging: they understand what the pull request does and stand behind its Calls. The Fixes and Nits belong to the reviewer.

A good review depends on knowing how the user writes code, how they build systems, and how the product works, and every LLM session starts knowing none of them. The plugin gives the reviewer a place to keep all three and start each review from them.

It's built for work, where the author is a colleague, reviews run over days, and several pull requests are open at once. The steps of a review and how its comments read are the same for every install, meaning each machine the user sets it up on. What differs is what that install knows.

## How It Works

**Set up,** once per install. The user says which products it reviews, where their repositories and docs are, and how this machine differs. It can start from another install's coding standards and systems, so a new machine reviews the same way on day one.

**Review.** The reviewer reads what it knows, then starts three independent reads of the pull request, and an orchestrator weighs them into one review. When it's ready, it asks "Process it?", then walks the user through it: what the pull request does in product language, which the user confirms they understand, then each Call, which the user confirms is real. Every Call is left to the author, with the reviewer's recommendation labeled as the reviewer's. On "Calls are right, post it", it posts. Every stop says who has the ball. Each re-review checks the author's push like a new diff, then walks the user through what changed. The user gives the final approval.

**Harvest,** without the user. The reviewer learns from what the user corrected, promotes a correction that keeps coming back into a rule, and, once the pull request has merged, brings the product's docs up to date. It tells the user what changed in a short note and asks nothing.

**Update the wiki,** whenever the user says, or from a harvest. It reads everything merged since the product's docs were last synced, reviewed or not, checks what each change made wrong against the code, and writes it where the docs live. A change in a git repository waits on a branch for `push the wiki`.

The whole flow is `docs/cpf_code-review.md`, and each procedure has its own CPF beside it.

## Directory Structure

```
code-review/
├── README.md                        ← this file
├── CONTRIBUTING.md                  ← what a change to the plugin has to satisfy
├── .gitignore                       ← leaves local/ out
├── LICENSE                          ← MIT
├── docs/
│   ├── README.md                    ← index of this directory
│   ├── cpf_code-review.md           ← the whole flow, step by step
│   ├── cpf_setting-up.md            ← setup, for the user to read
│   ├── cpf_reviewing-a-pr.md        ← a review, for the user to read
│   ├── cpf_harvesting.md            ← a harvest, for the user to read
│   └── cpf_updating-the-wiki.md     ← a wiki update, for the user to read
├── procedures/
│   ├── README.md                    ← index of this directory
│   ├── sop_set-up.md                ← point it at the user's work
│   ├── sop_review-a-pr.md           ← three reads, one review, the user's walk and approval
│   ├── sop_harvest.md               ← what this review taught it
│   └── sop_update-the-wiki.md       ← a product's docs, brought up to date
├── writing-styles/
│   ├── README.md                    ← index of this directory
│   └── review-comments/
│       ├── README.md                ← index of this directory
│       ├── ws_review-comments.md    ← what a review's comments have in common
│       └── examples/                ← a whole body, Call, Fix, Nit and Question
│           └── README.md            ← index of this directory
└── local/                           ← git-ignored; the install, never published
    ├── README.md                    ← index of this directory
    ├── ledger.md                    ← what's open, and who has the ball
    ├── coding-standards/            ← how the user writes code
    │   ├── README.md                ← index, and references the user can't change
    │   ├── short-term.md            ← in flight
    │   └── long-term/               ← settled, in folders by the question a reader asks
    │       └── README.md            ← the search index and routing table
    ├── system/                      ← how the user builds systems
    │   └── README.md                ← the search index and routing table, and references
    ├── product/                     ← each product
    │   ├── README.md                ← its repositories, where its docs live, and references
    │   └── [product]/               ← docs for a product with none of its own
    └── amendments/                  ← how this machine carries out a step
        └── README.md                ← index of this directory
```

## The Plugin and the Install

**The plugin** is the harness: the procedures, the flow and the comment style. It's published and the same for everyone, and a review never changes it. A change to it is a pull request on this repository.

**The install** is what the reviewer knows on one machine, kept in `local/`. It's its own git repository with no remote, since what it holds names real places. Every change a procedure makes is a commit, so the user can read the history or revert it. Each folder's README routes to everything about it, including docs that live elsewhere.

## Three Kinds of Knowledge

| | Coding standards | System | Product |
|---|---|---|---|
| Holds | How the user writes code | How the user builds a repo, an app, its integrations and infrastructure | What one product does, and where it departs from the system |
| True because | The user's taste | The user's choice, then the code | The code |
| Memory | Short-term, then long-term | Facts, with no short-term | Facts, with no short-term |
| Changed by | The harvest | The user, or a question `update the wiki` raises | `update the wiki` |
| Lives | `local/coding-standards/` | `local/system/` | The product's own docs, or `local/product/[product]/` |

- **Short-term** coding standards hold a correction in the user's words, with how many times it's been seen. A review raises one only as a Nit or a Question.
- **Long-term** holds what's settled. A review raises it as a Fix or a Call, or as a Nit when it's only about how code reads. A correction reaches it by coming back two or three times; the harvest writes the rule and says so in a line.
- **A reference** is a doc the user reads but can't change, such as a team's wiki at work. Its row in the matching README says where it is and which of its rules the user has adopted. A rule from it applies only once adopted.

## Review Comments

A review's body opens with what the pull request does, in product language, then restates it as the reviewer understood it. Its comments are Calls, Fixes, Nits and Questions.

- **Product call:** a decision about what the product does, headed by that decision as one plain question.
- **Code call:** the same, for a decision about how the code is written, such as two standards that disagree.
- **Fix:** a code change the pull request's author should make.
- **Nit:** a preference.
- **Question:** something the reviewer couldn't find out for itself.

Every Call is the author's to decide. It carries the reviewer's recommendation, labeled as the reviewer's, and the user confirms it's real before it's posted.

How they read is in `writing-styles/review-comments/`, and the examples are the style. Published examples are set in the PB&J Machine Company, from [`documentation-examples`](https://github.com/JonathanYeeW/documentation-examples).

## Amendments

An amendment is a file in `local/amendments/` that says how this machine carries out a step, such as posting through a reviewer account on one machine and having none on another, or running on a different LLM client. With no amendment, the plugin's plain default applies: a review is posted from the user's own GitHub sign-in.

An amendment can change how a step is carried out, never what the step is. It never edits the plugin, so updating the plugin never overwrites it.

## Getting Started

You need an LLM client that can read and write files and run commands, `git`, and the GitHub CLI (`gh`), signed in.

1. Clone this repository somewhere the LLM can read it
2. Tell the LLM to read this README, and say `set up the reviewer`
3. Answer its questions
4. Say `update the wiki` for each product, then `review this PR <link>`

| Say | Runs |
|---|---|
| `set up the reviewer` | `procedures/sop_set-up.md`, once per install |
| `review this PR <link>`, or `review PR <number>` | `procedures/sop_review-a-pr.md` |
| `re-review <pull request>` | `procedures/sop_review-a-pr.md`, Phase 5 |
| `review status` | `procedures/sop_review-a-pr.md`: what's open, and who has the ball |
| `harvest <pull request>` | `procedures/sop_harvest.md`, for a review that wasn't harvested at the time |
| `update the wiki` | `procedures/sop_update-the-wiki.md` |
| `push the wiki` | `procedures/sop_update-the-wiki.md`, Phase 4 |
