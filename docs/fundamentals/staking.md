---
tags:
  - Staking
---

# Staking

**Staking** is a process of holding a cryptocurrency or token as a way to support the operations of a blockchain network. It involves locking up a certain amount of tokens for a period of time to participate in the process of validating transactions, adding new blocks to the chain, and maintaining the security and integrity of the network.

In return for staking, users may receive rewards in the form of additional tokens issued by the network. In Parallelchain Mainnet Proof-of-Stake (PoS) [consensus algorithm](consensus_mechanism.md), the voting power of a validator depends on the its stake amount, rather than the computational power they can provide, as in Proof-of-Work (PoW) algorithms.

Elements inside a staking process include:

- **Pool**: represents an operator and its delegators who can combine their stakes as a whole. The stake amount reflects the voting power of the pool operator. The stake amount also determines the rewards earned from an [epoch](blocks.md#epoch-block) if the pool operator is a validator.
- **Stake**: the amount that a participant locks up as collateral to be contributed to a pool.
- **Deposit**: the locked balance used to determine the amount of stake.

## How Staking Works

In this staking process, there are two main actors: **pool operators** and **owner**. A pool operator can create, update, and destroy pools. An owner can deposit their [XPLL](../introduction.md#what-is-xpll) into a pool, top-up their deposit, withdraw their deposit, and set the amount they want to stake in the pool. However, they cannot stake more than the amount they have deposited.

If a pool has enough power (determined by the amount of stakes), it can join as a [validator](#validator) in the next epoch. In each [epoch](blocks.md#epoch-block), block rewards are distributed to owners' deposits, and the validator set is updated according to the validators' power, which will determine the validators in consensus for the current epoch.

An owner can withdraw their deposit from a pool, but the amount they withdraw cannot make their deposit less than the maximum stake amount in the current and previous epoch.

## Validator

In a delegated proof-of-stake (DPoS) blockchain network, a validator is a participant in the network who is responsible for validating [transactions](transactions.md) and creating new [blocks](blocks.md). To validate transactions, validators must check that the transaction is valid, for example, the sender has enough balance to complete the transaction.

Validators are selected based on the amount of tokens they hold and their stake in the network. This stake serves as collateral and incentivizes validators to act honestly and maintain the security of the network. The more stakes they have, the greater their chances of being selected to be one of the validators in the next epoch.

## Delegator

In a delegated proof-of-stake (DPoS) blockchain network, a delegator is a participant who delegates their tokens to a staking pool. Delegators do not validate transactions or create new blocks themselves, but they can participate in the network's consensus process by proxy through a pool.

Delegating tokens to a pool allows the delegator to earn rewards, for doing so, without having to run [nodes](nodes.md) themselves. Delegators typically choose a pool based on their reputation, track record, and the commission fee they charge. The exact amount of rewards earned may depend on various factors, such as the number of tokens staked, the length of time the tokens are staked for, and the network's overall performance.

See also:

- [Staking XPLL by Web Wallet](../for_users/web_wallet/staking.md)
- [Staking XPLL by Xperience Browser Extension](../for_users/xperience_browser_extension/staking.md)

## Rewards

Rewards serve as an incentive for validators who actively contribute to the creation of new blocks within the blockchain network. 

In DPoS blockchains, validators are selected to validate transactions and generate new blocks based on the quantity of XPLL they have staked within the network. As a reward for their contributions, validators receive two kinds of rewards:
1. **Priority Fees**: an amount of *existing* XPLL that transaction signers *transfer* to block proposers to reward them for including their transaction.
2. **Block Rewards**: an amount of *new* XPLL created ("issued") at the end of every epoch according to a function of the total amount of XPLL staked.

Block rewards play a crucial role in blockchain networks by serving multiple purposes. They incentivize miners and validators' participation in the network's consensus process, thereby upholding the security and integrity of the network. Additionally, block rewards facilitate the introduction of new XPLL into circulation, ensuring the long-term sustainability of the network.

This document provides a rough overview of the calculation of block rewards. The exact formulas that Fullnodes use to calculate rewards are specified near the end of the ["next epoch" section](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/Runtime.md#next-epoch) of the Runtime chapter of the ParallelChain Protocol specification.


### Reward Rate

The block reward amount gradually decreases as additional blocks are incorporated into the blockchain over time. This process, which is known as the **block reward reduction**, is a characteristic found in various blockchain networks, such as Bitcoin (where it is popularly called "halving"). By implementing the block reward reduction, the supply of cryptocurrency is regulated, effectively curbing inflationary pressures.

Block rewards in ParallelChain Mainnet gradually reduce according to the following formula:

- First year: 8% per annum
- Reduce 15% per annum
- After 10 year (3650 days): keep at 1.5% per annum

The **total amount of rewards (issuance)** introduced to the network is calculated as follows:

$$
\text{Total amount of stake} \times \text{Issuance rate}
$$

For example, if the *total amount of stake* of a validator is `100,000 XPLL` currently, and the *issuance rate* is `0.001`, then the *total amount of XPLL to be issued* to that validator in the next epoch will be `100,000 * 0.001 = 100 XPLL`.


The **issuance rate** is calculated as follows:

$$
\text{Issuance rate at n-th epoch} = 0.0835 \times \frac{0.85^{\frac{n}{365}}}{365}
$$

after 10 years (or equivalently, after $n >= 3650$), the issuance rate will become a constant:

$$
\text{Issuance rate per epoch after 10 years} = \frac{0.0150}{365} 
$$

### Delegator Reward

Delegators receive a portion of the newly issued rewards based on the amount they have staked with the validator. However, it's important to note that the delegator rewards will be reduced by a commission fee, which is determined by the validator and can be adjusted in their settings.

Let's assume that in the upcoming epoch, the validator `V` will receive a *total reward* of `100 XPLL`. `V` has set a *commission fee* of `1%`. Additionally, one of its delegators, `D`, has contributed `10%` of the total staked amount to `V`. Then in the next epoch, `D` will be rewarded:

$$
\text{Reward} = 100 \text{ XPLL} \times 10\% \times (100\% - 1\%) = 9.99 \text{ XPLL}
$$

## FAQ

#### Why am I not able to stake XPLL tokens?
You will need to retain a certain amount of XPLL in your account balance to pay for transaction fees. Ensure that you have reserved a small amount of XPLL tokens to pay for the gas fee of your deposit, stake, or unstake XPLL tokens.

---

#### How do I know which validator to stake with?
The validators securing the ParallelChain Mainnet are trusted participants who are required to stake a significant number of XPLL. You may check the yield and commission fee of each validator by looking into their account details on [ParallelChain Explorer](./networks.md#parallelchain-explorer).

---

#### How long is the unstaking period?
The staking duration is measured by epochs. If you stake and unstake within the same epoch, the waiting time is considerably shorter. If the staking and unstaking are not within the same epoch, you will need to wait for at least another two epochs before unstaking.

---

#### How long is an Epoch?
One full epoch lasts approximately one day before it enters the next epoch. The epoch itself is a protocol-defined period for measuring the performance of operators on the network.

---

#### How do I withdraw my staking rewards?
You can withdraw the rewards you have earned from staking by unstaking them to your Token Deposit. Additionally, you may toggle the `AUTO STAKE REWARDS` to avoid the process of unstaking your rewards. From there, you can then withdraw the amount to your wallet. Please refer to the section on Unstaking Your XPLL and Withdrawing Your Staking Rewards for the required steps.

---

#### Can I move my staked XPLL to another validator?
Yes, you can. You will need to unstake your XPLL tokens, withdraw them to the Token Deposit, and then back to your wallet. You can then transfer those tokens to a Token Deposit with another validator before staking your tokens with them. Please refer to the sections above to move your XPLL to another validator.

---

#### Can I stake to more than one node?
Yes, you can. To do so, you will need to create a Token Deposit on the validator node you wish to stake with. Once done, you can stake tokens with the validator node. To stake XPLL with more than one validator, repeat the same process of creating a Token Deposit with the other validator node, deposit tokens from your account to the Token Deposit, and you can stake with the second validator. Repeat this process with other validator nodes you wish to stake with.

---

#### The status is still showing `PENDING`. Why is my transaction not confirmed yet?
Transactions are prioritized by the amount of Priority Fee per Gas paid, and lower priority transactions may need more time to be confirmed. You may choose to pay a higher Priority Fee during peak periods for a faster transaction, or you may check your wallet address on the [ParallelChain Explorer](networks.md#parallelchain-explorer) to monitor its transaction status.

---

#### Are the rewards automatically staked?
Depends on the setting of your stake. Your staking rewards will not be automatically staked unless the **AUTO STAKE REWARDS** field is toggled on. You can toggle it on or off anytime you want.

---

#### Is there any locking period?
There is no locking period to stake your XPLL tokens, but the staking duration is measured by epochs. If you stake and unstake within the same epoch, the waiting time is considerably shorter. If the staking and unstaking are not within the same epoch, you will need to wait for at least another two epochs before unstaking.

