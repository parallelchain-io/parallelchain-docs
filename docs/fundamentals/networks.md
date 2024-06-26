---
tags:
  - ParallelChain Mainnet
  - ParallelChain Testnet 
  - Testnet 4
  - Explorer
  - Xperience Broswer Extension
---

# Networks


## ParallelChain Mainnet

**ParallelChain Mainnet** is a high-performance blockchain designed for enterprise-level use cases. It provides a platform for developers to build decentralised applications (dApps) with its efficient smart contract execution engine.

It is designed to balance high performance and genuine decentralization. It is protected by the a Byzantine Fault Tolerant (BFT) consensus protocol, which adopts a sophisticated class-based node system to ensure fast and accountable consensus.

For more information about the consensus protocol, please read the section [Consensus Mechanism](./consensus_mechanism.md).

## ParallelChain Testnet

To ensure the new features in ParallelChain Mainnet have what it takes to survive decentralised deployment, they must first be battle-tested. This is where ParallelChain Testnet comes into play by allowing developers, or simply users, to develop and run smart contracts or transactions on ParallelChain and experiment with the blockchain, at no cost.

Both Mainnet and Testnet are accessible through [Explorer](#parallelchain-explorer). But only onTestnet, a service called [Faucet Service](#faucet-service) provides free tokens to users.

## Accessing the Networks

There are two client tools for accessing Mainnet and Testnet. 

- **ParallelChain Explorer**: Web-based tool with a crypto wallet for interacting with Mainnet and Testnet.
- **Xperience Browser Extension**: Browser Extension of the ParallelChain Explorer Wallet.

### ParallelChain Explorer

**ParallelChain Explorer** is a web-based tool integrated with the ParallelChain Mainnet ecosystem, which allows users to visualize and interact with the Mainnet and Testnet. By using the explorer, users can search for real-time and historical information about the blockchain. 

What you can do:

- **Search and Navigation**: Find specific blocks, transactions, and accounts easily by using block hash, transaction hash, and account address.
- **Block Details**: Explore details of specific blocks, including block height, timestamp, transaction information, etc.
- **Transaction Details**: Explore details of specific transactions, including signer/recipient account information, timestamp, consumed gas, etc.
- **Account Details**: Explore details of specific accounts, including nonce, contract count, transaction history, etc.
- **Staking Details**: Explore details of staking information for each pool, including operator, pool power, and commission rate, etc.
- **Network Statistics**: Gain insights into network-wide statistics, including gas consumption, block production time, reward issuance, etc.
- **Wallet**: Seamlessly create a ParallelChain wallet account and make transactions.

**Check out our [ParallelChain Explorer](https://explorer.parallelchain.io/explorer)!**

### Xperience Browser Extension

**Xperience Browser Extension** is a browser extension that allows you to integrate your [dApp](../for_developers/xperience_browser_provider_apis/introduction.md) with ParallelChain Wallet.

This enables your dapp to interact with your dapp users' XPLL accounts, to:

- Send [transactions](transactions.md)
  - [Stake](staking.md) XPLL
- Trigger confirmation for [smart contract](../for_developers/smart_contracts/introduction.md) calls 

**The browser extension is available on the [Chrome Web Store](https://chromewebstore.google.com/detail/xperience-browser-extensi/gpfllmjckejjhmmdmgbgmclmhopekjpf) now.**

### Faucet Service

The **Faucet Service** of our Testnet issues free testing tokens to users to test the blockchain network on Testnet.

To obtain testing tokens, create your Testnet account first. Account can be easily created by using Web Wallet in **ParallelChain Explorer** (See [here](../for_users/web_wallet/create_account.md)), or **Xperience Browser Extension** (See [here](../for_users/xperience_browser_extension/create_account.md)).

The created account should have an empty balance initially. **You can request free tokens from [Faucet Service](https://faucet.parallelchain.io).**