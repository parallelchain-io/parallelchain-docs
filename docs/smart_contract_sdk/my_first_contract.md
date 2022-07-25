---
tags:
  - testnet 2.0
  - parallelchain sdk
---

# My First Contract

`Learning outcome`: _Download, build and deploy `my_first_contract` onto ParallelChain `testnet2`_

## Prerequisites
* Rust is installed in your system. See ["installation"](installation.md#linux-and-macos) on how to do this.
* Ensure `pchain` is installed by running 
=== "Linux / macOS"
    ```bash
    ./pchain --version
    ```
=== "Windows"
    ```PowerShell
    pchain.exe --version
    ```

## Building the contract
Download the smart contract examples respository at [https://github.com/parallelchain-io/example-smart-contracts.git](https://github.com/parallelchain-io/example-smart-contracts.git). 

After this folder is downloaded, enter into `my_first_contract` directory.

Build `my_first_contract` contract using `pchain_compile`. Please refer to ["Building and Deploying the Contract"](build_contract.md) session for detailed steps. The build process will generate an optimized Web Assembly (WASM) binary on your current path if no destination path is provided to the CLI 
by the name of <PACKAGE_NAME>.wasm where `PACKAGE_NAME` refers to the package name specified on your Cargo manifest. The user is free to use any other toolchain like Cargo as well to build the smart contract 
but for verification of the contract deployment through the ["Contract Publishing Service"](../getting_started/contract_publish.md), the use of `pchain_compile` is highly recommended.

<details>
  <summary>Click to see the output file for my_first_contract:</summary>
    1. Go to "my_first_contract"
    ```bash
    cd my_first_contract/
    ```

    1. If the destination path for `pchain_compile` is set to the output path within "my_first_contract" directory, you can navigate there as follows
    ```bash
    cd target/wasm32-unknown-unknown/release
    ```

    1. Your built contract can be found:
    ```bash
    $ ls
    my_first_contract.wasm
    ```
</details>


See ["Building and Deploying the Contract"](build_contract.md)
on how to achieve this. However, please note that the `optimize_tool` may lead to breaking changes in more complex code. Future updates in `testnet2` will focus on handling these issues.

## Deploying the contract

You will require a ParallelChain testnet account to deploy a contract. To create one, see ["Create a new Externally Owned Account (EOA)"](../cli/tutorial.md#create-a-new-externally-owned-account-eoa). Make sure you keep the keys in a safe place. The `Public Key` is your account address.

Please make sure your account has enough balance to pay for transactions. You can go to [ParallelChain Testnet Faucet](https://testnet.parallelchain.io/explorer/faucet) to submit token requests and get some tokens for your test. You can use `pchain` to check your account balance:
=== "Linux / macOS"
    ```bash
    ./pchain query account balance --address <YOUR_ACCOUNT_ADDRESS>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query account balance --address <YOUR_ACCOUNT_ADDRESS>
    ```

Check your account nonce. It is important to keep track of this number. You will need this number for deploying a contract in the next step:
=== "Linux / macOS"
    ```bash
    ./pchain query account nonce --address <YOUR_ACCOUNT_ADDRESS>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query account nonce --address <YOUR_ACCOUNT_ADDRESS>
    ```

```bash
Your value "214"
Your value(decoded) [219, 94]
```

Deploy the contract with your account nonce. You should get the contract address and transaction hash if the command has succeeded. See ["Building and Deploying the Contract"](build_deploy_contract.md) for detailed steps.
=== "Linux / macOS"
    ```bash
    ./pchain submit tx \
    --from-address  <YOUR_ACCOUT_ADDRESS> \
    --to-address null \
    --value 0 \
    --tip 0 \
    --gas-limit 5000000 \
    --gas-price 1 \
    --data <PATH_TO_WASM_BINARIES> \
    --nonce <ACCOUNT_NONCE> \
    --path_to_keypair_json <PATH_TO_KEYPAIR>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe submit tx \
    --from-address  <YOUR_ACCOUT_ADDRESS> \
    --to-address null \
    --value 0 \
    --tip 0 \
    --gas-limit 5000000 \
    --gas-price 1 \
    --data <PATH_TO_WASM_BINARIES> \
    --nonce <ACCOUNT_NONCE> \
    --path_to_keypair_json <PATH_TO_KEYPAIR>
    ```
Get the status of the contract by querying the smart contract code. A smart contract is correctly deployed if you get a stream of bytes in the terminal.
=== "Linux / macOS"
    ```bash
    ./pchain query account contract-code --address <CONTRACT_ADDRESS>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query account contract-code --address <CONTRACT_ADDRESS>
    ```

Congratulations, you have successfully downloaded and deployed your first contract on `testnet2`. The next section will describe in detail the structure of a ParallelChain Mainnet Smart Contract.
