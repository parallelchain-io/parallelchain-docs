---
tags:
  - testnet 1.0
  - parallelchain sdk
---

# Prepare Environment

`Learning outcome`: _Install the light client, create an account and add balance to it._

In this section, you need the tools and environment to develop and run smart contracts. You can utilize our testnet to deploy and make contract calls. 

Alternatively, we will also provide you with the ParallelChain fullnode binary with which you can simulate contract execution before deploying to our testnet. 

## Verify that the testnet is live

To verify that our testnet is live and running, please make sure that the following URLs are working:

* [https://testnet1.digital-transaction.net](https://testnet1.digital-transaction.net) 
* [https://testnet.parallelchain.io/explorer](https://testnet.parallelchain.io/explorer) 
* [https://testnet.parallelchain.io/explorer/faucet](https://testnet.parallelchain.io/explorer/faucet) 

We have a single native token in the `testnet1` phase. These tokens are called `TXPLL`.

## Testnet1 explorer

You can see the status of your transactions and whether they are added to the block here
- [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer) 

The testnet explorer can explore transactions, addresses, analytics (performance of `testnet1`), and blocks. Alternatively, you can use ParallelChain's light client (called `pchain`) to interact with the `testnet1` node.

## Setup ParallelChain Light Client 

In this section, we will configure the `pchain` executable. These steps include:

- [Prepare Environment](#prepare-environment)
  - [Verify that the testnet is live](#verify-that-the-testnet-is-live)
  - [Testnet1 explorer](#testnet1-explorer)
  - [Setup ParallelChain Light Client](#setup-parallelchain-light-client)
  - [Pointing the light client to the testnet](#pointing-the-light-client-to-the-testnet)
  - [Generate an account](#generate-an-account)
  - [Request for tokens from the faucet](#request-for-tokens-from-the-faucet)


You can download the precompiled binaries using terminal
```bash
wget https://cms.parallelchain.io/parallelchain-cli_linux_v0.1.0.tar.xz
```
You can also use other methods to download the light client binaries.

### **For Windows**, 
you can download the binaries from [https://cms.parallelchain.io/parallelchain-cli_win_x64_v0.1.0.zip](https://cms.parallelchain.io/parallelchain-cli_win_x64_v0.1.0.zip).

Unzip the compressed zip file and you will get a binary with this name: `parallelchain-cli_win_x64_v0.1.0.exe`.

** IMPORTANT: Rename this binary to `pchain`.**

You can follow this guide to add `pchain` to a universal path in windows: [https://stackoverflow.com/a/41895179](https://stackoverflow.com/a/41895179). With this step, `pchain`
is accessible from any directory in your command prompt.

To access `pchain`, open your command prompt by this method: [https://www.businessinsider.com/how-to-open-command-prompt](https://www.businessinsider.com/how-to-open-command-prompt)

### **For Linux**,
you can download the binaries from [https://cms.parallelchain.io/parallelchain-cli_linux_v0.1.0.tar.xz](https://cms.parallelchain.io/parallelchain-cli_linux_v0.1.0.tar.xz).

Extract the binary using this command:
```
tar -xvf parallelchain-cli_linux_v0.1.0.tar.xz
```

You will obtain a binary with this name: `parallelchain-cli_linux_v0.1.0`.

** IMPORTANT: Rename this binary to `pchain`.**

Add the light client to an installation path
```bash
sudo cp target/release/pchain /usr/bin
```

Restart your terminal and test if `pchain` is accessible from any location in your terminal.
```bash
pchain
```

For more details on the usage guide for `pchain`, please head to ParallelChain Light Client page. Alternatively, you can type the command below to see the subcommands available for use:
```bash
pchain --help
```

## Pointing the light client to the testnet

Type the command below to point the light client to the testnet:
```bash
pchain set config --target-address https://testnet1.digital-transaction.net
``` 

This command will write a config.json file in `$HOME/.parallelchain/pchain_cli/config.json`. It only needs to be executed once.

## Generate an account

See [Create a new Externally Owned Account (EOA)](/cli/real_world_walkthrough/#create-a-new-externally-owned-account-eoa)

## Request for tokens from the faucet

TODO: This should be removed and replaced with the testnet faucet page.

In `testnet1`, you will need native tokens to pay the node for deploying and executing your smart contracts. 

See [Faucet](/faucet) on how to request tokens from the faucet.

Congratulations, you have enough balance for writing and deploying a smart contract.
