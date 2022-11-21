---
tags:
  - testnet 3
  - parallelchain sdk
  - smart contract
  - cross contract call
---


# Cross Contract Call

The SDK includes two pairs of functions to make CtoC (contract-to-contract) internal calls:
- `call_action` and `call_action_untyped`, and
- `call_view` and `call_view_untyped`.

Each pair does the obvious: the former calls an Action method in a specified Contract with the given arguments, the latter does the same with View methods.

Here is to illustrate the details by example smart contracts "ContractProxy" in [chapter_3](https://github.com/parallelchain-io/example-smart-contracts) of repository of Parallelchain Lab.


## Call Contract by using Trait

SDK provides developer-friendly ways to call another contract by using macro `use_contract`. Address of the contract is specified inside the attribute list in the macro. 

```rust
/// ### Lesson 1:
/// use macro `use_contract` to specify the contract action entrypoint methods in a trait.
/// The address is hard-coded when using macro `use_contract`.
/// It is recommended to remove/comment out the methods that are not intended to be used.
#[use_contract("-jUt6jrEfMRD1JM9n6_yAASl2cwsc4tg1Bqp07gvQpU")]
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

Cross contract call can also be simply made by using function `pchain_sdk::call_action` or `pchain_sdk::call_action_untyped`.

```rust
/// ### Lesson 3:
/// It is also possible to use call_action_untyped() instead of macro `use_contract` to make a cross contract call.
/// Address can also be passed as argument so that contract address is not necessary hard-coded.
#[action]
fn grow_up_2() {
    let contract_address = pchain_types::Base64URL::decode("-jUt6jrEfMRD1JM9n6_yAASl2cwsc4tg1Bqp07gvQpU").unwrap().try_into().unwrap();
    pchain_sdk::call_action_untyped(
        contract_address,
        "grow_up", 
        Vec::new(),
        0);
}
```

Different from using [Trait](#call-contract-by-using-trait), it nolonger requires address to be hardcoded as it can be passed from method arguments.