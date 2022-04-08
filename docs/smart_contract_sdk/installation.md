---
tags:
  - testnet 1.0
  - rust 
---

# Getting Started

`Learning outcome`: _Rust installed on your system with `wasm32-unknown-unknown` toolchain._

This section will guide you in installing rust for your machine. 

## Install Rust

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
