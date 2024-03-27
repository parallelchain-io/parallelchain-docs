---
tags:
  - Block Header
---

# Blocks

**Block** is a fundamental data structure in a blockchain. It describes execution of a batch of [transactions](transactions.md). Blocks are "chained" by attaching a cryptographic hash of an earlier block. The hash is the proof of the data integrity of the earlier blocks because if their content have been changed, the hash will not be the same. This is how blockchain provides data immutability.

Summary of the elements inside a block:

- `Hash` - the unique identifier for the block
- `Height` - a number that represents the position of the block on the blockchain
- `Justify` - a certificate that proves the block was authorized by the network
- `Data Hash` - a hash that covers the content of the block header
- `Chain ID` - a number that identifies a particular Parallelchain Mainnet-based blockchain
- `Proposer` - the public address of the block producer
- `Timestamp` - the time the block was created, measured in seconds since 1970-01-01
- [Base Fee Per Gas](nodes.md#base-fee-per-gas-in-parallelchain-mainnet) - the minimum number of grays that a transaction must pay for every gas used to be included in the block
- `Gas Used` - the amount of [gas](gas.md) used in a block, which is the total sum of the gas used in executing the included transactions
- `Transactions Hash` - the root hash of the Merkle Tree that contains the transactions included in the block
- `Receipt Hash` - the root hash of the Merkle Tree that contains the execution results of the transactions included in the block
- `State Hash` - the root hash of the Merkle Tree that represents the current world state
- `Log Bloom` - a 256-byte block-level [Bloom Filter](https://en.wikipedia.org/wiki/Bloom_filter) that combines all the Bloom Filters of each Log topic from the block's receipts
- [Transactions](transactions.md) - the transactions that are included in the block
- [Receipts](transactions.md#receipt-and-logs) - the execution results of the transactions that are included in the block

## Inside a Block

Each block contains a unique identifier known as the `block hash`, which is calculated based on the contents of the block. The block hash acts as a fingerprint for the block, making it tamper-evident. Any change to the block's contents would alter its hash, making it easily detectable by network participants.

The block header contains important metadata about the block, including the block's `height`, `timestamp`, and the `address of the block producer`, among others. It also includes Merkle Patricia Tree root hashes for transactions, receipts, and the current state of the blockchain.

Transactions contained within the block are executed by the blockchain system, with the results recorded in receipts. These receipts contain information about the execution status of each transaction and are stored in a Merkle Patricia Tree, with the root hash included in the block header.

In addition to transactions and receipts, the block also contains a Bloom filter, which is a probabilistic data structure used to efficiently query logs associated with the transactions contained in the block.

Example of a block queried by [ParallelChain Client CLI](../for_users/pchain_client_cli/introduction.md):

```json
{
  "header": {
    "chain_id": 10000,
    "block_hash": "Yn0SAN6KDA3v2uDjN_j1P_tFCCqgqCbak1g7zvHwr9Q",
    "height": 2844080,
    "justify": {
      "chain_id": 10000,
      "view": 2844930,
      "block": "zT3iUT2sCmD80ztBDScBlAvUXhbzba7RTLtSiBLsvRg",
      "phase": "Generic",
      "signatures": {
        "signatures": [
          "hin8bfLnbuuUBRVB7GbClI0g1Ad0rfM_oDfvYILhwKPG_eqqZEDVyNj1c1VZeeRuUW94pqEQzAqbz4zQuTz0Dg",
          "5usLkVtsEsFCIHQOKi8X_LjvMxzBd4xxJJz7QSbl1pGLimFUkpV5MCBi1SGRY9jVHCArxu6v3B_jn3xCZfhrCg",
          null,
          null,
          "XqLSsQP6YkvtbBpPo7NX4o2Q7GvjAU-5dHfKPiDrdIBZPzTric_LvqJCgZLemUmzKkELkUl9BCBtFLth61qhAg",
          null,
          null,
          null,
          "PWhCgGM_SQpEf3NTZIdEIHecaFEd9MHptZqUyM4l7J4oR5aMrxkPYLRQ89msWfRtCF6fSvYVLZhTR-FjcIJqBg",
          "lLnEWWKxoToD-URLN7PDWFBQTyC1waS9KHBAskgdXdn_bZIyr3XYoqxIbTcHW6f0m6qozQGkZTXQukGbi08fCA"
        ]
      }
    },
    "data_hash": "Tr4Z99mUiaHGVuzW5eyZ73bKqEXUcYPV1C5RkUVCP2U",
    "timestamp": 1711515120,
    "base_fee": 8,
    "txs_hash": "wDUDiSjWumCmyEu_Fz0H8zqs3jSlpCMxN2NnST7fieM",
    "state_hash": "rU3ilPOG1Lr-4fYtGTHpSXsMvZM8v0l5JX3PgK-wauc",
    "receipts_hash": "uXRT1micHUt7orVPifaFg5BvOQJLd9K4mNAFo0zW0x4",
    "proposer": "TY0vjFcDm681NupNBVSU9DKNunP6JlwNNhc7lDJ2uU0"
  },
  "transactions": [
    {
      "commands": [
        {
          "Transfer": {
            "recipient": "kxrCl32WEzr7gr4Awb3jAMQtoh5zY8grabqlu-x42jY",
            "amount": 2000000000
          }
        }
      ],
      "signer": "qPReY5x_DPKAx1v30UrU1x1RI61Wd2OxeZo2eSWcjMU",
      "priority_fee_per_gas": 0,
      "gas_limit": 67500000,
      "max_base_fee_per_gas": 8,
      "nonce": 428,
      "hash": "dOrjtT7o5Y97mlLJFaCvdNUq6UgVQItVYGXHPoDw5n4",
      "signature": "RLM3k5VDDzBqkQ7ios4TMP9LwL50n8hNQ7OjH-wcTE0Ts6F3uWDaA5J52sCyYr3aGGGElXKiIatqH15qVwMUAA"
    }
  ],
  "receipts": [
    [
      {
        "status_code": "Success",
        "gas_used": 32820,
        "return_values": "",
        "logs": []
      }
    ]
  ]
}
```

## How Block Works

Blocks are produced by validator [Nodes](nodes.md). To produce a block, the node executes transactions and creates receipts as a result of execution. The transactions and receipts become the content of a block, and the node will then compute the metadata such as `block height`, `timestamp`, hashes, etc. in the block header.

When a block is created by a node, the validator nodes will validate it and then agree on the block content through a [consensus mechanism](consensus_mechanism.md). An agreed block is said to be **committed**. If the block is invalid, it will not be part of the blockchain. Nodes will keep producing new blocks, and this process repeats within a time interval (i.e. block time). As a result, the blockchain grows with more and more blocks.

## Epoch Block

**Epoch Block** is a special block created by executing a transaction with [Next Epoch Command](transactions.md#protocol-commands). This special block signifies the last block in an **Epoch**.

Epoch is a protocol-defined period for measuring the performance of operators on the blockchain network. 

The primary purpose of epochs in a blockchain is to facilitate various network functionalities and maintain consensus among network participants by defining a common reference point, which could be utilized to validate transactions. During one epoch, there would be block creation and block addition to the blockchain.

In the ParallelChain Mainnet ecosystem, an `epoch` is defined by a predetermined block height or the number of blocks added to the blockchain, which is 8640. When a specific block height is reached or another 8640 blocks are added to the blockchain, the blockchain network would move from the current epoch to the next. The epoch transition could trigger critical events:

- Reward stakes in Current Validator Set
    - *Increase deposits* of owner and operator
    - *Update stakes* if auto-stake-rewards is enabled
    
- Replacing Previous Validator Set with Current Validator Set
- Replacing Current Validator Set with Next Validator Set
- Returning Next Validator Set for the next leader selection

Epoch transition ensures that only one set of validators is active on the blockchain network at any given time, which makes the information on the blockchain tamper-proof.

