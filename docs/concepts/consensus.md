---
tags:
  - testnet 2.0
---

# Consensus

## What is consensus?
---

Consensus is an agreement to be reached among nodes in decentralized network. There are two types of nodes in ParallelChain F network, fullnode and light client.

**fullnode**: a network participant software with all the abilities of light clients, and further with the ability to:
- Respond to queries for transactions with the transaction itself and merkle proof of its inclusion in a block.
- Respond to queries for blocks.

**light client**: network participant software with the ability to:

- Query consensus and full nodes for finalized transactions as well as proof of its inclusion in a block.
- Query consensus and full nodes for full blocks.
- Form transactions and broadcast them into the mempool.
- Many more such 'thin client'-type functionalities.

## Consensus Mechanism
---

Consensus mechanism is a protocol to allow nodes to work in a distributed, fault tolerant and attacker-resistant network environment. Common consensus mechanism are `Proof-of-Work` and `Proof-of-Stake`. ParallelChain F uses BFT (Byzantine Fault Tolerant) based Proof of Stake Consensus Mechanism.
