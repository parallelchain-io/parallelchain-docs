---
tags:
  - testnet 1.0
  - parallelchain sdk
---

# Transaction Receipt and Status
After submitting a transaction, you may want to check if your transaction was succeeded or failed.

There are two ways to check the status :

* On ParallelChain Testnet Explorer
* Using ParallelChain's Light Client (`pchain`)

## Check on ParallelChain Testnet Explorer
Go to [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer). There is a search bar on top right of the page. Select `Transaction` and provide your transaction hash to query all transaction-related information.

## Use ParallelChain Light Client (`pchain`)
Query block information of your transaction with transaction hash.
=== "Linux / macOS"
    ```bash
    ./pchain query block --tx-hash <TRANSACTION_HASH>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query block --tx-hash <TRANSACTION_HASH>
    ```
<details><summary>Block Information Example</summary>
```bash
Your Block: Block {
    header: BlockHeader {
        blockchain_id: 0,
        block_version_number: 0,
        timestamp: 1648640283,
        prev_block_hash: "4u6kcp8zLQ0Bu9XCZUtJ4523YKAdwb2ao388c89wyZM=",
        this_block_hash: "QZ2Fbo2Wk8Ui3/qI2m8pOEwqB5f18O94cVHMdJi10pc=",
        txs_hash: "IgSQYVXPQV1hCfnA08rjAuwecg4YGYrqN+wxYDM+N3s=",
        state_hash: "oXVVgMQzeaWVUUf7qyTYAAiup6//PIBUS4qCSbofs+4=",
        events_hash: "mNs5o9eEvAacv65cHlXEBZrdRP6tI2KCdwPepnTibvc=",
        receipts_hash: "VZR2OgElY6DD4Fo+hWJCPNYGrSsc+ieBym7kMfN2pG0=",
        proposer_public_key: "+kGldjWdZhTKKiHu47PlkqbasPEuSXaLkl13rBb0NpI=",
        signature: "zQKYo+DNJlTeKzzc+iUYzrGnoJLX0aANCBFoNaYOoVxxCvowPJSFPSOwdAotBq6thU1+SwOYqH2w2TpFC3rmAw==",
    },
    transactions: [
        Transaction {
            from_address: "+pCTEj/WWymtEGYfFY6WQhoN4blZQLB9MaB11b8nIeY=",
            to_address: "mQ7oIRuyhCy1SePMyEPc3C69LWdiY+mygL2EDC7L/sE=",
            value: 0,
            tip: 0,
            gas_limit: 300000000,
            gas_price: 1,
            data: "AwAAAEFsaQQAAABCYWJhJAAAADE1YzMxOThlLWEwM2UtNGVlOS05YTBhLWY5NWE1MjIwMjU4NwEA6HZIFwAAAAA=",
            n_txs_on_chain_from_address: 1320,
            hash: "9Hbm/zfgUGVtT+tvz3qzfA+8idyJ2M8lmJ5ZB6o3g1k=",
            signature: "rb33IiANvBBug1p0Us2adNzoZyvVY+aAYfz+IcJs/lJy5sHmOiZjlWErvjXjxlWYqOHrNzqC8xCGpDAojtitBw==",
        },
        Transaction {
            from_address: "7drQ4X12qHr3WEmtcHfqJntUH7yVFxWImScRMDS0QuM=",
            to_address: "lIru3j+3SJxN3lNnMWaeewmd7gkFgAs6OBVObVXayS8=",
            value: 0,
            tip: 0,
            gas_limit: 300000000,
            gas_price: 1,
            data: "AwAAAEFsaQQAAABCYWJhJAAAAGYwMjQ2MGIzLTc4M2ItNDljZi1hZmUwLTYzMzJmZTRkMWEzZQEQJwAAAAAAAAI=",
            n_txs_on_chain_from_address: 1325,
            hash: "+JSfc9aaT4Be2gPzLPZJIpjr+Yvnpa8BNzt7vqQG+6g=",
            signature: "TswFc4sEawDl+oDeUyY17ORbx92aGWFy+wK37BnJYJVTa6au6WGdkghCCe+raoQUGOGadwTFjKRBKQgHCkS9Dw==",
        },
    ],
    events: [
        Event {
            topic: "bank: withdraw_money",
            value: "The updated balance is: 100000000000",
        },
        Event {
            topic: "bank: open_account",
            value: "Account Name: Ali Baba\n\n Account Number: 68a5acfe-7c92-4950-9ae8-378d293ca970\n\n                        Balance: 100000000000",
        },
    ],
    receipts: [
        Receipt {
            status_code: [
                0,
            ],
            gas_consumed: 50800,
            return_value: [],
        },
        Receipt {
            status_code: [
                0,
            ],
            gas_consumed: 50800,
            return_value: [],
        },
    ],
}
```
</details>

In each block, you can find the following information : 

* `header`, Block header contains the metadata of this block.

* `transactions`, A list of transactions included in this block.

* `events`, 

* `receipts`, Transaction receipts with information of the execution results

The transaction stored in `transactions` and `receipts` in same order. That means you can always find the status of your transaction according to the ordinal position.

Inside the receipt, you can see the `status_code` in digit, and the gas consumed for the transaction.

## Status code
| code      | Description                          |
| --------- | ------------------------------------ |
| `0`       | Success  |
| `1`       | Failure (Insufficient Pre-execution Gas), <br>Insufficent account balance to pay for the transaction, <br>or set the gas limit too low |
| `2`       | Failure (Execution Gas Exhaustion), <br> _Only appear in EtoC transaction_. Insufficent gas limit to execute WASM binary|
| `3`       | Failure (Runtime Error), <br> _Only appear in EtoC transaction_. Error occurs when executing the contract functions|
| `4`       | Failure (Invalid Contract Called), <br> _Only appear in EtoC transaction_.<br> The smart contract failed to be compiled to WebAssembly instance. |
| `5`       | Failure (Invalid Nonce), <br> Incorrect account nonce is used in transaction. Each nonce should only be used once and in sequence order.|
| `6`       | Failure (Others)|
