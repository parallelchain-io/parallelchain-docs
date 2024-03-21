---
tags:
  - nodes
  - runtime
  - world state
  - p2p
  - mempool
---

# Nodes

ParallelChain Lab currently operates 10 [validator](staking.md#validator) nodes. They are all in the same node type - Fullnode.

Fullnode is node software that stores and maintains a full copy of the blockchain. The specification can be found in the [ParallelChain Mainnet Protocol](https://github.com/parallelchain-io/parallelchain-protocol). 

Fullnode starts with exchanging messages with other nodes in a [P2P Network](#p2p-network). Through a [Consensus Machanism](consensus_mechanism.md), it produces and validates blocks by executing transactions. The execution takes place in a component call [Runtime](#runtime) which transits the state of the blockchain. The resulting state is then stored in a data structure called [World State](#world-state).

Fullnode also runs a HTTP server to serve RPC requests. Transactions can be submitted in the RPC request. Fullnode buffers those transactions in the [Mempool](#mempool), and then waits for executing them.

## Runtime

The [Runtime](https://github.com/parallelchain-io/pchain-runtime) is the component in the ParallelChain protocol which executes transactions.

Runtime can be considered as a **Transition Function** that deterministically executes transactions. Execution of the transition function proceeds through three phases: Pre-Charge, Commands, and Charge. The sequence flow of the pre-charge and charge phases are common to all transactions, while the sequence flow of the commands phase varies according to the composition of a transaction's vector of [commands](transactions.md#commands).

![Runtime Sequence Flow](../img/Runtime%20Phases.png)

- **Pre-Charge Phase**: makes simple checks to ensure that the transaction can be included in the block.
- **Commands Phase**: executes the sequence of commands included in the transaction.
- **Charge Phase**: performs settlement of balances for charging the total gas used in previous phases.


## World State

The [World State](https://github.com/parallelchain-io/pchain-world-state) is a set of key-value tuples representing the state of every account, stored inside a set of Merkle Patricia Trie (MPT) data structures. It is organized into five sections:

- **Merkle Patricia Trie** specifies the variant of the MPT data structure that the ParallelChain protocol uses. The world state is comprised of two kinds of MPTs.
- **Accounts Trie** specifies the first kind of MPT in the world state: the singular Accounts Trie.
- **Storage Tries** specifies the second kind of MPT: the Storage Trie. Unlike the accounts trie, which is singular, there can be many storage tries.
- **Network Account Storage** specifies the contents of the storage trie under a special account called the Network Account.
- **Genesis State** specifies the initial contents of the entire world state, before any transaction is executed.

## P2P Network

ParallelChain replicas maintain a peer-to-peer network (P2P). P2P implements two high-level functionalities:

- **Messaging**: P2P enables replicas to send messages to each other to replicate the state machine, maintain a pool of pending transactions, as well as notify each other of dropped transactions.
- **Peer Discovery**: in order for messages to eventually be received by all of their intended recipients, the P2P network needs to be connected. However, forming a connected network topology is challenging, because the number of replicas for any particular ParallelChain blockchain is unbounded and can far exceed the number of transport-layer connections that a computer can feasibly maintain. Starting from a small set of "boot nodes", P2P's peer discovery forms and maintains a connected network topology without requiring every replica to be aware of every other replica.

## Mempool

A **mempool** is a component of the blockchain network that keeps transactions that have not yet been added to a block. It can refer to two things:

- The mempool of validating nodes and fullnodes, which accepts pending transactions for future inclusion in a block. (Only validating nodes can create new blocks.)
- The set of not-yet-confirmed transactions that are stored in the individual mempools of nodes in the network. This set can vary between nodes due to network delays and partitions.

### How does Mempool Work?

When a transaction is submitted to the blockchain network, it is first sent to the mempool where it waits to be executed. The mempool acts like a queue, storing the pending transactions and arranging them to be executed by block producers. Transactions are prioritized based on how much the sender is willing to pay for network resources, which is indicated in a field called `Max Base Fee Per Gas` within the transaction. The higher the fee, the higher the priority of the transaction.

### Max Base Fee Per Gas and EIP-1559 in Ethereum

The priority ranking method based on the `Max Base Fee Per Gas` is a crucial component of the EIP-1559 model for transaction processing in Ethereum.

In the EIP-1559 model, each transaction specifies two fees: the `base fee` and the `priority fee`. The `base fee` is dynamically determined by the network based on the current demand for block space and adjusts itself to keep block utilization at a target level. The `priority fee` is the amount that the user is willing to pay in addition to the base fee to prioritize their transaction.

When a transaction is submitted to the network, it is placed in the mempool, and the `priority fee` value of the transaction is used to rank it. Transactions with higher priority fees are placed higher in the mempool, making them more likely to be included in the next block. However, unlike the traditional method of simply sorting transactions by the fee amount, EIP-1559 allows for more efficient use of block space by dynamically adjusting the `base fee`, which incentivizes users to pay only what they need to get their transaction included. This helps to prevent congestion and ensures that transactions are processed in an efficient and timely manner.

### Base Fee Per Gas in ParallelChain Mainnet

In ParallelChain Mainnet, the `base fee per gas` is calculated according to the [Base fee formula](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/Blockchain.md#base-fee-formula). This is nearly identical to Ethereum's EIP-1559 and shares the same purpose: *adjust the cost of a transaction based on how busy the network is*.

!!! Note
    Validator nodes in ParallelChain Mainnet provided at the initial Launch do not take `priority fee` into the ranking calculation.