---
tags:
  - testnet 2.0
---

# Smart Contract

## What is Smart Contract?
---

Smart contract is a program that runs on blockchain. In Parallelchain F, smart contract is an executable WASM binary and it resides in the blockchain as an account which also own its address, balance and nonce. External-owned account can make a transaction with smart contract as transaction data, which is the deployment process. A valid smart contract contains an entrypoint that can be executed when an account makes a transaction to its address. 

**Entrypoint**: A starting point of contract execution.


## Smart Contract Programming Model
---

`Rust` is the primitive programing language to create a smart contract. Parallelchain F Smart Contract SDK considers the contract as a `struct` that stores fields as data in world-state, and its `impl` consists of methods representing entrypoints that can be executed.