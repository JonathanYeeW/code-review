**Code call:** does removing a sandwich live on `OrderProvider`, or on the review screen?

**What's happening:** nothing on screen changes either way. Removing is written in `OrderReviewScreen`, while every other change to an order is on `OrderProvider`.

**The problem:** the standard (`mobile/providers.md`) says "a provider owns its writes", but this one is only ever called from one screen, and `mobile/screens.md` says "logic only one screen uses stays on that screen". The two rules point opposite ways.

**Options:**

- **A.** Move it to `OrderProvider`. A means every change to an order is in one place.
- **B.** Leave it on the screen. B means less code moves, and it moves when a second screen needs it.

**The reviewer's recommendation, not reviewed by Sam: A.** Receipt lists the same rows, so a second caller is likely.

**How I checked:** made both changes in a worktree. Typecheck and all 640 tests pass either way. Then undone.
