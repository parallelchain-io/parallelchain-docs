---
tags:
  - Account
  - EOA
  - Network Account
---

# Accounts

**Account** is the basic identity of an agent on the blockchain. An account is identified by its address. 

In Parallelchain Mainnet, accounts are divided into two types:

- __Externally Owned Accounts (EOA)__: The address of an EOA is the public key of the keypair that is compatible with `ed25519_dalek`.
- __Contract Accounts__: A contract account is created from contract deployment. The address is the hash of the concatenation of a contract's bytecode and the nonce of the EOA that deploys the contract. 

Elements inside an account include:

- **Nonce**: The number of transactions made to the blockchain under this account.
- **Balance**: The balance of the account.

The followings are elements that only apply to Contract Accounts:

- **Contract Code**: The binary of the [smart contract](../for_developers/smart_contracts/introduction.md) that was deployed to the blockchain (applies to Contract Accounts).
- **CBI Version**: The version of the Contract Binary Interface.
- **Storage Hash**: The 32-byte SHA256 root hash of its Storage Trie. This is empty for an External.

!!! Note
    These accounts cannot be distinguished from each other just by looking at the address format.

A **Network Account** is a single identified network-wide account that maintains the state of ParallelChain Mainnet. This account is not associated with Ed25519 material. The network-significant data that the Network Account stores are composed of various fields:

- **Previous Validator Set**: The set of pools that form the validator set in the previous epoch. The stake in this validator set is locked until the next epoch.
- **Current Validator Set**: The set of pools that form the validator set in the current epoch.
- **Next Validator Set**: The limited-size pools with the largest powers. These will become the validator set in the next epoch.
- **Pools**: The set of pools that are accepting stakes and currently competing to become part of the next validator set.
- **Deposits**: The locked balance used to determine the amount of stake that can be contributed to a pool.
- **Current Epoch**: The current epoch number.

