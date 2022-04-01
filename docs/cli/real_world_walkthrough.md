---
tags:
  - testnet 1.0
  - parallelchain light client
  - walkthrough
---

# Real World Walkthrough

`Learning outcome`: _Run all four kinds of transactions and check its result using `pchain`._

## Acronyms

* `EOA`: Externally Owned Account

There are three kinds of transactions in the testnet:

* `EtoE`: EOA to EOA (Token transfers between EOAs)
* `EtoC`:  EOA to Contract Account (Contract calls)
* `DeployC`: Deploy Contract


## Create a new Externally Owned Account (EOA)

In `testnet1`, accounts are divided into two types:

- __Externally Owned Accounts (EOA)__: a keypair that is `ed25519_dalek` compatible
- __Contract Accounts__: a key that is generated from deploying a contract 

These accounts are indistinguishable from one another. In future versions of the testnet, it is easy to distinguish between contract accounts and externally owned accounts. 

To generate an account, type the following command below:
```bash
pchain set key --generate
```

This command generates a set of ed25519_dalek compatible keys. These are the keys required for making transactions in the network. Keep these details in a safe place. You cannot retrieve the account details again. An example is shown below:

**IMPORTANT: Keep the keys in a safe place.**

<details><summary>Terminal Output</summary>
```bash
Private Key: "V9MFJMnGcZwzJWNJUzAIKmhjQTl3MPq0BXuZZYbN7Ww="
Public Key: "/5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs="
Keypair: "V9MFJMnGcZwzJWNJUzAIKmhjQTl3MPq0BXuZZYbN7Wz/misQ24j+G1vC0DK77e3qtif3Sre+XLXJV2iwEe41Kw=="
```
</details>

You will require the keypair and public key to submit transactions within the testnet.

You can check the account balance using the account address (public key).
```bash
pchain query account balance --address /5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs=
```
<details><summary>Terminal Output</summary>
```bash
Your value is: "0"
```
</details>

The balance of a new account is 0. Therefore, you need to request for tokens from faucet. Please refer to [Faucet](/faucet) for details.

## `EtoE`: Transferring Tokens from One Account to Another

Before you transfer token to someone, you need to know his account address. In here, we just create another dummy account for our tutorial. You will only need the public key as the receiver address in EtoE transaction.
```bash
pchain set key --generate
```
<details><summary>Terminal Output</summary>
```bash
Private Key: "0AgnYPHTrQ9gYywgyNOD2RtPuPvCRUcd78VBuVETiDQ="
Public Key: "UFG7PkGOOu4tdg9sYnplB1jlwClwuHimirA3S/kOhVw="
Keypair: "0AgnYPHTrQ9gYywgyNOD2RtPuPvCRUcd78VBuVETiDRQUbs+QY467i12D2xiemUHWOXAKXC4eKaKsDdL+Q6FXA=="
```
</details>

Check your account nonce. It is important to keep track of this number. You will need this number to submit transaction.
```bash
pchain query account nonce --address <YOUR_ACCOUNT_ADDRESS>
```
<details><summary>Click to view real world example</summary>

Do not blindly copy the values in the real world example
```bash
pchain query account nonce --address /5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs=
```
</details>
<details><summary>Terminal Output</summary>
```bash
Your value "0"
```
</details>

Transfer tokens from your account to the dummy acccount using `pchain`
```bash
pchain submit tx \
--from-address  <YOUR_ACCOUNT_ADDRESS> \
--to-address <DUMMY_ACCOUNT_ADDRESS> \
--value <AMOUNT_TO_TRANSFER> \
--tip 0 \
--gas-limit 50000 \
--gas-price 1 \
--data null \
--nonce <ACCOUNT_NONCE> \
--keypair <ACCOUNT_KEYPAIR>
```
The real world example code used will be denoted like this:

<details><summary>Click to view real world example</summary>

