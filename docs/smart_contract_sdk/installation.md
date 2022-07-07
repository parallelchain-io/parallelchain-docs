---
tags:
  - testnet 2.0
  - rust 
---

# Install SDK


## Introduction
---

ParallelChain Mainnet Smart Contract Development Kit (SDK) is an open-source rust crate that lets anyone build smart contracts easily. The SDK consists of methods and macros that allow you to interact with the blockchain from the smart contract. We provide some smart contract templates so that you can launch a new smart contract out of the box. 

We provide a hosted testnet blockchain called ParallelChain Mainnet that is a Turing complete smart contract platform. We will explain how to connect to this testnet in a later section. Anyone can upload their smart contracts to the testnet and share their contract address with others using our "light client".

This documentation is for both novice and advanced users alike. We have extra guides and explanations if you are unclear about some sections of the guide. The next section summarizes the topics covered in the SDK guide.


## Install Rust
---

We will require toolkits. The standard method is by `rustup`, which maintains dependencies and is a version manager for `cargo` and `rustc` (the rust compiler). 

### Linux / macOS

In your terminal, type the following command:
```bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
``` 
Restart your terminal session to get access to `cargo`.  

### Windows 10

1. Download and execute the Windows Package Installer (.msi) file from [here](https://forge.rust-lang.org/infra/other-installation-methods.html#standalone-installers). You can browse through the page to find the appropriate installer for your platform.

2. If requested, manually download and install Visual C++ Build Tools 2019, from [here](https://visualstudio.microsoft.com/visual-cpp-build-tools/). Make sure "Windows 10 SDK" and "English language pack" are selected.

3. Continue running the installer, and proceed with the installation.

## Install Wasm Toolchain

`wasm32-unknown-unknown` is the toolchain required for smart contracts to compile as wasm. Type the command below to add the wasm toolchain in rust.
```bash
rustup target add wasm32-unknown-unknown
```

## Setting up a Development Environment

A good editor is valuable when we are just starting with rust. A recommended Integrated Development Environment (IDE) is [VSCode](https://code.visualstudio.com/).

You can add the [rust](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust) plugin or [rust-analyzer](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer) to type-check all of your code on save. This feature lets you know in the code where an error is located. Furthermore, the error messages displayed in the IDE are the same as when you have executed the command `cargo build`.

Other editors that we recommend are:

1. [IntelliJ IDEA](https://www.jetbrains.com/idea/) and [Rust Plugin for IntelliJ](https://www.jetbrains.com/rust/#:~:text=IntelliJ%20Rust%20brings%20JetBrains-quality%20language%20support%20and%20the,support%2C%20built-in%20test%20runner%2C%20and%20code%20coverage%20tooling.)
2. [Sublime Text](https://www.sublimetext.com/) and [Rust Plugin for Sublime Text](https://github.com/rust-lang/rust-enhanced)
3. Emacs/ Vim

Any other editor that sits comfortably with you is fine.
