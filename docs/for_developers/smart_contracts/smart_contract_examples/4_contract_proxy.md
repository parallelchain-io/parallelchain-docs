---
tags:
  - pchain-sdk
  - tutorial
  - Smart Contract
---

# Example - Contract Proxy

We will look at how the contract `ContractProxy` use the `use_contract` macro to interact with other contracts. In general, `ContractProxy` is a contract that serves as a middle-man to another contract [My Little Pony](2_my_little_pony.md). 

There are two kinds of interaction between the two contracts:

- calling another contract's entrypoint methods.
- sending tokens from the balance of one contract to another contract.

To call another contract, firstly use the macro `use_contract` to specify the contract entrypoint methods in a trait. The address is hard-coded when using this macro. It is recommended to remove or comment out  the methods that are not intended to be used. 

Recall that we have a deployed contract called `MyLittlePony` that consists of three methods,
`self_introduction()`, `grow_up()`, and `change_person()`. We are going to use `grow_up()` in 
`ContractProxy`, so we can comment out the rest of them. 

!!! Pre-requisites
    Deploy [My Little Pony](./2_my_little_pony.md) smart contract, and then replace the address supplied to `use_contract` macro with the smart contract address of My Little Pony.

### lib.rs: define a trait
```rust
use pchain_sdk::{
    use_contract, call, contract, contract_methods
};

#[use_contract("xxT5OXMonFm8wcDw-io1jeyAEnxSddGPOG-ZezArCV4")]
pub trait MyLittlePony {
    //fn self_introduction() -> String;
    fn grow_up();
    //fn change_person(name: String, age: u32, gender_name: String, description: String);
}
```

The trait will be transformed to mod by using the macro `use_contract`, Calling the contract `MyLittlePony`
can be simply calling the methods in the trait as [associated functions](https://doc.rust-lang.org/rust-by-example/fn/methods.html). For example, one way of calling 
`grow_up()` is to simply do `my_little_pony::grow_up(0)`.

After defining the trait that specifies the other contract, we start implementing the functionalities of our contract `ContractProxy`.

### lib.rs: use_contract macro
```rust
#[contract]
pub struct ContractProxy {}

#[contract_methods]
impl ContractProxy {

    #[call]
    fn grow_up() {
        my_little_pony::grow_up(0);
    }
}
```

The above example has shown how we can use the `use_contract` macro to do [cross-contract calls](../advanced/cross_contract_call.md). It is also possible
to use `pchain_sdk::call_untyped()` to do so. We pass the contract address as an argument so that the contract
address does not need to be hard-coded in the contract.

!!! note
    Add `base64url = "0.1.0"` under `[Dependency]` in `Cargo.toml` for the project.

### lib.rs: pchain_sdk::call_untyped()
```rust
#[call]
fn grow_up_2(address: String) {
    let contract_address = base64url::decode(&address).unwrap().try_into().unwrap();
    pchain_sdk::call_untyped(
        contract_address,
        "grow_up", 
        Vec::new(),
        0
    );
}
```

### lib.rs: transfer contract balance
Except by calling the entrypoint methods from `MyLittlePony`, we can also transfer the balance from the 
contract to a specific address by `pchain_sdk::transfer()` (see [Transferring Balance](../advanced/transferring_balance.md)).

```rust
#[call]
fn send_tokens(to_address: String, value :u64){
    let contract_address = base64url::decode(&to_address).unwrap().try_into().unwrap();
    pchain_sdk::transfer(
        contract_address,
        value
    );
}

```