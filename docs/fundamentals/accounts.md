---
tags:
  - Account
  - EOA
  - Network Account
---

# Accounts

**Account** is the basic identity of an agent on the blockchain. An account is identified by its address. 

In Parallelchain Mainnet, accounts are divided into two types:

- __Externally Owned Accounts (EOA)__: The address of an EOA is the public key of the keypair that is compatible with [ed25519-dalek](https://github.com/dalek-cryptography/curve25519-dalek/).
- __Contract Accounts__: A contract account is created from contract deployment. The address is the [SHA256](https://en.wikipedia.org/wiki/SHA-2) hash of the concatenation of a contract's bytecode and the nonce of the **EOA** that deploys the contract. 

Elements inside an account include:

- **Nonce**: The number of transactions made to the blockchain under this account.
- **Balance**: The balance of the account.

The followings are elements that only apply to Contract Accounts:

- **Contract Code**: The binary of the [smart contract](../for_developers/smart_contracts/introduction.md) that was deployed to the blockchain (applies to Contract Accounts).
- **CBI Version**: The version of the Contract Binary Interface.
- **Storage Hash**: The 32-byte SHA256 root hash of its Storage Trie. This is empty for an External.

## How Account Works

You can create **EOA** account by **ed25519-dalek** key generation algorithm. The account is composed of a 32-byte public key and a 32-byte private key. This public key is the **Address** which can be known to others. While your private key, which is used for signing [transactions](transactions.md), **MUST** be kept in secret.

Address is usually displayed in [Base64url](https://datatracker.ietf.org/doc/html/rfc4648#section-5) format in ParallelChain Mainnet ecosystem. Here is an example:

```text
Tc0bU2oS0A0_GrjPKsDLSyVa3gc7PvzwWIAGRk-_SmA
```

If you want to transfer [XPLL](../introduction.md#what-is-xpll) to Alice's account, first you need to know her address. Then, you create a transaction with Alice's address as [recipient](transactions.md#account-commands), and sign it by using your private key. The **Balance** of your and Alice's account will be updated when this signed transaction is made to blockchain successfully. The **Nonce** of your account will be increased by 1.

On the other hand, a **Contract Account** is not made up of cryptographic keys. Its address is a 32-byte hash. It is not able to sign a transaction. But similar to **EOA**, it has its own balance, and can receive XPLL from other accounts. See [Smart Contract](../for_developers/smart_contracts/introduction.md) for details.

!!! Note
    The type of account cannot be distinguished from each other just by looking at the address format.

See also:

- [Create Account by CLI](../for_users/pchain_client_cli/manage_account.md)
- [Create Account by Web Wallet](../for_users/web_wallet/create_account.md)
- [Create Account by Xperience Browser Extension](../for_users/xperience_browser_extension/create_account.md)


## Network Account

A **Network Account** is a single identified network-wide account that maintains the [state](nodes.md#world-state) of ParallelChain Mainnet. The purpose of this state is to implement the [staking](staking.md) related protocol. This account is not associated with Ed25519 material. The network-significant data that the Network Account stores are composed of various fields:

- **Previous Validator Set**: The set of pools that form the validator set in the previous epoch. The stake in this validator set is locked until the next epoch.
- **Current Validator Set**: The set of pools that form the validator set in the current epoch.
- **Next Validator Set**: The limited-size pools with the largest powers. These will become the validator set in the next epoch.
- **Pools**: The set of pools that are accepting stakes and currently competing to become part of the next validator set.
- **Deposits**: The locked balance used to determine the amount of stake that can be contributed to a pool.
- **Current Epoch**: The current epoch number.

