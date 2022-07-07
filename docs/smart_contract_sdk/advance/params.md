---
tags:
  - testnet 2.0
  - parallelchain sdk
  - smart contract
  - transaction
  - parameters
---


# Parameters from blockchain and transaction

Contract execution is triggered by [EtoC](../../getting_started/call_contract.md) transaction to blockchain. The contract being executed can obtain information from this transaction, and also the blockchain during execution. The information can obtained and save to the struct `Transaction` by instantiation.

Example:
```rust
let tx = Transaction::new();
```

The struct `Transaction` consists of fields:

|name| type| Description|
|:---|:---|:---|
|`this_block_number`| u64 | Block number of this block |
|`prev_block_number`| 32 bytes | Block hash of the previous block |
|`timestamp`| u32 | Unix timestamp |
|`to_address`| 32 bytes | Address of a recipient |
|`from_address`| 32 bytes | Address of a sender |
|`value`| u64 | Amount of tokens to be send to the smart contract address |
|`transaction_hash`| 32 bytes | Hash of a transaction's signature. The transaction hashes of other transactions included in a block are used to obtain the Merkle root hash of a block. |
|`arguments`| variable-length data | Transaction data as arguments to this contract call. See [Input Arguments](../develop_contract.md) |
