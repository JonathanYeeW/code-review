**Product call:** when an order loses a sandwich, should the pickup time move up?

**What's happening:** the pickup time is set when the order is created, from how many sandwiches it holds. Removing one leaves it as it was.

**The problem:** an operator who removes four of five sandwiches is told to wait for all five. It only matters on large orders.

**Options:**

- **A.** The pickup time stays as promised. A means nobody is told an earlier time the kitchen might miss.
- **B.** The pickup time moves up. B means the customer waits less, and the kitchen's queue has to be asked again.

**No recommendation.** This one is yours to decide.

**How I checked:** traced the pickup time from order creation to the receipt.
