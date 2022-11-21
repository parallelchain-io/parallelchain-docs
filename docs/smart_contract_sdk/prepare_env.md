---
tags:
  - testnet 3
  - parallelchain sdk
  - explorer
---

# Prepare Environment

In this section, you need the tools and environment to develop and run smart contracts. You can utilize our testnet to deploy and make contract calls. 

## Verify that the testnet is live
---

To verify that our testnet is live and running, please make sure that the following URLs are working:

* [https://node-t3-1.digital-transaction.net](https://node-t3-1.digital-transaction.net) 
* [https://node-t3-2.digital-transaction.net](https://node-t3-2.digital-transaction.net) 
* [https://node-t3-3.digital-transaction.net](https://node-t3-3.digital-transaction.net) 
* [https://node-t3-4.digital-transaction.net](https://node-t3-4.digital-transaction.net) 
* [https://service-t3.digital-transaction.net/rich_api](https://service-t3.digital-transaction.net/rich_api) 
* [https://service-t3.digital-transaction.net/analytics_api](https://service-t3.digital-transaction.net/analytics_api) 
* [https://testnet.parallelchain.io/explorer](https://testnet.parallelchain.io/explorer) 

The ParallelChain Mainnet native tokens are called `TXPLL`.

## Testnet Explorer
---

You can see the status of your transactions and whether they are added to the block here
- [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer) 

The testnet explorer can explore transactions, addresses, analytics (performance of `testnet`), and blocks. Alternatively, you can use ParallelChain Client (called `pchain_client`) to interact with the `testnet` node.

## Setup ParallelChain Client 
---

In this section, we will configure the `pchain_client` executable. We will assume that `pchain_client` runs on both Windows and Linux/MacOs as we have guides on setting up the Light Client for these platforms. 

Therefore, when you use `pchain_client`, the same command applies to Windows, Linux and MacOS command line interface.

See ["Installation"](../getting_started/installation.md) to install `pchain_client` on your pc.

## Generate an account and receive tokens
---

See [Create Account](../getting_started/create_account.md)

Congratulations, you have enough balance for writing and deploying a smart contract.
