---
tags:
  - wallet
  - browser extension
  - provider API
---

# Introduction to Wallet Browser Provider APIs

The ParallelChain Wallet Provider API is a Javascript API designed to deliver a friendly developer experience and consistency across clients and applications. 

ParallelChain Wallet Extension injects a global Javascript API into the browser using the `#!js window.xpll` [provider][1] object. 

This API is specified by [EIP-1193][2], which promotes wallet interoperability by preventing conflicting interfaces and behaviours between wallets. 

ParallelChain Wallet Provider API is designed to be minimal, event-driven, and agnostic of transport and [RPC][3] protocols. Its functionality is easily extended by defining new RPC methods and message event types.

[1]: ../definition.md#provider
[2]: https://eips.ethereum.org/EIPS/eip-1193
[3]: ../definition.md#remote-procedure-call-(rpc)
