**Product call:** where does removing the last sandwich leave the order: open and empty, or cancelled?

**What's happening:** removing any sandwich but the last works. Removing the last one:

```
1. Open an order with one sandwich, tap ✕     the row goes, the total reads $0
2. Tap Submit                                 the kitchen rejects the empty order
```

**The problem:** the operator finds out only after Submit that the order can't be made. It comes up whenever someone swaps their only sandwich.

**Options:**

- **A.** The order stays open and empty, and Submit is disabled. A means the operator keeps the name and pickup time.
- **B.** The order is cancelled and the screen goes back to the menu. B means the operator starts over.

**Sam's recommendation: A.** "They're usually about to add another sandwich, and retyping the pickup time is what people hate."

**How I checked:** traced from the ✕ on the last row through to the kitchen's response.
