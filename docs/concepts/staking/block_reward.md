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

The block reward amount gradually decreases as additional blocks are incorporated into the blockchain over time. This process, which is known as the **block reward halving**, is a characteristic found in various blockchain networks, such as Bitcoin. By implementing the block reward halving, the supply of cryptocurrency is regulated, effectively curbing inflationary pressures.

The reward rate is calcuated as following:

- First year: 8.5% per annum
- Reduce 15% per annum
- After 10 year (3650 days): keep at 1.5% per annum


The **total amount of rewards (issuance)** introduced to the network is calculated as follows:

$$
\operatorname{Total amount of stake} \times \operatorname{Issuance rate}
$$

For example, if the *total amount of stake* of a validator is `100,000 XPLL` currently, and the *issuance rate* is `0.001`, then the *total amount of XPLL to be issued* to that validator in the next epoch will be `100,000 * 0.001 = 100 XPLL`.


The issuance rate is calculated as follows:

$$
\operatorname{Issuance rate at n-th epoch} = 0.0835 \times \frac{0.85^{\frac{n}{365}}}{365}
$$

but after 10 years, the formula will be changed to:

$$
\operatorname{Issuance rate} = \frac{0.0150}{365} 
$$

### Delegator Reward

Delegators share the newly issued rewards in proportion to their staked amount in the validator. In addition, the delegator rewards will be subtracted as a commission fee to the validator. The commission fee is set in the validator's setting.

Consider there will be a total amount of rewards `100 XPLL` issued to the validator `V` in the next epoch. `V` has a commission fee of `1%`, and one of its delegators, `D`, contributes `10%` of the total amount of stakes to `V`.

Then in the next epoch, `D` will be rewarded:

$$
\operatorname{Reward} = 100 XPLL \times 10\% \times (100\% - 1\%) = 9.99 XPLL
$$

--- 

Note:

- The calculated values in the examples on this page are approximate estimations. Exact calculated values are affected by, for example, decimal truncation in integer division, sequence of arithmetic operations that are applied, etc.
- The formulas are valid since protocol version v0.4 but could be subjected to change in future protocol versions.