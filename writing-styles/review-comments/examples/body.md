Reviewed `a1b2c3d` against `main` at `f4e5d6c`. This is how I understand the pull request. If it's wrong, start there.

## Context

**Where this sits in the journey**

- At the machine, an operator reviews an order on Order Review before sending it to the kitchen.
- Each sandwich is a row, with the order's total below.

**Terms**

- **Remove sandwich** — takes one sandwich off an order. The rest of the order stays as it was.
- **Cancel order** — drops the whole order, including its name and pickup time.

## Summary

- An operator who added the wrong sandwich can now tap ✕ on its row, instead of cancelling the whole order and retyping its name and pickup time.
- Removing a sandwich asks first, then its row goes and the total drops by its price. There's no undo after that.
- Removing the last sandwich leaves an order with nothing in it.

## Questions

- Should the ✕ also appear on Receipt, or only here? The description doesn't say, and Receipt lists the same rows.
