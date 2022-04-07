---
tags:
  - testnet 1.0
  - parallelchain sdk
  - rust
  - sdk
---

# Building and deploying the contract

## Prerequisites
* `Rust` and `wasm32-unknown-unknown` are installed in your system. See ["installation"](./installation.md#linux-and-macos) on how to do this.
* Ensure `pchain` is installed by running. See ["Setup ParallelChain Light Client"](prepare_env.md#setup-parallelchain-light-client).
```shell
pchain --version
```
* Install `wasm-opt` with npm (for code size optimization)
```bash
npm i wasm-opt -g
```
* Install `wasm-snip` with Cargo (for code size optimization)
```bash
cargo install wasm-snip
```

## Building the contract
Go to your smart contract workspace.
```bash
cd <FOLDER_PATH>
```
Build the target. The build process will generate a Web Assembly (WASM) binary at `target/wasm32-unknown-unknown/release/<YOUR_CONTRACT>.wasm` of your workspace.
```bash
cargo build --target wasm32-unknown-unknown --release
```

<details>
  <summary>To view a sample output of a contract that has been built:</summary>
    1. Go to your_contract, in this case we will call it "my_great_contract"
    ```bash
    cd my_great_contract/
    ```

    2. Go to the output folder of "my_great_contract"
    ```bash
    cd target/wasm32-unknown-unknown/release
    ```

    3. Your built contract can be found:
    ```bash
    $ ls
    my_great_contract.wasm
    ```
</details>

## **Optimizing WASM binary for code size (optional)**

The generated WASM binary has a large file size, which leads to higher gas costs for deployment and contract calls. You can compress and optimize the WASM binary through our `optimize_tool` in bash script in Linux or terminal commands in Windows.

### Linux and macOS

We recommend that you use this optimization tool, otherwise you will not have enough tokens to run a contract. The faucet gives enough tokens to run contracts that have been built with the optimizer tool.

You can download the `optimize_tool` using terminal
```bash
wget https://raw.githubusercontent.com/parallelchain-io/example-smart-contracts/main/optimize.sh
```

Set up permissions on the script
```bash
chmod u+x optimize.sh
```

Run the script by providing your compiled contract file path.
```bash
./optimize.sh -f <file path>
```
The output file `optimized-<ORIGINAL_FILENAME>.wasm` is generated under the same location of your script.

### Windows 10 

We recommend that you use this optimization commands, otherwise you will not have enough tokens to run a contract. The faucet gives enough tokens to run contracts that have been built with the optimized commands.

In your terminal, type the following commands:
```bash
cd <file path>
``` 
```bash
wasm-opt -Oz  <ORIGINAL_FILENAME>.wasm  --output temp-<ORIGINAL_FILENAME>.wasm
``` 
```bash
wasm-snip temp-<ORIGINAL_FILENAME>.wasm --output temp2-<ORIGINAL_FILENAME>.wasm --snip-rust-fmt-code --snip-rust-panicking-code
``` 
```bash
wasm-opt -Oz  temp2-<ORIGINAL_FILENAME>.wasm  --output  optimized-<ORIGINAL_FILENAME>.wasm
``` 
The output file `optimized-<ORIGINAL_FILENAME>.wasm` is generated under the same location of your terminal.


## Deploying the contract

See ["`DeployC`: Deploying your Smart Contract"](../cli/tutorial.md#deployc-deploying-your-smart-contract)
