---
tags:
  - pchain-sdk
  - Smart Contract
---

# Introduction to Smart Contracts

A **smart contract** presents a method of executing computer programs on a decentralized network with security and fault tolerance. Through smart contracts, users can develop code to support their most crucial business applications, all within a global, decentralized network.

To use smart contracts on ParallelChain Mainnet, developers can write a Contract Binary Interface (CBI) Subprotocol that is implemented in [WebAssembly](https://webassembly.org) (WASM). However, for most developers, it's easier to use the [ParallelChain Mainnet Contract SDK](#parallelchain-mainnet-contract-sdk) (`pchain-sdk`) to write a Contract in **Rust**, and subsequently use a tool named [pchain-compile](getting_started.md#building-the-contract-with-pchain_compile) to compile the Rust source code into WASM bytecode.


## Smart Contract Programming Model

The Smart Contract Programming Model follows Object-Oriented Programming (OOP) principles. In this model, a contract resembles a Rust `struct` that regulates access to persistent storage. Accounts can engage with contracts by submitting transactions containing a call command to invoke the contract's methods. These methods are commonly referred to as just "methods".

## Why Rust and WebAssembly (WASM)?

Writing smart contracts in Rust and executing them as WebAssembly (WASM) bytecode on a blockchain has several benefits:

1. **Efficiency and Security**: [Rust](https://www.rust-lang.org) is a systems programming language that provides a balance between performance and safety. It is designed to prevent common programming errors such as null pointer dereferencing and buffer overflows, which can cause security vulnerabilities. Rust's ownership model and borrow checker also help prevent memory leaks and race conditions. When compiled to WASM, Rust code can run efficiently and securely on a blockchain.

2. **Interoperability**: WASM is a binary format that can be executed in different environments, such as web browsers and servers. This means that smart contracts written in programming languages and compiled to [WASM](https://webassembly.org) can be executed on the blockchain that supports the WASM execution context. This provides more options for developers to choose from and promotes interoperability between different blockchain ecosystems.

2. **Tooling and Ecosystem**: Rust has a rich ecosystem of libraries and tools for building systems and applications. This includes the mentioned `pchain-sdk`, which provides types and macros for writing smart contracts in Rust for the ParallelChain Mainnet blockchain. Rust's [cargo](https://crates.io) package manager also makes it easy to manage dependencies and build projects. By leveraging the Rust ecosystem, developers can build smart contracts faster and with less effort.

3. **Community Support**: Rust has a growing community of developers and contributors who are passionate about the language and its ecosystem. This means that there are resources available for learning Rust and getting help when needed. It also means that the language and ecosystem are constantly evolving and improving.

## ParallelChain Mainnet Contract SDK

The [ParallelChain Mainnet Contract SDK](https://github.com/parallelchain-io/pchain-sdk) (pchain-sdk) is an open-source Rust crate designed to aid in smart contract development. It furnishes Rust structs, functions, types, and macros to facilitate the creation of smart contracts executable in **WebAssembly (WASM)** engines, implementing the **ParallelChain Mainnet Contract Binary Interface (CBI) Subprotocol.**

This documentation caters to both novice and advanced users, aiming to guide them toward successfully creating their first smart contract in ParallelChain.


For sharing smart contract code, addresses, or any assistance with smart contract creation, please visit [GitHub Discussions](https://github.com/parallelchain-io/parallelchain-sdk/discussions).


For issues related to the SDK or smart contracts, please visit [GitHub issues](https://github.com/parallelchain-io/parallelchain-sdk/issues).
