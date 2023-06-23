# ParallelChain Request for Comments 3 (PRFC 3)

| PRFC | Title | Author | Version | Date First Published |
| --- | ----- | ---- | --- | --- |
| 3   | Lockup and Vesting Standard | ParallelChain Lab | 1 | May 2nd, 2023 | 

## Summary
---
  
ParallelChain Request for Comments 3 defines a standard interface for XPLL's lockup and vesting. Lockup and vesting are applied to maintain the value and stability of XPLL. Lockup refers to a period in which the XPLLs are locked and cannot be transferred or sold, while vesting refers to a process of releasing the locked XPLLs gradually over a period of time, at regular intervals. This ensures that the XPLLs are distributed in a controlled and predictable manner, as well as incentivising long-term holding of XPLLs, which can ultimately benefit the development and success of ParallelChain Lab's projects.

To encourage staking in the network, staking functionalities are provided in the PRFC3 smart contract, where the investors can deposit and stake their locked XPLLs into pools.

The "Required Calls" section lists the set of methods that all smart contracts that want to be PRFC 3-compliant must implement, as well as the behaviour that each defined method must exhibit.

## Glossary
---

**token_owner**: The address to which the XPLLs in this contract will be transferred after being claimed.

**initial_token_amount**: Total amount of XPLLs (in Grays) that were locked in this lockup contract initially.


There are five possible phases in the vesting schedule:

- **Preparing**: The period before the start date of the schedule.
- **Locked**<sup>\*optional</sup>: The period when the XPLLs are locked.
- **PartialUnlock**<sup>\*optional</sup>: The day when a portion of XPLLs is unlocked and is available to be claimed by the token owner. 
- **Vesting**: The period when an equal portion of XPLLs are unlocked regularly, daily or monthly, and the rest remains locked, until the next period.
- **FullUnlock**: The period after the whole schedule is ended, all tokens are available to be claimed by the token owner.



## Lockup and Vesting Schedule
---

There are different lockup and vesting schedules which can be categorized as follows:

- Type 1 : A certain percentage of tokens are released on day 1, and is followed by daily vesting over a period of time (30, 90, 150, 180, 240 days) starting from day 2. For example:


 |Unlock on day 1 | Daily vesting starting on day 2
 |:---:           | :---: |
 |25%             | 30 days



- Type 2: A certain percentage of tokens are released on day 1. The rest of the tokens will be locked for a period of time (30, 90,180 days) and start daily vesting after the locked period is over. For example: 

| Unlock on day 1 | Locked period starting on day 2 | Daily vesting after locked is over
| :---:           | :---:                           | :---: |
| 2%              | 180 days                        | 720 days


- Type 3: All tokens are locked for a period of time (240, 300, 360 days). All the tokens will vest over a period of time (300, 330, 360 days), once every 30 days.

| Locked period  | Monthly vesting after Locked is over
| :---:          | :---: |
| 240 days       | 300 days 



 - Type 4: All tokens are vested over a period of time (540, 630, 720 days), once every 30 days. There is no initial unlocked tokens or lockup period for this type.


| Monthly vesting period |
| :---:          | 
| 540 days       |  


## Require Types
---

The types alias used in the specification.

```rust
type Timestamp = u32;
type Days = u32;
```

Phase is borsh-serializable enum to define different phases.

```rust
enum Phase {
    Preparing,
    Locked,
    PartialUnlock,
    Vesting,
    FullUnlock
}
```

## Required Views
---

### overview

```rust
fn overview(timestamp: Timestamp) -> (u64, u64, u64, Phase, Option<Phase>, Timestamp)
```

Returns the overview of the Lockup and Vesting information, as a 6-tuple of 

- balance: the balance of the contract
- locked: the locked balance of the contract
- unlocked: the unlocked balance of the contract
- phase: the current phase
- next phase: the next phase. None if there is no next phase.
- timestamp of the next phase. None if there is no next phase.

### token_owner

```rust
fn token_owner() -> pchain_types::PublicAddress
```

Returns the account address of the token owner.

### locked_duration

```rust
fn locked_duration() -> (Timestamp, Timestamp)
```

Returns the start and end timestamp of the Locked phase.

