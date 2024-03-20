---
tags:
  - Consensus
  - hotstuff.rs
---

# HotStuff-rs 
HotStuff-rs is a Rust Programming Language implementation of the HotStuff consensus protocol. It offers:

1. Guaranteed Safety and Liveness in the face of up to 1/3rd of Voting Power being Byzantine at any given moment,
2. A small API (`Executor`) for plugging in state machine-based applications like blockchains, and
3. Well-documented, 'obviously correct' source code, designed for easy analysis and extension.

A Rust Programming Language library for Byzantine Fault Tolerant state machine replication, intended for production 
systems. 
  
HotStuff-rs implements a variant of the HotStuff consensus protocol, but with extensions like block-sync and dynamic
validator sets that make this library suited for real-world use cases (and not just research systems). Some desirable
properties that HotStuff-rs has are:

1. **Provable Safety** in the face of up to 1/3rd of voting power being Byzantine at any given moment.
2. **Optimal Performance**: consensus in (amortized) 1 round trip time.
3. **Simplicity**: A small API (App) for plugging in arbitrary stateful applications.
4. **Modularity**: pluggable networking, state persistence, and view-synchronization mechanism.
5. **Dynamic Validator Sets** that can update without any downtime based on state updates: a must for PoS blockchain 
   applications.
6. **Batteries included**: comes with a block-sync protocol and (coming soon) default implementations for networking,
   state, and pacemaker: you write the app, and we handle the replication.

## Terminology
---
 
- **App**: user code which implements the App trait. This can be any business logic that can be expressed
  as a deterministic state machine, i.e., a pure function of kind: `(Blockchain, App State, Validator Set, Block) ->
  (Next Blockchain, Next App State, Next Validator Set)`.
- **Replica**: a public-key-identified process that hosts an implementation of the HotStuff-rs protocol, e.g., this
  library. There are two kinds of replicas: validators, and listeners. 
- **Blockchain**: a growing sequence of **Blocks**, which can be thought of as instructions to update a replica's App
  State and Validator Set.
- **App State**: a key-value store that applications can use to store anything; two replicas with the same Blockchain
  are guaranteed to have the same app state.
- **Validator Set**: the set of replicas who can vote in a consensus decision.
- **Progress protocol**: the protocol replicas use to create new blocks through consensus and grow the blockchain.
- **Sync protocol**: the protocol new or previously offline replicas use to quickly catch up to the head of the
  blockchain.

## The HotStuff Consensus Protocol
---

HotStuff works by building a 'BlockTree': a directed acyclic graph of Blocks. Block is a structure with a `data` field which applications are free to populate with arbitrary byte-arrays. In consensus algorithm literature, we typically talk of consensus algorithms as maintaining state machines that change their internal states in response to commands, hence the choice of terminology.

HotStuff guarantees that committed Blocks are *immutable*. That is, they can never be *un*-committed as long as at least a supermajority of voting power faithfully executes the protocol. This guarantee enables applications to make hard-to-reverse actions with confidence. 

![A graphic depicting a Tree (DAG) of Blocks. Blocks are coloured depending on how many confirmations they have.](/docs/img/concept_consensus_blocktree%20_diagram.png)

A Block becomes *committed* the instant its third confirmation is written into the BlockTree. Confirmation for a Block `A` is another Block `B` such that there is a path between `B` to `A`.

The choice of third confirmation to define commitment--as opposed to first or second--is not arbitrary. HotStuff's safety and liveness properties hinge upon this condition. You can read this [paper](https://github.com/parallelchain-io/hotstuff_rs/blob/master/readme_assets/HotStuff%20paper.pdf) to learn more about it. To summarize:

1. Classic BFT consensus algorithms such as PBFT require only 2 confirmations for commitment, but this comes at the cost of expensive leader-replacement flows.
2. Tendermint requires only 2 confirmations for commitment and has a simple leader-replacement flow, but needs an explicit 'wait-for-N seconds' step to guarantee liveness.

HotStuff is the first consensus algorithm with a simple leader-replacement algorithm that does not have a 'wait-for-N seconds' step and thus can make progress as fast as network latency allows.


## Getting Started
---

If you're trying to learn about HotStuff-rs by reading the source code or Cargo docs, we recommend starting from
the [replica](https://github.com/parallelchain-io/hotstuff_rs/blob/master/src/replica.rs) module. This is the entry point of user code into this library.
