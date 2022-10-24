---
tags:
  - testnet 2.0
  - parallelchain sdk
  - smart contract
---

# Building the contract with 'pchain_compile'
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

## Pre-Requisites

`pchain_compile` builds the source code in a docker environment. To know more about Docker and to install it you may refer to the instructions 
  provided [here](https://docs.docker.com/get-docker/).

## Toolchain
The components described in the table below constitute the build toolchain for smart contract deployment on ParallelChain F. The toolchain is a docker image hosted on a public DockerHub repository of ParallelChain F see [here](https://hub.docker.com/r/parallelchainlab/pchain_compile),which is pulled in when **pchain_compile** is executed and removed at the end of the lifetime of the program. 
     
|Toolchain Component | Version | Utility
|:---                |:---  | :--- |
rustc                | 1.63.0 (fd9c4297c 2022-07-01) | Compiler for Rust. |
wasm-snip   | 0.4.0 | WASM utility which removes functions within the wasm file that are neither transitively called by helper methords or are never be called at runtime. |   
wasm-opt     | 109  | WASM utility to load WebAssembly in text format and run Binaryen IR passes to optimize its size. For more information on Binaryen IR see [here](http://webassembly.github.io/binaryen/). |


## Description 

*  Arguments

   1. build

      This command can be accessed by appending "help" or "--help" to `pchain_compile`.

*  Explanation about arguments for build subcommand.  

   1. **source**
      -  The path to the Cargo.toml package of the smart contract code directory. 
      -  This is a *mandatory* field.
      -  `pchain_compile` supports both absolute and relative paths to your smart contract code directory. 
      -  If "." is passed to the source field the path will be set to the current working directory. 
   2. **destination**
      -  The path where the resulting WASM smart contract with the name <package_name>.wasm will be stored. `package_name` 
         is the name of the package denoted in the manifest file of the source code directory. 
      -  This is an *optional* field. `pchain_compile` saves the generated wasm file to the **source** path if this flag is not supplied.
      -  If **destination** path is relative, `pchain_compile` interprets the corresponding absolute path and displays it with the WASM Binary filename after the build process is complete. 
      -  If "." is passed to the destination field the **destination** path will be set to the current working directory. 


## Steps 
To run `pchain_compile` after installing prerequisites in the following section.

   1. In Linux, give permission to run the executable. 
      ```
      chmod +x pchain_compile
      ```
      For Windows there is no need for this step.
      
   2. The format of the command on Linux is shown as follows.
      `./pchain_compile build --source <PATH_TO_YOUR_CONTRACT> --destination  <DESTINATION_PATH>`

## Example 
The following is a real life example of how `pchain_compile` can be used where some source code is kept with a manifest file on a path
`<PATH_TO_YOUR_CONTRACT>` in two different OS platforms.

=== "Linux / macOS"

```
./pchain_compile  build \
--source <PATH_TO_YOUR_CONTRACT> 

Output: Finished compiling. ParallelChain F smart contract (<MANIFEST_NAME>.wasm) is saved at (<PATH_TO_YOUR_CONTRACT>).
```
     
```
./pchain_compile  build \
--source <PATH_TO_YOUR_CONTRACT> \
--destination <DESTINATION_PATH>

Output: Finished compiling. ParallelChain F smart contract (<MANIFEST_NAME>.wasm) is saved at (<DESTINATION_PATH>).
```

=== "Windows"

```
pchain_compile.exe  build \
--source <PATH_TO_YOUR_CONTRACT> 

Output: Finished compiling. ParallelChain F smart contract (<MANIFEST_NAME>.wasm) is saved at (<PATH_TO_YOUR_CONTRACT>).
```

```
pchain_compile.exe  build \
--source <PATH_TO_YOUR_CONTRACT> \
--destination <DESTINATION_PATH>

Output: Finished compiling. ParallelChain F smart contract (<MANIFEST_NAME>.wasm) is saved at (<DESTINATION_PATH>).
```

## Download **pchain_compile**

'pchain_compile' supports Linux, macOS and Windows. Depending on the operating system, they can be downloaded from the 
following links below.

=== "Linux / macOS"

For Linux / macOS users can download `pchain_compile` from [here](https://cms.parallelchain.io/pchain_compile_linux_v1.1.tar.xz)

=== "Windows"

For Windows users can download `pchain_compile` from [here](https://cms.parallelchain.io/pchain_compile_win_v1.1.zip)


## Deploy Contract
---

Contract deployment requires using `pchain` program. Please see ["Deploy Contract (DeployC)"](../../getting_started/deploy_contract)
