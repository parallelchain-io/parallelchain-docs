---
tags:
  - pchain-sdk
  - tutorial
  - Smart Contract
  - Staking
---

# Chapter 6 - Network Commands

In the last chapter of the tutorial, we are going to talk about how to use network commands in smart contracts. 

In ParallelChain Mainnet, there are six different network commands:
- Create deposit
- Set deposit settings
- Top-up deposit
- Withdraw deposit
- Stake deposit
- Unstake deposit

We will demonstrate how the above network commands can be created and sent to 
the network through the use of a smart contract. We will use the contract, `MyPool` 
to guide you through the steps of creating a stake in a pool with the network commands.

### lib.rs: define a struct
```rust
use pchain_sdk::{
    call, contract, contract_methods
};

type Address = [u8;32];

#[contract]
pub struct MyPool {
    pool_operator: Address,
    my_friend: Address
}

```
We have created the struct `MyPool`, which consists of the addresses of the `pool_operator` and `my_friend`. However, as mentioned in previous chapters, it will be initialized to 0 upon deployment. Therefore, we have to add an init function to initialize the address
to the values we specified.

### lib.rs: initialise struct
```rust
#[contract_methods]
impl MyPool {

    #[call]
    fn init(pool_operator: Address, my_friend: Address){
        MyPool { pool_operator, my_friend }.set();
    } 
    
}
```

---

After adding the `init()` function, we can try creating a deposit into the pool. `pchain_sdk::network:defer_create_deposit()` allows us to deposit some XPLL into a specified pool. In this contract, we have already specified the pool in the field `pool_operator`.


The network commands are "deferred" because the actual execution of such commands occurs after the execution of a successful call. 
> **Note**: 
> The deposit is created on behalf of the contract address, not from your account address, so make sure to transfer
sufficient balance to the contract for the operation.


To check if the deposit is successful, you can check the deposit using `pchain-client` with the following command:
> ./pchain_client query deposit --operator <OPERATOR_ADDRESS> --owner <CONTRACT_ADDRESS>

### lib.rs: successful network command
```rust
    #[call]
    fn create_deposit(balance: u64, auto_stake_rewards: bool) {
        pchain_sdk::network::defer_create_deposit(Self::get_pool_operator(), balance, auto_stake_rewards)
    }
```

It was mentioned above that the `defer` call will only take place after a successful call. Here, we are making the transaction fail deliberately by transferring more than what we have in balance. As a result, the transaction call will fail, and the stake should not be deposited. 

Check the deposit again using `pchain-client`, the deposit balance in the pool should remain unchanged.

### lib.rs: failed network command

```rust
    #[call]
    fn transfer_too_much() {
        let balance = pchain_sdk::blockchain::balance();
        pchain_sdk::transfer(Self::get_my_friend(), balance + 1);
        pchain_sdk::network::defer_stake_deposit(Self::get_pool_operator(), balance);
    }
```

---

Another characteristic of the network commands is that the return value in the transaction receipt will be
overwritten by the deferred staking commands. 

In the method `stake_deposit()`, we should be expecting that the `return_values` in the receipt will be the 
balance of the contract. However, since there is a defer network command in the method call, the `return_values`
will be overwritten by the return value from the `pchain_sdk::network::defer_staking_deposit()` command.

### lib.rs: overwriting return value
```rust
    #[call]
    fn stake_deposit(max_amount: u64) -> u64{
        pchain_sdk::network::defer_stake_deposit(Self::get_pool_operator(), max_amount);
        pchain_sdk::blockchain::balance()
    }
```

---

Lastly, we can include multiple network commands within one transaction. In this `multiple_defer()` function,
we put the commands for unstaking and withdrawing deposits in the same function.

Both the commands will be executed after the success of the transaction, and in the order they were called
in the method call. After invoking this method call, the stake of the deposit should have been successfully withdrawn.

### lib.rs: multiple network commands
```rust
    #[call]
    fn multiple_defer(max_amount: u64) {
        let operator = Self::get_pool_operator();
        pchain_sdk::network::defer_unstake_deposit(operator, max_amount);
        pchain_sdk::network::defer_withdraw_deposit(operator, max_amount);
    }

```

---

**Congratulations! You have completed all tutorials and are ready to write your smart contract!**