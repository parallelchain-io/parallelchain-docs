---
tags:
  - pchain-sdk
  - pchain-compile
  - Smart Contract
---

# Building the Contract with 'pchain_compile'

**pchain_compile** is a CLI build tool for smart contract developers to build their source code to WASM binaries for deployment on 
ParallelChain Mainnet. 

The WASM binary generated from a generic cargo build process has the following disadvantages: 
  
  - The build process is not toolchain version agnostic i.e. it does not guarantee uniform WASM binaries with different versions of compiler and 
    rust crate dependencies. This will make it difficult for smart contract developers to verify their source code on the blockchain.
  
  - The build process generates a large file size, which leads to higher gas costs for deployment and contract calls.

**pchain_compile** solves these shortcomings by: 

  - Executing the build process in a docker environment with pre-defined toolchain versioning.
  
  - Optimizing and compressing the size of the WASM binary with the help of `wasm-snip` and `wasm-opt` packages.


## Downloading **pchain_compile**
---

`pchain_compile` supports Linux, macOS and Windows. Depending on the operating system, they can be downloaded from Assets of ParallelChain Lab's Git Hub [release page](https://github.com/parallelchain-io/pchain-compile/releases/tag/v0.4.0).

