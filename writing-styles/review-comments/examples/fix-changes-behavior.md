**Fix** (changes behavior): tapping ✕ during a submit removes the sandwich from the receipt, but the machine still makes it

**What's happening:**

```
1. Tap Submit on an order with two sandwiches     "Sending to the kitchen"
2. Tap ✕ on the first sandwich before it lands    the row goes
3. The submit finishes                            the receipt shows one, the machine makes two
```

**The problem:** the customer pays for one sandwich and the machine uses ingredients for two. It can only happen in the second or so a submit takes, so it's rare.

**Recommended change:** disable ✕ during a submit, the way Submit already is.

```tsx
<RemoveButton disabled={orderStatus === "submitting"} onPress={onRemove} />
```

**How I checked:** reproduced in a test with a delayed submit, which fails today and passes with this change. Then undone.
