---
tags:
  - Staking
---

# Staking

**Staking** is a process of holding a cryptocurrency or token as a way to support the operations of a blockchain network. It involves locking up a certain amount of tokens for a period of time to participate in the process of validating transactions, adding new blocks to the chain, and maintaining the security and integrity of the network.

In return for staking, users may receive rewards in the form of additional tokens issued by the network. In Parallelchain Mainnet, Staking is used in delegated proof-of-stake (DPoS) consensus algorithms, where validators are chosen based on the number of tokens they have staked, rather than the computational power they can provide, as in proof-of-work (PoW) algorithms.

Elements inside a staking process include:

- **Pool**: a group of validators and delegators who combine their staking resources to increase their chances of being chosen to validate blocks and earn rewards.
- **Stake**: the cryptocurrency tokens that a participant locks up as collateral to become a validator or delegator.
- **Deposit**: the locked balance used to determine the amount of stake that can be contributed to a pool.

## How does Staking Work?

In this staking process, there are two main actors: **pool operators** and **owner**. A pool operator can create, update, and destroy pools. An owner can deposit their cryptocurrency into a pool, top-up their deposit, withdraw their deposit, and set the amount they want to stake in the pool. However, they cannot stake more than the amount they have deposited.

If a pool has enough power (determined by the amount of cryptocurrency staked), it can join as a validator in the next epoch. In each epoch, rewards are distributed to owners' deposits, and the validator set is updated according to the validators' power, which will determine the validators in consensus for the current epoch.

An owner can withdraw their deposit from a pool, but the amount they withdraw cannot make their deposit less than the maximum stake amount in the current and previous epoch.

Overall, the process allows operators to create and manage pools, owners to deposit and withdraw their funds while setting a stake amount, and rewards to be distributed to owners' deposits. The process also ensures the validator set is updated each epoch, which is important for the security and consensus of the network. Additionally, the restriction on withdrawals ensures that the pool always has enough stake to participate in the consensus process and maintains the security of the network.

## Validator
In a delegated proof-of-stake (DPoS) blockchain network, a `validator` is a participant in the network who is responsible for validating transactions and creating new blocks. Validators are selected based on the amount of tokens they hold and their **stake** in the network. This stake serves as collateral and incentivizes validators to act honestly and maintain the security of the network.

Validators in a DPoS network are responsible for two key functions: proposing new blocks and validating transactions. Validators propose new blocks by creating a block with a set of new transactions and adding it to the blockchain. To validate transactions, validators must check that the transaction is valid, that the sender has enough tokens to complete the transaction, and that the transaction is not a double spend.

Validators in a DPoS network are chosen according to the amount they staked in the network. The more tokens a user stakes, the greater their chances of being selected to be one of the validators in the next epoch.

## Delegator

In a delegated proof-of-stake (DPoS) blockchain network, a delegator is a participant who delegates their tokens holdings to a staking pool to participate in the network's consensus process. Delegators do not validate transactions or create new blocks themselves, but they can participate in the network's consensus process by proxy through a pool.

Delegating tokens to a pool allows the delegator to participate in the network's consensus process and earn rewards for doing so, without having to run a pool node themselves. Delegators typically choose a pool based on their reputation, track record, and the rewards they offer.

Delegators are responsible for selecting a pool to delegate their tokens to and must be careful to choose a reputable and trustworthy one. Delegators can also switch pools at any time, which allows them to take advantage of new opportunities or switch to a different pool if their current pool is underperforming.

Delegators typically earn rewards for participating in the network's consensus process, which is shared between the delegators and the pool operator they delegate to. The exact amount of rewards earned may depend on various factors, such as the number of tokens staked, the length of time the tokens are staked for, and the network's overall performance.

Overall, delegation allows more participants to participate in the network's consensus process, which can help to increase the network's security and decentralization.


## FAQ

#### Why am I not able to stake XPLL tokens?
You will need to retain a certain amount of XPLL in your account balance to pay for transaction fees. Ensure that you have reserved a small amount of XPLL tokens to pay for the gas fee of your deposit, stake, or unstake XPLL tokens.

---

#### How do I know which validator to stake with?
The validators securing the ParallelChain Mainnet are trusted participants who are required to stake a significant number of XPLL. You may check the yield and commission fee of each validator by tapping on the drop-down arrow of the ‘Operator’ field.

---

#### How long is the unstaking period?
The staking duration is measured by epochs. If you stake and unstake within the same epoch, the waiting time is considerably shorter. If the staking and unstaking are not within the same epoch, you will need to wait for at least another two epochs before unstaking.

---

#### How long is an Epoch?
One full epoch lasts approximately one day before it enters the next epoch. The epoch itself is a protocol-defined period for measuring the performance of operators on the network.

---

#### How do I withdraw my staking rewards?
You can withdraw the rewards you have earned from staking by unstaking them to your Token Deposit. Additionally, you may toggle the ‘AUTO STAKE REWARDS’ to avoid the process of unstaking your rewards. From there, you can then withdraw the amount to your wallet. Please refer to the section on Unstaking Your XPLL and Withdrawing Your Staking Rewards for the required steps.

---

#### Can I move my staked XPLL to another validator?
Yes, you can. You will need to unstake your XPLL tokens, withdraw them to the Token Deposit, and then back to your wallet. You can then transfer those tokens to a Token Deposit with another validator before staking your tokens with them. Please refer to the sections above to move your XPLL to another validator.

---

#### Can I stake to more than one node?
Yes, you can. To do so, you will need to create a Token Deposit on the validator node you wish to stake with. Once done, you can stake tokens with the validator node. To stake XPLL with more than one validator, repeat the same process of creating a Token Deposit with the other validator node, deposit tokens from your account to the Token Deposit, and you can stake with the second validator. Repeat this process with other validator nodes you wish to stake with.

---

#### The status is still showing ‘PENDING’. Why is my transaction not confirmed yet?
Transactions are prioritized by the amount of Priority Fee per Gas paid, and lower priority transactions may need more time to be confirmed. You may choose to pay a higher Priority Fee during peak periods for a faster transaction, or you may check your wallet address on the blockchain explorer to monitor its transaction status.

---

#### Are the rewards automatically staked?
Depends on the setting of your stake. Your staking rewards will not be automatically staked unless the **AUTO STAKE REWARDS** field is toggled on. You can toggle it on or off anytime you want.

---

#### Is there any locking period?
There is no locking period to stake your XPLL tokens, but the staking duration is measured by epochs. If you stake and unstake within the same epoch, the waiting time is considerably shorter. If the staking and unstaking are not within the same epoch, you will need to wait for at least another two epochs before unstaking.

