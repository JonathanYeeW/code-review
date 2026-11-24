# Procedures

What an LLM follows, step by step. Each one has a CPF in `../docs/` that tells the same story.

| File | Trigger | What It Produces |
|---|---|---|
| `sop_set-up.md` | `set up the reviewer` | An install that knows the user's standards, products and machine |
| `sop_review-a-pr.md` | `review this PR <link>` or `review PR <number>`, `re-review <pull request>`, `review status` | A review the user understands, posted and seen through to the final approval |
| `sop_harvest.md` | The user's go or no-go on a pull request, or `harvest <pull request>` | Updated coding standards, and a short note |
| `sop_update-the-wiki.md` | `update the wiki`, a harvest of a merged pull request, or `push the wiki` | A product's docs brought up to date with what merged |
