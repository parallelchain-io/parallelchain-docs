---
tags:
  - Staking
---


# Staking


## How does staking work?
---

In this staking process, there are two main actors: **pool operators** and **owner**. A pool operator can create, update, and destroy pools. An owner can deposit their cryptocurrency into a pool, top-up their deposit, withdraw their deposit, and set the amount they want to stake in the pool. However, they cannot stake more than the amount they have deposited.

If a pool has enough power (determined by the amount of cryptocurrency staked), it can join as a validator in the next epoch. In each epoch, rewards are distributed to owners' deposits, and the validator set is updated according to the validators' power, which will determine the validators in consensus for the current epoch.

An owner can withdraw their deposit from a pool, but the amount they withdraw cannot make their deposit less than the maximum stake amount in the current and previous epoch.

Overall, the process allows operators to create and manage pools, owners to deposit and withdraw their funds while setting a stake amount, and rewards to be distributed to owners' deposits. The process also ensures the validator set is updated each epoch, which is important for the security and consensus of the network. Additionally, the restriction on withdrawals ensures that the pool always has enough stake to participate in the consensus process and maintains the security of the network.