Do not blindly copy the values in the real world example
```bash
pchain submit tx \
--from-address /5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs= \
--to-address UFG7PkGOOu4tdg9sYnplB1jlwClwuHimirA3S/kOhVw= \
--value 50 \
--tip 0 \
--gas-limit 50000 \
--gas-price 1 \
--data  null \
--nonce 0 \
--keypair V9MFJMnGcZwzJWNJUzAIKmhjQTl3MPq0BXuZZYbN7Wz/misQ24j+G1vC0DK77e3qtif3Sre+XLXJV2iwEe41Kw==
```
</details>
<details><summary>Terminal Output</summary>
```bash
Hash of tx: "/dg7QxEUKDApdO/OuOufGewo1r6OO/XXLsFay65ozlg="
Status 200
Response "Your request has been received."
```
</details>

You can check the transaction 
history with `transaction hash` using `pchain`, or check at [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer)
```bash
pchain query block --tx-hash <TRANSACTION_HASH>
```
<details><summary>Click to view real world example</summary>

Do not blindly copy the values in the real world example
```bash
pchain query block --tx-hash /dg7QxEUKDApdO/OuOufGewo1r6OO/XXLsFay65ozlg=
```
</details>
<details><summary>Terminal Output</summary>
```bash
Your Block: Block {    
    header: BlockHeader {
        blockchain_id: 0,
        block_version_number: 0,
        timestamp: 1648711616,
        prev_block_hash: "BqjbkODoCFZfHct0bjMITmbE7B/Si0BCVg8lnlaOJ6A=",
        this_block_hash: "hLEzWEpiRiFNsdTYDn7k7M42lftxW2+/q9wut25Sq0U=",
        txs_hash: "D5OpkQZY8g3OMyxDKLLGVFBSb4d/pO92n3SAqshyCCA=",
        state_hash: "xChhGxck++VDr/MfObhQ/9aX4StDld7Dh7PUBPFxtFk=",
        events_hash: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
        receipts_hash: "7LFJwDJBsD+fu7kivOXukaW0eGkzeBiJk/zhOeKFRLA=",
        proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
        signature: "aNlTXwSm8kYJqd29pxpRaCY1+SeszjUejb7DPPK8h9YloXjQ4PDHXmUJCWY+hcWsVjGFSBOynwmmiydGzjFJCA==",
    },
    transactions: [
        Transaction {
            from_address: "/5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs=",
            to_address: "UFG7PkGOOu4tdg9sYnplB1jlwClwuHimirA3S/kOhVw=",
            value: 50,
            tip: 0,
            gas_limit: 50000,
            gas_price: 1,
            data: "",
            n_txs_on_chain_from_address: 0,
            hash: "/dg7QxEUKDApdO/OuOufGewo1r6OO/XXLsFay65ozlg=",
            signature: "ioMFMSC4POjTtcQ4mTneo9pGEyWf14hPEou+XY6DF1gwmQSEX5DbrwmmR21ffCTG3IZmC90HZeUxSFz+RSOyDw==",
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

Please refer to ["Transaction Status Code"](/protocol/receipt_and_status/#status-code) section for details.


## `DeployC`: Deploying your Smart Contract

We will walk through the process of deploying your own smart contract. In this section, we will use the `bank` smart contract.

Remember to get your latest account nonce before submitting transaction.

Deploy the contract using `pchain`
```bash
pchain submit tx \
--from-address  <YOUR_ACCOUNT_ADDRESS> \
--to-address null \
--value 0 \
--tip 0 \
--gas-limit <GAS_LIMIT> \
--gas-price 1 \
--data <RELATIVE_PATH_TO_WASM_BINARIES> \
--nonce <ACCOUNT_NONCE> \
--keypair <ACCOUNT_KEYPAIR>
```

The real world example code used will be denoted like this:
<details><summary>Click to view real world example of a DeployC transaction</summary>
Do not blindly copy the values in the real world example 
```bash
./pchain submit tx \
--from-address /5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs= \
--to-address null \
--value 0 \
--tip 0 \
--gas-limit 10000000 \
--gas-price 1 \
--data "../wasm32-unknown-unknown/release/minified-bank.wasm" \
--nonce 1 \
--keypair V9MFJMnGcZwzJWNJUzAIKmhjQTl3MPq0BXuZZYbN7Wz/misQ24j+G1vC0DK77e3qtif3Sre+XLXJV2iwEe41Kw==
```
</details>

You should get the `contract address` and `transaction hash` if the command has succeeded. 

<details><summary>Terminal Output</summary>
```bash
Try to retrieve contract from designated path
Contract address: "Ns9DuNe8aS5QISfCyjEoAcZq20OVr2nKQTKsYGmo/Jw="
Hash of tx: "JOKxDTNqlMUjZ65DDxONrRkOjPOKqW3zO44F48SJOec="
Status 200
Response "Your request has been received."
```
</details>

A smart contract is correctly deployed if you get a stream of bytes in the terminal through the `contract-code` flag.
```bash
pchain query account contract-code --address <CONTRACT_ADDRESS>
```

<details><summary>Click to view real world example to query a smart contract's code</summary>
```bash
pchain query account contract-code --address Ns9DuNe8aS5QISfCyjEoAcZq20OVr2nKQTKsYGmo/Jw=
```
</details>

<details><summary>Terminal Output (Warning: Very Long Code)</summary>
```bash
Your value "AGFzbQEAAAABdRJgAn9/AX9gA39/fwF/YAJ/fwBgAX8Bf2ADf39/AGABfwBgBH9/f38AYAF/AX5gBX9/f39/AGAEf39/fwF/YAAAYAN/f38BfmAGf39/f39/AX9gAn9/AX5gBX9/f39/AX9gA35/fwF/YAZ/f39/f38AYAABfwKIAQYDZW52B3Jhd19zZXQABgNlbnYKcmF3X3JldHVybgACA2VudghyYXdfZW1pdAACA2Vudh9yYXdfZ2V0X3BhcmFtc19mcm9tX3RyYW5zYWN0aW9uAAMDZW52HnJhd19nZXRfcGFyYW1zX2Zyb21fYmxvY2tjaGFpbgADA2VudgdyYXdfZ2V0AAEDxQHDAQoEAwEIAgEBDAECBQAEAwICCQ0GAQEJAQIABAIAAAABAwAAAAAOAAIIDwAEBAEFBAIEAgACAAAABgABAAQQAgYAAAAAAwgAAwQEBAQAAAACEQAAAAQAAAAAAAAABgQABgQBAQIBBQAAAwkEAAUCAAYGCwQEAwAABgIAAAMFAAIECwcFBQUGAgIDAgIBAAAAAAMFAAAAAAMFAAAAAAIDAAADAwEDAwIDCgABAAAAAwADAwMDAQACAAAAAwMHBwMHBwUEAgQFAXABb28FAwEAEQYZA38BQYCAwAALfwBBqNbAAAt/AEGw1sAACwc4BQZtZW1vcnkCAAhjb250cmFjdAAGBWFsbG9jAHoKX19kYXRhX2VuZAMBC19faGVhcF9iYXNlAwIJrQEBAEEBC26TAXGYAbABf5cBaipgxgGSAbIBlwGbAZoBuwHCAcMBjQHGAWcnW50BEsEBZLEBlwHGAWmMAcYBWGtsciOAAZcBWaABngGuAcYBqAE7XIgBgwGCAVPGAXvGAZkBxgFUiwHHAWJjZWZ1eHZ5yAHGAWgoXR+hAW2JAZoBigE4RHOiAcQBwQG0AZQBxgFoKV58Uj+kAaUBnwFMIsYBxQEdPWEwrwE8X7oBOQqx8wPDAZ9fAhV/BH4jAEHQAWsiBiQAIwBB8AFrIgMkACADQQA2AjAgA0EwahADIQogA0H4AGohByADKAIwIgshASMAQdABayIAJAAgAEEUaiIFIAo2AgAgACABNgIQIABCADcDCCAAIABBCGo2AhwgAEEgaiIBQgA3AwAgAUHcAGpCADcCACABQaCnwAAoAgAiBDYCWCABQdAAakIANwMAIAEgBDYCTCABQcQAakIANwIAIAEgBDYCQCABQThqQgA3AwAgASAENgI0IAFBLGpCADcCACABIAQ2AiggAUEIakIANwMAIAFBEGpCADcDACABQRhqQgA3AwAgAUEga......"
```
</details>

If there is no result from querying the smart contract code, your transaction might have failed with a specific status code. You can check the transaction 
history with `transaction hash` using `pchain`, or check at [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer)
```bash
pchain query block --tx-hash <TRANSACTION_HASH>
```
<details><summary>Click to view real world example to query block</summary>
```bash
pchain query block --tx-hash JOKxDTNqlMUjZ65DDxONrRkOjPOKqW3zO44F48SJOec=
```
</details>
<details><summary>Terminal Output (Warning: Very Long Code)</summary>
```bash
Your Block: Block {
    header: BlockHeader {
        blockchain_id: 0,
        block_version_number: 0,
        timestamp: 1648712232,
        prev_block_hash: "hLEzWEpiRiFNsdTYDn7k7M42lftxW2+/q9wut25Sq0U=",
        this_block_hash: "JOKxDTNqlMUjZ65DDxONrRkOjPOKqW3zO44F48SJOec=",
        txs_hash: "NdMebZGmIqwKRTxsFGiHTIlNsw+s9XB0MGE1p70sFU8=",
        state_hash: "xChhGxck++VDr/MfObhQ/9aX4StDld7Dh7PUBPFxtFk=",
        events_hash: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
        receipts_hash: "4nDvbTBnOr56/KgFdsUb4TBRb43RMk1/w29sEGxeHro=",
        proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
        signature: "g96TxeHABU8GxhXMgsXoojqJ/bmxh8JC37f26BlV9ACsmsjaibz5bjcKpNZAL54WpQtF3H+o2nLT2Nabmtl6Ag==",
    },
    transactions: [
        Transaction {
            from_address: "/5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs=",
            to_address: "AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=",
            value: 0,
            tip: 0,
            gas_limit: 7000000,
            gas_price: 1,
            data: "AGFzbQEAAAABdRJgAn9/AX9gA39/fwF/YAJ/fwBgAX8Bf2ADf39/AGABfwBgBH9/f38AYAF/AX5gBX9/f39/AGAEf39/fwF/YAAAYAN/f38BfmAGf39/f39/AX9gAn9/AX5gBX9/f39/AX9gA35/fwF/YAZ/f39/f38AYAABfwKIAQYDZW52B3Jhd19zZXQABgNlbnYKcmF3X3JldHVybgACA2VudghyYXdfZW1pdAACA2Vudh9yYXdfZ2V0X3BhcmFtc19mcm9tX3RyYW5zYWN0aW9uAAMDZW52HnJhd19nZXRfcGFyYW1zX2Zyb21fYmxvY2tjaGFpbgADA2V
            ....
            "
            n_txs_on_chain_from_address: 1,
            hash: "JOKxDTNqlMUjZ65DDxONrRkOjPOKqW3zO44F48SJOec=",
            signature: "qPKaUnDvNiYJBvJlWmtaBe0PyRrCLvR26yjTd/Qklee9AZSHojIq3wBfsMoYXmluOrKNFp4qmU9jwHxYklftAQ==",
        },
    ],
    events: [],
    receipts: [
        Receipt {
            status_code: [
                1,
            ],
            gas_consumed: 0,
            return_value: [],
        },
    ],
}
```
</details>
Please refer to ["Transaction Status Code"](/protocol/receipt_and_status/#status-code) section for details.

## `EtoC`: Making calls to your Smart Contract

Please make sure your account has enough balance to pay for transactions. 
```bash
pchain query account balance --address <YOUR_ACCOUNT_ADDRESS>
```

Check your account nonce. 
```bash
pchain query account nonce --address <YOUR_ACCOUNT_ADDRESS>
```

Submit transaction with your account nonce and contract address. You should get the transaction hash if the command has succeeded.
The gas limit is depended on the complexity of the smart contract. On average, it takes no more than 500000 gas to execute. You can always set a higher gas limit for safety.

```bash
pchain submit tx \
--from-address  <YOUR_ACCOUT_ADDRESS> \
--to-address null <CONTRACT_ADDRESS> \
--value 0 \
--tip 0 \
--gas-limit <GAS_LIMIT> \
--gas-price 1 \
--data <BASE64_ENCODED_ARGUMENT> \
--nonce <ACCOUNT_NONCE> \
--keypair <ACCOUNT_KEYPAIR>
```
<details><summary>Click to view real world example to call the smart contract</summary>
```bash
./pchain submit tx \
--from-address  /5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs= \
--to-address Ns9DuNe8aS5QISfCyjEoAcZq20OVr2nKQTKsYGmo/Jw= \
--value 0 \
--tip 0 \
--gas-limit 1000000 \
--gas-price 1 \
--data AwAAAEFsaQQAAABCYWJhCgAAAGpIRDIzVmtCaXkBECcAAAAAAAAA \
--nonce 2 \
--keypair V9MFJMnGcZwzJWNJUzAIKmhjQTl3MPq0BXuZZYbN7Wz/misQ24j+G1vC0DK77e3qtif3Sre+XLXJV2iwEe41Kw==
```
</details>
<details><summary>Terminal Output</summary>
```bash
Hash of tx: "PWWBPzaKfRLDSSmL5iGvxhRnF7BcXmhIlBO9vI4AKFE="
Status 200
Response "Your request has been received."
```
</details>

Check the transaction 
history with `transaction hash` using `pchain`, or check at [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer)
```bash
pchain query block --tx-hash <TRANSACTION_HASH>
```
<details><summary>Click to view real world example to query block</summary>
```bash
pchain query block --tx-hash PWWBPzaKfRLDSSmL5iGvxhRnF7BcXmhIlBO9vI4AKFE=
```
</details>
<details><summary>Terminal Output</summary>
```bash
Your Block: Block {
    header: BlockHeader {
        blockchain_id: 0,
        block_version_number: 0,
        timestamp: 1648713238,
        prev_block_hash: "4d31WnmCFNyRCPUn15ejDvpYu7Ctt/GbnLyRnVeYqCk=",
        this_block_hash: "uibwQtGyEwVRGi+afoOXDrgAN2ZIOwkGDR5m9+J4gco=",
        txs_hash: "jhmR5Uts2EHxIElDXvV4bUThqiA9R+s1966E2QK+bC8=",
        state_hash: "nyQ3Uje/RQcX7f1+xvoM90jpUdJjBYeTava6RiaNp3k=",
        events_hash: "IQneqPwum+GRnGJWrFlV7YSoYASTkUKaFc5LJ81YcdY=",
        receipts_hash: "lSrEkgBqDciO//fAYcVmN+C1itdY15wUaZR+OEF7Pg4=",
        proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
        signature: "v0RuefGM6z8tgamsv7OiN8LHHLYhwRhRWWyuEXWDsOe4jwORzxVm4VqW7Izyr3b/lcZjp/beH4fOkdBNgspzDQ==",
    },
    transactions: [
        Transaction {
            from_address: "/5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs=",
            to_address: "Ns9DuNe8aS5QISfCyjEoAcZq20OVr2nKQTKsYGmo/Jw=",
            value: 0,
            tip: 0,
            gas_limit: 1000000,
            gas_price: 1,
            data: "AwAAAEFsaQQAAABCYWJhCgAAAGpIRDIzVmtCaXkBECcAAAAAAAAA",
            n_txs_on_chain_from_address: 2,
            hash: "PWWBPzaKfRLDSSmL5iGvxhRnF7BcXmhIlBO9vI4AKFE=",
            signature: "0zNUv41i8KOKkcl1ug5RwSujVUBMmyA0zZgMv9Ttm5mLgTYkvneI2nZhKyZf3Secmo8hgVeeYWKBB9lmLjd4Cg==",
        },
    ],
    events: [
        Event {
            topic: "bank_account: Open",
            value: "Successfully opened \n            account for Ali, Baba \n            with account_id: jHD23VkBiy",
        },
    ],
    receipts: [
        Receipt {
            status_code: [
                0,
            ],
            gas_consumed: 170377,
            return_value: [],
        },
    ],
}
```
</details>

## Comparison Table

Although the three transactions appear similar when using `pchain`, there are notable differences in their argument values. The table below shows this:

|        Fields       |  EtoE                         |   DeployC                |       EtoC               |
| ------------------- |-------------------------------| ------------------------ |------------------------- |
|   `from_address`    | Your account address          | Your account address     | Your account address     |
|   `to_address`      | Destination account address   | null                     | Contract address         |
|   `value`           | The token amount to transfer  | Set to 0                 | Set to 0                 |
|   `gas_limit`       | Set to 50000                  | At least CODE_SIZE * 105 | At least 500000 gas      |
|   `data`            | null                          |  Relative path to your smart contract | Base64 encoded of the contract arguments|
