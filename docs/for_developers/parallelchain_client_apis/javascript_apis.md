---
tags:
  - Tools
  - ParallelChain RPC API
  - Javascript
  - Client
---

ParallelChain RPC APIs are the HTTP APIs that Fullnodes make available to clients. 

In the section [ParallelChain Client Library](#parallelchain-client-library-javascript), we introduce the Javascript Library to make RPC calls programatically. 

The Client Library allows you to easily submit RPC requests and parse RPC responses without the need to understand the details of HTTP URL, request format or response format. The formation of HTTP APIs is specified in [ParallelChain Protocol](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/RPC.md).

## ParallelChain Client Library (Javascript)

[ParallelChain Client Library](https://github.com/parallelchain-io/pchain-client-js) is a Javascript package providing a HTTP client for interacting with the ParallelChain RPC APIs. We will show how to use the library by example code that runs in [Node.js](https://nodejs.org/). To use the library, install the [npm](https://www.npmjs.com) package [pchain-client-js](https://www.npmjs.com/pchain-client-js). You will also need the package [pchain-types-js](https://www.npmjs.com/package/pchain-types-js) for necessary basic types.

```sh
npm install pchain-client-js
npm install pchain-types-js
```

You can instantiate the object `Client` by providing the link to a ParallelChain RPC endpoint. Let's start with a simple Node.js script as below.

```js
const Client = require("pchain-client-js").Client;

(async () => {
    /// Connect to Testnet RPC endpoint
    const client = new Client('https://pchain-test-rpc02.parallelchain.io');
    /// ...
})();
```

!!! Note
    RPC endpoints for [Mainnet](../../fundamentals/networks.md#parallelchain-mainnet) and [Testnet](../../fundamentals/networks.md#parallelchain-testnet) are `https://pchain-main-rpc02.parallelchain.io` and `https://pchain-test-rpc02.parallelchain.io` respectively.

## Understanding Basic Types

Throughout the API Reference, basic types provided by [pchain-type-js](https://www.npmjs.com/package/pchain-types-js) are often being used in the RPC requests and responses. For better understanding their data, Let's go through them.  

First, the integer types `u16`, `u32` and `u64` are [type-alias](https://www.w3schools.com/typescript/typescript_aliases_and_interfaces.php) to types `number`, `number` and [BN](https://www.npmjs.com/package/bn.js) in typescript respectively.

The class `Option<T>` contains a value with type `T` or value of [null](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#null-and-undefined).

The classes `Sha256Hash` and `PublicAddress` store a data `Buffer`. They are similar in the way that they can be instantiated from a Base64URL encoded string.

```ts
// Instantiates a PublicAddress Object by passing the 32-byte address in base64url encoded string.
const public_address = new PublicAddress('oK8Kvd-2cWYloQaPNlGtG3Q5dV6JFKzVrXOAhBRt5hs');
// Instantiates a Sha256Hash Object by passing the Sha256 hash in base64url encoded string.
const sha256_hash = new Sha256Hash('x96G2mLXgaCNaAABXNICWHh_DH33khSg_T7UawTGnGg');
```

The abstract class `Enum` functions as the [enum](https://doc.rust-lang.org/book/ch06-01-defining-an-enum.html) in Rust Programming Language so that the inherited class can be used and serialized as requred by ParallelChain RPC API. An example is [ValidatorSet](#staking-related-types).

The class extends the `Enum` and provides two variants.
```ts
class ValidatorSet extends Enum {
    poolsWithDelegator?: PoolWithDelegator[];
    poolsWithoutDelegator?: PoolWithoutDelegator[];
}
```

Example to get the object of `PoolWithoutDelegator`:

```ts
const validatorSet = // an object of ValidaterSet
const poolsWithoutDelegator = validatorSet?.poolsWithoutDelegator || [];
```

## Transaction Related RPCs

### submit_transaction 

Submit a transaction to the mempool.

```js
const transaction = await new Transaction({
    signer: keypair.public_key,
    // nonce, commands, ... 
})
.toSignedTx(keypair);

const response = await client.submit_transaction(
    new SubmitTransactionRequest({ transaction })
);
```
!!! Note
    Types such as `Keypair` and `SubmitTransactionRequest` can be imported by the package `pchain-types-js`. E.g.
    ```js
    const { Keypair, SubmitTransactionRequest } = require("pchain-types-js");
    ```

    In case you need free tokens for submitting transactions to Testnet, check out the [Faucet Service](../../fundamentals/networks.md#faucet-service) for details.

**Request**

```ts
class SubmitTransactionRequest {
    transaction: Transaction,
}
```

**Response**

```ts
class SubmitTransactionResponse {
    error: Option<SubmitTransactionError>,
}
```

Where `SubmitTransactionError`:
```ts
enum SubmitTransactionError {
    UnacceptableNonce = 0,
    MempoolFull = 1,
    Other = 2
}
```

---

### transaction

Get a transaction and optionally its receipt.


```js
const response = await client.transaction(
    new TransactionRequest({ transaction_hash, include_receipt })
);
```

**Request**

```ts
class TransactionRequest {    
    transaction_hash: Sha256Hash;
    include_receipt: boolean;
}
```

**Response**

```ts
class TransactionResponse {
    transaction: Option<Transaction>;
    receipt: Option<Receipt>;
    block_hash: Option<Sha256Hash>;
    position: Option<u32>;
}
```

---


### transaction_position

Find out where a transaction is in the blockchain.

```js
const response = await client.transaction_position(
    new TransactionPositionRequest({ transaction_hash })
);
```

**Request**

```ts
class TransactionPositionRequest {
    transaction_hash: Sha256Hash,
}
```

**Response**

```ts
class TransactionPositionResponse {
    transaction_hash: Option<Sha256Hash>;
    block: Option<Sha256Hash>;
    position: Option<u32>;
}
```

---


### receipt

Get a transaction's receipt.

```js
const response = await client.receipt(
    new ReceiptRequest({ transaction_hash })
);
```

**Request**

```ts
class ReceiptRequest {    
    transaction_hash: Sha256Hash;
}
```

**Response**

```ts
class ReceiptResponse {
    transaction_hash: Sha256Hash;
    receipt: Option<Receipt>;
    block_hash: Option<Sha256Hash>;
    position: Option<u32>;
}
```

## Block Related RPCs

### block

Get a block by its block hash.

```js
const response = await client.block(
    new BlockRequest({ block_hash })
);
```

**Request**

```ts
class BlockRequest {
    block_hash: Sha256Hash;
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

```js
const response = await client.block_header(
    new BlockHeaderRequest({ block_hash })
);
```

**Request**

```ts
class BlockHeaderRequest {
    block_hash: Sha256Hash;
}
```

**Response**

```ts
class BlockHeaderResponse {
    block_header: Option<BlockHeader>,
}
```

---

### block_height_by_hash

Get the height of the block with a given block hash. 

```js
const response = await client.block_height_by_hash(
    new BlockHeightByHashRequest({ block_hash })
);
```

**Request**

```ts
class BlockHeightByHashRequest {
    block_hash: Sha256Hash;
}
```

**Response**

```ts
class BlockHeightByHashResponse {
    block_hash: Sha256Hash;
    block_height: Option<u64>;
}
```

---

### block_hash_by_height

Get the hash of a block at a given height.

```js
const response = await client.block_hash_by_height(
    new BlockHashByHeightRequest({ block_height })
);
```

**Request**

```ts
class BlockHashByHeightRequest {
    block_height: u64;
}
```

**Response**

```ts
class BlockHashByHeightResponse {
    block_height: u64;
    block_hash: Option<Sha256Hash>;
}
```

---

### highest_committed_block

Get the hash of the highest committed block. 

```js
const response = await client.highest_committed_block();
```

**Request**

None.

**Response**

```ts
class HighestCommittedBlockResponse {
    block_hash: Option<Sha256Hash>;
}
```

## State Related RPCs

State RPCs return multiple entities in the world state in a single response. This allows clients to get a consistent snapshot of the world state in a single call.

Every response structure includes the hash of the highest committed block when the snapshot is taken.

Some of the following RPCs' response structures reference types unique to this document. These are specified in [types referenced in state RPC responses](#types-referenced-in-state-rpc-responses).

---

### state

Get the state of a set of accounts (optionally including their contract code), and/or a set of storage tuples.

```js
const response = await client.state(
    new StateRequest({ accounts, include_contract, storage_keys })
);
```

**Request**

```ts
class StateRequest {
    accounts: Set<PublicAddress>;
    include_contract: boolean;
    storage_keys: Map<PublicAddress, Set<Uint8Array>>;
}
```

**Response**

```ts
class StateResponse {
    accounts: Map<PublicAddress, Account>;
    storage_tuples: Map<PublicAddress, Map<Uint8Array, Uint8Array>>;
    block_hash: Sha256Hash;
}
```

---

### validator_sets

Get the previous, current, and next validator sets, optionally including the stakes delegated to them.  

```js
const response = await client.validator_sets(
    new ValidatorSetsRequest({
        include_prev,
        include_prev_delegators,
        include_curr,
        include_curr_delegators,
        include_next,
        include_next_delegators
    })
);
```

**Request**

```ts
class ValidatorSetsRequest {
    include_prev: boolean;
    include_prev_delegators: boolean;
    include_curr: boolean;
    include_curr_delegators: boolean;
    include_next: boolean;
    include_next_delegators: boolean;
}
```

**Response**

```ts
class ValidatorSetsResponse {
    // The inner Option is None if we are at Epoch 0.
    prev_validator_set: Option<Option<ValidatorSet>>;
    curr_validator_set: Option<ValidatorSet>;
    next_validator_set: Option<ValidatorSet>;
    block_hash: Sha256Hash;
}
```

---


### pools

Get a set of pools.

```js
const response = await client.pools(
    new PoolsRequest({ operators, include_stakes })
);
```

**Request**

```ts
class PoolsRequest {
    operators: Set<PublicAddress>;
    include_stakes: boolean;
}
```

**Response**

```ts
class PoolsResponse {
    pools: Map<PublicAddress, Option<Pool>>;
    block_hash: Sha256Hash;
}
```

---

### deposits

Get a set of deposits.

```js
const response = await client.deposits(
    new DepositsRequest({ stakes })
);
```

**Request**

```ts
class DepositsRequest {
    stakes: Set<[PublicAddress, PublicAddress]>;
}
```

**Response**

```ts
class DepositsResponse {
    deposits: Map<[PublicAddress, PublicAddress], Option<Deposit>>;
    block_hash: Sha256Hash;
}
```

---

### stakes

Get a set of stakes.

```js
const response = await client.stakes(
    new StakesRequest({ stakes })
);
```

**Request**

```ts
class StakesRequest {
    stakes: Set<[PublicAddress, PublicAddress]>;
}
```

**Response**

```ts
class StakesResponse {
    stakes: Map<[PublicAddress, PublicAddress], Option<Stake>>;
    block_hash: Sha256Hash;
}
```

---


### view

Call a method in a contract in a read-only way.

```js
const response = await client.view(
    new ViewRequest({ target, method, args })
);
```

**Request**

```ts
class ViewRequest {
    target: PublicAddress;
    method: Uint8Array;
    args: Option<Uint8Array[]>;
}
```

**Response**

```ts
class ViewResponse {
    receipt: CommandReceipt;
}
```

---


## Types Referenced in State RPC Responses

### Account-related types

```ts
class Account extends Enum {
  accountWithContract?: AccountWithContract;
  accountWithoutContract?: AccountWithoutContract;
}

class AccountWithContract {
    nonce: u64;
    balance: u64;
    contract: Option<Uint8Array>;
    cbi_version: Option<u32>;
    storage_hash: Option<Sha256Hash>;
}

class AccountWithoutContract {
    nonce: u64;
    balance: u64;
    cbi_version: Option<u32>;
    storage_hash: Option<Sha256Hash>;
}
```

### Staking-related types 

```ts
class ValidatorSet extends Enum {
    poolsWithDelegator?: PoolWithDelegator[];
    poolsWithoutDelegator?: PoolWithoutDelegator[];
}

class PoolWithDelegators {
    operator: PublicAddress;
    power: u64;
    commission_rate: number;
    operator_stake: Option<Stake>;
    delegated_stakes: Stake[];
}

class PoolWithoutDelegators {
    operator: PublicAddress;
    power: u64;
    commission_rate: number;
    operator_stake: Option<Stake>;
}

class Deposit {
    owner: PublicAddress;
    balance: u64;
    auto_stake_rewards: boolean;
}

class Stake {
    owner: PublicAddress;
    power: u64;
}

class Pool extends Enum {
    poolWithDelegator?: PoolWithDelegator;
    poolWithoutDelegator?: PoolWithoutDelegator;
}
```