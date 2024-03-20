---
tags:
  - Tools
  - CLI
  - pchain-cli-rust
---

# pchain-cli-rust

**pchain-cli-rust** is a Rust client library for the ParallelChain Protocol fullnode [RPC API](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/RPC.md).

It is designed to be user-friendly and accessible to developers and non-developers alike. It is intended to provide a simple and intuitive way to interact with the blockchain network, without requiring extensive technical knowledge or expertise.

## Getting started

Get started by creating an instance of client. 

```rust
use pchain_client::Client;

let client = Client::new("https://rpc_base_url.xyz");
```

You will then be able to access each RPC through a corresponding method of the same name.

```rust
client.submit_transaction(txn);
client.block(block_request);
client.state(state_request);
```
