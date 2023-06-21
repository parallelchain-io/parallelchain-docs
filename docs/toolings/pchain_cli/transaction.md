---
tags:
  - Tools
  - CLI
  - pchain-cli-rust
---


## Transaction 
A transaction is a digitally signed instruction that tells the ParallelChain state machine to execute a sequence of commands. There are different kinds of [Commands](https://docs.rs/pchain-types/0.4.3/pchain_types/blockchain/enum.Command.html) in ParallelChain protocol. 

`pchain_client` accepts transaction in json format. This section will demonstrate how to prepare your transaction file and submit it with your account keys.
### Prepare Transaction File
`pchain_client` provides user-friendly way to prepare your transaction file without prior knowledge of JSON (JavaScript Object Notation) format.
The transaction file consists of 2 parts: `Parameters` and `Subcommand`.

Here are some CLI subcommands to indicate corresponding Protocol Transaction Command. 

| Subcommand | Action          | Description                                           |
|------------|-----------------|-------------------------------------------------------|
| transfer   |                 | Transfer balance from transaction signer to recipient |
| deploy     |                 | Deploy smart contract to the state of the blockchain  |
| call       |                 | Trigger method call of a deployed smart contract      |
| deposit    |                 | Deposit some balance into the network account         |
|            | create          | Instantiation of a Deposit of an existing Pool        |
|            | top-up          | Increase balance of an existing Deposit               |
|            | withdraw        | Withdraw balance from an existing Deposit             |
|            | update-settings | Update settings of an existing Deposit                |
| stake      |                 | Stake to a particular pool                            |
|            | stake           | Increase stakes to an existing Pool                   |
|            | unstake         | Remove stakes from an existing Pool                   |
| pool       |                 | Create and manage Pool                                |
|            | create          | Instantiation of a Pool in the network account        |
|            | update-settings | Update settings of an existing Pool                   |
|            | delete          | Delete an existing Pool in the network account        |

#### Create new Transaction File
`Transaction` in ParallelChain protocol specifies a set of parameters included in the instruction. You don't need to provide all parameters, some of them would be computed and filled in automatically when you submit the transaction.

```sh
pchain_client transaction create --help
```

First, provide the following 4 parameters:
```sh
pchain_client transaction create \
  --nonce <NONCE> \
  --gas-limit <GAS_LIMIT> \
  --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
  --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
...
```
Then, decide the transaction type using the [CLI subcommand](#prepare-transaction-file). Each of them takes different inputs. You can always check the help menu using `--help`.

Make sure you provide both `Parameters` and `Subcommand` parts in one command. The output transaction file (tx.json) will be saved in the current directory. You can also specify the designated file with the flag `--destination`

Example - Transfer tokens:
```sh
pchain_client transaction create \
  --nonce 0 \
  --gas-limit 100000 \
  --max-base-fee-per-gas 8 \
  --priority-fee-per-gas 0 \
  transfer \
    --recipient kRPL8cXI73DNgVSSQz9WfIi-mAAvFvdXKfZ9UPBEv_A \
    --amount 100
```

Example - Deploy contract and save to designated file `deposit-tx.json`:
```sh
pchain_client transaction create \
  --destination ~/Documents/deposit-tx.json \
  --nonce 0 \
  --gas-limit 100000 \
  --max-base-fee-per-gas 8 \
  --priority-fee-per-gas 0 \
  deploy \
    --contract-code /home/document/code.wasm \
    --cbi-version 0
```

#### Append Command to existing file
As explained in the beginning of [Transaction](#transaction) section, Transaction in ParallelChain protocol accepts sequence of commands. But you may find that `transaction create` in previous section only support a single Command in Transaction. 

If you want to support multiple Commands, use the following command with the [subcommand](#prepare-transaction-file). This appends a `Command` element to the back of the command array in Transaction. Please note that commands in array will be executed in sequential order.

Example:
```sh
pchain_client transaction append \
  --file ~/Documents/deposit-tx.json \
  transfer \
    --recipient kRPL8cXI73DNgVSSQz9WfIi-mAAvFvdXKfZ9UPBEv_A \
    --amount 100
```

### Submit Transaction to ParallelChain
After preparing the transaction json file, you can now submit the transaction with keypair.

Example:

```sh
pchain_client transaction submit \
--file <FILE> \
--keypair-name <KEYPAIR_NAME>
```

You will get a response like the following example if the transaction is accepted by your provider.
```sh
{
  "API Response:": "Your Transaction has been received.",
  "Command(s):": [
    {
      "Deploy":
      {
        "cbi_version": 0,
        "contract": "<contract in 53476 bytes>"
      }
    }
  ],
  "Contract Address:": "EH-0Im5Pb5mZQumIP6AAxyqTU7fBWQsNfLdGfaBh8AE",
  "Signature:": "DdRr2l-f3SwWtQP7M5JKdOUEvIb-th2mBrV1z06dkvB2rpp0qKQZwBBzJBh8czCqplUsmzSlSjPNrvOQbx2jAA",
  "Transaction Hash:": "POikFlLT8sVuVt3RHJvxmzPKP8dfvi55TrME6Muc80I"
}
```
