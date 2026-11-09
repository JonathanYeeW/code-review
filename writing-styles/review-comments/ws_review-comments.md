# Writing Style — Review Comments

**Created:** 2026-11-06
**Who reads it:** a person reading a comment cold, and an LLM applying it to the pull request. Both have to be able to act on it
**When to use it:** writing a review's body and every comment posted on a pull request

## Summary

A review comment says what's wrong in product language, anchored to the code, so a person can follow it without opening the code and an LLM can apply it without working it out again. The body restates the pull request as the reviewer understood it, like a PR description, so the author can correct that before reading anything else. The examples are the style; this file only says what they have in common.

**What it does**

- **Gives a Fix four parts under its title:** What's happening, The problem, Recommended change, and How I checked.
- **Says what's happening from what the person does**, naming the code only to anchor it: "Open an order with one sandwich and tap ✕", then `OrderProvider` where it helps. A Fix the person would notice shows that as a journey.
- **Shows the recommended change as code**, trimmed to the lines that change.
- **Runs about as long as its example.** If it runs much longer, it holds something the reader doesn't need to decide.
- **Holds one finding per comment.** A second change gets its own comment, unless the first can't be applied without it.
- **Heads a Call with its decision**, as one plain question that leans to neither option, labelled a Product call or a Code call. It has the same What's happening and The problem, then Options, each ending in what it means for the reader, then How I checked. What's happening says what already works before the case that doesn't.
- **Says whose recommendation a Call carries:** the user's, in their words, the reviewer's, marked as not reviewed by the user, or none, left to the author.
- **Asks a Question without stating an answer**: "Can the kitchen receive an empty order?", not "the kitchen receives an empty order, should it?" Under it: What's happening, What I couldn't check, and How to try it.
- **Says how it was checked, in a line or two.** Every Fix and Call ends with How I checked.
- **Keeps a Nit to one or two lines.**
- **Organizes the body like the top of a PR description, at product level:** Context, with where this sits in the journey and Terms, then Summary, as bullets of what's changed for the person. A pull request nothing changes on screen for says so in a line.
- **Ends the body with any overall Questions.** Fixes, Nits and Calls are never summarized there, because the comments hold them.

**What it leaves out**

Anything the reader doesn't need to decide: the reviewer's route to the finding, every file it opened, and a second statement of the same point.

## Examples

Each example in `examples/` is a whole piece, and together they're one review of one pull request in the PB&J Machine Company. Read them before writing a review.

The user's real reviews go in `local/examples/`, which is never published. Read those too.

## Adding an Example

When a comment is accepted without a change, ask whether it shows the style somewhere the examples don't yet: a different kind of finding, or a problem the others didn't have.

Copy it whole into `local/examples/`, real product and all. When a newer example shows the same thing better, it replaces the older one rather than joining it.
