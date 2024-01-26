---
tags:
  - Tools
  - CLI
  - pchain-cli-rust
---

# ParallelChain Client CLI (pchain-client)

`pchain_client` is an easy-to-use, fully-featured CLI for interacting with ParallelChain. 
For a detailed description of all available commands, execute `pchain_client --help`. 

## Usage 
---

```terminal
ParallelChain Client CLI 0.4.3
<ParallelChain Lab>
ParallelChain client (`pchain_client`) is a command-line tool for you to connect and interact with
the ParallelChain Mainnet/Testnet.

USAGE:
    pchain_client <SUBCOMMAND>

OPTIONS:
    -h, --help       Print help information
    -V, --version    Print version information

SUBCOMMANDS:
    transaction    Construct and submit Transactions to ParallelChain network
    query          Query blockchain and world state information for ParallelChain network
    keys           Locally stores and manage account keypairs you created. (Password required)
    parse          Utilities functions to deserialize return values in CommandReceipt, and
                       compute contract address
    config         Get and set Fullnode RPC url to interact with ParallelChain
    help           Print this message or the help of the given subcommand(s)
```

## Why pchain_client
---

`pchain_client` allows you to query data from the ParallelChain, submit transactions, and more, all at the comfort of your command line.
Check out the examples below for details or see the full list of commands. The following document walks through the CLI's essential workflows. 

New users can begin either by 

1. [Install and Setup](./install_and_setup.md) or,
2. [Prepare Environment](../../getting_started/prepare_env.md) or,
3. [Setting up New Account](./manage_account.md)


If you are lost at any step, you can always type `pchain_client --help`.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

## Common Use Cases
---

- [Install and Setup](install_and_setup.md#install-and-setup)
    - [Installation](install_and_setup.md#installation)
    - [Running pchain_client](install_and_setup.md#running-pchain_client)
    - [Prepare Environment](install_and_setup.md#prepare-environment)
- [Manage Account](./manage_account.md#manage-account)
    - [Generate new keypair](./manage_account.md#generate-new-keypair)
    - [Import existing keypair](./manage_account.md#import-existing-keypair)
    - [List accounts](./manage_account.md#list-accounts)
- [Transaction](./transaction.md#transaction)
    - [Prepare Transaction file](./transaction.md#prepare-transaction-file)
      - [Create new Transaction file](./transaction.md#create-new-transaction-file)
      - [Append Command to existing file](./transaction.md#append-command-to-existing-file)
    - [Submit Transaction to ParallelChain](./transaction.md#submit-transaction-to-parallelchain)
- [Query](./query.md#query)
    - [Check Account related information](./query.md#check-account-related-information)
    - [Get Transaction with receipt](./query.md#get-transaction-with-receipt)
    - [Get Deposit and Stake](./query.md#get-deposit-and-stake)
- [Smart Contract](./smart_contract.md#smart-contract)
    - [Retrieve contract address](./smart_contract.md#retrieve-contract-address)
    - [Prepare contract method arguments file](./smart_contract.md#prepare-contract-method-arguments-file)

## Opening an Issue
---

Open an issue in [GitHub](https://github.com/parallelchain-io/pchain-client-cli/issues) if you:

1. Have a feature request / feature idea,
2. Have any questions (particularly software related questions),
3. Think you may have discovered a bug.

Please try to label your issues appropriately.