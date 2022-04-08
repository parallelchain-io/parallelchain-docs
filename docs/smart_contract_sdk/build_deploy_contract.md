---
tags:
  - testnet 1.0
  - parallelchain sdk
  - rust
  - sdk
---

# Building and deploying the contract

## Prerequisites

### Node.js

We require the latest LTS (Long Term Support) release of `Node.js` with its package manager `npm`. 

=== "Linux"
    In your terminal, use `nvm` (Node Version Manager) to quickly install a version of `Node.js`
    ```bash 
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
    ```

    Reopen your terminal and check if `nvm` is working:
    ```bash
    nvm
    ```
    <details>
    <summary>Output</summary>
    ```bash
    Node Version Manager (v0.39.1)

    Note: <version> refers to any version-like string nvm understands. This includes:
      - full or partial version numbers, starting with an optional "v" (0.10, v0.1.2, v1)
      - default (built-in) aliases: node, stable, unstable, iojs, system
      - custom aliases you define with `nvm alias foo`
    ```
    </details>

    Type this command to install `v16.14.2` (latest LTS) of `Node.js`
    ```bash
    nvm install v16.14.2
    ```

    You can check if `Node.js` and `npm` are installed by typing 
    ```bash
    node --version && npm --version
    ```
    <details>
    <summary>Output</summary>
    ```bash
    v16.14.2
    8.5.0
    ```
    </details>

=== "macOS"
    Download from this link [https://nodejs.org/dist/v16.14.2/node-v16.14.2.pkg](https://nodejs.org/dist/v16.14.2/node-v16.14.2.pkg) and click to follow the installation instructions.

    You can check if `Node.js` and `npm` are installed by typing the following command in your terminal:
    ```bash
    node --version && npm --version
    ```
    <details>
    <summary>Output</summary>
    ```bash
    v16.14.2
    8.5.0
    ```
    </details>

=== "Windows"
    Download from this link [https://nodejs.org/dist/v16.14.2/node-v16.14.2-x86.msi](https://nodejs.org/dist/v16.14.2/node-v16.14.2-x86.msi) and click to follow the installation instructions.

    You can check if `Node.js` and `npm` are installed by typing the following command in your command prompt:
    ```bash
    node --version && npm --version
    ```
    <details>
    <summary>Output</summary>
    ```bash
    v16.14.2
    8.5.0
    ```
    </details>

### Other requirements
* `Rust` and `wasm32-unknown-unknown` are installed in your system. See ["installation"](./installation.md#linux-and-macos) on how to do this.
* Ensure `pchain` is installed by running. See ["Setup ParallelChain Light Client"](prepare_env.md#setup-parallelchain-light-client).
=== "Linux / macOS"
    ```bash
    ./pchain --version
    ```
=== "Windows"
    ```PowerShell
    pchain.exe --version
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
The generated WASM binary has a large file size, which leads to higher gas costs for deployment and contract calls. You can compress and optimize the WASM binary through our `optimize_tool` in bash script. 

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

## Deploying the contract

See ["`DeployC`: Deploying your Smart Contract"](../cli/tutorial.md#deployc-deploying-your-smart-contract)
