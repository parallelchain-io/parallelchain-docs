---
tags:
  - block reward
  - Staking
---


# Staking


## Block Reward
---

Block rewards in blockchain are a form of incentive that is given to validators who participate in the process of adding new blocks to a blockchain network. The process of adding new blocks to a blockchain is known as validating in delegated proof-of-stake (DPoS) blockchains.


In DPoS blockchains, validators are chosen to validate transactions and create new blocks based on the amount of XPLL they have staked in the network. Validators are rewarded with a portion of the transaction fees and a portion of the block rewards for creating new blocks.


Block rewards serve several purposes in blockchain networks. They incentivize miners and validators to participate in the network's consensus process, which helps to maintain the security and integrity of the network. Block rewards also provide a way to introduce new XPLL into circulation, which helps to ensure that the network is sustainable in the long term.


Reward formulas can be found in the technical documentation [parallelchain-protocol](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/Runtime.md).

### Reward Rate

Over time, the block reward amount decreases as more blocks are added to the blockchain. This is known as the block reward halving, and it is a feature of many blockchain networks, including Bitcoin. The block reward halving helps to control the supply of cryptocurrency and prevent inflation.

The reward rate is:

- Initially 8.5% per annum in the first year
- Reduced gradually 15% per annum
- Kept at 1.5% per annum after 10 years (3650 days)

The total amount of rewards (issuance) introduced to the network is calculated as follows:

```
Total amount of stake x Issuance rate
```

For example, the total amount of stake of a validator is `100,000 XPLL` currently, and the issuance rate is `0.001`, then the total amount of XPLL to be issued to that validator in next epoch will be `100,000 * 0.001 = 100 XPLL`.


The issuance rate is calculated as follows:

```
Issuance rate at n-th epoch = 0.0835 * 0.85^(n/365) / 365
```

but after 10 years, the formula will be changed to:

```
Issuance rate = 0.0150 / 365 
```

### Delegator Reward

Delegators share the newly issued rewards in proportion to their staked amount in the validator. In addition, the delegator rewards will be substracted for paying commission fee to the validator. The commission fee is set in the validator's setting.

Consider there will be total amount of rewards `100 XPLL` issued to the validator `V` in next epoch. `V` has a commission fee `1%`, and one of its delegator, `D`, contributes `10%` of the total amount of stakes to `V`.

Then in next epoch, `D` will be rewarded:

```
reward = 100 XPLL * 10% * (100% - 1%) = 9.99 XPLL
```

--- 

Note:

- The calculated values in the examples in this page are approximate estimation. Exact calculated values are affected by, for example, decimal truncation in integer division, and sequence of arithmetic opertaions that applied, etc.
- The formulas are valid since protocol version v0.4, but could be subjected to change in future protocol versions.