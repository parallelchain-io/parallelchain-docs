---
tags:
  - Tools
  - ParallelChain RPC API
---

# ParallelChain RPC API


## Transaction RPCs
---
### submit_transaction 

Submit a transaction to the mempool.

#### Request

```rust
struct SubmitTransactionRequest {
    transaction: Transaction,
}
```

#### Response

```rust
struct SubmitTransactionResponse {
    error: Option<SubmitTransactionError>,
}
```

Where `SubmitTransactionError`:
```rust
enum SubmitTransactionError {
    UnacceptableNonce,
    MempoolFull,
    Other,
}
```

---

### transaction

Get a transaction and optionally its receipt.

#### Request

```rust
struct TransactionRequest {    
    transaction_hash: CryptoHash,
    include_receipt: bool,
}
```

#### Response

```rust
struct TransactionResponse {
    transaction: Option<Transaction>,
    receipt: Option<Receipt>,
    block_hash: Option<CryptoHash>,
    position: Option<u32>,
}
```

---

### transaction_position

Find out where a transaction is in the blockchain.

#### Request

```rust
struct TransactionPositionRequest {
    transaction_hash: CryptoHash,
}
```

#### Response

```rust
struct TransactionPositionResponse {
    transaction_hash: Option<CryptoHash>,
    block_hash: Option<CryptoHash>,
    position: Option<u32>,
}
```

---

### receipt

Get a transaction's receipt.

#### Request

```rust
struct ReceiptRequest {    
    transaction_hash: CryptoHash,
}
```

#### Response

```rust
struct ReceiptResponse {
    transaction_hash: CryptoHash,
    receipt: Option<Receipt>,
    block_hash: Option<CryptoHash>,
    position: Option<u32>,
}
```