---
tags:
  - pchain-client
  - account
  - mainnet
  - testnet 4
  - faucet
---

The guide will walk you through the steps to generate and manage your Mainnet / Testnet account by using `pchain_client`. Alternatively, you can generate account by using [ParallelChain Explorer](https://explorer.parallelchain.io). *Please refer to the [Explorer Account Tutorial](https://parallelchain.io/company/newsroom/explorer-account-tutorial)*.

## Generating Keypair
---

To create an account, type the following command below:
=== "Linux / macOS"
    ```bash
    ./pchain_client keys create --name <NAME>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe keys create --name <NAME>
    ```

You will be asked to input your password to save the new keypair.

This command generates a set of ed25519_dalek-compatible keys. A keypair file in json format is created in the folder specified by the environment variable `PCHAIN_CLI_HOME`. These are the keys required for making transactions in the network. Its public key is the public address of your account. You can view it as follows:

=== "Linux / macOS"
    ```bash
    ./pchain_client keys list
    ```
=== "Windows"
    ```PowerShell
    ./pchain_client.exe keys list
    ```

<details><summary>Terminal Output</summary>
```bash
Keypair Name (First 50 char)                        Public key 
-------------------------                           ------------------------- 
doc                                                 mF0a_r31a_cNNnKbngoUGUuy71V_l872yGxy7iwIBwA
```
</details>

You will need the keypair and public key to submit transactions.

You can check the account balance using the account address (public key).
=== "Linux / macOS"
    ```bash
    ./pchain_client query balance --address <PUBLIC_KEY>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe query balance --address <PUBLIC_KEY>
    ```
<details><summary>Terminal Output</summary>
```bash
0
```
</details>

The balance of a new account is 0. Therefore, you need to request tokens from the faucet.

## Exporting Keypair

Right now, the key pair is only in your machine. It will go away if your machine somehow breaks. It may also be lost if you forget your password. 

To secure your keypair and money, you must export it and save it somewhere secure, or at least make a copy of it.

=== "Linux / macOS"
    ```bash
    ./pchain_client keys export --name <NAME>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe keys export --name <NAME>
    ```

You will be asked to input your password to export the keypair. The keypair will be saved in the current directory, in JSON format, with the same name as the keypair itself.

## Importing Keypair

To add your keypair, type the following command below:
=== "Linux / macOS"
    ```bash
    ./pchain_client keys import --public <PUBLIC_KEY> --private <PRIVATE_KEY> --name <NAME>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe keys import --public <PUBLIC_KEY> --private <PRIVATE_KEY> --name <NAME>
    ```

You will be asked to input your password to import the new keypair.

## Request for tokens from Faucet (Testnet)
---

The Faucet Service of our Testnet issues free testing tokens to users to test the blockchain network on Testnet. 

Right now, you have your keypair (account) to associate with the blockchain. But the account should have an empty balance. You can now request tokens from [Faucet Service](https://faucet.parallelchain.io).