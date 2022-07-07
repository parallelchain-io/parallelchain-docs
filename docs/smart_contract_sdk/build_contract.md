---
tags:
  - testnet 2.0
  - parallelchain sdk
  - smart contract
---

# Build Contract

## Prerequisites
---

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
* `Rust` and `wasm32-unknown-unknown` are installed in your system. See ["installation"](../smart_contract_sdk/installation.md/#install-rust) on how to do this.
* Ensure `pchain` is installed by running. See ["Setup ParallelChain Light Client"](../getting_started/installation.md).
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

## Building the contract with 'pchain_compile'
---

**pchain_compile** is a CLI build tool for smart contract developers to build their source code to WASM binaries for deployment on 
ParallelChain F. 

The WASM binary generated from a generic cargo build process has the following disadvantages: 
  
  - The build process is not toolchain version agnostic i.e. it does not guarantee uniform WASM binaries with different versions of compiler and 
    rust crate dependencies. This will make it difficult for smart contract developers to verify their source code on the blockchain.
  
  - The build process generates large file size, which leads to higher gas costs for deployment and contract calls.

**pchain_compile** solves these shortcomings by: 

  - Executing the build process in a docker environment with pre-defined toolchain versioning.
  
  - Optimizing and compressing the size of the WASM binary with the help of `wasm-snip` and `wasm-opt` packages.

The pre-requisites needed to run `pchain_compile`, the description of the its build toolchain, the CLI arguments and some usage examples are provided in the following sections. 

### Pre-Requisites

`pchain_compile` builds the source code in a docker environment. To know more about Docker and to install it you may refer to the instructions 
  provided [here](https://docs.docker.com/get-docker/).

### Toolchain
The components described in the table below constitute the build toolchain for smart contract deployment on ParallelChain F. The toolchain is a docker image hosted on a public DockerHub repository of ParallelChain F see [here](https://hub.docker.com/r/parallelchainlab/pchain_compile),which is pulled in when **pchain_compile** is executed and removed at the end of the lifetime of the program. 
     
|Toolchain Component | Version | Utility
|:---                |:---  | :--- |
rustc                | 1.59.0 (9d1b2106e 2022-02-23) | Compiler for Rust. |
wasm-snip   | 0.4.0 | WASM utility which removes functions within the wasm file that are neither transitively called by helper methords or are never be called at runtime. |   
wasm-opt     | 109  | WASM utility to load WebAssembly in text format and run Binaryen IR passes to optimize its size. For more information on Binaryen IR see [here](http://webassembly.github.io/binaryen/). |


### Description 

*  Arguments

   1. build

      This command can be accessed by appending "help" or "--help" to `pchain_compile`.

*  Explanation about arguments for build subcommand.  

   1. **source**
      -  The path to the Cargo.toml package of the smart contract code directory. 
      -  This is a *mandatory* field.
      -  `pchain_compile` supports both absolute and relative paths to your smart contract code directory. 
      -  If "." is passed to the source field the path will be set to the current working. directory. 
   2. **destination**
      -  The path where the resulting WASM smart contract with the name <package_name>.wasm will be stored. package_name 
         is the name of the package denoted in the manifest file of the source code directory. 
      -  This is an *optional* field. `pchain_compile` saves the generated wasm file to the **source** path if this flag is not supplied.
      -  If **destination** path is relative, `pchain_compile` interprets the corresponding absolute path and displays it with the WASM Binary filename after the build process is complete. 
      -  If "." is passed to the destination field the **destination** path will be set to the current working directory. 


### Steps 
To run `pchain_compile` after installing prerequisites in the following section.

   1. In Linux, give permission to run the executable. 
      ```
      chmod +x pchain_compile
      ```
      For Windows there is no need for this step.
      
   2. The format of the command on Linux is shown as follows.
      `./pchain_compile --source <MANIFEST_PATH> --destination  <DESTINATION_PATH>`

### Example 
The following is a real life example of how `pchain_compile` can be used where some source code is kept with a manifest file on a path
`<MANIFEST_PATH>` in two different OS platforms.

=== "Linux / macOS"

```
./pchain_compile  build \
--source <MANIFEST_PATH> 

Output: Finished compiling. ParallelChain F smart contract (<MANIFEST_NAME>.wasm) is saved at (<MANIFEST_PATH>).
```
     
```
./pchain_compile  build \
--source <MANIFEST_PATH> \
--destination <DESTINATION_PATH>

Output: Finished compiling. ParallelChain F smart contract (<MANIFEST_NAME>.wasm) is saved at (<DESTINATION_PATH>).
```

=== "Windows"

```
pchain_compile.exe  build \
--source <MANIFEST_PATH> 

Output: Finished compiling. ParallelChain F smart contract (<MANIFEST_NAME>.wasm) is saved at (<MANIFEST_PATH>).
```

```
pchain_compile.exe  build \
--source <PATH_TO_YOUR_CONTRACT> \
--destination <DESTINATION_PATH>

Output: Finished compiling. ParallelChain F smart contract (<MANIFEST_NAME>.wasm) is saved at (<DESTINATION_PATH>).
```


## Deploy Contract
---

Contract deployment requires using `pchain` program. Please see ["Deploy Contract (DeployC)"](../../getting_started/deploy_contract)
