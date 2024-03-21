---
tags:
  - Block Header
---

# Blocks

**Block** is a data structure that describes and authorizes the execution of a batch of transactions (state transitions) on the blockchain.

Elements inside a block include:

- `Hash` - the unique identifier for the block
- `Height` - a number that represents the position of the block on the blockchain
- `Justify` - a certificate that proves the block was authorized by the network
<!-- - `Data Hash` - a hash that covers the content of the block header -->
- `Chain ID` - a number that identifies a particular Parallelchain Mainnet-based blockchain
- `Proposer` - the public address of the block producer
- `Timestamp` - the time the block was created, measured in seconds since 1970-01-01
- `Base Fee Per Gas` - the minimum number of grays that a transaction must pay for every gas used to be included in the block
- `Gas Used` - the amount of gas used in a block, which is the total sum of the gas used in executing the included transactions
- `Transactions Hash` - the root hash of the Merkle Tree that contains the transactions included in the block
- `Receipt Hash` - the root hash of the Merkle Tree that contains the execution results of the transactions included in the block
- `State Hash` - the root hash of the Merkle Tree that represents the current world state
- `Log Bloom` - a 256-byte block-level Bloom Filter that combines all the Bloom Filters of each Log topic from the block's receipts
- `Transactions` - the transactions that are included in the block
- `Receipts` - the execution results of the transactions that are included in the block

---

A block is a fundamental data structure in a blockchain system that serves as a container for a batch of transactions. It plays a critical role in maintaining the integrity and immutability of the blockchain.

Each block contains a unique identifier known as the `block hash`, which is calculated based on the contents of the block. The block hash acts as a fingerprint for the block, making it tamper-evident. Any change to the block's contents would alter its hash, making it easily detectable by network participants.

The block header contains important metadata about the block, including the block's `height`, `timestamp`, and the `address of the block producer`, among others. It also includes Merkle tree root hashes for transactions, receipts, and the current state of the blockchain.

Transactions contained within the block are executed by the blockchain system, with the results recorded in `receipts`. These receipts contain information about the execution status of each transaction and are stored in a Merkle tree, with the root hash included in the block header.

In addition to transactions and receipts, the block also contains a Bloom filter, which is a probabilistic data structure used to efficiently query logs associated with the transactions contained in the block.

Overall, block is an essential component of a blockchain system, providing a secure and reliable means of recording and validating transactions while ensuring the immutability and integrity of the blockchain.