**Fix** (doesn't change behavior): the order's status is held twice

**What's happening:** on the review screen, Submit reads the order's status from `OrderProvider`, and the leave-the-screen prompt reads a copy the screen keeps itself, `submitStatus`. Both are set at the same three points around the submit.

**The problem:** nothing on screen is wrong today, but a later change that updates one and not the other leaves Submit saying "Sending" while the prompt says it's safe to leave.

**Recommended change:** one copy, on the provider.

```tsx
// OrderProvider
const value = { order, orderStatus, onSubmit };

// OrderReviewScreen, with submitStatus removed
const { orderStatus } = useOrder();
```

**How I checked:** made the change in a worktree. Typecheck and all 640 tests pass. Then undone.
