---
tags:
  - Tools
  - pchain-sdk
  - Smart Contract
---

# SDK

The **ParallelChain Mainnet Contract SDK (pchain-sdk)** is an open-source Rust crate that helps to build smart contracts. It provides Rust structs, functions, types, and macros that aid with the development of smart contracts executable in **WebAssembly (WASM)** engines, implementing the **ParallelChain Mainnet Contract Binary Interface (CBI) Subprotocol.**


Contracts are the run-time programmability mechanism of ParallelChain Mainnet networks. They allow users (Account owners) to implement arbitrary logic in a global, decentralized, and Byzantine Fault Tolerant replicated state machine to support their most business-critical applications.


Theoretically, any WebAssembly (WASM) module that implements the CBI Subprotocol can be deployed onto a ParallelChain Mainnet blockchain. Practically, however, all developers (except perhaps those who like to experiment, or that would like to stretch the limits of the system) will want to use the types and macros provided in this pchain-sdk to write a Contract and the commands in pchain-compile to compile the Rust source code into WASM bytecode that can be included in a Deploy Transaction.


This documentation is for both novice and advanced users alike. We hope to guide you to the success of your first smart contract creation in ParallelChain.

For sharing the smart contract code, address, or any help with creating smart contracts, please go to [GitHub Discussions](https://github.com/parallelchain-io/parallelchain-sdk/discussions).

If you have any issues related to the SDK or smart contract, please go to [GitHub issues](https://github.com/parallelchain-io/parallelchain-sdk/issues).
