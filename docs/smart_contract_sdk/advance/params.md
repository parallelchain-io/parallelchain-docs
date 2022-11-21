---
tags:
  - testnet 3
  - parallelchain sdk
  - smart contract
  - transaction
  - parameters
---


# Accessing information about the Blockchain

Contract Methods can be written to not only depend on call arguments and the contract's storage, but also on information about the Blockchain, e.g., the previous block hash, or the identity of the External Account that originated the EtoC Transaction. 

Functions for getting information about the Transaction that triggered a Contract call and information about the larger Blockchain in general are defined in `pchain_sdk::transaction` and `pchain_sdk::blockchain` respectively. Internally, these functions are thin wrappers around functions defined in the Imports Set of the Contract ABI.



## Parameters from blockchain and transaction

Contract execution is triggered by [EtoC](../../getting_started/call_contract.md) transaction to blockchain. The contract being executed can obtain information from this transaction, and also the blockchain during execution. The information can obtained by calling API methods in `pchain_sdk`.

Example:
```rust
// Get the timestamp of this block
let timestamp = pchain_sdk::blockchain::timestamp();
```

Related APIs in `pchain_sdk::blockchain`:

```rust
/// Get the `number` field of the Block that contains the Transaction which triggered this Contract call. 
fn block_number() -> Vec<u8>;
/// Get the `prev_hash` field of the Block that contains the Transaction which triggered this Contract call.
fn prev_hash() -> Vec<u8>;
/// Get the `timestamp` field of the Block that contains the Transaction which triggered this Contract call.
fn timestamp() -> u32;
/// Get the `random_bytes` field of the Block that contains the Transaction which triggered this Contract call.
fn random_bytes() -> Vec<u8>;
```

Related APIs in `pchain_sdk::transaction`:

```rust
/// Get from address of invoking transaction
fn from_address() -> [u8;32];
/// Get to address of invoking transaction
fn to_address() -> [u8;32];
/// Get value of invoking transaction
fn value() -> u64;
/// Get nonce of invoking transaction
fn sequence_number() -> u64;
/// Get transaction hash of invoking transaction
fn hash() -> [u8;32];
/// get input data/arguments for entrypoint
fn data() -> Vec<u8>;
```

