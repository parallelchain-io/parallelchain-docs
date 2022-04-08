---
tags:
  - testnet 1.0
  - parallelchain light client
  - references
---

# `pchain` References

!!! Important
    **We recommend that you store `pchain` in a directory of your choice so that it is easier to follow the commands in the reference.** 
    
    **For Linux/macOS users** - **For example, we created a folder in our home directory called parallelchain_client**:
    ```bash
    $ mkdir -p /home/my_user/parallelchain_client
    $ cp pchain /home/my_user/parallelchain_client/
    $ cd /home/my_user/parallelchain_client
    $ ./pchain
    ```

    So from now on, when you see a command like this in linux/macOS. It means that `pchain` shall be executed from the directory you stored `pchain` in.
    ```bash
    ./pchain
    ```

    **For Windows users** - **Windows uses a different command structure compared to Linux and MacOS. All you need to do is to go to the directory where `pchain` is located. For example we have a command in `PowerShell` that shows this:**
    ```PowerShell
    PS> Set-Location C:Users\my_user\parallelchain_client
    C:Users\my_user\parallelchain_client> pchain.exe
    ```

    So from now on, when you see a command like this in Windows. It means that `pchain` shall be executed from the directory you stored `pchain` in.
    ```PowerShell
    pchain.exe
    ```

## Help
You can always use the flag `--help` or `-h` as the last flag for any commands in the following sections to learn more about any built-in commands.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain --help
    ```
=== "Windows"
    ```PowerShell
    pchain.exe --help
    ```

---

## **Launching `pchain`**

|   Command                  |      Description                                        |
| -------------------------- | ------------------------------------------------------- |
| [`pchain`](#version) | Check the version number of the light client |
| [`pchain config`](#config) | View/edit the configuration of the light client |

### **Version**

Checks the version of the light client installed on your machine.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain --version
    ```
=== "Windows"
    ```PowerShell
    pchain.exe --version
    ```

**Returns**

_Returns the version number of the light client_
<details>
  <summary>Output</summary>
    ```bash
    verylight 0.1.0
    <Parallel Chain Lab>
    Parallel Chain Mainnet verylight
    ```
</details>

### **Config**

Configures the light client to connect with our testnet:

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain set config --target-address https://testnet1.digital-transaction.net
    ```
=== "Windows"
    ```PowerShell
    pchain.exe set config --target-address https://testnet1.digital-transaction.net
    ```

**Returns**

_Returns a success message indicating the target address set._
<details>
  <summary>Output</summary>
    ```bash
    Success: Target Address has been setted.
    ```
</details>

---

## Account

|   Command                  |      Description                                        |
| -------------------------- |-------------------------------------------------------- |
| [`pchain set key`](#account-creation)       | Generate keypair for account creation |
| [`pchain query account balance`](#check-balance)  | Check account balance |
| [`pchain query account nonce`](#check-nonce)  | Check account nonce |

### **Account creation**

Generates a set of ed25519_dalek compatible keys required for making transactions in the network. Keep these details in a safe place. You cannot retrieve the account details again.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain set key --generate
    ```
=== "Windows"
    ```PowerShell
    pchain.exe set key --generate
    ```

**Returns**

_Returns a private key, public key and keypair to the user._
<details>
  <summary>Output</summary>
    ```bash
    Private Key: "YcJhQgEX/yY0/kKBwd7M3fyVRZ1DCaqrwfZSgxpqc1l="
    Public Key: "xNkAWN4XS/ejYtB9EwDbxzvKmWhuiJoLbpO1kleBKzd="
    Keypair: "YcJhQgEX/yY0/kKBwd7M3fyVRZ1DCaqrwfZSgxpqc1nE2QAY3hdL96Ni0H0TANvHO8qZaG6Imgtuk7WSV4ErNA=="
    ```
</details>

### **Check balance**

Checks the account balance by providing the account address.

