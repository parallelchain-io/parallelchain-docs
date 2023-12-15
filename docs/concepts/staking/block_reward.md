---
tags:
  - block reward
  - Staking
---


# Staking


## Block Reward
---

Block rewards in a blockchain serve as an incentive for validators who actively contribute to the creation of new blocks within the blockchain network. This rewarding process, commonly referred to as validation, is particularly relevant in **delegated proof-of-stake (DPoS)** blockchains.

In DPoS blockchains, validators are selected to validate transactions and generate new blocks based on the quantity of XPLL they have staked within the network. As a reward for their contributions, validators receive a share of the transaction fees and a portion of the block rewards for successfully creating new blocks. 

Block rewards play a crucial role in blockchain networks by serving multiple purposes. They incentivize miners and validators' participation in the network's consensus process, thereby upholding the security and integrity of the network. Additionally, block rewards facilitate the introduction of new XPLL into circulation, ensuring the long-term sustainability of the network.


Reward formulas can be found in the technical documentation [parallelchain-protocol](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/Runtime.md).

### Reward Rate

Over time, the block reward amount decreases as more blocks are added to the blockchain. This is known as the **block reward halving**, and it is a feature of many blockchain networks, including Bitcoin. The block reward halving helps to control the supply of cryptocurrency and prevent inflation.

The reward rate is:

- Initially 8.5% per annum in the first year
- Reduced gradually 15% per annum
- Kept at 1.5% per annum after 10 years (3650 days)

The total amount of rewards (issuance) introduced to the network is calculated as follows:

```
Total amount of stake x Issuance rate
```

For example, if the total amount of stake of a validator is `100,000 XPLL` currently, and the issuance rate is `0.001`, then the total amount of XPLL to be issued to that validator in the next epoch will be `100,000 * 0.001 = 100 XPLL`.


The issuance rate is calculated as follows:

```
Issuance rate at n-th epoch = 0.0835 * 0.85^(n/365) / 365
```

but after 10 years, the formula will be changed to:

```
Issuance rate = 0.0150 / 365 
```

### Delegator Reward

Delegators share the newly issued rewards in proportion to their staked amount in the validator. In addition, the delegator rewards will be subtracted as a commission fee to the validator. The commission fee is set in the validator's setting.

Consider there will be a total amount of rewards `100 XPLL` issued to the validator `V` in the next epoch. `V` has a commission fee of `1%`, and one of its delegators, `D`, contributes `10%` of the total amount of stakes to `V`.

Then in the next epoch, `D` will be rewarded:

```
reward = 100 XPLL * 10% * (100% - 1%) = 9.99 XPLL
```

--- 

Note:

- The calculated values in the examples on this page are approximate estimations. Exact calculated values are affected by, for example, decimal truncation in integer division, sequence of arithmetic operations that are applied, etc.
- The formulas are valid since protocol version v0.4 but could be subjected to change in future protocol versions.