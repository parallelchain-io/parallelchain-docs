---
tags:
  - pchain-sdk
  - Smart Contract
  - Rust
---

Before we dive deep into the structure of a contract and the SDK's semantics, we require some background information about the semantics enforced by ParallelChain Mainnet. The full capabilities of ParallelChain Mainnet continue to grow with active developers 
and a growing community. Let's see how smart contracts are called and processed by a node in the form of transactions.

First, the ParallelChain Client (`pchain_client`) will submit a transaction when making a smart contract call to the validating node using a serialization crate [pchain-types](https://github.com/parallelchain-io/pchain-types-rust). The node's RPC service and mempool will process the list of transactions and check for validity and correctness. 

Transactions that fail the validation check may not be included in a block. The transactions are then packed in a block and sent to the execution engine in the node to execute the transactions. 

The executor will call `wasmer` (Web Assembly Engine) which provides an isolated context to perform the execution. This enables the smart contract code to read the current state of the blockchain and interact with it. However, the execution results are temporarily stored and subject to further checks before the block may be committed or rolled back on error. 

`wasmer` also computes the gas fees through a metering module, which is responsible for limiting the execution to the amount of gas paid. Each transaction returns a result in the form of receipts along with logs. The receipts (with or without log), transactions and block itself will contain its hash (or Merkle proof) and will be all included in the same block.


## Smart Contract Development Kit
---

Smart Contract can be created by using [ParallelChain SDK](https://github.com/parallelchain-io/pchain-sdk). Example contracts can be found in [example-smart-contracts](https://github.com/parallelchain-io/example-smart-contracts).

A ParallelChain Smart Contract is a rust crate that imports the SDK. It uses the SDK's features to interact with the blockchain. The folder structure of a typical ParallelChain
Mainnet Smart Contract looks like this:
```bash
my_first_contract
├── src/
│   └── lib.rs # The main source code of your smart contract.  
└── Cargo.toml # You import your packages and the SDK here
```

For more information on Rust's crate system, see [Rust Book Chapter 7: Packages and Crates](https://doc.rust-lang.org/stable/book/ch07-01-packages-and-crates.html?highlight=crate#packages-and-crates)

Please specify the dependency in `Cargo.toml` for using SDK by fetching from [crates.io](https://crates.io/) or repository in Github.

=== "Crates.io"
    ```rust
    [dependencies]
    pchain-sdk = "0.4"
    ```

=== "Github"
    ```rust
    [dependencies]
    pchain-sdk = { git = "https://github.com/parallelchain-io/pchain-sdk" }
    ```