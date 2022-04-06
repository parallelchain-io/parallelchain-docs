---
tags:
  - testnet 1.0
  - parallelchain sdk
---

# Prepare Environment

`Learning outcome`: _Install the light client, create an account and add balance to it._

In this section, you need the tools and environment to develop and run smart contracts. You can utilize our testnet to deploy and make contract calls. 

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

In this section, we will configure the `pchain` executable. We will assume that `pchain` runs on both Windows and Linux/MacOs as we have guides on setting up the Light Client for these platforms. 

Therefore, when you use `pchain`, the same command applies to Windows, Linux and MacOS command line interface.

See ["ParallelChain Client Setup"](../cli/index.md#setup) to install `pchain` on your pc.

## Pointing the light client to the testnet

Type the command below to point the light client to the testnet:
```bash
pchain set config --target-address https://testnet1.digital-transaction.net
``` 

This command will write a config.json file in `$HOME/.parallelchain/pchain_cli/config.json`. It only needs to be executed once.

## Generate an account

See [Create a new Externally Owned Account (EOA)](../cli/tutorial.md#create-a-new-externally-owned-account-eoa)

## Request for tokens from the faucet

TODO: This should be removed and replaced with the testnet faucet page.

In `testnet1`, you will need native tokens to pay the node for deploying and executing your smart contracts. 

See [Faucet](/faucet) on how to request tokens from the faucet.

Congratulations, you have enough balance for writing and deploying a smart contract.
