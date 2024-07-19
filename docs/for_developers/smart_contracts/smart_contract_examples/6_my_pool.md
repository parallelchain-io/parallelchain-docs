---
tags:
  - pchain-sdk
  - tutorial
  - Smart Contract
  - Staking
---

# Example - My Pool

We are going to study how to use [staking commands](../../../fundamentals/transactions.md#staking-commands) in smart contracts. 

In ParallelChain Mainnet, there are six different staking commands:

- Create deposit
- Set deposit settings
- Top-up deposit
- Withdraw deposit
- Stake deposit
- Unstake deposit

We will demonstrate how the above staking commands can be created within a smart contract 
which acts as owner of the deposit/stakes (See [Staking in Contract](../advanced/staking_in_contract.md)). We will use the contract, `MyPool`, 
to guide you through the steps of creating a stake in a pool.

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
We have created the struct `MyPool`, which consists of the addresses of the `pool_operator` and `my_friend`. These data will be all zeros by default. Therefore, we have to add an initialization function to set the addresses we want.

### lib.rs: initialize struct
```rust
#[contract_methods]
impl MyPool {

    #[call]
    fn init(pool_operator: Address, my_friend: Address){
        MyPool { pool_operator, my_friend }.set();
    } 
    
}
```

After adding the `init()` function, we can try creating a deposit into the pool. `pchain_sdk::network:defer_create_deposit()` allows us to deposit some XPLL into a specified pool. In this contract, we have already specified the pool in the field `pool_operator`.


The staking commands are "deferred" because the actual execution of such commands occurs after the execution of a successful call. 

!!! Note 
    The deposit is created on behalf of the contract address, not from your account address, so make sure to transfer sufficient balance to the contract for the operation.


To check if the deposit is successful, you can check the deposit using `pchain-client` with the following [command](../../../for_users/pchain_client_cli/querying_blockchain.md#get-deposit-and-stake):
```sh
./pchain_client query deposit --operator <OPERATOR_ADDRESS> --owner <CONTRACT_ADDRESS>
```

### lib.rs: successful staking command
```rust
    #[call]
    fn create_deposit(balance: u64, auto_stake_rewards: bool) {
        pchain_sdk::network::defer_create_deposit(Self::get_pool_operator(), balance, auto_stake_rewards)
    }
```

It was mentioned above that the `defer` call will only take place after a successful call. Here, we are making the transaction fail deliberately by transferring more than what we have in balance. As a result, the transaction call will fail, and the stake should not be deposited. 

Check the deposit again using `pchain-client`, the deposit balance in the pool should remain unchanged.

### lib.rs: failed staking command

```rust
    #[call]
    fn transfer_too_much() {
        let balance = pchain_sdk::blockchain::balance();
        pchain_sdk::transfer(Self::get_my_friend(), balance + 1);
        pchain_sdk::network::defer_stake_deposit(Self::get_pool_operator(), balance);
    }
```

!!! Note
    The [return values](../../../fundamentals/transactions.md#receipt-and-logs) in the transaction receipt will be overwritten by the deferred staking commands. 

In the method `stake_deposit()`, we should be expecting that the `return_values` in the receipt will be the 
balance of the contract (see [ParallelChain Mainnet Protocol](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/Runtime.md)). However, since there is another deferred staking command in the method call, the `return_values`
will be overwritten by the result of executing `pchain_sdk::network::defer_staking_deposit()` command.

### lib.rs: overwriting return value
```rust
    #[call]
    fn stake_deposit(max_amount: u64) -> u64{
        pchain_sdk::network::defer_stake_deposit(Self::get_pool_operator(), max_amount);
        pchain_sdk::blockchain::balance()
    }
```

Lastly, we can include multiple deferred staking commands within one transaction. In this method `multiple_defer()`,
we put the deferred staking commands for unstaking and withdrawing deposits together.

Both the commands will be executed after the success of the transaction, and in the order they were called. After invoking this method call, the stake of the deposit should have been successfully unstaked and withdrawn.

### lib.rs: multiple staking commands
```rust
    #[call]
    fn multiple_defer(max_amount: u64) {
        let operator = Self::get_pool_operator();
        pchain_sdk::network::defer_unstake_deposit(operator, max_amount);
        pchain_sdk::network::defer_withdraw_deposit(operator, max_amount);
    }

```

**Congratulations! You have completed all tutorials and are ready to write your smart contract!**