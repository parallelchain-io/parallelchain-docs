---
tags:
  - testnet 2.0
---

# Transaction and Receipt

## Transaction
---

**Transaction**: A transaction authorizes the blockchain to make changes to the world state, in the form of, either:

- Tokens transfer from one account to another, or 
- A smart contract invocation.

There are three kinds of transactions in the testnet:

* `External Account to External Account (EtoE)`: A transaction that only transfers native token (TXPLL) balance from one external account to another external account. A transaction message is inferred to be EtoE if both its from address and to address have empty code.
* `External Account to Contract (EtoC)`: A transaction that calls a smart contract. A transaction message is inferred to be EtoC if its from address has empty code, and its to address has non-empty code.
* `Deploy Contract (DeployC)`: A transaction that deploys a smart contract. A transaction message is inferred to be Deploy if its from address has empty code and its to address is 0x0000000000000000000000000000000000000000000000000000000000000000 (32-byte zero).

## Receipt
---

**Receipt**: A receipt is produced upon execution of each transaction. Its fields provide a succinct summary of ‘what happened’ during the transaction execution flow:

- Exit Code
- Gas Consumed
- Return Value
- Events

**Event**: Events are key-value pairs emitted during a smart contract execution. Events allow decentralized applications to quickly prove to users that a particular branch/line of code was executed during a transaction, by querying for the existence of events in blocks.

