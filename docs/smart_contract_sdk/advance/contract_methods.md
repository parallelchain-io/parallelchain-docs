---
tags:
  - pchain-sdk
  - Smart Contract
  - Contract Methods
---

In ParallelChain Smart Contract Programming Model, a contract is like a `Rust` `struct` that controls access to persistent storage. Accounts can interact with contracts by submitting transactions that include a call command to invoke methods of the contract. These methods are sometimes called just "methods" for short.

## Contract Methods
---

The Model defines methods with macro `#[call]` as Contract Methods, each corresponding to a method that is callable to a Call Command in the CBI Subprotocol.

In order to produce to appropriate CBI Exports Set bindings that ultimately allow Methods to be called from the outside world, you must write Method definitions inside an `impl Contract` statement marked with the `#[contract_methods]` macro, as illustrated in the following examples.

```rust
#[contract_methods]
impl PrinceTheDog {
    #[call]
    pub fn eat_food(&mut self, food: DogFood) {
        ...
    }
}
```

Methods may mutate Contract Storage. Note however, that (as specified in the Transaction Subprotocol) mutations to Contract Storage made in a Call Transaction only get applied if the Transaction is Successful (e.g., the Transaction must exit with sufficient gas, must have not panicked during execution, etc.).

A function can be called if and only if:

1. The macro #[call] is added above the function declaration.
2. Its (zero or more) other arguments implement BorshDeserialize.
3. Its return value implements BorshSerialize, or it does not have a return value.


## Accepting Parameters and Returning Values
---

Some of the code snippets provided as examples in this document depict Contract Methods that take in function arguments (besides a borrow of the Contract struct) and/or return a value. In order for a Contract to receive arguments from and return values to the 'outside world' (callers), both Contract and caller need to agree on a serialization format.

`pchain-sdk` expects callers to serialize Method arguments using the [borsh](https://crates.io/crates/borsh) serialization standard, and generates code to serialize values into borsh for inclusion in a Transaction's Receipt. To be precise, Transaction Command Call specify the Contract Method to call and provide the arguments for the call by including a borsh-serialized data structure `Option<Vec<Vec<u8>>>` in its `arguments` field, and contracts include a borsh-serialized `ContractMethodOutput` struct. The former type is defined in `pchain-types`, while the latter is defined in `pchain-sdk`. In the future, we plan to move both into the SDK.