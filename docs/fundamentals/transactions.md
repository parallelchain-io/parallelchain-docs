---
tags:
  - Transaction
  - Receipt
  - Nonce
  - Exit Status
  - Log
---

# Transactions

A **transaction** is a digitally signed instruction that tells the Mainnet state machine to execute a sequence of commands. 

A transaction has the following fields:

- `Signer`: the public address of the external-owned account that signed this transaction.
- `Nonce`: the number of transactions signed by the signer that has been included on the blockchain before this transaction. This ensures that all of the signer’s transactions are included in the blockchain in an expected order and prevents the same transaction from being included in multiple blocks.
- `Commands`: a sequence of [Commands](#commands) to be executed in a transaction.
- `Gas Limit`: the maximum number of [gas](gas.md) units that should be used in executing this transaction.
- `Max Base Fee per Gas`: the maximum number of grays that the signer is willing to burn for a gas unit used in this transaction.
- `Priority Fee per Gas`: the number of grays that the signer is willing to pay the block proposer for including this transaction in a block.
- `Signature`: the signature formed by signing over content in this transaction using the signer’s private key.
- `Hash`: the cryptographic hash of the signature.

Example of a transaction queried by [ParallelChain Client CLI](../for_users/pchain_client_cli/introduction.md):

```json
{
  "transaction": {
    "commands": [
      {
        "Transfer": {
          "recipient": "kxrCl32WEzr7gr4Awb3jAMQtoh5zY8grabqlu-x42jY",
          "amount": 2000000000
        }
      }
    ],
    "signer": "qPReY5x_DPKAx1v30UrU1x1RI61Wd2OxeZo2eSWcjMU",
    "priority_fee_per_gas": 0,
    "gas_limit": 67500000,
    "max_base_fee_per_gas": 8,
    "nonce": 428,
    "hash": "dOrjtT7o5Y97mlLJFaCvdNUq6UgVQItVYGXHPoDw5n4",
    "signature": "RLM3k5VDDzBqkQ7ios4TMP9LwL50n8hNQ7OjH-wcTE0Ts6F3uWDaA5J52sCyYr3aGGGElXKiIatqH15qVwMUAA"
  },
  "receipt": [
    {
      "status_code": "Success",
      "gas_used": 32820,
      "return_values": "",
      "logs": []
    }
  ]
}
```

## How Transaction Works

A transaction is created by an [EOA](accounts.md#accounts) who sets the fields such as `nonce`, `gas_limit` of a transaction. The field `signer` is the address (public key) of the account, and the signature is created by signing the the content of the transaction with the private key of the account. Finally, the field `hash` is computed from the signature field.

EOA submits transaction to the blockchain [network](networks.md). The [nodes](nodes.md) will then execute the transaction in order to produce a block. The transaction execution mutates the [state](nodes.md#world-state) of blockchain. For example, if the transaction includes a [Transfer Command](#account-commands), balances of the signer and recipient can be updated upon a successful execution.

In the end, the transaction becomes part of a [block](blocks.md) if it is executed and validated successfully, Otherwise, the transaction will be dropped by the network.

See also:

- [Create Transaction by CLI](../for_users/pchain_client_cli/transaction.md)
- [Transferring Tokens by Web Wallet](../for_users/web_wallet/transfer_tokens.md)
- [Transferring Tokens by Xperience Browser Extension](../for_users/xperience_browser_extension/transfer_tokens.md)

## Commands

A command is a useful operation that Parallelchain allows the user to do. A sequence of commands can be inserted into a transaction to be carried out. There are currently 13 different kinds of commands, each corresponding to a variant of the command enum type. These are further divided into three categories: [account commands](#account-commands), [staking commands](#staking-commands), and [protocol commands](#protocol-commands). Most commands take inputs, which are part of the command type as the fields of its corresponding variant.

In the below subsections, the input types are represented in [Rust programming language](https://www.rust-lang.org). For example, `PublicAddress` is 32-byte data. `Vec<u8>` is vector of unsigned 8-bit integers (bytes). 

### Account commands

**Account commands** are the commands specific to mutate state of [Accounts](accounts.md) in the blockchain.

|**Name**|**Input**|**Description**|
|---|---|---|
|Transfer|<li>Recipient (`PublicAddress`)</li> <li>Amount (`u64`)</li>|Transfer an amount from the signer's balance to the recipient's balance.|
|Deploy|<li>Contract (`Vec<u8>`)</li> <li>CBI version (`u32`)</li>|Deploy a contract that implements a given CBI version.|
|Call|<li>Target (`PublicAddress`)</li> <li>method (`String`)</li> <li>arguments (`Option<Vec<Vec<u8>>>`)</li> <li>amount (`Option<u64>`)</li>|Call a contract method with given arguments, optionally transferring some tokens.|

### Staking commands

**Staking commands** are mainly associated with the [staking](staking.md) process in ParallelChain Mainnet ecosystem.

|**Name**|**Input**|**Description**|
|---|---|---|
|Create pool| <li>Commission rate (`u8`)</li> |Create a new pool with the signer as the operator which charges a given commission rate on the validator rewards of delegated stakes
|Set pool settings| <li>Commission rate (`u8`)</li> |Set the commission rate of an existing pool, operated by the signer.|
|Delete pool|<li>None</li>|Delete an existing pool, as well as all of its existing stakes.|
|Create Deposit| <li>Operator (`PublicAddress`)</li> <li>Balance (`u64`)</li> <li>Auto stake rewards (`bool`)</li> |Create a deposit that targets a given operator, with the given balance, and stake the validator rewards that it receives optionally.|
|Set deposit settings| <li>Operator (`PublicAddress`)</li> <li>Auto stake rewards? (`bool`)</li> |Set whether a given deposit automatically stakes validator rewards.|
|Top up deposit| <li>Operator (`PublicAddress`)</li> <li>Balance (`u64`)</li> |Transfer more tokens into an existing deposit.|
|Withdraw Deposit| <li>Operator (`PublicAdddress`)</li> <li>Max amount (`u64`)</li> |Try to withdraw a given amount from a deposit into the signer's balance.|
|Stake| <li>Operator (`PublicAddress`)</li> <li>Max amount (`u64`)</li> |Try to stake a given amount from a deposit.|
|Unstake| <li>Operator (`PublicAddress`)</li> <li>Max amount (`u64`)</li> |Try to reduce a deposit's stake by a given amount.|

!!! Note
    *The actual amount of tokens that could be withdrawn, staked, or unstaked by the final three kinds of commands depends on timing and order, factors that users do not have precise control over. For example, the number of tokens that can be withdrawn from a deposit can change significantly between epochs as the deposit's [bonded balance](https://github.com/parallelchain-io/parallelchain-protocol/blob/master/Blockchain.md#delegated-proof-of-stake) decreases, or increases.*

    *Therefore, these commands accept as input a "max amount" instead of a precise amount. These commands try to withdraw, stake, or unstake as close to the maximum amount as possible, and inform the precise amount in its return value.*

### Protocol commands

**Protocol commands** are the commands which mutates the state of the whole network (i.e. mutates the state of [Network Account](accounts.md#network-account) in the blockchain). The current protocol version has only one protocol command, `Next Epoch`.

|**Name**|**Input**|**Description**|
|---|---|---|
|Next epoch|None|Reward the current [epoch](blocks.md#epoch-block)'s validators, and confirm the next epoch's validator set.|

## Receipt and Logs

A **receipt** is produced upon execution of each transaction. It consists of a sequence of `CommandReceipt`, which provides a succinct summary of what happened during the execution of the corresponding `Command` in a transaction.

A Command Receipt has the following fields:

- `Exit Status`: tells whether the corresponding command in the sequence succeeded in doing its operation and, if it failed, whether the failure is because of gas exhaustion or some other reason.
- `Gas Used`: how much gas was used in the execution of the transaction. This will at most be the transaction’s gas limit.
- `Return Values`: the return values of the corresponding command.
- `Logs`: the logs emitted during the corresponding call command.

Possible values for `Exit Status` are:

|Code|Name|Description|
|:---|:---|:---|
|0x00|Operation successful|The command successfully did what it is supposed to do.|
|0x01|Operation failed|The command failed to do what it is supposed to do.|
|0x02|Gas exhausted|Execution halted in the middle of the command because the gas limit was hit.|

**Logs** are topic-value pairs emitted during smart contract execution. Logs allow decentralised applications to quickly prove to users that a particular branch/line of code was executed during a transaction, by querying for the existence of logs in blocks.