### unlock_duration

```rust
fn unlock_duration() -> (Timestamp, Timestamp)
```

Returns the start and end timestamp of the PartialUnlock phase.

### vesting_duration

```rust
fn vesting_duration() -> (Timestamp, Timestamp)
```

Returns the start and end timestamp of the Vesting phase.

### balance_in_deposit

```rust
fn balance_in_deposit() -> u64
```

Returns the number of locked XPLLs (in Grays) that are deposited in a pool.

## Required Calls (General)
---

### init

```rust
fn init(
    token_owner: PublicAddress, 
    token_amount: u64, 
    locked_start_timestamp: Timestamp, 
    locked_end_timestamp: Timestamp, 
    unlock_amount: u64, 
    unlock_timestamp: Timestamp, 
    vesting_start_timestamp: Timestamp, 
    vesting_end_timestamp: Timestamp, 
    vesting_days: Days, 
    daily_vesting: bool, 
    staking_allowed: bool
)
``` 

Initialises the lockup contract by specifying the recipient of the XPLL, and the start and end timestamp of each of the phases. `daily_vesting` determines if the XPLLs are released daily or every 30 days. `staking_allowed` determines if staking operations are allowed in the current vesting schedule.

### phase (Viewable)

```rust
fn phase(timestamp: Option<Timestamp>) -> Phase
```

Returns the phase that the account is currently in: *Preparing(0), PartialUnlock(1), Locked(2), Vesting(3), FullUnlock(4)*. If `timestamp` is `None`, the timestamp will be retrieved by `pchain_sdk::blockchain::timestamp()`.


### available_to_claim (Viewable)

```rust
fn available_to_claim(timestamp: Option<Timestamp>) -> u64
```

Returns the number of unlocked XPLLs (in Grays) that are available for claiming at the given timestamp. If `timestamp` is `None`, the timestamp will be retrieved by `pchain_sdk::blockchain::timestamp()`.

Note that for token amounts that are not divisible by the number of vesting periods, the remainder will be released in the FullUnlock phase.  

### claim_unlocked_tokens

```rust
fn claim_unlocked_tokens(release_amount: u64)
```

Unlocked XPLLs specified with the `release_amount`(in Grays) will be released and transferred back to the `token_owner`'s account.

This action will fail when:
-  the caller address does not match with the `token_owner` specified during contract initialisation
-  `released_amount` exceeds the amount that is unlocked at that timestamp 

## Required Calls (Staking)
These calls will fail when 
- the caller address does not match with the `token_owner` specified during contract initialisation
- `staking_allowed` is set to false
- the staking pool of `operator` does not exist

### create_deposit

```rust
fn create_deposit(operator: PublicAddress, balance: u64, auto_stake_rewards: bool)
```

Create a deposit by transferring the balance to the deposit and setting up auto-stake-rewards.

Note that `balance` can only be taken from the amount that is still locked in the contract. The XPLLs that are available to claim at the timestamp cannot be deposited in the pool.

### topup_deposit

```rust
fn topup_deposit(operator: PublicAddress, amount: u64)
```

Top up the deposit by transferring the balance to the deposit.

Note that `amount` can only be taken from the amount that is still locked in the contract. The XPLLs that are available to claim at the timestamp cannot be deposited in the pool.

### set_deposit_settings

```rust
fn set_deposit_settings(operator: PublicAddress, auto_stake_rewards: bool)
```

Set deposit settings by updating auto-stake-rewards.

### withdraw_deposit

```rust
fn withdraw_deposit(operator: PublicAddress, max_amount: u64)
```

Withdraw the deposit and reduce Pool's power. The requested withdrawal amount could be less even if the transaction succeeds. The actual withdrawal amount is limited by factors: previous epoch locked power, current epoch locked power. 

### stake_deposit

```rust
fn stake_deposit(operator: PublicAddress, max_amount: u64)
```

Stake deposit to a pool and update Pool's power. The resulting stake must not be greater than the deposit balance.

### unstake_deposit

```rust
fn unstake_deposit(operator: PublicAddress, max_amount: u64)
```

Unstake deposits from a pool and reduce the pool's power.