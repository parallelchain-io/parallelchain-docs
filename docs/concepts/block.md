---
tags:
  - testnet 2.0
---

# Block

## What is a block?
---

**Block**: A data structure that describes and authorizes the execution of a batch of transactions (state transitions) on the blockchain.

**Blockchain:** A broad term that can refer to:

- The canonical sequence or 'chain' of blocks and the raw data directly available in this sequence, for example: block hashes, transactions, and events.
- The above, but further including data that is derivable from raw block data. This includes, most prominently, the world state.
- The network of full nodes and light clients that maintain the correctness and consistency of the blockchain data structure.

## What's in a block?
---

- `Blockchain ID` - Id of the blockchain
- `Block version number` - Version number of the block
- `Timestamp` - Unix timestamp (number of seconds since 1970-01-01)
- `Previous block hash` - Block hash of previous block
- `This block hash` - Block hash of this block
- `State root hash` - Merkle Tree root hash of current world-state
- `Transaction root hash` - Merkle Tree root hash of transactions
- `Receipt root hash` - Merkle Tree root hash of receipts
- `Proposer public key` - Public key of proposer
- `Signature on the block header` - A digital Signature on the block header
- `Transactions` - the transactions included in the block
- `Receipts` - the execution result of transactions which are included in the block

