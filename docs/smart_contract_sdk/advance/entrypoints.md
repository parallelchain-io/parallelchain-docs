---
tags:
  - pchain-sdk
  - Smart Contract
  - Contract Methods
---

In ParallelChain Smart Contract Programming Model, a contract is like a `Rust` `struct` that controls access to persistent storage. Accounts can interact with contracts by submitting transactions that include a call command to invoke methods of the contract. These methods are sometimes called just "methods" for short.

### Methods

The model defines Contract Methods as methods with the `#[call]` macro. Each of these corresponds to a method that can be called via a Call Command in the CBI Subprotocol. To allow methods to be called from the outside world, you must write method definitions within an `impl Contract` statement marked with the `#[contract_methods]` macro. The following example illustrates this:

```rust
#[contract_methods]
impl PrinceTheDog {

    #[call]
    pub fn eat_food(&mut self, food: DogFood) -> Poop {
        // ...
    }
}
```

Methods can change Contract Storage. However, mutations made in a Call Transaction only apply if the transaction is successful. This means that the transaction must have enough gas, not have panicked during execution, and so on.

A function can be called an Action Method only if:

The `#[call]` macro is added above the function declaration.
Its arguments (if any) implement `BorshDeserialize`.
Its return value (if any) implements `BorshSerialize`.

### Accepting parameters and returning values

Some of the code snippets provided in this document show Contract Methods that accept arguments (in addition to borrow of the Contract struct) and/or return a value. To enable a contract to receive arguments from and return values to callers, both the contract and the caller need to agree on a serialization format.

The `pchain-sdk` expects callers to serialize method arguments using the Borsh serialization standard. It generates code to serialize values into borsh for inclusion in a Transaction's Receipt. To be precise, a Call Transaction specifies the Contract Method to call and provide the arguments for the call by including a borsh-serialized data structure `Option<Vec<Vec<u8>>` in its `arguments` field, and then returning a borsh-serialized value (if any).