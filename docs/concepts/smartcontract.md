---
tags:
  - testnet 3
---

# Smart Contract

## What is Smart Contract?
---

Contracts are the run-time programmability mechanism of ParallelChain F networks. They allow users (Account owners) to implement arbitrary logic in a global, decentralized, and Byzantine Fault Tolerant replicated state machine to support their most business-critical applications.

Theoretically, any WebAssembly (WASM) module that implements the Contract ABI Subprotocol can be deployed onto a ParallelChain F blockchain. Practically, however, all developers (except perhaps those who like to experiment, or that would like to stretch the limits of the system) will want to use the types and macros in this pchain-sdk to write a Contract in Rust, and the commands in pchain-compile to compile the Rust source code into WASM bytecode that can be included in a Deploy Transaction.

## Smart Contract Programming Model
---

The Contract Programming Model is inspired by Object-Oriented Programming (OOP). In the Model, a Contract can be thought of as a `Rust` `struct` that controls access to persistent Storage. Accounts interact with Contracts by making EtoC Transactions calling its public methods, or just Methods for short. The following two sections elaborate on the two essential concepts of programming with the Contract Programming Model: Contract Methods, and Contract Storage.

See [Develop Contract](../smart_contract_sdk/develop_contract.md)