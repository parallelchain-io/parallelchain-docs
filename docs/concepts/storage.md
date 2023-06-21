---
tags:
  - Storage
  - World State
---

# Storage

**Storage** is a decentralized database to store all necessary information to maintain a node and support data query of the blockchain. Unlike traditional decentralized databases which split data into chunks on different peers, each node instead broadcast the transactions that it received to other peer nodes. Once other nodes receive the transactions, they would execute transactions, verify the changes and update their database locally.

Elements inside storage include:

- `Blocks` - the full history of blocks from genesis.
- `World State` - a singular user-visible state maintained by the blockchain.
- `System state` - system data for running the node.

## Blocks
---
A block is a data structure in a blockchain system. Except for the first block (a.k.a genesis), each block contains the block hash of the previous block in the header. The whole chain of blocks is stored in the database and can be retrieved using block hash or block height easily.

See section [Block](block.md) for more information.

## World State
---
The world state is a singular user-visible state maintained by the blockchain stored inside a Merkle Patricia Trie (MPT). Each account is associated with some states, representing in the set of key-value pairs, and which can trigger state changes in specific ways.

There are three kinds of accounts, and they differ in how they trigger state changes:

- **Externally Owned Accounts**, which trigger state changes by digitally signing transactions
- **Contract Accounts**, which trigger state changes when called according to the logic in its code.
- **Network Accounts**, which triggers state changes by the next epoch transaction.

See section [Account](account.md) for more information about different types of accounts.

## System state
---
The system state stores system-level information to sync and communicate with other nodes in the blockchain. This part is non-accessible to the public and triggers state changes by consensus protocol.