---
tags:
  - testnet 2.0
  - parallelchain light client
  - transaction
---

# Call Contract

---

## Call Contract by using Light Client

Submit transaction with your account nonce and contract address. You should get the transaction hash if the command has succeeded.
The gas limit is depended on the complexity of the smart contract. On average, it takes no more than 500000 gas to execute. You can always set a higher gas limit for safety.
=== "Linux / macOS"
    ```bash
    ./pchain submit tx \
    --from-address <YOUR_ACCOUT_ADDRESS> \
    --to-address <CONTRACT_ADDRESS> \
    --value 0 \
    --tip 0 \
    --gas-limit <GAS_LIMIT> \
    --gas-price 1 \
    --data <BASE64_ENCODED_ARGUMENT> \
    --nonce <ACCOUNT_NONCE> \
    --path-to-keypair-json <ACCOUNT_KEYPAIR>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe submit tx \
    --from-address <YOUR_ACCOUT_ADDRESS> \
    --to-address <CONTRACT_ADDRESS> \
    --value 0 \
    --tip 0 \
    --gas-limit <GAS_LIMIT> \
    --gas-price 1 \
    --data <BASE64_ENCODED_ARGUMENT> \
    --nonce <ACCOUNT_NONCE> \
    --path-to-keypair-json <ACCOUNT_KEYPAIR>
    ```

<details><summary>Click to view real world example to call the smart contract</summary>
```bash
./pchain submit tx \
--from-address /5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs= \
--to-address Ns9DuNe8aS5QISfCyjEoAcZq20OVr2nKQTKsYGmo/Jw= \
--value 0 \
--tip 0 \
--gas-limit 1000000 \
--gas-price 1 \
--data AwAAAEFsaQQAAABCYWJhCgAAAGpIRDIzVmtCaXkBECcAAAAAAAAA \
--nonce 2 \
--path-to-keypair-json ./keypair.json
```
</details>
<details><summary>Terminal Output</summary>
```bash
Hash of tx: "PWWBPzaKfRLDSSmL5iGvxhRnF7BcXmhIlBO9vI4AKFE="
Status 200
Response "Your request has been received."
```
"Status 200" is a HTTP response status codes. It implies that your transaction request was successfully sent to our node to process. See <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status">here</a> for details.
</details>

Check the transaction 
history with `transaction hash` using `pchain`, or check at [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer)
=== "Linux / macOS"
    ```bash
    ./pchain query blocks --tx-hash <TRANSACTION_HASH> --size 1
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query blocks --tx-hash <TRANSACTION_HASH> --size 1
    ```
<details><summary>Click to view real world example to query block</summary>
```bash
./pchain query blocks --tx-hash PWWBPzaKfRLDSSmL5iGvxhRnF7BcXmhIlBO9vI4AKFE= --size 1
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
    receipts: [
        Receipt {
            status_code: [
                0,
            ],
            gas_consumed: 170377,
            return_value: [],
            events: [
                Event {
                    topic: "bank_account: Open",
                    value: "Successfully opened \n            account for Ali, Baba \n            with account_id: jHD23VkBiy",
                },
            ]
        },
    ],
}
```
</details>


In each block, you can find the following information : 

* `header`, Block header contains the metadata of this block.

* `transactions`, A list of transactions included in this block.

* `events`, Events emitted from the contract

* `receipts`, Transaction receipts with information of the execution results

The transaction stored in `transactions` and `receipts` in same order. That means you can always find the status of your transaction according to the ordinal position.

Inside the receipt, you can see the `status_code` in digit, and the gas consumed for the transaction.

Please refer to ["Status Code"](./params_status_code.md) section for details.

## Call Contract with arguments

ParallelChain Light Client supports calling a contract entrypoint method with arguments. Light client supports including submitting arguments along with the making EtoC transaction.

Take below `action` method as example:

```rust
    #[action]
    fn hello_from(name :String) -> u32 {
        Transaction::emit_event(
            "topic: Hello From".as_bytes(), 
            format!("Hello, Contract. From: {}", name).as_bytes()
        );
        name.len() as u32
    }
```

The method takes a string as argument. It can be specfied in the field `<Data>` when submitting transaction.

```bash
./pchain submit tx \
--from-address 3_AI1h9ODjUJnKALkFmcxzfFXAVjDuMqgN8hwd03c3g \
--to-address null \
--value 0 \
--tip 0 \
--gas-limit 10000000 \
--gas-price 1 \
--data AAAAAAoAAAARAAAAaGVsbG9fZnJvbQEAAAAJAAAABQAAAGFsaWNl \ 
--nonce 1 \
--path-to-keypair-json ../keypair.json
```

`<DEPLOY_ARGS>` is base64 urlencoded string. Light client also provides tool to parse a json file to required input string. 

```bash
./pchain parse calldata --json-file ./call_data.json 
```
<details><summary>Terminal Output</summary>
Note: Base64 encoded output string for `data` can be used in command `submit tx` and `query account view`.

AAAAAAoAAAARAAAAaGVsbG9fZnJvbQEAAAAJAAAABQAAAGFsaWNl
</details>

The json file consists of two fields: `method_name` and `arguments`.
```json
{
    "method_name": "hello_from",
    "arguments": [
        {"type":"String", "value": "alice"}
    ]
}
```

## Response from Contract 

ParallelChain Light Client supports parsing [Callback](../smart_contract_sdk/develop_contract.md#return-value) data which is the return value from entrypoint method.

In the above [Example](#call-contract-with-arguments), the method return data as `u32` integer. The return value from `submit tx` can be found in `receipt`, which is in bytes format. The base64 urlencoded representation is `BAAAAAUAAAA`.


Light Client needs input field `data-type` to correctly parse the `Callback` data structure.


```bash
pchain parse callback --value BAAAAAUAAAA --data-type u32
```

<details><summary>Terminal Output</summary>
5
</details>


More details can be found by running command `help` to see the usage of the tool.