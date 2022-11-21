---
tags:
  - testnet 3
---

# Accounts

**Account** is the basic identity of an agent on the blockchain. An account is identified by its address. 

In `testnet 3`, accounts are divided into two types:

- __Externally Owned Accounts (EOA)__: the address of EOA is public key of the keypair which is `ed25519_dalek` compatible.
- __Contract Accounts__: contract account is created from contract deployment. The address is hash of the concatenation of a contract's bytecode and the deployer's address (which can itself be a smart contract) and nonce of the EOA that deploys the contract.

Elements inside an account:

- Tokens: The balance of the account
- Nonce: Number of transaction made to the blockchain
- Contract Code: Binary of the contract that deployed to blockchain (applied to Contract Accounts)

These accounts are indistinguishable from one another. In future versions of the testnet, contract accounts and externally owned accounts will be distinguishable. 