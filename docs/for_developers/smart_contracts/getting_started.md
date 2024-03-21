---
tags:
  - pchain-sdk
  - Smart Contract
---

# Getting Started

Before we dive deep into the structure of a contract and the SDK's semantics, we require some background information about the semantics enforced by ParallelChain Mainnet. The full capabilities of ParallelChain Mainnet continue to grow with active developers 
and a growing community. Let's see how smart contracts are called and processed by a node in the form of transactions.

First, the ParallelChain Client (`pchain_client`) will submit a transaction when making a smart contract call to the validating node using a serialization crate [pchain-types](https://github.com/parallelchain-io/pchain-types-rust). The node's RPC service and mempool will process the list of transactions and check for validity and correctness. 

Transactions that fail the validation check may not be included in a block. The transactions are then packed in a block and sent to the execution engine in the node to execute the transactions. 

The executor will call `wasmer` (Web Assembly Engine) which provides an isolated context to perform the execution. This enables the smart contract code to read the current state of the blockchain and interact with it. However, the execution results are temporarily stored and subject to further checks before the block may be committed or rolled back on error. 

`wasmer` also computes the gas fees through a metering module, which is responsible for limiting the execution to the amount of gas paid. Each transaction returns a result in the form of receipts along with logs. The receipts (with or without log), transactions and block itself will contain its hash (or Merkle proof) and will be all included in the same block.


## Smart Contract Development Kit

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

## Building the Contract with pchain_compile

**pchain_compile** is a CLI build tool for smart contract developers to build their source code to WASM binaries for deployment on 
ParallelChain Mainnet. 

The WASM binary generated from a generic cargo build process has the following disadvantages: 
  
  - The build process is not toolchain version agnostic i.e. it does not guarantee uniform WASM binaries with different versions of compiler and 
    rust crate dependencies. This will make it difficult for smart contract developers to verify their source code on the blockchain.
  
  - The build process generates a large file size, which leads to higher gas costs for deployment and contract calls.

**pchain_compile** solves these shortcomings by: 

  - Executing the build process in a docker environment with pre-defined toolchain versioning.
  
  - Optimizing and compressing the size of the WASM binary with the help of `wasm-snip` and `wasm-opt` packages.


### Downloading **pchain_compile**

`pchain_compile` supports Linux, macOS and Windows. Depending on the operating system, they can be downloaded from Assets of ParallelChain Lab's Git Hub [release page](https://github.com/parallelchain-io/pchain-compile/releases).

The binary can also be installed and built by executing the following commands:
```
cargo install pchain_compile
```

!!! Pre-Requisites
    `pchain_compile` builds the source code in a docker environment. To know more about Docker and install it, refer to the [official instructions](https://docs.docker.com/engine/install/).



### Building WASM Binary

In order to build a `WASM` binary of the smart contract, run the following command:

=== "Linux / macOS"
    ```bash
    ./pchain_compile build --source <PATH_TO_SMART_CONTRACT_CODE>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_compile.exe build --source <PATH_TO_SMART_CONTRACT_CODE>
    ```

If you installed `pchain_compile` with `cargo install`, you can simply run:

```
pchain_compile build --source <PATH_TO_SMART_CONTRACT_CODE>
```

A `.wasm` file will be generated in the current directory. It will be needed for the [next section](./deploy_contract.md).

## Deploying a Smart Contract

You can deploy the contract using the pchain_client command line tool. You should make sure to obtain your latest account nonce before submitting the transaction.

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create
    --nonce <NONCE> \
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    deploy \
    --contract-code <WASM_BINARY_PATH> \
    --cbi-version <CBI_VERSION> 
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe transaction create `
    --nonce <NONCE> `
    --gas-limit <GAS_LIMIT> `
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> `
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> `
    deploy `
    --contract-code <WASM_BINARY_PATH> `
    --cbi-version <CBI_VERSION> 
    ```

You can follow the instruction in [Create Transaction](../developer_guides/pchain_cli/transaction.md) about submiting a transaction through `pchain-client`.

## Checking Contract in State

To verify that the smart contract is deployed correctly, you can run the command `query` with the flag `contract`. It queries the state of the blockchain and saves the wasm code as `code.wasm` in the current directory.
If you want to store the contract file in your preferred location, you need to provide the flag `destination` to specify the path with your preferred file extension:

=== "Linux / macOS"
    ```bash
    ./pchain_client query contract --address <CONTRACT_ADDRESS>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe query contract --address <CONTRACT_ADDRESS>
    ```

## Calling Contract

Suppose you have already deployed a contract that contains a `call` method as follows:

```rust
    #[call]
    fn hello_from(name: String) -> u32 {
        pchain_sdk::log(
            "topic: Hello From".as_bytes(), 
            format!("Hello, Contract. From: {}", name).as_bytes()
        );
        name.len() as u32
    }
```

To invoke this contract in the blockchain by using `pchain_client`, you can prepare a JSON file that contains a list of arguments and matches with the `call` method. For example, this `call` method takes a string argument. Then, the content of the JSON file should be as follows:

```json
{
    "arguments": [
        {"argument_type":"String", "argument_value": "\"Alice\""}
    ]
}
```

This JSON file will be used in the subcommand `call` as mentioned in the subsequent sections on this page.

### Submitting Transaction with Call Command

To call a smart contract, submit a transaction with your account nonce and contract address using the `pchain_client`.

Here is the command to call a contract:

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create \
    --nonce <NONCE> \
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    call \
    --target <CONTRACT_ADDRESS> \
    --method <contract_METHOD> \
    --arguments <CALL_ARGUMENT_FILE_PATH_WITH_FILE_NAME>
    --amount <AMOUNT_TO_CONTRACT> \
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe transaction create `
    --nonce <NONCE> `
    --gas-limit <GAS_LIMIT> `
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> `
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> `
    call `
    --target <CONTRACT_ADDRESS> `
    --method <contract_METHOD> `
    --arguments <CALL_ARGUMENT_FILE_PATH_WITH_FILE_NAME> `
    --amount <AMOUNT_TO_CONTRACT>
    ```

The gas limit required for the transaction depends on the complexity of the smart contract. For safety reasons, you can always set a higher gas limit. You can also test contract calls on testnet to reassure.

You can follow the instruction in [Create Transaction](../developer_guides/pchain_cli/transaction.md) about submiting a transaction through `pchain-client`.

To query the resulting receipt of the transaction, 

=== "Linux / macOS"
    ```bash
    ./pchain_client query tx --hash <TRANSACTION_HASH> 
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe query tx --hash <TRANSACTION_HASH>
    ```

The commands stored in `transaction` and command receipts in `receipt` are following the same order. That means you can always find the corresponding transaction from a command receipt.

### Parsing Call Result

To parse the response from the contract method, represented in the field named `return value` , which is in `CallResult` format, you can use the `parse call-result` command in ParallelChain Client.

For example, if the contract method returns a u32 integer, the `return value` is "BAAAAAUAAAA" you can parse the `CallResult` data structure using the `--data-type u32` flag:

=== "Linux / macOS"
    ```bash
    pchain_client parse call-result --value BAAAAAUAAAA --data-type u32
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe parse call-result --value BAAAAAUAAAA --data-type u32
    ```

The output will be the parsed value of the `CallResult`, which in this case is `4`. For more details, you can use the `help` command to see the usage of the tool or take a look at the example `argument,json`.