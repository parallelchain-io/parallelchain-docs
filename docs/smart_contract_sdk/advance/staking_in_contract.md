---
tags:
  - pchain-sdk
  - Smart Contract
  - Staking
---

Contract address can be used as owner in staking, in other words, to perform staking on behalf of a contract. It is done by invoking a contract call to create [staking command(s)](/concepts/transaction/#staking-commands) (i.e. Create Deposit, TopupDeposit, ... etc) inside the contract method. The execution of these staking commands will be deferred after the successful execution of the entire contract call. To create deferred staking command, you can call functions `defer_*` provided by the SDK.

__Notes:__

- The owner of the staking is using a contract address.
- There is no additional command receipt. Failure of deferred staking command execution results in failure of the entire contract call. 
- The `return values` in the final receipt will be overwritten (if any) by the staking command.
- In the cross-contract-call scenario, deferred staking command is not executed after the internal contract call, but after the origin contract call.

Related APIs in `pchain_sdk::network`:

```rust
/// Instantiation of a Deposit in the state.
fn defer_create_deposit(operator: PublicAddress, balance: u64, auto_stake_rewards: bool);
/// Update settings of an existing Deposit.
fn defer_set_deposit_settings(operator: PublicAddress, auto_stake_rewards: bool);
/// Increase the balance of an existing Deposit.
fn defer_topup_deposit(operator: PublicAddress, amount: u64);
/// Withdraw balance from an existing Deposit.
fn defer_withdraw_deposit(operator: PublicAddress, max_amount: u64);
/// Increase stakes to an existing Pool
fn defer_stake_deposit(operator: PublicAddress, max_amount: u64);
/// Remove stakes from an existing Pool.
fn defer_unstake_deposit(operator: PublicAddress, max_amount: u64);
```

Check out the [Tutorial 6](/smart_contract_sdk/tutorial/chapter_6.md) for the tutorial on using `defer` calls in smart contracts.