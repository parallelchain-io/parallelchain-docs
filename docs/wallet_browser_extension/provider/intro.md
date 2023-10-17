---
tags:
  - wallet
  - browser extension
  - provider API
---

# ParallelChain Wallet Provider API

A Javascript Wallet Provider API for consistency across clients and applications.

## Introduction

A common convention in the [“dapp”][2] ecosystem is for key management software [“wallets”][3]
to expose their API via a JavaScript object in the web page.
This object is called “[Provider][4]”.

Historically, Provider implementations have exhibited conflicting interfaces and behaviors between wallets.
The [EIP-1193][1] formalizes the Provider API to promote wallet interoperability.
The API is designed to be minimal, event-driven, and agnostic of transport and RPC protocols.
Its functionality is easily extended by defining new RPC methods and message event types.

The ParallelChain Provider implementation based on [EIP-1193][1]
with additional help to provide a friendly developer experience.

After the ParallelChain wallet extension has been installed,
provider has been made available as `#!js window.xpll` in web browsers.

[1]: https://eips.ethereum.org/EIPS/eip-1193
[2]: ./definition.md#dapp
[3]: ./definition.md#wallet
[4]: ./definition.md#provider
