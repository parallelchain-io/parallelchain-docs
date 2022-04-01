---
tags:
  - testnet 1.0
  - parallelchain light client
---
# Commands Overview

## Setup
See ["Setup ParallelChain Light Client"](/smart_contract_sdk/prepare_env/#setup-parallelchain-light-client) to configure `pchain`.

|   Command                  |      Description                                        |
| -------------------------- | ------------------------------------------------------- |
| [`pchain`](#version) | Check the version number of the light client |
| [`pchain config`](#config) | View/edit the configuration of the light client |

### `Version`
Check the version of the light client installed on your machine.
```bash
pchain --version
```

### `Config`
To configure the light client to connect with our testnet:
```bash
pchain set config --target-address https://testnet1.digital-transaction.net
```


## Account

|   Command                  |      Description                                        |
| -------------------------- |-------------------------------------------------------- |
| [`pchain set key`](#account-creation)       | Generate keypair for account creation |
| [`pchain query account balance`](#check-balance)  | Check account balance |
| [`pchain query account nonce`](#check-nonce)  | Check account nonce |

###`Account creation`

This command generates a set of ed25519_dalek compatible keys. These are the keys required for making transactions in the network. Keep these details in a safe place. You cannot retrieve the account details again.
```bash
pchain set key --generate
```

###`Check balance`
You can check your account balance through your address
```bash
pchain query account balance --address <YOUR_ACCOUNT_ADDRESS>
```

###`Check nonce`
You can check your account nonce through your address.
```bash
pchain query account nonce --address <YOUR_ACCOUNT_ADDRESS>
```


## Transaction 

|   Command                  |      Description                                        |
| -------------------------- |-------------------------------------------------------- |
| [`pchain submit tx`](#submit-transaction)    | Submit transactions to ParallelChain Blockchain |
| [`pchain query tx`](#query-transaction)    | Get executed transaction information with transaction hash|

###`Submit transaction`
You can submit different kinds of transactions using the same command with varying arguments. All arguments are required.

```bash
./pchain submit tx \
--from-address <FROM_ADDRESS> \
--to-address <TO_ADDRESS> \
--value <VALUE> \
--tip <TIP> \
--gas-limit <GAS_LIMIT> \
--gas-price 1 \
--data <DATA> \
--nonce <YOUR_ACCOUNT_NONCE> \
--keypair <YOUR_ACCOUNT_KEYPAIR>
```
##### Transfer token
* `from-address`: the source account address. It is usually your account address.
* `to-address`: the destination account address. This address must be an Externally Owned Account(EOA).
* `value`: the token amount to transfer
* `gas-limit`: Any token transfer between EOAs consumes a fixed amount of gas. We recommend setting this to 44,600.
* `data`: no data is needed. Set this to `null`
* `nonce`: you can get the nonce by ['query command'](#check-nonce)
* `keypair`: your account keypair  

##### Deploy contract
See ["Building and deploying the contract"](/smart_contract_sdk/build_deploy_contract) for detailed steps.

* `from-address`: your account address
* `to-address`: Set to `null`
* `value`: Set to 0
* `gas-limit`: It depends on the contract code size. We recommend setting the gas limit to at least _CODE\_SIZE * 105_
* `nonce`: You can get the nonce by [`query command`](#check-nonce)
* `keypair`: Your account keypair 

##### Call a contract
See ["Calling a Contract from an Externally Owned Account](/smart_contract_sdk/etoc_call) for detailed steps.

* `from-address`: your account address
* `to-address`: contract address
* `value`: Set to 0
* `gas-limit`: We recommend setting the gas limit to _at least 500000 gas_
* `nonce`: you can get the nonce by [`query command`](#check-nonce)
* `keypair`: your account keypair that is generated before 

###`Query transaction`
You can check the transaction details with the transaction hash.
```bash
pchain query tx --tx-hash <TRANSACTION_HASH>
```

## Block 
|   Command                  |      Description                                        |
| -------------------------- |-------------------------------------------------------- |
| [`pchain query block`](#query-single-block) | Get information of a particular block (also for latest block)|
| [`pchain query multi-blocks-query`](#query-multiple-block)    | Get multiple blocks information|
| [`pchain query block-num-query`](#query-block-number)    | Get the block number with block hash |

###`Query single block`

##### By Block number
```bash
pchain query block --block-num <BLOCK_NUM>
```
##### By Block hash
```bash
pchain query block --block-hash <BLOCK_HASH>
```
##### By Transaction hash
```bash
pchain query block --tx-hash <TRANSACTION_HASH>
```
##### Latest block
```bash
pchain query block --latest
```

###`Query multiple block`
You can get multiple block information through two parameters. These are the "**block hash of starting block**" and the "**query window size**".
```bash
pchain query multi-blocks-query --block-hash <BLOCK_HASH> --size <SIZE>
```

###`Query block number`
You can get the block number through the "**block hash**".
```bash
pchain query block-num-query --block-hash <BLOCK_HASH>
```

## World state 
|   Command                  |      Description                                        |
| -------------------------- |-------------------------------------------------------- |
| [`pchain query state`](#query-the-state)  | Get data from world state with provided key|

###`Query the state`
You can query data from the `world state` in a particular block.

* `block-hash`: The hash of the block to query.
* `address`: It can be either an Externally Owned Account(EOA) or a Contract Account
* `visibility`: **Must** be `public` or `private`. Set to `public` for contract account and `private` for EOA
* `key`: The key in the world state.

```bash
pchain query state \
--block-hash <BLOCK_HASH> \
--address <ADDRESS> \
--visibility <VISIBILITY> \
--key <KEY>
```

## Analyze
|   Command                  |      Description                                        |
| -------------------------- |-------------------------------------------------------- |
| [`pchain analyze gas-per-block`](#gas-usage-per-block)  | Get the gas usage per block for a specified period|
| [`pchain analyze mempool-size`](#mempool-size)  | Get mempool size for a specified period|

###`Gas usage per block`
You can get the gas usage of blocks for a specified period.

* `from-time`: Start time (_in unix timestamp formatting_)
* `to-time`: End time (_in unix timestamp formatting_)
* `window-size`:  Specified period of analysis. (_in seconds_)
* `step-size`: The amount of time that moves between each window (_in seconds_)

```bash
pchain analyze gas-per-block \
--from-time <START_TIME> \
--to-time <END_TIME> \
--window-size <WINDOW_SIZE> \
--step-size <SETO_SIZE>
```

###`Mempool size`
You can get the mempool size for a specified period.

* `from-time`: Start time (_in unix timestamp formatting_)
* `to-time`: End time (_in unix timestamp formatting_)
* `window-size`:  Specified period of analysis. (_in seconds_)
* `step-size`: The amount of time that moves between each window (_in seconds_)

```bash
pchain analyze mempool-size \
--from-time <START_TIME> \
--to-time <END_TIME> \
--window-size <WINDOW_SIZE> \
--step-size <STEP_SIZE>
```

## Help
You can always use the flag `--help` or `-h` to learn more about any built-in commands.
```bash
pchain --help
```
