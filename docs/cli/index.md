---
tags:
  - testnet 1.0
  - parallelchain light client
---
# Commands Overview

## Setup

### For Windows 

This section will use `PowerShell` as the command line utility to install ParallelChain Light Client. Open up `PowerShell` using the `run` keyboard shortcut. That is *Ctrl+R* and type in `powershell` to proceed. 

Download the binaries from [https://cms.parallelchain.io/parallelchain-cli_win_x64_v0.1.0.zip](https://cms.parallelchain.io/parallelchain-cli_win_x64_v0.1.0.zip)

Unzip the compressed zip file by using the following `PowerShell` command. Make sure that you replace the two variables below with your own.
* `<SOURCE_PATH>`: the directory where `parallelchain-cli_win_x64_v0.1.0.zip` is located.
* `<DESTINATION_PATH>`: the directory you intend to install `pchain`. 
```PowerShell
Expand-Archive -LiteralPath 'C:\<SOURCE_PATH>\parallelchain-cli_win_x64_v0.1.0.zip' -DestinationPath 'C:\<DESTINATION_PATH>\parallelchain-cli_win_x64_v0.1.0.exe'
```

Switch the operating mode of `PowerShell` from a normal mode to administrator mode with this command:
```PowerShell
Start-Process powershell -Verb runAs
```

Head to the destination directory where `parallelchain-cli_win_x64_v0.1.0.exe` is extracted:
```PowerShell
Set-Location C:\<DESTINATION_PATH>\
```

In the destination directory (`<DESTINATION_PATH>`), we need to rename the binary to `pchain` so that it is easier to follow this guide:
```PowerShell
Rename-Item -Path 'parallelchain-cli_win_x64_v0.1.0.exe' -NewName 'pchain.exe'
```

<!-- We need to add the path to `pchain` to the system environment in windows. 
```PowerShell
[System.Environment]::SetEnvironmentVariable('PATH',$Env:PATH+';C:\<DESTINATION_PATH>\')
``` -->

Type the command `pchain` to see if it launches.
```PowerShell
./pchain.exe
```
<details>
  <summary>To verify that the Light Client works</summary>
    `pchain` is now an executable from anywhere on your system
    ```bash
    verylight 0.1.0
    <Parallel Chain Lab>
    Parallel Chain Mainnet verylight
    ```
</details>

Congratulations. You have successfully installed `pchain` and are ready to proceed to the following sections.

### For Linux

You can download the precompiled compressed binaries using the terminal command below:
```bash
wget https://cms.parallelchain.io/parallelchain-cli_linux_v0.1.0.tar.xz
```

After downloading the file `parallelchain-cli_linux_v0.1.0.tar.xz`, head to the directory where your binaries are located. 

Type this command to extract and use the light client immediately. **The following two commands require privileged access; you can look at `Administrative Action in Linux` at the bottom of this section on allowing these types of actions.**
```bash
sudo tar -xvf parallelchain-cli_linux_v0.1.0.tar.xz --directory /usr/bin 
```

We need to rename the binary to `pchain` so that it is easier to follow this guide:
```bash
sudo mv /usr/bin/parallelchain-cli_linux_v0.1.0 /usr/bin/pchain
```

<details>
  <summary>Administrative Action in Linux</summary>
When you add a "sudo" keyword to the front of a command in linux, you need to enter your password like this:
```bash
[sudo] password for my_user:
```
and press enter to continue with the operation.
</details>

Type the command `pchain` to see if it launches.
```bash
pchain
```
<details>
  <summary>To verify that the Light Client works</summary>
    `pchain` is now an executable from anywhere on your system
    ```bash
    verylight 0.1.0
    <Parallel Chain Lab>
    Parallel Chain Mainnet verylight
    ```
</details>

Congratulations. You have successfully installed `pchain` and are ready to proceed to the following sections.

## Launching `pchain`

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
You can check the account balance by provide the account address.
See ["Create a New Externally Owned Account (EOA)"](http://pc-tracy.digital-transaction.net:12345/cli/real_world_walkthrough/#create-a-new-externally-owned-account-eoa) for account address details.
```bash
pchain query account balance --address <ACCOUNT_ADDRESS>
```

###`Check nonce`
The nonce is the number of valid transactions sent from a given address. Each time you send a transaction, the nonce increases by 1.
```
pchain query account nonce --address <ACCOUNT_ADDRESS>
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
--nonce <FROM_ACCOUNT_NONCE> \
--keypair <FROM_ACCOUNT_KEYPAIR>
```
##### Transfer token
See ["EtoE: Transferring Tokens from One Account to Another"](/cli/real_world_walkthrough/#etoe-transferring-tokens-from-one-account-to-another) for detailed steps.

* `from-address`: the source account address. It is usually your account address.
* `to-address`: the destination account address. This address must be an Externally Owned Account(EOA).
* `value`: number in TXPLL. The token amount to transfer
* `gas-limit`: Any token transfer between EOAs consumes a fixed amount of gas. We recommend setting this to 44,600.
* `data`: no data is needed. Set this to `null`
* `nonce`: number. You can get the nonce by [`query command`](#check-nonce)
* `keypair`: your account keypair  

##### Deploy contract
See ["Building and deploying the contract"](/smart_contract_sdk/build_deploy_contract) for detailed steps.

* `from-address`: the source account address
* `to-address`: Set to `null`
* `value`: Set to 0
* `gas-limit`: It depends on the contract code size. We recommend setting the gas limit to at least _CODE\_SIZE * 105_
* `nonce`: number. You can get the nonce by [`query command`](#check-nonce)
* `keypair`: Your account keypair 

##### Call a contract
See ["Calling a Contract from an Externally Owned Account](/smart_contract_sdk/etoc_call) for detailed steps.

* `from-address`: the source account address
* `to-address`: contract address
* `value`: Set to 0
* `gas-limit`: We recommend setting the gas limit to _at least 500000 gas_
* `nonce`: number. You can get the nonce by [`query command`](#check-nonce)
* `keypair`: your account keypair that is generated before 

###`Query transaction`
Once you submitted a transaction, you will get the transaction hash on terminal output. You can check the transaction details with this hash.
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
Block number refers to the number of blocks that have been created on the blockchain prior to the block. You can get the block number of the block by its hash. See ["Query blcok number"](#query-block-number)
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
* `key`: The key in the world state.

```bash
pchain query state \
--block-hash <BLOCK_HASH> \
--address <ADDRESS> \
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
