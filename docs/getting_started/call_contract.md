---
tags:
  - testnet 3
  - parallelchain client
  - transaction
---

# Call Contract

---

## Call Contract by using Client

Submit transaction with your account nonce and contract address. You should get the transaction hash if the command has succeeded.
The gas limit is depended on the complexity of the smart contract. On average, it takes no more than 500000 gas to execute. You can always set a higher gas limit for safety.
=== "Linux / macOS"
    ```bash
    ./pchain_client submit tx \
    --to-address <CONTRACT_ADDRESS> \
    --value 0 \
    --tip 0 \
    --gas-limit <GAS_LIMIT> \
    --gas-price 1 \
    --data <BASE64_ENCODED_ARGUMENT> \
    --nonce 1 \
    --keypair-name <KEYPAIR_NAME>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe submit tx \
    --to-address <CONTRACT_ADDRESS> \
    --value 0 \
    --tip 0 \
    --gas-limit <GAS_LIMIT> \
    --gas-price 1 \
    --data <BASE64_ENCODED_ARGUMENT> \
    --nonce 1 \
    --keypair-name <KEYPAIR_NAME>
    ```

<details><summary>Click to view real world example to call the smart contract</summary>
```bash
./pchain_client submit tx \
--to-address OZpYmAk7LEDh6pND1hRhul66vSWOYBHTyedsBXezJ40 \
--value 0 \
--tip 0 \
--gas-limit 10000000 \
--gas-price 1 \
--data AwAAAEFsaQQAAABCYWJhCgAAAGpIRDIzVmtCaXkBECcAAAAAAAAA \
--nonce 12 \
--keypair-name user
```
</details>
<details><summary>Terminal Output</summary>
```bash
Signature of tx: "aC7xyBatLpvkuRrVnBQ813aOprOXrT9_Gl2s5g-HOE6ihBvU8Cnx2mqRg3SbYMcDCmeRSnFk1uir8rYLthxWAw"
Hash of tx: "m6ef1Ipm3d8yxs2LrBnwwOh8retD8gW5k69O_hcFRM4"
Submit Transaction {
  "to_address": "OZpYmAk7LEDh6pND1hRhul66vSWOYBHTyedsBXezJ40",
  "value": 0,
  "tip": 0,
  "gas_limit": 10000000,
  "gas_price": 1,
  "data": "AwAAAEFsaQQAAABCYWJhCgAAAGpIRDIzVmtCaXkBECcAAAAAAAAA",
  "deploy_args": "",
  "nonce": 12,
  "keypair_name": "user"
}
Status 202
Response "Transaction added to mempool."
```
"Status 202" is a HTTP response status codes. It implies that your transaction request was successfully sent to our node to process. See <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status">here</a> for details.
</details>

Check the transaction 
history with `transaction hash` using `pchain_client`, or check at [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer)
=== "Linux / macOS"
    ```bash
    ./pchain_client query blocks --tx-hash <TRANSACTION_HASH> --limit 1 --order desc
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe query blocks --tx-hash <TRANSACTION_HASH> --limit 1 --order desc
    ```
<details><summary>Click to view real world example to query block</summary>
```bash
./pchain_client query blocks --tx-hash PWWBPzaKfRLDSSmL5iGvxhRnF7BcXmhIlBO9vI4AKFE= --limit 1 --order desc
```
</details>
<details><summary>Terminal Output</summary>
```bash
Your Block: Block {    
    header: BlockHeader {
        app_id: 0,
        block_hash: "hLEzWEpiRiFNsdTYDn7k7M42lftxW2+/q9wut25Sq0U=",
        height: 12,
        justify: QuorumCertificate {
            view_number: 12,
            block_hash: "Kc6VA_V4hsuucMAxF9cPOHQD75jL2Raaa5KZHpO47sM",
            sigs: SignatureSet {
                signatures: [
                    Some("JVrGUj_gK8zWieushZcoIVAgMr7WLQ5PeW4TGo6WftI"),
                    Some("YESSKvjZvHI9MH_RMMLsqrSBoHZ_56OA5IMsCwpu8lQ"),
                    Some("q7r9YDzfaolm1TtCwfMGaBPSJgUwOXGHmdEQsfLR7dw")
                ],
                count_some: 3
            }
        },
        data_hash: 7LFJwDJBsD+fu7kivOXukaW0eGkzeBiJk/zhOeKFRLA=,
        version_number: 0,
        timestamp: 1648711616,
        txs_hash: "D5OpkQZY8g3OMyxDKLLGVFBSb4d/pO92n3SAqshyCCA=",
        state_hash: "xChhGxck++VDr/MfObhQ/9aX4StDld7Dh7PUBPFxtFk=",
        receipts_hash: "uDZkMCNxR4DZM8LjFi6dKcY-dysl9BnLqefiz0balRk",
    },
    transactions: [
        Transaction {
            from_address: "LOUZDKy2cF7PfqVQ45oBR07ofHEe8LHxjsKKs22tHC4",
            to_address: "OZpYmAk7LEDh6pND1hRhul66vSWOYBHTyedsBXezJ40",
            value: 0,
            tip: 0,
            gas_limit: 10000000,
            gas_price: 1,
            data: "",
            n_txs_on_chain_from_address: 12,
            hash: "m6ef1Ipm3d8yxs2LrBnwwOh8retD8gW5k69O_hcFRM4",
            signature: "aC7xyBatLpvkuRrVnBQ813aOprOXrT9_Gl2s5g-HOE6ihBvU8Cnx2mqRg3SbYMcDCmeRSnFk1uir8rYLthxWAw",
        },
    ],
    receipts: [
        Receipt {
            status_code: 0,
            gas_consumed: 44200,
            return_value: [],
            events: [],
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

ParallelChain Client supports calling a contract entrypoint method with arguments. Client supports including submitting arguments along with the making EtoC transaction.

Take below `action` method as example:

```rust
    #[action]
    fn hello_from(name :String) -> u32 {
        pchain_sdk::emit_event(
            "topic: Hello From".as_bytes(), 
            format!("Hello, Contract. From: {}", name).as_bytes()
        );
        name.len() as u32
    }
```

The method takes a string as argument. It can be specfied in the field `<Data>` when submitting transaction.

```bash
./pchain_client submit tx \
--to-address OZpYmAk7LEDh6pND1hRhul66vSWOYBHTyedsBXezJ40 \
--value 0 \
--tip 0 \
--gas-limit 10000000 \
--gas-price 1 \
--data AAAAAAoAAAARAAAAaGVsbG9fZnJvbQEAAAAJAAAABQAAAGFsaWNl \
--nonce 1 \
--keypair-name user
```

`<DATA>` is base64 urlencoded string. Client also provides tool to parse a json file to required input string. 

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

ParallelChain Client supports parsing [CallResult](../smart_contract_sdk/develop_contract.md#return-value) data which is the return value from entrypoint method.

In the above [Example](#call-contract-with-arguments), the method return data as `u32` integer. The return value from `submit tx` can be found in `receipt`, which is in bytes format. The base64 urlencoded representation is `BAAAAAUAAAA`.


Client needs input field `data-type` to correctly parse the `CallResult` data structure.


```bash
pchain_client parse call-result --value BAAAAAUAAAA --data-type u32
```

<details><summary>Terminal Output</summary>
5
</details>


More details can be found by running command `help` to see the usage of the tool.