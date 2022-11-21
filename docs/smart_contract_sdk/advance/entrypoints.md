---
tags:
  - testnet 3
  - parallelchain sdk
  - smart contract
  - contract methods
---

In Parallelchain F Smart Contract Programming Model, there are three kinds of Methods, each corresponding to an 'Entrypoint' in the Contract ABI Subprotocol.

1. **Action Methods**: can mutate Contract Storage. Can only be called through an on-chain, EtoC Transaction.
2. **View Methods**: can read, but not mutate Contract Storage. Can be called through the 'Contract View' endpoint of the Standard HTTP API, as well as through on-chain EtoC Transactions.
3. **Init Methods**: if defined on a Contract, is called *once* during the Contract's Deploy Transaction. In OOP lingo, this can be thought of as the 'constructor' of a Contract.

In order to produce to appropriate Contract ABI Exports Set bindings that ultimately allow Methods to be called from the outside world, you must write Method definitions inside an `impl Contract` statement marked with the `#[contract_methods]` macro, as illustrated in the following examples.


### Action Methods

```rust
#[contract_methods(meta)]
impl PrinceTheDog {

    #[action]
    pub fn eat_food(&mut self, food: DogFood) -> Poop {
        ...
    }
}
```

Actions Methods may mutate Contract Storage. Note however, that (as specified in the Transaction Subprotocol) mutations to Contract Storage made in an EtoC Transaction only get applied if the Transaction is Successful (e.g., the Transaction must exit with sufficient gas, must have not panicked during execution, etc.).

A function is callable as an Action Method if and only if:
1. It is marked using the `#[action]` macro.
2. It takes a `&mut` receiver in its argument list.
3. Its (zero or more) other arguments implement `BorshDeserialize`.
4. Its return value implements `BorshSerialize`, or it does not have a return value.

### View Methods

```rust
#[contract_methods(meta)]
impl PrinceTheDog {

    #[view]
    pub fn nicknames(&self) -> Vec<String> {
        ...
    }
}
```

View Methods have a consistent read-view into Contract Storage. The special thing about View Methods is that they may be called without incurring gas using the Contract View endpoint of the Standard HTTP API. Note however that calling them as part of a Contract Internal Transaction, does incur gas at normal rates.

A function is callable as a View Method if and only if:
1. It is marked using the `#[view]` macro.
2. It takes a `&self` receiver in its argument list.
3. Its (zero or more) other arguments implement `BorshDeserialize`.
4. Its return value implements `BorshSerialize`, or it does not have a return value. 

### Init Methods

```rust
#[contract_methods(meta)]
impl PrinceTheDog {

    #[init]
    fn init(dog_shelter: DogShelter) {
        PrinceTheDog {
            ...
        }.set()
    }
```

The purpose of an Init Method is to initialize a Contract's Storage. A Contract can have zero or one Init Methods defined on it. An Init Method has full access to Contract Storage, just like an Action Method, but unlike an Action Method, it may not take `self` (or borrows of `self`) in its arguments list. The init method should call `.set` on your Contract struct.

A function is callable as an Init Method if and only if: 
1. It is marked using the `#[init]` macro.
2. It does not take `self` or its borrows in its arguments list.
3. Its (zero or more) arguments implement `BorshDeserialize`.

### Accepting parameters and returning values

Some of the code snippets provided as examples in this document depict Contract Methods that take in function arguments (besides a borrow of the Contract struct) and/or return a value. In order for a Contract to receive arguments from and return values to the 'outside world' (callers), both Contract and caller need to agree on a serialization format.

`pchain-sdk` expects callers to serialize Method arguments using the [borsh](https://github.com/near/borsh) serialization standard, and generates code to serialize values into borsh for inclusion in a Transaction's Receipt, or transmission over the wire as part of the response for the Contract View endpoint of the Standard HTTP API. To be precise, EtoC Transactions specify the Contract Method to call and provide the arguments for the call by including a borsh-serialized `CallData` struct in its `data` field, and contracts include a borsh-serialized `CallResult` struct. The former type is defined in `pchain-types`, while the latter is defined in `pchain-sdk`. In the future, we plan to move both into the SDK. 
