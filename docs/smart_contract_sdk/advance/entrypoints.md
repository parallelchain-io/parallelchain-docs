---
tags:
  - testnet 2.0
  - parallelchain sdk
  - smart contract
  - entrypoints
---

# Entrypoints

The section [Develop Contract](../develop_contract.md) describes various type entrypoint methods: `init`, `action` and `view`.

Here is to illustrate the details by example smart contracts [my_bank](https://github.com/parallelchain-io/example-smart-contracts) and [my_little_pony](https://github.com/parallelchain-io/example-smart-contracts)

## Init
---

Init entrypoint method is optional in the contract. It is enabled if the contract defines a constructor in contract Impl.

```rust
  /// ### Lesson 4:
  /// This method is `init` method that will be execution during contract deployment process
  #[init]
  fn new(name: String, age: u32 ) {
      Transaction::emit_event(
          "Init Contract".to_string().as_bytes(),
          format!("{} at age{} was born.", name, age).as_bytes()
      );
      MyLittlePony {
          name,
          age,
          gender: Gender { 
              name: String::default(), 
              description: String::default()
          }
      }.set(); // this setter applies to all fields in whole struct
  }
```

The init entrypoint methods are recognized in the same way of actions entrypoint methods except that

- macro init is applied on the method
- must be associate method (no recevier, i.e. self, as method argument)
- there should be only one init entrypoint method

## Action
---

Action entrypoint method is __required__ in a contract as it defines the body of execution of a contract. The methods enjoy full power of operations (mutatble and immutable) in the blockchain.

```rust
/// ### Lesson 2:
/// The macro `contract` generates entrypoint methods that can be called in transaction
#[contract]
impl MyBank {

    /// entrypoint method "open_account"
    #[action]
    fn open_account( ...

    /// entrypoint method "query_account_balance"
    #[action]
    fn query_account_balance( ...

    /// entrypoint method "withdraw_money"
    #[action]
    fn withdraw_money( ...

```

Multiple action entrypoint methods can be defined so that contract caller can choose specific function of a contract in easy way.


## View

View entrypoint is optional in the contract. It is enabled by applying macro `view` on a method inside contract impl.

```rust
  /// ### Lesson 8:
  /// View entrypoint method provides cost-free execution of a contract.
  /// View methods are limited by allowing only execution of getting world-state data.
  #[view]
  fn age() -> u32 {
      Self::get_age()
      // This will cause panic:
      // Self::set_name("my name".to_string())
  }
```

The view entrypoint methods are recognized in the same way of actions entrypoint methods except that

- macro view is applied on the method
- must be associate method (no recevier, i.e. self, as method argument)

__note__:

Execution a View entrypoint is supposed to be an immutable operation. View method is limited by allowing using only basic functions: 

- Transaction::get
- Transaction::return_value

It will cause panic if other functions (e.g. Transaction::set) are included in view entrypoint method. 
