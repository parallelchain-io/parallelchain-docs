---
tags:
    - Epoch
    - Epoch Transition
---

# Epoch

**Epoch** is a protocol-defined period for measuring the performance of operators on the blockchain network. 

The primary purpose of epochs in a blockchain is to facilitate various network functionalities and maintain consensus among network participants by defining a common reference point, which could be utilized to validate transactions. During one epoch, there would be block creation and block addition to the blockchain.

In the ParallelChain Mainnet ecosystem, an `epoch` is defined by a predetermined block height or the number of blocks added to the blockchain, which is 8640. When a specific block height is reached or another 8640 blocks are added to the blockchain, the blockchain network would move from the current epoch to the next. The epoch transition could trigger critical events:

- Reward stakes in Current Validator Set
    - *Increase deposits* of owner and operator
    - *Update stakes* if auto-stake-rewards is enabled
    
- Replacing Previous Validator Set with Current Validator Set
- Replacing Current Validator Set with Next Validator Set
- Returning Next Validator Set for the next leader selection

Epoch transition ensures that only one set of validators is active on the blockchain network at any given time, which makes the information on the blockchain tamper-proof.

