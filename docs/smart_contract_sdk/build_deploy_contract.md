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
* Ensure `pchain` is installed by running. See ["Setup ParallelChain Light Client"](/smart_contract_sdk/prepare_env/#setup-parallelchain-light-client).
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
Build the target. The build process will generate a Web Assembly (WASM) binary at target/wasm32-unknown-unknown of your workspace.
```bash
cargo build --target wasm32-unknown-unknown --release
```
## Optimizing WASM binary for code size (optional)
The generated WASM binary has a large file size, which leads to higher gas costs for deployment and contract calls. You can compress and optimize 
the WASM binary through our `optimize_tool` in bash script.

You can download the `optimize_tool` using terminal
```bash
wget https://github.com/parallelchain-io/parallelchain-sdk/optimize.sh
```

Alternatively, you can download the script [here](https://github.com/parallelchain-io/parallelchain-sdk/optimize.sh).

Set up permissions on the script
```bash
chmod u+x optimize.sh
```

Run the script by providing your compiled contract file path.
```bash
./optimize.sh -f <file path>
```
The output file `optimized-<ORIGINAL_FILENAME>.wasm` is generated under the same location of your script.

## Deploying the contract

See ["`DeployC`: Deploying your Smart Contract"](/cli/real_world_walkthrough/#deployc-deploying-your-smart-contract)
