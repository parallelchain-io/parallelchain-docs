---
tags:
  - testnet 1.0
  - Smart Contract Development Kit (SDK)
---

# Introduction

ParallelChain Mainnet Smart Contract Development Kit (SDK) is an open-source rust crate that lets anyone build smart contracts easily. The SDK consists of methods and macros that allow you to interact with the blockchain from the smart contract. We provide some smart contract templates so that you can launch a new smart contract out of the box. 

We provide a hosted testnet blockchain called ParallelChain Mainnet (`testnet1`) that is a Turing complete smart contract platform. We will explain how to connect to this testnet in a later section. Anyone can upload their smart contracts to the testnet and share their contract address with others using our "light client".

This documentation is for both novice and advanced users alike. We have extra guides and explanations if you are unclear about some sections of the guide. The next section summarizes the topics covered in the SDK guide.

## Sections

- [Installation](./installation.md) guides you through installing rust and the necessary toolchains in your system. Rust is the technology stack used in ParallelChain Mainnet. 
- [Prepare Environment](./prepare_env.md) walks through the tools you will require to write your first smart contract. These include creating your account, adding balance to the account, and using the light client (`pchain`) to interact with the testnet.
- [My First Contract](./my_first_contract.md) gives a hands-on introduction to writing smart contracts using the SDK. You will download `my_first_contract` smart contract template, compile it into web assembly (`wasm`), and deploy it to the testnet. If you want to have a theoretical understanding of a contract first, feel free to skip this section for [Using ParallelChain Mainnet Smart Contract Development Kit (SDK](/sdk_and_writing_contract.md). You are welcome to return when ready to deploy your first smart contract.
- [Using ParallelChain Mainnet Smart Contract Development Kit (SDK)](./sdk_and_writing_contract.md) explains much of the high-level design and application of the SDK in a smart contract. This section also provides an in-depth explanation of `my_first_contract` and more complex examples in ParallelChain SDK. It is good to understand the capabilities and features available to you when writing the smart contract. You will learn how to use the macros provided by the SDK to simplify development. Furthermore, the macros help you write smart contracts as if writing a rust crate. 
- [Building and Deploying the Contract](./build_deploy_contract.md) is similar to the steps in [My First Contract](./my_first_contract.md) though it comes with a few extra steps. These are "minifying" your smart contract to reduce gas usage for smart contract deployment. 
- [Calling a Contract from an Externally Owned Account](./etoc_call.md) teaches you how to prepare your data input and make calls to the smart contract using the light client.
