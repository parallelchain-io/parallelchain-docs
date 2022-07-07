---
tags:
  - testnet 2.0
---

# Mempool

## What is mempool?
---

**Mempool**: A broad term which can refer to:

- A component of validating and full nodes which accepts not-yet-finalized transactions for future inclusion in a block (only validating nodes can produce new blocks).
- The fuzzy set of not-yet-finalized transactions included in the network's individual mempools. This set is fuzzy because individual mempools are unlikely to consistent at any given time due to network delays and and partitions.

## How does mempool work?
---

Transaction requests are sent to mempool and be pending for execution. Mempool functions as a queue to store the pending transaction and arrange their order to be executed by nodes. The ranking formulation follows the criteria:

- Transactions are executed sequentially according to `nonce`, a component of the account requesting these transactions.
- Validating node chooses transactions reasonably to maximize the benefits (Tip, Gas Price, etc.) gained by executing them.