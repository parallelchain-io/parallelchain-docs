---
tags:
  - Tools
  - ParallelChain RPC API
---

# ParallelChain RPC API

## State RPCs

State RPCs return multiple entities in the world state in a single response. This allows clients to get a consistent snapshot of the world state in a single call.

Every response structure includes the hash of the highest committed block when the snapshot is taken.

Some of the following RPCs' response structures reference types unique to this document. These are specified in [types referenced in state RPC responses](#types-referenced-in-state-rpc-responses).

---

### state

Get the state of a set of accounts (optionally including their contract code), and/or a set of storage tuples.

#### Request

```rust
struct StateRequest {
    accounts: HashSet<PublicAddress>,
    include_contracts: bool,
    storage_keys: HashMap<PublicAddress, HashSet<Vec<u8>>>,
}
```

#### Response

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

#### Request

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

#### Response

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

#### Request

```rust
struct PoolsRequest {
    operators: HashSet<Operator>,
    include_stakes: bool,
}
```

#### Response

```rust
struct PoolsResponse {
    pools: HashMap<Operator, Option<Pool>>,
    block_hash: CryptoHash,
}
```

---

### deposits

Get a set of deposits.

#### Request

```rust
struct DepositsRequest {
    stakes: HashSet<(Operator, Owner)>,
}
```

#### Response

```rust
struct DepositsResponse {
    deposits: HashMap<(Operator, Owner), Option<Deposit>>,
    block_hash: CryptoHash,
}
```

---

### stakes

Get a set of deposits.

#### Request

```rust
struct StakesRequest {
    stakes: HashSet<(Operator, Owner)>,
}
```

#### Response

```rust
struct StakesResponse {
    stakes: HashMap<(Operator, Owner), Option<Stake>>,
    block_hash: CryptoHash,
}
```

---

### view

Call a method in a contract in a read-only way.

#### Request

```rust
struct ViewRequest {
    target: PublicAddress,
    method: Vec<u8>,
    arguments: Option<Vec<Vec<u8>>>,
}
```

#### Response

```rust
struct ViewResponse {
    receipt: CommandReceipt,
}
```

---

## Types referenced in state RPC responses

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