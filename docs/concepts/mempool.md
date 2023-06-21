---
tags:
  - mempool
  - MaxBaseFeePerGas
---

# Mempool

## What is mempool?
---

A **mempool** is a component of the blockchain network that keeps transactions that have not yet been added to a block. It can refer to two things:

- The mempool of validating nodes and fullnodes, which accepts pending transactions for future inclusion in a block. (Only validating nodes can create new blocks.)
- The set of not-yet-confirmed transactions that are stored in the individual mempools of nodes in the network. This set can vary between nodes due to network delays and partitions.

## How does mempool work?
---

When a transaction is submitted to the blockchain network, it is first sent to the mempool where it waits to be executed. The mempool acts like a queue, storing the pending transactions and arranging them to be executed by block producers. Transactions are prioritized based on how much the sender is willing to pay for network resources, which is indicated in a field called `Max Base Fee Per Gas` within the transaction. The higher the fee, the higher the priority of the transaction.

## Max Base Fee Per Gas and EIP-1559 in Ethereum
---

The priority ranking method based on the `Max Base Fee Per Gas` is a crucial component of the EIP-1559 model for transaction processing in Ethereum.

In the EIP-1559 model, each transaction specifies two fees: the `base fee` and the `priority fee`. The `base fee` is dynamically determined by the network based on the current demand for block space and adjusts itself to keep block utilization at a target level. The `priority fee` is the amount that the user is willing to pay in addition to the base fee to prioritize their transaction.

When a transaction is submitted to the network, it is placed in the mempool, and the `priority fee` value of the transaction is used to rank it. Transactions with higher priority fees are placed higher in the mempool, making them more likely to be included in the next block. However, unlike the traditional method of simply sorting transactions by the fee amount, EIP-1559 allows for more efficient use of block space by dynamically adjusting the `base fee`, which incentivizes users to pay only what they need to get their transaction included. This helps to prevent congestion and ensures that transactions are processed in an efficient and timely manner.

## Base Fee Per Gas in ParallelChain Mainnet
---

In ParallelChain Mainnet, the `base fee per gas` is calculated according to the [Base fee formula](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/Blockchain.md#base-fee-formula). This is nearly identical to Ethereum's EIP-1559 and shares the same purpose: *adjust the cost of a transaction based on how busy the network is*.

> **Note:** Validator nodes in ParallelChain Mainnet provided at the initial Launch do not take `priority fee` into the ranking calculation.