# Code Review

## What Is Code Review?

Code Review is a set of plain-text procedures an LLM follows to review pull requests as close as it can to how the user would. A good review depends on knowing how the user likes code written and how their product works, and every LLM session starts knowing neither. The plugin gives the reviewer, the LLM following these procedures, a way to learn both, keep what it learned, and start each review from it.

The steps of a review and how its comments read are the same for every install, meaning each place the user sets it up. What differs is what the reviewer has learned in that place. The user answers the decisions only they can make and gives the final approval.

## How It Works

Every install follows the same three procedures. Setup runs once per install. Review and harvest repeat for every pull request.

**Set up.** The user puts the plugin where their LLM can read it and says which repositories they work in and where those repositories' documentation and coding standards live. The reviewer then opens stacks of five pull requests. Each pull request is one small change and one small guess: "I'd make this change, what do you think?" One change can touch more than one file, such as a utility and its test. The user's answers become its first standards. When the user is comfortable, they point it at a real pull request.

**Review.** The reviewer loads its standards and what it knows about the product, then starts three independent reads of the pull request. An orchestrator weighs the three, decides what's important, and writes one review, which the user sees before it's posted. The pull request's author responds, the reviewer checks the result, and it says when it thinks the pull request is ready. The user gives the final approval.

**Harvest.** The reviewer asks two questions: did this review teach anything about how the user likes code written, and did it teach anything about how the product works? It updates its memory and tells the user what changed, or that nothing did, without walking through its thinking.

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
│   └── cpf_harvesting.md            ← a harvest, for the user to read
├── procedures/
│   ├── README.md                    ← index of this directory
│   ├── sop_set-up.md                ← point it at the user's work and teach it
│   ├── sop_review-a-pr.md           ← three reads, one review, the user's approval
│   └── sop_harvest.md               ← what this review taught it
├── writing-styles/
│   ├── README.md                    ← index of this directory
│   └── review-comments/
│       ├── README.md                ← index of this directory
│       ├── ws_review-comments.md    ← what a review's comments have in common
│       └── examples/                ← a whole body, Call, Fix, Nit and Question
│           └── README.md            ← index of this directory
└── local/                           ← git-ignored; the install, never published
    ├── README.md                    ← index of this directory
    ├── sources.md                   ← the repositories, and where their documentation lives
    ├── teaching.md                  ← where each repository's last teaching stack stopped
    ├── held-back.md                 ← a harvest's change to a source it couldn't push
    ├── coding-standards/
    │   ├── README.md                ← index of this directory
    │   ├── short-term.md            ← in flight
    │   └── long-term/
    │       ├── README.md            ← the search index, or a pointer to a source the user controls
    │       └── [area].md            ← rules on one topic, when the install holds them
    ├── product/
    │   ├── README.md                ← index of this directory
    │   ├── short-term.md            ← in flight
    │   └── long-term/
    │       ├── README.md            ← the search index, or a pointer to a source the user controls
    │       └── [wiki or cpf files]  ← one topic per file, when the install holds them
    ├── examples/                    ← the user's own real reviews, in the comment style
    │   └── README.md                ← index of this directory
    └── amendments/                  ← how this machine carries out a step
        └── README.md                ← index of this directory
```

## The Plugin and the Install

**The plugin** is the harness: the procedures, the flow and the comment style. It's published and the same for everyone, and a review never changes it. A change to it is a pull request on this repository.

**The install** is what the reviewer knows in one place the user works, kept in `local/`. One install can cover many repositories, since a company's standards and documentation usually apply across all of them. A rule that holds in only one repository says so.

The install points to the primary documentation and reads it. A rule from a team's standards becomes the reviewer's once the user adopts it. When a primary source disagrees with the install, the reviewer asks the user once and remembers the answer.

Where long-term memory lives depends on who controls the source, for coding standards and product documentation alike. The LLM running setup decides for each source and records it in `sources.md`.

- **The user controls an established source:** that source is the long-term memory. The `long-term/` directory points to it, and a harvest commits and pushes changes there directly.
- **Someone else controls it:** the install reads it and keeps its own long-term memory. A harvest never changes it.
- **None exists:** the install builds its own long-term memory from nothing.

Every change a harvest makes is a commit, so the user can read the history or revert it without spending time on each one. `local/` is its own git repository, and never gets a remote, since what it holds names real places.

## Memory

Coding standards and product knowledge are each kept in two kinds of memory.

- **Short-term** holds what's in flight, such as a correction seen once, in the user's words. A review raises it only as a Nit or a Question. If it never comes back, it fades.
- **Long-term** holds what's settled. A review raises it as a Fix or a Call, or as a Nit when it's only about how code reads. It changes only with real force behind it, and it keeps its exceptions. It's a directory: its README is the search index, and each file covers one topic. A product file is a [CPF](https://github.com/JonathanYeeW/documentation-examples/tree/main/docs/product/core-product-flows) or a [wiki](https://github.com/JonathanYeeW/documentation-examples/tree/main/docs/product/wiki). A coding standards file holds rules: the rule, why, a Yes and a No in code, where it came from, and its exceptions.

A preference reaches long-term by coming back: after two or three times, the reviewer shows the user their words and asks whether it's a rule. A "no" is remembered, and the reviewer asks again only if it keeps coming up. A product fact reaches long-term by being checked against the code, with no question to the user, and a comment still checks its claim against the code.

## Review Comments

A review's body restates the pull request as the reviewer understood it. Its comments are Calls, Fixes, Nits and Questions.

- **Product call:** a decision about what the product does, headed by that decision as one plain question.
- **Code call:** the same, for a decision about how the code is written, such as two standards that disagree.
- **Fix:** a code change the pull request's author should make.
- **Nit:** a preference.
- **Question:** something the reviewer couldn't find out for itself.

Each Call is addressed with the user before the review is posted.

How they read is in `writing-styles/review-comments/`, and the examples are the style. Published examples are set in the PB&J Machine Company, from [`documentation-examples`](https://github.com/JonathanYeeW/documentation-examples). The user's real ones go in `local/examples/`.

## Amendments

An amendment is a file in `local/amendments/` that says how this machine carries out a step, such as posting through a reviewer account on one machine and having none on another, or running on a different LLM client. With no amendment, the plugin's plain default applies: a review is posted from the user's own GitHub sign-in.

An amendment can change how a step is carried out, never what the step is. It never edits the plugin, so updating the plugin never overwrites it.

## Getting Started

You need an LLM client that can read and write files and run commands, `git`, and the GitHub CLI (`gh`), signed in.

1. Clone this repository somewhere the LLM can read it
2. Tell the LLM to read this README, and say `set up the reviewer`
3. Answer its questions and its first stack of pull requests
4. When comfortable, say `review this PR <link>`

| Say | Runs |
|---|---|
| `set up the reviewer` | `procedures/sop_set-up.md`, once per install |
| `teach the reviewer` | `procedures/sop_set-up.md`, Phase 4, for another stack |
| `review this PR <link>`, or `review PR <number>` | `procedures/sop_review-a-pr.md` |
| `re-review <pull request>` | `procedures/sop_review-a-pr.md`, Phase 6 |
| `harvest <pull request>` | `procedures/sop_harvest.md`, for a review that wasn't harvested at the time |
