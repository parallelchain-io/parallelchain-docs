---
tags:
  - testnet 2.0
  - parallelchain sdk
  - smart contract
  - cross contract call
---

# Cross Contract Call

Cross Contract Call refers to triggering execution of another contract from a contract. The execution is similar to an [EtoC Transaction](../../getting_started/call_contract.md) except that:

- Tokens being transferred are from the Contract instead of an External Owned Account.
- Gas is consumed in External Owned Account
- No transaction receipt

Moreover, 

- Events emitted from the contract being called are included in the transaction receipt of the EtoC Transaction.
- All changes (include data change in world state) are effective only after success of the entire transaction.

Here is to illustrate the details by example smart contracts [my_little_pony](https://github.com/parallelchain-io/example-smart-contracts) and [contract_proxy](https://github.com/parallelchain-io/example-smart-contracts).


## Call Contract by using Trait

SDK provides developer-friendly ways to call another contract by using macro `use_contract`. Address of the contract is specified inside the attribute list in the macro. 

```rust
/// ### Lesson 1:
/// use macro `use_contract` to specify the contract action entrypoint methods in a trait.
/// The address is hard-coded when using macro `use_contract`.
/// It is recommended to remove/comment out the methods that are not intended to be used.
#[use_contract("b12EJf3vd+UESLj4ZCBVSOqruUAgseQ6zVa3RW2Jcqg=")]
pub trait MyLittlePony {
    //fn self_introduction() -> String;
    fn grow_up();
    //fn change_person(name :String, age :u32, gender_name :String, description :String);
}
```

In the example above, module `my_little_pony` is generated which name which is the trait name in snake case format. Associate methods are generated inside the module for being used to make cross contract call.

```rust
/// ### Lesson 2:
/// The trait will be transformed to mod by using macro `use_contract`. 
/// Calling the contract `MyLittlePony` can be simply calling associate methods according to defined method in the trait.
/// Value and Gas will be needed in cross contract call
#[action]
fn grow_up() {
    my_little_pony::grow_up(0, 120000);
}
```

## Call Contract by using function

Cross contract call can also be simply made by using function `Transaction::call_contract`.

```rust
/// ### Lesson 3:
/// It is also possible to use call_contract() instead of macro `use_contract` to make a cross contract call.
/// Address can also be passed as argument so that contract address is not necessary hard-coded.
#[action]
fn grow_up_2() {
    Transaction::call_contract(
        smart_contract::decode_contract_address("b12EJf3vd+UESLj4ZCBVSOqruUAgseQ6zVa3RW2Jcqg=".to_string()),
        "grow_up", 
        Vec::new(),
        0, 120000);
}
```

Different from using [Trait](#call-contract-by-using-trait), it nolonger requires address to be hardcoded as it can be passed from method arguments.