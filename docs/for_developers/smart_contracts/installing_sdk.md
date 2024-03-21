---
tags:
  - pchain-sdk
  - Smart Contract
  - Rust 
---

## Installing Rust

To install the required toolkits, the standard method is by `rustup`, which maintains dependencies and is a version manager for `cargo` and `rustc` (the rust compiler). Detailed installation instructions can be found on the [official website](https://www.rust-lang.org/tools/install)

## Setting up a Development Environment

A recommended Integrated Development Environment (IDE) is [VSCode](https://code.visualstudio.com/), which allows the addition of the plugin [rust-analyzer](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer) to type-check all of your code on save. This feature allows you to locate errors in the code easily, and the error messages displayed in the IDE are the same as when executing the `cargo build` command. However, any other editor that is comfortable for you can be used. However, any other editor that is comfortable for you can be used.

## Installing Wasm Toolchain (Optional)

ParallelChain Mainnet also provides a smart contract compilation tool called `pchain-compile` to compile your Rust smart contract into WASM bytecode. While not necessary, the Installation of Wasm Toolchain can help check if your code is compilable before using the time-consuming process in the use of `pchain-compile`.

The `wasm32-unknown-unknown` toolchain is required for smart contracts to compile as WASM. To add the wasm toolchain in Rust, type the command below:
```bash
rustup target add wasm32-unknown-unknown
```

To build your smart contract as WASM bytecode, use the command below:
```bash
cargo build --target wasm32-unknown-unknown
```