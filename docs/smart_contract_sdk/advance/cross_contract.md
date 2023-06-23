---
tags:
  - pchain-sdk
  - Smart Contract
  - Cross Contract Call
---

# Cross Contract Call

The SDK includes two functions to make Contract-to-Contract calls: `call` and `call_untyped`, the former calls a method in a specified Contract with the given arguments. Here is to illustrate the details by example of smart contract "ContractProxy".


## Call Contract by Using Trait
---

SDK provides developer-friendly ways to call another contract by using macro `use_contract`. The address of the contract is specified inside the attribute list in the macro. 

```rust
/// ### Lesson 1:
/// Use macro `use_contract` to specify the contract action entrypoint methods in a trait.
/// The address is hard-coded when using the macro `use_contract`.
/// It is recommended to remove/comment out the methods that are not intended to be used.
#[use_contract("-jUt6jrEfMRD1JM9n6_yAASl2cwsc4tg1Bqp07gvQpU")]
pub trait MyLittlePony {
    //fn self_introduction() -> String;
    fn grow_up();
    //fn change_person(name :String, age :u32, gender_name :String, description :String);
}
```

In the example above, module `my_little_pony` is generated which name which is the trait name in a snake case format. Associate methods are generated inside the module for being used to make cross-contract calls.

```rust
/// ### Lesson 2:
/// The trait will be transformed to mod by using macro `use_contract`. 
/// Calling the contract `MyLittlePony` can be simply calling associate methods according to the defined method in the trait.
/// Value and Gas will be needed in cross contract call
#[call]
fn grow_up() {
    my_little_pony::grow_up(0, 120000);
}
```

## Call Contract by Using Function
---

Cross-contract calls can also be simply made by using the function `pchain_sdk::call` or `pchain_sdk::call_untyped`.

```rust
/// ### Lesson 3:
/// It is also possible to use call_untyped() instead of macro `use_contract` to make a cross contract call.
/// Address can also be passed as an argument so that the contract address is not necessarily hard-coded.
#[call]
fn grow_up_2() {
    let contract_address = Base64URL::decode("-jUt6jrEfMRD1JM9n6_yAASl2cwsc4tg1Bqp07gvQpU").unwrap().try_into().unwrap();
    pchain_sdk::call_untyped(
        contract_address,
        "grow_up", 
        Vec::new(),
        0);
}
```

Different from using [Trait](#call-contract-by-using-trait), it no longer requires the address to be hard coded as it can be passed from method arguments.