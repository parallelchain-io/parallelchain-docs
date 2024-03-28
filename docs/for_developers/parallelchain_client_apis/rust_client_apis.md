---
tags:
  - Tools
  - ParallelChain RPC API
  - Rust
  - Client
---

# Rust Client APIs

ParallelChain RPC APIs are the HTTP APIs that Fullnodes make available to clients. 

In the section [ParallelChain Client Library](#parallelchain-client-library-rust), we introduce the Rust Library to make RPC calls programatically. 

The Client Library allows you to easily submit RPC requests and parse RPC responses without the need to understand the details of HTTP URL, request format or response format. The formation of HTTP APIs is specified in [ParallelChain Protocol](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/RPC.md).

## ParallelChain Client Library (Rust) 

[ParallelChain Client Library](https://github.com/parallelchain-io/pchain-client-rust) is a Rust programming language implementation of ParallelChain RPC API. We will show how to use the library by example code. To use the library in your rust project, first add dependency [pchain-client](https://crates.io/crates/pchain-client) in `Cargo.toml` file. You will also need the dependency [pchain-types](https://crates.io/crates/pchain-types) for necessary basic types, and the dependency [tokio](https://crates.io/crates/tokio) to run the client in asynchronous application.

Cargo.toml:
```rust
[dependencies]
pchain-client = "0.4.3"
pchain-types = "0.4.3"
tokio = {version = "1", features = ["full"]}
```

You can instantiate the struct `pchain_client::Client` by providing the link to a ParallelChain RPC endpoint.

```rust
#[tokio::main]
async fn main() {
    /// Connect to Testnet RPC endpoint
    let client = Client::new("https://pchain-test-rpc02.parallelchain.io");
    /// ...
}
```

!!! Note
    RPC endpoints for [Mainnet](../../fundamentals/networks.md#parallelchain-mainnet) and [Testnet](../../fundamentals/networks.md#parallelchain-testnet) are `https://pchain-main-rpc02.parallelchain.io` and `https://pchain-test-rpc02.parallelchain.io` respectively.

## Transaction Related RPCs

### submit_transaction 

Submit a transaction to the mempool.


```rust
// The function submit_transaction creates SubmitTransactionRequest from the input transaction.
let response: SubmitTransactionResponse = client
    .submit_transaction(&Transaction::new(
        &signer,
        nonce,
        vec![Command::Transfer(TransferInput {
            amount,
            recipient,
        })],
        gas_limit,
        max_base_fee_per_gas,
        priority_fee_per_gas,
    ))
    .await
    .unwrap();
```
!!! Note
    The variable `signer` is the [Keypair](https://docs.rs/pchain-types/0.4.3/pchain_types/cryptography/type.Keypair.html) of the [EOA](../../fundamentals/accounts.md) as the signer of the transaction. Check out the module [pchain_types::cryptograhy](https://docs.rs/pchain-types/0.4.3/pchain_types/cryptography/index.html) for generating or importing keypair.

    In case you need free tokens for submitting transactions to Testnet, check out the [Faucet Service](../../fundamentals/networks.md#faucet-service) for details.

**Request**

```rust
struct SubmitTransactionRequest {
    transaction: Transaction,
}
```

**Response**

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


```rust
let response: TransactionResponse = client
    .transaction(&TransactionRequest {
        transaction_hash,
        include_receipt,
    })
    .await
    .unwrap();
```

**Request**

```rust
struct TransactionRequest {    
    transaction_hash: CryptoHash,
    include_receipt: bool,
}
```

**Response**

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

```rust
let response: TransactionPositionResponse = client
    .transaction_position(&TransactionPositionRequest { transaction_hash })
    .await
    .unwrap();
```

**Request**

```rust
struct TransactionPositionRequest {
    transaction_hash: CryptoHash,
}
```

**Response**

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

```rust
let response: ReceiptResponse = client
    .receipt(&ReceiptRequest { transaction_hash })
    .await
    .unwrap();
```

**Request**

```rust
struct ReceiptRequest {    
    transaction_hash: CryptoHash,
}
```

**Response**

```rust
struct ReceiptResponse {
    transaction_hash: CryptoHash,
    receipt: Option<Receipt>,
    block_hash: Option<CryptoHash>,
    position: Option<u32>,
}
```

## Block Related RPCs

### block

Get a block by its block hash.

```rust
let response: BlockResponse = client
    .block(&BlockRequest { block_hash })
    .await
    .unwrap();
```

**Request**

```rust
struct BlockRequest {
    block_hash: CryptoHash,
}
```

**Response**

```rust
struct BlockResponse {
    block: Option<Block>,
}
```

---

### block_header

Get a block header by its block hash.

```rust
let response: BlockHeaderResponse = client
    .block_header(&BlockHeaderRequest { block_hash })
    .await
    .unwrap();
```

**Request**

```rust
struct BlockHeaderRequest {
    block_hash: CryptoHash,
}
```

**Response**

```rust
struct BlockHeaderResponse {
    block_header: Option<BlockHeader>,
}
```

---

### block_height_by_hash

Get the height of the block with a given block hash. 

```rust
let response: BlockHeightByHashResponse = client
    .block_height_by_hash(&BlockHeightByHashRequest { block_hash })
    .await
    .unwrap();
```

**Request**

```rust
struct BlockHeightByHashRequest {
    block_hash: CryptoHash,
}
```

**Response**

```rust
struct BlockHeightByHashResponse {
    block_hash: CryptoHash,
    block_height: Option<BlockHeight>,
}
```

---

### block_hash_by_height

Get the hash of a block at a given height.

```rust
let response: BlockHashByHeightResponse = client
    .block_hash_by_height(&BlockHashByHeightRequest { block_height })
    .await
    .unwrap();
```

**Request**

```rust
struct BlockHashByHeightRequest {
    block_height: BlockHeight,
}
```

**Response**

```rust
struct BlockHashByHeightResponse {
    block_height: BlockHeight,
    block_hash: Option<CryptoHash>,
}
```

---

### highest_committed_block

Get the hash of the highest committed block. 

```rust
let response: HighestCommittedBlockResponse = client
    .highest_committed_block()
    .await
    .unwrap();
```

**Request**

None.

**Response**

```rust
struct HighestCommittedBlockResponse {
    block_hash: Option<CryptoHash>,
}
```

## State Related RPCs

State RPCs return multiple entities in the world state in a single response. This allows clients to get a consistent snapshot of the world state in a single call.

Every response structure includes the hash of the highest committed block when the snapshot is taken.

Some of the following RPCs' response structures reference types unique to this document. These are specified in [types referenced in state RPC responses](#types-referenced-in-state-rpc-responses).

---

### state

Get the state of a set of accounts (optionally including their contract code), and/or a set of storage tuples.

```rust
let response: StateResponse = client
    .state(&StateRequest {
        accounts,
        include_contract,
        storage_keys,
    })
    .await
    .unwrap();
```

**Request**

```rust
struct StateRequest {
    accounts: HashSet<PublicAddress>,
    include_contracts: bool,
    storage_keys: HashMap<PublicAddress, HashSet<Vec<u8>>>,
}
```

**Response**

```rust
struct StateResponse {
    accounts: HashMap<PublicAddress, Account>,
    storage_tuples: HashMap<PublicAddress, HashMap<Vec<u8>, Vec<u8>>>,
    block_hash: CryptoHash,
}
```

---

### validator_sets

Get the previous, current, and next validator sets, optionally including the stakes delegated to them.  

```rust
 let response: ValidatorSetsResponse = client
    .validator_sets(&ValidatorSetsRequest {
        include_prev,
        include_prev_delegators,
        include_curr,
        include_curr_delegators,
        include_next,
        include_next_delegators,
    })
    .await
    .unwrap();
```

**Request**

```rust
struct ValidatorSetsRequest {
    include_prev: bool,
    include_prev_delegators: bool,
    include_curr: bool,
    include_curr_delegators: bool,
    include_next: bool,
    include_next_delegators: bool,
}
```

**Response**

```rust
struct ValidatorSetsResponse {
    // The inner Option is None if we are at Epoch 0.
    prev_validator_set: Option<Option<ValidatorSet>>,
    curr_validator_set: Option<ValidatorSet>,
    next_validator_set: Option<ValidatorSet>,
    block_hash: CryptoHash,
}
```

---

### pools

Get a set of pools.

```rust
let response = client
    .pools(&PoolsRequest {
        include_stakes,
        operators,
    })
    .await
    .unwrap();
```

**Request**

```rust
struct PoolsRequest {
    operators: HashSet<Operator>,
    include_stakes: bool,
}
```

**Response**

```rust
struct PoolsResponse {
    pools: HashMap<Operator, Option<Pool>>,
    block_hash: CryptoHash,
}
```

---

### deposits

Get a set of deposits.

```rust
let response: DepositsResponse = client
    .deposits(&DepositsRequest { stakes })
    .await
    .unwrap();
```

**Request**

```rust
struct DepositsRequest {
    stakes: HashSet<(Operator, Owner)>,
}
```

**Response**

```rust
struct DepositsResponse {
    deposits: HashMap<(Operator, Owner), Option<Deposit>>,
    block_hash: CryptoHash,
}
```

---

### stakes

Get a set of stakes.

```rust
let response: StakesResponse = client
    .stakes(&StakesRequest { stakes })
    .await
    .unwrap();
```

**Request**

```rust
struct StakesRequest {
    stakes: HashSet<(Operator, Owner)>,
}
```

**Response**

```rust
struct StakesResponse {
    stakes: HashMap<(Operator, Owner), Option<Stake>>,
    block_hash: CryptoHash,
}
```

---

### view

Call a method in a contract in a read-only way.

```rust
let response: ViewResponse = client
    .view(&ViewRequest {
        target,
        method,
        arguments,
    })
    .await
    .unwrap();
```

**Request**

```rust
struct ViewRequest {
    target: PublicAddress,
    method: Vec<u8>,
    arguments: Option<Vec<Vec<u8>>>,
}
```

**Response**

```rust
struct ViewResponse {
    receipt: CommandReceipt,
}
```

---

## Types Referenced in State RPC Responses

### Account-related types
```rust
enum Account {
    WithContract(AccountWithContract),
    WithoutContract(AccountWithoutContract),
}

struct AccountWithContract {
    nonce: Nonce,
    balance: Balance,
    contract: Option<Vec<u8>>, 
    cbi_version: Option<CBIVersion>,
    storage_hash: Option<CryptoHash>,
}

struct AccountWithoutContract {
    nonce: Nonce,
    balance: Balance,
    cbi_version: Option<CBIVersion>,
    storage_hash: Option<CryptoHash>,
}
```

### Staking-related types 

```rust
enum ValidatorSet {
    WithDelegators(Vec<PoolWithDelegators>),
    WithoutDelegators(Vec<PoolWithoutDelegators>),
}

type Operator = PublicAddress;
type Owner = PublicAddress;

struct PoolWithDelegators {
    operator: PublicAddress,
    power: Balance,
    commission_rate: u8, 
    operator_stake: Option<Stake>,
    delegated_stakes: Vec<Stake>,
}

struct PoolWithoutDelegators {
    operator: PublicAddress,
    power: Balance,
    commission_rate: u8, 
    operator_stake: Stake,
}

struct Deposit {
    owner: PublicAddress,
    balance: u64,
    auto_stake_rewards: bool,
}

struct Stake {
    owner: PublicAddress,
    power: Balance,
}

enum Pool {
    WithStakes(PoolWithStakes),
    WithoutStakes(PoolWithoutStakes),
}
```