`<ACCOUNT_ADDRESS>` can be an Externally Owned Account (EOA) or Contract Account. See ["Create a New Externally Owned Account (EOA)"](tutorial.md#create-a-new-externally-owned-account-eoa) for account address details.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain query account balance --address <ACCOUNT_ADDRESS>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query account balance --address <ACCOUNT_ADDRESS>
    ```

**Returns**

_Returns the account balance to the user._
<details>
  <summary>Output</summary>
    ```bash
    Your value "22475000"
    Your value(decoded) [219, 110, 59, 231, 77, 52]
    ```
</details>

### **Check nonce**

The nonce is the number of valid transactions sent from a given address. Each time you send a transaction, the nonce increases by 1. 

`<ACCOUNT_ADDRESS>` can be an Externally Owned Account (EOA) or Contract Account. See ["Create a New Externally Owned Account (EOA)"](tutorial.md#create-a-new-externally-owned-account-eoa) for account address details.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain query account nonce --address <ACCOUNT_ADDRESS>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query account nonce --address <ACCOUNT_ADDRESS>
    ```
**Returns**

_Returns the account nonce to the user._
<details>
  <summary>Output</summary>
    ```bash
    Your value is: "0"
    ```
</details>

---

## Transaction 

|   Command                  |      Description                                        |
| -------------------------- |-------------------------------------------------------- |
| [`pchain submit tx`](#submit-transaction)    | Submit transactions to ParallelChain Blockchain |
| [`pchain query tx`](#query-transaction)    | Get executed transaction information with transaction hash|

### **Submit transaction**

You can submit different kinds of transactions using `pchain submit tx`. The value in each field varies based on its usage. All arguments are required, and the testnet node returns the same acknowledgment message. See the points below on configuring each argument based on your usage:

* [Transfer Token](#transfer-token)
* [Deploy Contract](#deploy-contract)
* [Call a Contract](#call-a-contract)

#### **Transfer token**

See ["EtoE: Transferring Tokens from One Account to Another"](tutorial.md#etoe-transferring-tokens-from-one-account-to-another) for detailed steps.

* `<FROM_ADDRESS>`: the source account address. It is usually your account address.
* `<TO_ADDRESS>`: the destination account address. This address must be an Externally Owned Account(EOA).
* `<VALUE>`: number in TXPLL. The token amount to transfer
* `<GAS_LIMIT>`: Any token transfer between EOAs consumes a fixed amount of gas. We recommend setting this to 44,600.
* `<DATA>`: no data is needed. Set this to `null`
* `<FROM_ACCOUNT_NONCE>`: number. You can get the nonce by [`query command`](#check-nonce)
* `<FROM_ACCOUNT_KEYPAIR>`: your account keypair  

**Command**
=== "Linux / macOS"
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
=== "Windows"
    ```PowerShell
    pchain.exe submit tx \
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

**Returns**

_Returns a successful acknowledgment message from the testnet node, including the transaction hash. Use this hash to verify if your transaction has succeeded. Please see the section on ["Query transaction"](#query-transaction) for details_
<details>
  <summary>Output</summary>
    ```bash
    Hash of tx: "EB9nbVTRFZwxr3tVYmQI8fjawWxW/F0KVe6B1shWGr1="
    Status 200
    Response "Your request has been received."
    ```
</details>

##### Deploy contract
See ["Building and deploying the contract"](../smart_contract_sdk/build_deploy_contract.md) for detailed steps.

* `<FROM_ADDRESS>`: the source account address
* `<TO_ADDRESS>`: Set to `null`
* `<VALUE>`: Set to 0
* `<GAS_LIMIT>`: It depends on the contract code size. We recommend setting the gas limit to at least _CODE\_SIZE * 105_
* `<FROM_ACCOUNT_NONCE>`: number. You can get the nonce by [`query command`](#check-nonce)
* `<FROM_ACCOUNT_KEYPAIR>`: Your account keypair 

**Command**

=== "Linux / macOS"
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
=== "Windows"
    ```PowerShell
    pchain.exe submit tx \
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

**Returns**

_Returns a successful acknowledgment message from the testnet node, including the transaction hash. Use this hash to verify if your transaction has succeeded. Please see the section on ["Query transaction"](#query-transaction) for details_
<details>
  <summary>Output</summary>
    ```bash
    Hash of tx: "EB9nbVTRFZwxr3tVYmQI8fjawWxW/F0KVe6B1shWGr1="
    Status 200
    Response "Your request has been received."
    ```
</details>

##### Call a contract
See ["Calling a Contract from an Externally Owned Account](../smart_contract_sdk/etoc_call.md) for detailed steps.

* `<FROM_ADDRESS>`: the source account address
* `<TO_ADDRESS>`: contract address
* `<VALUE>`: Set to 0
* `<GAS_LIMIT>`: We recommend setting the gas limit to _at least 500000 gas_
* `<FROM_ACCOUNT_NONCE>`: number. You can get the nonce by [`query command`](#check-nonce)
* `<FROM_ACCOUNT_KEYPAIR>`: your account keypair

**Command**

=== "Linux / macOS"
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
=== "Windows"
    ```PowerShell
    pchain.exe submit tx \
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

**Returns**

_Returns a successful acknowledgment message from the testnet node, including the transaction hash. Use this hash to verify if your transaction has succeeded. Please see the section on ["Query transaction"](#query-transaction) for details_
<details>
  <summary>Output</summary>
    ```bash
    Hash of tx: "EB9nbVTRFZwxr3tVYmQI8fjawWxW/F0KVe6B1shWGr1="
    Status 200
    Response "Your request has been received."
    ```
</details>

### **Query transaction**

Checks the details of a transaction using a transaction hash that is obtained from submitting a transaction.

`<TRANSACTION_HASH>` is the hash obtained when you submit a transaction using `pchain`. See the return output in ["Submit Transaction"](#submit-transaction) for details.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain query tx --tx-hash <TRANSACTION_HASH>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query tx --tx-hash <TRANSACTION_HASH>
    ```
**Returns**

_Returns the transaction details that correspond to the transaction hash._
<details>
  <summary>Example Output</summary>
    ```bash
    Your Tx Transaction {
        from_address: "xNkAWN4XS/ejYtB9EwDbxzvKmWhuiJoLbpO1kleBKzd=",
        to_address: "bvxhscIRoJvmu2JQrJyTakMBufl1ogN4py84rtZl7sA=",
        value: 0,
        tip: 0,
        gas_limit: 44600,
        gas_price: 1,
        data: "",
        n_txs_on_chain_from_address: 0,
        hash: "EB9nbVTRFZwxr3tVYmQI8fjawWxW/F0KVe6B1shWGr1=",
        signature: "KRqJUE66qPFwnWOfMsUc9xO4q9ZFNrA8OILKS7dqQiVaaWmQda4pD7BgTr9cnz7R9p9ZvdSpWLze8skGpW97BS==",
    }
    ```
</details>

---

## Block 
|   Command                  |      Description                                        |
| -------------------------- |-------------------------------------------------------- |
| [`pchain query block`](#query-single-block) | Get information of a particular block (also for latest block)|
| [`pchain query multi-blocks-query`](#query-multiple-block)    | Get multiple blocks information|
| [`pchain query block-num-query`](#query-block-number)    | Get the block number with block hash |

You can query a single block or multiple block with the commands above. 

### **Query single block**

##### By Transaction hash

This command gets the details of a block through the transaction hash of one of its transactions.

`<TRANSACTION_HASH>` is the hash obtained when you submit a transaction using `pchain`. See the return output in ["Submit Transaction"](#submit-transaction) for details.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain query block --tx-hash <TRANSACTION_HASH>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query block --tx-hash <TRANSACTION_HASH>
    ```

**Returns**

_Returns the details of a block queried through the transaction hash._
<details>
  <summary>Example Output</summary>
    ```bash
    Your Block: Block {
        header: BlockHeader {
            blockchain_id: 0,
            block_version_number: 0,
            timestamp: 1649218702,
            prev_block_hash: "dhl8rTmp/UcHuPzbWC/EbCw+Eih9RwQiQ8qdriprf8Y=",
            this_block_hash: "rhIBT39sJY2/OxLTT5xtsMVNtpNIt2gLBSIUGUlzrD0=",
            txs_hash: "ATPVmMIWakFFXecV7yb/7C6MQHynabdPP4XLQM5/tFA=",
            state_hash: "NxkH4DwL6WnX45DL9JnmnOTwwMX2IlUENErBL6zA3PA=",
            events_hash: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
            receipts_hash: "7LFJwDJBsD+fu7kivOXukaW0eGkzeBiJk/zhOeKFRLA=",
            proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
            signature: "s2GBl2UxzUNE/2QZ1scB/Y6TSRDLrmG8v5XiaBRzQlpaK0RiLBLjeJurMOU97aBFoUr/8cxg0h3Q/p2I5WvyBg==",
        },
        transactions: [
            Transaction {
                from_address: "xNkAWN4XS/ejYtB9EwDbxzvKmWhuiJoLbpO1kleBKzd=",
                to_address: "bvxhscIRoJvmu2JQrJyTakMBufl1ogN4py84rtZl7sA=",
                value: 0,
                tip: 0,
                gas_limit: 44600,
                gas_price: 1,
                data: "",
                n_txs_on_chain_from_address: 0,
                hash: "EB9nbVTRFZwxr3tVYmQI8fjawWxW/F0KVe6B1shWGr1=",
                signature: "KRqJUE66qPFwnWOfMsUc9xO4q9ZFNrA8OILKS7dqQiVaaWmQda4pD7BgTr9cnz7R9p9ZvdSpWLze8skGpW97BS==",
            },
        ],
        events: [],
        receipts: [
            Receipt {
                status_code: [
                    0,
                ],
                gas_consumed: 44200,
                return_value: [],
            },
        ],
    }
    ```
</details>


##### Latest block

This command gets the details of the latest block in the blockchain.
=== "Linux / macOS"
    ```bash
    ./pchain query block --latest
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query block --latest
    ```
**Returns**

_Returns the details of the latest block._
<details>
  <summary>Example Output</summary>
    ```bash
    Your Block: Block {
        header: BlockHeader {
            blockchain_id: 0,
            block_version_number: 0,
            timestamp: 1649220017,
            prev_block_hash: "VBIVWsTFi5wAqGnvhG6p/bsJbTUSl2nWpzxEF8eQFF8=",
            this_block_hash: "RV9VhZvGyb0Z/7DF2FfYcdBq/yvsU1epxHEs9eNSYZA=",
            txs_hash: "sFNt1JOrs5geUJoghAtg/Jjy9+iGbtfKwQFWT+wmRc8=",
            state_hash: "vDCOmj0cwqLhgMPJkBbH6quJhpXRtnVxH1Mt7SZohmk=",
            events_hash: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
            receipts_hash: "dV7J+1GeOXqicQV0VKnYa29PxYvYrlPsJ58eb5LHAnM=",
            proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
            signature: "ZHtEkGe17u/9aEgSY9M6CR2oKxUC6AS40UvbzhrW3G61BHE70uW7NN7tkQy0nGYrPPS8z0bIBMs2E2/rGbCUAg==",
        },
        transactions: [
            Transaction {
                from_address: "Jw0yNqmQrHR9fQi5BpJu/wa1W3+nc1y3KzMh/XFax/A=",
                to_address: "3FCI7oThxiSkHJfM4wiQsWl2h1tthwoAsSjcngon4Lo=",
                value: 0,
                tip: 0,
                gas_limit: 20000000,
                gas_price: 1,
                data: "DAAAAEhhcnJ5IFBvdHRlcigAAAAB",
                n_txs_on_chain_from_address: 1,
                hash: "58B1swFyHMCxfxL71pEN/flKpi5KISQOaf90rNPVsmE=",
                signature: "ZZe4VD5PJ31Q4v02lYqvfHfEktUw7UDvusdwgEstp93YT6x8H1Q/PFHrlTlh5KvGy1tUKziPIWs/Lx/Kvj++AA==",
            },
        ],
        events: [],
        receipts: [
            Receipt {
                status_code: [
                    0,
                ],
                gas_consumed: 46400,
                return_value: [],
            },
        ],
    }
    ```
</details>

##### By Block number

This command gets the details of a block through its block number.

`<BLOCK_NUM>` refers to the number of blocks that have been created on the blockchain prior to this block. You can get the block number through this block's hash. See ["Query block number"](#query-block-number) on how to get the block number.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain query block --block-num <BLOCK_NUM>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query block --block-num <BLOCK_NUM>
    ```
**Returns**

_Returns the block details from the block number_
<details>
  <summary>Example Output</summary>
    ```bash
    Your Block: Block {
        header: BlockHeader {
            blockchain_id: 0,
            block_version_number: 0,
            timestamp: 1649219200,
            prev_block_hash: "jn2bJYTwDUi4mXj1TZwbdX38BiDBvjQeDgb0txguCfs=",
            this_block_hash: "8CG5Eqp3B/YMxzKyYWpWnD5XbXhPqXfp08vGOnZ8X1k=",
            txs_hash: "nETcvllPoZTJ2jN3F6VqxCJongWEuR8whaPq0DgYadk=",
            state_hash: "dSK/08ya1OM9k4pSQxOgv6rezedW/oePleRDFY9wcVc=",
            events_hash: "gJhGqqv5TFxM/oZ6Zv3jEDfA0nOHw1hEr70r8SPALQ4=",
            receipts_hash: "lr9Pg2qK6eAFQFl8elYtE+J1vaiUAgnI/l/HKYumvuY=",
            proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
            signature: "H8slMvVxBlATz1euYrQNYcSg0Yo8LQyOYC5LWPgUMMkSgyjk0JExVRDoj26iWIjTEtbDTw6K9aHvVVqz4MFLBw==",
        },
        transactions: [
            Transaction {
                from_address: "2oQRJJZ62AQteFw632lGYUVeEiHOaNn8oqLf3Q89jDU=",
                to_address: "yWBrd5PVuF9f9P7mD2tjRihf8AGRFXeRB8lk20PqRIk=",
                value: 0,
                tip: 0,
                gas_limit: 200000000,
                gas_price: 1,
                data: "CwAAAGhlbGxvIHdvcmxk",
                n_txs_on_chain_from_address: 13,
                hash: "f22B5G1Bzz0JzwRC92Ftzy829xGRBWwu/nlDmiDM7l0=",
                signature: "W7OKwBWGZuROkdLLzNIPSjbZa8sFJ4SdICOgkzG2lw2Qm6yedSxGUnCUb6POOfPQMaE95h+7l3CPL2GAMQo1BQ==",
            },
        ],
        events: [
            Event {
                topic: "my_first_contract: START",
                value: "The smart contract received an argument of: \"hello world\"",
            },
            Event {
                topic: "system: ",
                value: "total memory: 0\nused memory: 0\ntotal swap: 0\nused swap: 0\n\nSystem name: None\nSystem kernel version: None\n System OS version: None\nSystem host name:None\nnumber of processors: 0",
            },
            Event {
                topic: "total memory",
                value: "0",
            },
            Event {
                topic: "my_first_contract: END",
                value: "Completed Call",
            },
        ],
        receipts: [
            Receipt {
                status_code: [
                    0,
                ],
                gas_consumed: 195017,
                return_value: [
                    84,
                    104,
                    101,
                    32,
                    108,
                    101,
                    110,
                    103,
                    116,
                    104,
                    32,
                    111,
                    102,
                    32,
                    116,
                    104,
                    101,
                    32,
                    97,
                    114,
                    103,
                    117,
                    109,
                    101,
                    110,
                    116,
                    32,
                    105,
                    115,
                    58,
                    32,
                    49,
                    49,
                ],
            },
        ],
    }
    ```
</details>


##### By Block hash

This command gets the details of a block through its hash.

`<BLOCK_HASH>` refers to the block's hash before it was signed and committed onto the blockchain. You can get this value by using the previous commands for querying a block and inspecting the field `this_block_hash` in the output to get the block hash.

<details>
  <summary>Click to see where to find "this_block_hash" which is highlighted in the output.</summary>
    ```bash hl_lines="7"
    Your Block: Block {
        header: BlockHeader {
            blockchain_id: 0,
            block_version_number: 0,
            timestamp: 1649220017,
            prev_block_hash: "VBIVWsTFi5wAqGnvhG6p/bsJbTUSl2nWpzxEF8eQFF8=",
            this_block_hash: "RV9VhZvGyb0Z/7DF2FfYcdBq/yvsU1epxHEs9eNSYZA=",
            txs_hash: "sFNt1JOrs5geUJoghAtg/Jjy9+iGbtfKwQFWT+wmRc8=",
            state_hash: "vDCOmj0cwqLhgMPJkBbH6quJhpXRtnVxH1Mt7SZohmk=",
            events_hash: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
            receipts_hash: "dV7J+1GeOXqicQV0VKnYa29PxYvYrlPsJ58eb5LHAnM=",
            proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
            signature: "ZHtEkGe17u/9aEgSY9M6CR2oKxUC6AS40UvbzhrW3G61BHE70uW7NN7tkQy0nGYrPPS8z0bIBMs2E2/rGbCUAg==",
        },
        transactions: [
            Transaction {
                from_address: "Jw0yNqmQrHR9fQi5BpJu/wa1W3+nc1y3KzMh/XFax/A=",
                to_address: "3FCI7oThxiSkHJfM4wiQsWl2h1tthwoAsSjcngon4Lo=",
                value: 0,
                tip: 0,
                gas_limit: 20000000,
                gas_price: 1,
                data: "DAAAAEhhcnJ5IFBvdHRlcigAAAAB",
                n_txs_on_chain_from_address: 1,
                hash: "58B1swFyHMCxfxL71pEN/flKpi5KISQOaf90rNPVsmE=",
                signature: "ZZe4VD5PJ31Q4v02lYqvfHfEktUw7UDvusdwgEstp93YT6x8H1Q/PFHrlTlh5KvGy1tUKziPIWs/Lx/Kvj++AA==",
            },
        ],
        events: [],
        receipts: [
            Receipt {
                status_code: [
                    0,
                ],
                gas_consumed: 46400,
                return_value: [],
            },
        ],
    }
    ```
</details>

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain query block --block-hash <BLOCK_HASH>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query block --block-hash <BLOCK_HASH>
    ```
**Returns**

_Returns the block details from the block hash_
<details>
  <summary>Example Output</summary>
    ```bash
    Your Block: Block {
        header: BlockHeader {
            blockchain_id: 0,
            block_version_number: 0,
            timestamp: 1649220017,
            prev_block_hash: "VBIVWsTFi5wAqGnvhG6p/bsJbTUSl2nWpzxEF8eQFF8=",
            this_block_hash: "RV9VhZvGyb0Z/7DF2FfYcdBq/yvsU1epxHEs9eNSYZA=",
            txs_hash: "sFNt1JOrs5geUJoghAtg/Jjy9+iGbtfKwQFWT+wmRc8=",
            state_hash: "vDCOmj0cwqLhgMPJkBbH6quJhpXRtnVxH1Mt7SZohmk=",
            events_hash: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
            receipts_hash: "dV7J+1GeOXqicQV0VKnYa29PxYvYrlPsJ58eb5LHAnM=",
            proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
            signature: "ZHtEkGe17u/9aEgSY9M6CR2oKxUC6AS40UvbzhrW3G61BHE70uW7NN7tkQy0nGYrPPS8z0bIBMs2E2/rGbCUAg==",
        },
        transactions: [
            Transaction {
                from_address: "Jw0yNqmQrHR9fQi5BpJu/wa1W3+nc1y3KzMh/XFax/A=",
                to_address: "3FCI7oThxiSkHJfM4wiQsWl2h1tthwoAsSjcngon4Lo=",
                value: 0,
                tip: 0,
                gas_limit: 20000000,
                gas_price: 1,
                data: "DAAAAEhhcnJ5IFBvdHRlcigAAAAB",
                n_txs_on_chain_from_address: 1,
                hash: "58B1swFyHMCxfxL71pEN/flKpi5KISQOaf90rNPVsmE=",
                signature: "ZZe4VD5PJ31Q4v02lYqvfHfEktUw7UDvusdwgEstp93YT6x8H1Q/PFHrlTlh5KvGy1tUKziPIWs/Lx/Kvj++AA==",
            },
        ],
        events: [],
        receipts: [
            Receipt {
                status_code: [
                    0,
                ],
                gas_consumed: 46400,
                return_value: [],
            },
        ],
    }
    ```
</details>


### **Query multiple block**

This command shows the information across a vector of blocks. It takes two parameters that are the "**block hash of starting block**" and the "**query window size**"; denoted in the command as `<BLOCK_HASH>` and `<SIZE>` respectively.

To find out how to get the `<BLOCK_HASH>`, see ["Query Single Block"](#query-single-block) and observe the output to get this value.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain query multi-blocks-query --block-hash <BLOCK_HASH> --size <SIZE>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query multi-blocks-query --block-hash <BLOCK_HASH> --size <SIZE>
    ```
**Returns**

_Returns the vector of blocks and its corresponding details through a specified window_
<details>
  <summary>Example Output</summary>
    ```bash
    Your Blocks: Blocks {
        blocks: [
            Block {
                header: BlockHeader {
                    blockchain_id: 0,
                    block_version_number: 0,
                    timestamp: 1649219932,
                    prev_block_hash: "8CG5Eqp3B/YMxzKyYWpWnD5XbXhPqXfp08vGOnZ8X1k=",
                    this_block_hash: "VBIVWsTFi5wAqGnvhG6p/bsJbTUSl2nWpzxEF8eQFF8=",
                    txs_hash: "nETcvllPoZTJ2jN3F6VqxCJongWEuR8whaPq0DgYadk=",
                    state_hash: "dSK/08ya1OM9k4pSQxOgv6rezedW/oePleRDFY9wcVc=",
                    events_hash: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
                    receipts_hash: "/8yo5dgqHzr9eheXOX11s74gCyllvRGn6C+rHAYNen8=",
                    proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
                    signature: "43lh60w3Pje/zVnmVkIVhcbGEVnGJWGeVK835pEOjSVrLy14HhhGaBW87gtR4ZXuoRQ3DtRKw1QpdrS6qNxhCw==",
                },
                transactions: [],
                events: [],
                receipts: [
                    Receipt {
                        status_code: [
                            5,
                        ],
                        gas_consumed: 0,
                        return_value: [],
                    },
                ],
            },
            Block {
                header: BlockHeader {
                    blockchain_id: 0,
                    block_version_number: 0,
                    timestamp: 1649219200,
                    prev_block_hash: "jn2bJYTwDUi4mXj1TZwbdX38BiDBvjQeDgb0txguCfs=",
                    this_block_hash: "8CG5Eqp3B/YMxzKyYWpWnD5XbXhPqXfp08vGOnZ8X1k=",
                    txs_hash: "nETcvllPoZTJ2jN3F6VqxCJongWEuR8whaPq0DgYadk=",
                    state_hash: "dSK/08ya1OM9k4pSQxOgv6rezedW/oePleRDFY9wcVc=",
                    events_hash: "gJhGqqv5TFxM/oZ6Zv3jEDfA0nOHw1hEr70r8SPALQ4=",
                    receipts_hash: "lr9Pg2qK6eAFQFl8elYtE+J1vaiUAgnI/l/HKYumvuY=",
                    proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
                    signature: "H8slMvVxBlATz1euYrQNYcSg0Yo8LQyOYC5LWPgUMMkSgyjk0JExVRDoj26iWIjTEtbDTw6K9aHvVVqz4MFLBw==",
                },
                transactions: [],
                events: [],
                receipts: [
                    Receipt {
                        status_code: [
                            0,
                        ],
                        gas_consumed: 195017,
                        return_value: [
                            84,
                            104,
                            101,
                            32,
                            108,
                            101,
                            110,
                            103,
                            116,
                            104,
                            32,
                            111,
                            102,
                            32,
                            116,
                            104,
                            101,
                            32,
                            97,
                            114,
                            103,
                            117,
                            109,
                            101,
                            110,
                            116,
                            32,
                            105,
                            115,
                            58,
                            32,
                            49,
                            49,
                        ],
                    },
                ],
            },
        ],
    }
    ```
</details>

### **Query block number**

This command gets the block number through the "**block hash**".

To find out how to get the `<BLOCK_HASH>`, see ["Query Single Block"](#query-single-block) and observe the output to get this value.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain query block-num-query --block-hash <BLOCK_HASH>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query block-num-query --block-hash <BLOCK_HASH>
    ```
**Returns**

_Returns the block number from its hash_
<details>
  <summary>Example Output</summary>
    ```bash
    Your value "9127"
    Your value(decoded) [247, 93, 187]
    ```
</details>

---

## World state 
|   Command                  |      Description                                        |
| -------------------------- |-------------------------------------------------------- |
| [`pchain query state`](#query-the-state)  | Get data from world state with provided key|

### **Query the state**
You can query data from the `world state` in a particular block.

Where:

* `<BLOCK_HASH>`: The block's hash for the query.
* `<ADDRESS>`: The address of contract account.
* `<KEY>`: The key in the world state in format of base64 string.

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain query state \
    --block-hash <BLOCK_HASH> \
    --address <ADDRESS> \
    --key <KEY>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query state \
    --block-hash <BLOCK_HASH> \
    --address <ADDRESS> \
    --key <KEY>
    ```
**Returns**

_Returns the world state of a particular block_

<details>
  <summary>Example Output</summary>
  The value is in format of base64 string. If key is not found (or not set before), it outputs empty string (decoded to empty array)
    ```bash
    Your value "AAAAAA=="
    Your value(decoded) [0, 0, 0, 0]
    ```
</details>

---

## Analyze
|   Command                  |      Description                                        |
| -------------------------- |-------------------------------------------------------- |
| [`pchain analyze gas-per-block`](#gas-usage-per-block)  | Get the gas usage per block for a specified period|
| [`pchain analyze mempool-size`](#mempool-size)  | Get mempool size for a specified period|

### **Gas usage per block**
You can get the gas usage of blocks for a specified period.

* `<START_TIME>`: Start of the time window (_in unix timestamp formatting_)
* `<END_TIME>`: End of the time window (_in unix timestamp formatting_)
* `<WINDOW_SIZE>`:  Specified period of analysis. (_in seconds_)
* `<STEP_SIZE>`: The amount of time that moves between each window (_in seconds_)

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain analyze gas-per-block \
    --from-time <START_TIME> \
    --to-time <END_TIME> \
    --window-size <WINDOW_SIZE> \
    --step-size <STEP_SIZE>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe analyze gas-per-block \
    --from-time <START_TIME> \
    --to-time <END_TIME> \
    --window-size <WINDOW_SIZE> \
    --step-size <STEP_SIZE>
    ```

**Returns**

_Returns the gas usage of a block._
<details>
  <summary>Example Output</summary>
    The output array contains gas usage value representing period of &lt;WINDOW_SIZE&gt; seconds. The starting time for period is scheduled by every &lt;STEP_SIZE&gt; seconds. The lenght of the array varies depends on the input.
    ```bash
    Your value [0, 0, 5057700, 44200, 44200, 44200, 44200]
    ```
</details>

###`Mempool size`
You can get the mempool size for a specified period.

* `<START_TIME>`: Start of the time window (_in unix timestamp formatting_)
* `<END_TIME>`: End of the time window (_in unix timestamp formatting_)
* `<WINDOW_SIZE>`:  Specified period of analysis. (_in seconds_)
* `<STEP_SIZE>`: The amount of time that moves between each window (_in seconds_)

**Command**
=== "Linux / macOS"
    ```bash
    ./pchain analyze mempool-size \
    --from-time <START_TIME> \
    --to-time <END_TIME> \
    --window-size <WINDOW_SIZE> \
    --step-size <STEP_SIZE>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe analyze mempool-size \
    --from-time <START_TIME> \
    --to-time <END_TIME> \
    --window-size <WINDOW_SIZE> \
    --step-size <STEP_SIZE>
    ```
**Returns**

_Returns the mempool size for a specified time window_
<details>
  <summary>Example Output</summary>
    The length of the array varies depends on the input.
    ```bash
    Your value [0, 0, 0, 0, 0, 0, 0]
    ```
</details>