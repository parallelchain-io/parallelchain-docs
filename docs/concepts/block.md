---
tags:
  - testnet 3
---

# Block

## What is a block?
---

**Block**: A data structure that describes and authorizes the execution of a batch of transactions (state transitions) on the blockchain.

**Blockchain:** A broad term that can refer to:

- The canonical sequence or 'chain' of blocks and the raw data directly available in this sequence, for example: block hashes, transactions, and events.
- The above, but further including data that is derivable from raw block data. This includes, most prominently, the world state.
- The network of full nodes and clients that maintain the correctness and consistency of the blockchain data structure.

## What's in a block?
---

- `App ID` - Id of the blockchain
- `Block Hash` - Block hash of this block
- `Height` - Identifier for a height of a block on the blockchain encoded as a number
- `Justify` - Quorum Certificate
- `Data Hash` - Hash over Data
- `Version number` - Identifier for the set of block validation rules for the blockchain
- `Timestamp` - Unix timestamp (number of seconds since 1970-01-01)
- `Transaction Trie root hash` - Merkle Tree root hash of transactions
- `State root hash` - Merkle Tree root hash of current world-state
- `Receipt Trie root hash` - Merkle Tree root hash of receipts
- `Transactions` - the transactions included in the block
- `Receipts` - the execution result of transactions which are included in the block

