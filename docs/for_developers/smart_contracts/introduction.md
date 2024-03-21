---
tags:
  - pchain-sdk
  - Smart Contract
---

# Introduction to Smart Contracts

A **smart contract** is a way to run computer programs on a decentralised network that is secure and fault-tolerant. With smart contracts, users can write code to support their most critical business applications, all on a global, decentralised network.

To use smart contracts on ParallelChain Mainnet, developers can write a Contract Binary Interface (CBI) Subprotocol that is implemented using a WebAssembly (WASM) module. However, for most developers, it's easier to use the `pchain-sdk` types and macros to write a Contract in **Rust**, and then use the `pchain-compile` commands to compile the Rust source code into WASM bytecode.


## Smart Contract Programming Model

The Smart Contract Programming Model is based on Object-Oriented Programming (OOP). In this model, a contract is like a `Rust` `struct` that controls access to persistent storage. Accounts can interact with contracts by submitting transactions that include a call command to invoke methods of the contract. These methods are sometimes called just "methods" for short.

## Why Rust and WebAssembly (WASM)?

Writing smart contracts in Rust and executing them as WebAssembly (WASM) bytecode on a blockchain has several benefits:

1. **Efficiency and Security**: Rust is a systems programming language that provides a balance between performance and safety. It is designed to prevent common programming errors such as null pointer dereferencing and buffer overflows, which can cause security vulnerabilities. Rust's ownership model and borrow checker also help prevent memory leaks and race conditions. When compiled to WASM, Rust code can run efficiently and securely on a blockchain.

2. **Interoperability**: WASM is a binary format that can be executed in different environments, such as web browsers and servers. This means that smart contracts written in programming languages and compiled to WASM can be executed on the blockchain that supports the WASM execution context. This provides more options for developers to choose from and promotes interoperability between different blockchain ecosystems.

2. **Tooling and Ecosystem**: Rust has a rich ecosystem of libraries and tools for building systems and applications. This includes the mentioned `pchain-sdk`, which provides types and macros for writing smart contracts in Rust for the ParallelChain Mainnet blockchain. Rust's cargo package manager also makes it easy to manage dependencies and build projects. By leveraging the Rust ecosystem, developers can build smart contracts faster and with less effort.

3. **Community Support**: Rust has a growing community of developers and contributors who are passionate about the language and its ecosystem. This means that there are resources available for learning Rust and getting help when needed. It also means that the language and ecosystem are constantly evolving and improving.

Overall, writing smart contracts in Rust and executing them as WASM bytecode on a blockchain provides a secure, efficient, interoperability, and well-supported programming environment for developers.

## ParallelChain Mainnet Contract SDK

The **ParallelChain Mainnet Contract SDK (pchain-sdk)** is an open-source Rust crate that helps to build smart contracts. It provides Rust structs, functions, types, and macros that aid with the development of smart contracts executable in **WebAssembly (WASM)** engines, implementing the **ParallelChain Mainnet Contract Binary Interface (CBI) Subprotocol.**


Contracts are the run-time programmability mechanism of ParallelChain Mainnet networks. They allow users (Account owners) to implement arbitrary logic in a global, decentralised, and Byzantine Fault Tolerant replicated state machine to support their most business-critical applications.


Theoretically, any WebAssembly (WASM) module that implements the CBI Subprotocol can be deployed onto a ParallelChain Mainnet blockchain. Practically, however, all developers (except perhaps those who like to experiment, or that would like to stretch the limits of the system) will want to use the types and macros provided in this pchain-sdk to write a Contract and the commands in pchain-compile to compile the Rust source code into WASM bytecode that can be included in a Deploy Transaction.


This documentation is for both novice and advanced users alike. We hope to guide you to the success of your first smart contract creation in ParallelChain.

For sharing the smart contract code, address, or any help with creating smart contracts, please go to [GitHub Discussions](https://github.com/parallelchain-io/parallelchain-sdk/discussions).

If you have any issues related to the SDK or smart contract, please go to [GitHub issues](https://github.com/parallelchain-io/parallelchain-sdk/issues).
