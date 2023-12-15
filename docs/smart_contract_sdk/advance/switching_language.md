---
tags:
  - pchain-sdk
  - Smart Contract
  - Solidity
  - Rust
---

# Switching from Solidity to Rust

todo. 

To illustrate how to switch from writing a Solidity smart contract to a Rust smart contract, an example smart contract in Solidity is obtained from [Solidity Documentation](https://docs.soliditylang.org/en/latest/introduction-to-smart-contracts.html#subcurrency-example). The aim of this contract is to create a coins that can be sent and minted. 

## Define Contract
In Solidity, `contract Coin { ... }` is a collection of its functions and its state, as well as specifying the name of the contract. 

We can do the same in Rust with the help of `pchain_sdk`.
```rust
// The macro `contract` defines the basic struct of the contract,
// the fields are the data representation of the contract storage.
#[contract]
pub struct Coin { ... }

// The macro `contract_methods` defines the impl for the contract sturct. 
// The public methods declared in impl will be callable by the Transaction command.
#[contract_methods]
impl Coin { ... }
```
## Define Contract Struct

In this example, we will need an address
that acts as the owner of the contract, we can call it `minter`. We also need table to store the corresponding balances of each address.

We can see from the example contract that both states and functions will be declared inside `contract`. We can simply add the two fields in:

```solidity
contract Coin {
    address public minter;
    mapping(address => uint) public balances;
}
```

In Rust, we will place the fields inside the `struct Coin`. 
```rust
use pchain_types::cryptography::PublicAddress;
use pchain_sdk::collections::FastMap;

#[contract]
pub struct Coin {
    // Instead of using `address`, the type of address we used will be in the type of
    // pchain_types::cryptography::PublicAddress, ie. [u8;32].
    minter: PublicAddress,
    // `FastMap` is a data structure defined in pchain_sdk::collections that supports
    // lazy read/ write on key-value tuples.
    balances: FastMap<PublicAddress, u64>,
}
```

!!!info 
    You can find more information on data structures provided by `pchain_sdk` [here](https://github.com/parallelchain-io/pchain-sdk/tree/main/src/collections).

## Initializing Contract

Sometimes, a contract requires some initalization before using it. In Solidity, the `constructor` keyword is usually used for initializing state variables of a contract such as setting `msg.sender` as `minter`. 

However, a `constructor` function is not available in Rust, so you can name the constructor function as anything you want. In the example, `init()` is the function we used to initialize the variable `minter` as the address that calls this function. Unlike the constructor in Solidity smart contract, this function can be called more than once, and someone else can change the values of the variables when it should only be done by the owner. 

There are two ways to prevent someone else other than the designated address calling the initilize function. 

1. Instead of setting `minter` as the address that call the function, we can set it as a hardcoded address, so the `minter` cannot be set to other custom addresses.

2. Add a boolean variable in `struct Coin` to indicate if the initialize function is called before.  


```rust
#[contract_methods]
impl Coin { 
    
    #[call]
    fn init(&mut self) {
        assert!(!Self::get_initialized());
        Self::set_minter(pchain_sdk::transaction::calling_account());
        Self::set_initialized(true);
    }
}
```

## Functions Implementation

In this section, we will be implemented the two required functions, `mint()` and `send()`.

This is the Solidity implementation of `mint()`:

```solidity
function mint(address receiver, uint amount) public {
    require(msg.sender == minter);
    balances[receiver] += amount;
}
```

1. To specify the conditions for executing the function, we can use `assert_eq!()` or `assert!()` in Rust, which are equivalent to `require()` in Solidity. 

2. In Rust, we cannot simply access the balance with `balances[receiver]`. Instead, do `self.balances.get(&receiver)` or `self.balances.get_mut(&receiver)` if the obtained value will be modified.

```rust
#[call]
pub fn mint(&mut self, receiver: PublicAddress, amount: u64) {
    assert_eq!(Self::get_minter(), pchain_sdk::transaction::calling_account());
    
    match self.balances.get_mut(&receiver) {
        Some(balance) => *balance += amount,
        None => self.balances.insert(&receiver, amount)
    } 
}
```
!!!info 
    You can find explanation and example usage of `pchain_sdk::collection::FastMap` [here](https://github.com/parallelchain-io/pchain-sdk/blob/main/src/collections/fast_map.rs).

This is how we can implement `send()` in Solidity:

```solidity
error InsufficientBalance(uint requested, uint available);

function send(address receiver, uint amount) public {
    if (amount > balances[msg.sender])
        revert InsufficientBalance({
            requested: amount,
            available: balances[msg.sender]
        });

    balances[msg.sender] -= amount;
    balances[receiver] += amount;
}
```

`revert` in Solidity allow us to return a value, such as error message or number corresponding to an error type. It also refund any remaining gas to the caller if the condition fails.

```rust 
#[call]
pub fn send(&mut self, receiver: PublicAddress, amount: u64) {
    let sender_balance = self.balances.get(&pchain_sdk::transaction::calling_account());
    assert!(sender_balance.is_some());
    assert!(sender_balance.unwrap() > amount);

    match self.balances.get_mut(&receiver) {
        Some(balance) => *balance += amount,
        None => self.balances.insert(&receiver, amount)
    };

    let sender_balance = self.balances.get_mut(&pchain_sdk::transaction::calling_account()).unwrap();
    *sender_balance += amount;
}
```

## Logging and Events



```solidity
event Sent(address from, address to, uint amount);

function send(address receiver, uint amount) public {
    ...
    emit Sent(msg.sender, receiver, amount);
}
```







```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.4;

contract Coin {
    // The keyword "public" makes variables
    // accessible from other contracts
    address public minter;
    mapping(address => uint) public balances;

    // Events allow clients to react to specific
    // contract changes you declare
    event Sent(address from, address to, uint amount);

    // Constructor code is only run when the contract
    // is created
    constructor() {
        minter = msg.sender;
    }

    // Sends an amount of newly created coins to an address
    // Can only be called by the contract creator
    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        balances[receiver] += amount;
    }

    // Errors allow you to provide information about
    // why an operation failed. They are returned
    // to the caller of the function.
    error InsufficientBalance(uint requested, uint available);

    // Sends an amount of existing coins
    // from any caller to an address
    function send(address receiver, uint amount) public {
        if (amount > balances[msg.sender])
            revert InsufficientBalance({
                requested: amount,
                available: balances[msg.sender]
            });

        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```