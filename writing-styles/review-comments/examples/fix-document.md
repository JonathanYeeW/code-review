**Fix** (doesn't change behavior): `docs/design/review-screen.md:12` says every row on Order Review is fixed

**What's happening:** the line is outside this diff: "An order's rows can't be changed once they're on Order Review." Each row now has a ✕, labelled Remove sandwich.

**The problem:** the next person designing this screen starts from a rule this pull request ended.

**Recommended change:**

```md
- An order's rows can be removed on Order Review, one at a time. Nothing else about a row changes there.
```

**How I checked:** compared the line with `OrderReviewScreen`'s rows. This pull request doesn't touch that document.
