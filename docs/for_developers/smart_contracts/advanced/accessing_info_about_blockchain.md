---
tags:
  - pchain-sdk
  - Smart Contract
  - transaction
---


# Accessing Information about the Blockchain

Contract Methods can be written to not only depend on call arguments and the contract's storage, but also on information about the Blockchain, e.g., the previous block hash, or the identity of the External Account that originated the Transaction with Call Command.

Functions for getting information about the Transaction that triggered a Contract call and information about the larger Blockchain in general are defined in `pchain_sdk::transaction` and `pchain_sdk::blockchain` respectively. Internally, these functions are thin wrappers around functions defined in the Imports Set of the CBI.


## Parameters from Blockchain and Transaction
---

Contract execution is triggered by a Call Transaction to the blockchain. The contract being executed can obtain information from this transaction, and also the blockchain during execution. The information can be obtained by calling API methods in `pchain_sdk`.

Example:
```rust
// Get the timestamp of this block
let timestamp = pchain_sdk::blockchain::timestamp();
```

Related APIs in `pchain_sdk::blockchain`:

```rust
/// Get the `block_number` field of the Block that contains the Transaction which triggered this Contract call. 
fn block_number() -> Vec<u8>;
/// Get the `prev_block_hash` field of the Block that contains the Transaction which triggered this Contract call.
fn prev_block_hash() -> Vec<u8>;
/// Get the `timestamp` field of the Block that contains the Transaction which triggered this Contract call.
fn timestamp() -> u32;
/// Get the `balance` of the current account.
fn balance() -> u53;
```

Related APIs in `pchain_sdk::transaction`:

```rust
/// Get from the address of invoking the transaction
fn calling_account() -> [u8;32];
/// Get to the address of invoking the transaction
fn current_address() -> [u8;32];
/// Get the value of invoking transaction
fn amount() -> u64;
/// Returns whether it is an internal call
fn is_internal_call() -> bool;
/// Get transaction hash of invoking transaction
fn transaction_hash() -> [u8;32];
/// Get the method name of the invoking Contract Method
fn method() -> String;
/// Get method arguments of the invoking Contract Method
fn arguments() -> Vec<u8>;
```

