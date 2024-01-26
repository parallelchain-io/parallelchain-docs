---
tags:
  - Tools
  - ParallelChain RPC API
---

# ParallelChain RPC API

This document specifies the ParallelChain RPC API, an HTTP API that Fullnodes make available to clients. 

We spell out the general properties of the API, and then list the available RPCs. With each RPC, we define a request structure and a response structure. 

For readability, we categorize RPCs into [transaction-related RPCs](./transaction_rpc.md), [block-related RPCs](./block_rpc.md), and [state-related RPCs](./state_rpc.md).


## General Properties
---

1. Procedures are reachable over HTTP at a URL suffixed by the procedure’s name.
2. All HTTP requests should be **POST**.
3. Request and response structures are serialized using **Borsh** and carried in **HTTP message bodies**. 
4. Procedures are “strongly typed”. If a procedure receives a request that cannot be deserialized, it will send back a response with an empty body and a **Bad Request (400)** status code.
5. Conversely, if a procedure receives a request that can be deserialized, the response it sends back must have an **OK (200)** status code. This is even if, for example, something was “not found” (e.g., a block was not found with a specified hash), or a transaction was not added to the mempool. Error statuses are reported in response structures.
6. All requests and OK responses should have a content-type of "application/octet-stream". This is so that we can easily extend the RPC API in the future to support other serialization schemes, e.g., JSON using "application/json".