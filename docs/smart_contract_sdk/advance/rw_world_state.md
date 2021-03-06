---
tags:
  - testnet 2.0
  - parallelchain sdk
  - smart contract
  - world-state
---


# Read/Write of World-State

Other than using basic functions `Transaction::set` and `Transaction::get`, SDK provides developer-friendly ways to intract with data in World State. Developer can derive trait [ContractField](#nested-struct) to support nested fields inside a contract, so that the key is canonical indexed as vector of u8.

[Getter and Setter](#getter-and-setter) methods are generated by SDK to ease to access to data in World Wtate.

The following section takes the smart contract [my_little_pony](https://github.com/parallelchain-io/example-smart-contracts) on ParallelChain Lab's Github as example.

## Nested struct
---

Nested struct is supported by deriving macro trait named ContractField. The nested struct also follows key-value conditions as same as Contract struct.

```rust
/// ### Lesson 1:
/// The macro `contract` on struct allow loading/storing fields from/into world state.
/// The key to be store is u8 integer ordered by the order of the fields. E.g. `name` has key [0] while `age` has key [1]
#[contract]
pub struct MyLittlePony {
    name: String,
    age: u32,
    gender: Gender,
}

/// ### Lesson 2:
/// The contract field can be used in contract struct so that the key-value pair can be accessed in canonical format.
/// For example, `name` in Gender has a key [2][0] for contract `MyLittlePony`.
#[derive(ContractField)]
struct Gender {
    name: String,
    description: String
}
```

In the above example, the key for storing in world-state will be as below:

| field | key (bytes) |
|:---|:---|
| name | [0] |
| age | [1] |
| gender.name | [2, 0] |
| gender.description | [2, 1] |


## Getter and Setter
---

SDK provides convenience to access data in Contract Storage by creating getter and setting methods of fields defined in the contract struct.
If field name is data, then associate methods get_data and set_data could be called to obtain data stored in the Contract Storage.

```rust
    /// ### Lesson 6:
    /// This method use contract getter and setter to store the updated data to field `data` to world state. 
    /// Write cost is small because there is only one key-value pair in world state to be mutated.
    #[action]
    fn grow_up() {
        let age = Self::get_age();
        Self::set_age(age+1)
    }
```

__Note__: Getter and setter methods are not generated for Contract Field. Getting and setting this field will always load/save all data from/to Contract Storage. E.g. 
```rust
// save `name` and `description` in this operation
Self::set_gender(Gender{ name: "name".to_string(), description: "description".to_string()});
```