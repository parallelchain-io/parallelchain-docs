---
tags:
  - testnet 2.0
  - parallelchain light client
  - transaction
---

# Deploy Contract

---

## Deploy by using light client

We will walk through the process of deploying your own smart contract. In this section, we will use the `bank` smart contract. Remember to get your latest account nonce before submitting transaction.

Deploy the contract using `pchain`
=== "Linux / macOS"
    ```bash
    ./pchain submit tx \
    --from-address <FROM_ACCOUNT_ADDRESS> \
    --to-address null \
    --value 0 \
    --tip 0 \
    --gas-limit <GAS_LIMIT> \
    --gas-price 1 \
    --data <RELATIVE_PATH_TO_WASM_BINARIES> \
    --nonce <ACCOUNT_NONCE> \
    --path-to-keypair-json <ACCOUNT_KEYPAIR>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe submit tx \
    --from-address <FROM_ACCOUNT_ADDRESS> \
    --to-address null \
    --value 0 \
    --tip 0 \
    --gas-limit <GAS_LIMIT> \
    --gas-price 1 \
    --data <RELATIVE_PATH_TO_WASM_BINARIES> \
    --nonce <ACCOUNT_NONCE> \
    --path-to-keypair-json <PATH_TO_KEYPAIR>
    ```

The real world example code used will be denoted like this:
<details><summary>Click to view real world example of a DeployC transaction</summary>
Do not blindly copy the values in the real world example 
```bash
./pchain submit tx \
--from-address 3_AI1h9ODjUJnKALkFmcxzfFXAVjDuMqgN8hwd03c3g \
--to-address null \
--value 0 \
--tip 0 \
--gas-limit 10000000 \
--gas-price 1 \
--data "../wasm32-unknown-unknown/release/minified-bank.wasm" \
--nonce 1 \
--path-to-keypair-json ../keypair.json
```
</details>

You should get the `contract address` and `transaction hash` if the command has succeeded. 

<details><summary>Terminal Output</summary>
```bash
Signature of tx: "aC7xyBatLpvkuRrVnBQ813aOprOXrT9_Gl2s5g-HOE6ihBvU8Cnx2mqRg3SbYMcDCmeRSnFk1uir8rYLthxWAw"
Hash of tx: "m6ef1Ipm3d8yxs2LrBnwwOh8retD8gW5k69O_hcFRM4"
Submit Transaction {
  "from_address": "3_AI1h9ODjUJnKALkFmcxzfFXAVjDuMqgN8hwd03c3g",
  "to_address": "null",
  "value": 0,
  "tip": 0,
  "gas_limit": 1000000,
  "gas_price": 1,
  "data": "../wasm32-unknown-unknown/release/minified-bank.wasm",
  "deploy_args": "",
  "nonce": 1,
  "path-to-keypair-json": "../keypair.json"
}
Status 202
Response "Transaction added to mempool."
```
"Status 202" is a HTTP response status codes. It implies that your transaction request was successfully sent to our node to process. See <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status">here</a> for details.
</details>

A smart contract is correctly deployed if you get a stream of bytes in the terminal through the `contract-code` flag.
=== "Linux / macOS"
    ```bash
    ./pchain query account contract-code --address <CONTRACT_ADDRESS>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query account contract-code --address <CONTRACT_ADDRESS>
    ```

<details><summary>Click to view real world example to query a smart contract's code</summary>
```bash
./pchain query account contract-code --address Ns9DuNe8aS5QISfCyjEoAcZq20OVr2nKQTKsYGmo/Jw=
```
</details>

<details><summary>Terminal Output</summary>
```bash
Your result is saved to binary file `contract-code.bin` in same directory
```
</details>

If there is no result from querying the smart contract code, your transaction might have failed with a specific status code. You can check the transaction 
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
./pchain query blocks --tx-hash JOKxDTNqlMUjZ65DDxONrRkOjPOKqW3zO44F48SJOec=
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
    receipts: [
        Receipt {
            status_code: [
                1,
            ],
            gas_consumed: 0,
            return_value: [],
            events: [],
        },
    ],
}
```
</details>
Please refer to ["Transaction Status Code"](../protocol/index.md#status-code) section for details.


## Deploy with arguments

ParallelChain Light Client supports calling a contract during deployment. This executing method in the contract is called `init` entrypoint method. This method is like general methods in a contract that accepts inputs of method arguments. Light client supports including submitting arguments along with the deploying transaction.

Take below `init` method as example:

```rust
    #[init]
    fn new(hello_msg : String) {
        Transaction::emit_event(
            "topic: Init".as_bytes(), 
            format!("{}", hello_msg).as_bytes()
        );
    }
```

The method takes a string as argument. It can be specfied in the field `<DEPLOY_ARGS>` when submitting transaction.

```bash
./pchain submit tx \
--from-address 3_AI1h9ODjUJnKALkFmcxzfFXAVjDuMqgN8hwd03c3g \
--to-address null \
--value 0 \
--tip 0 \
--gas-limit 10000000 \
--gas-price 1 \
--data "../wasm32-unknown-unknown/release/minified-bank.wasm" \
--deploy-args AAAAAAAAAAARAAAAAQAAAAkAAAAFAAAAYWxpY2U
--nonce 1 \
--path-to-keypair-json ../keypair.json
```

`<DEPLOY_ARGS>` is base64 urlencoded string. Light client also provides tool to parse a json file to required input string. 

```bash
./pchain parse calldata --json-file ./call_data.json 
```
<details><summary>Terminal Output</summary>
Note: Base64 encoded output string for `data` can be used in command `submit tx` and `query account view`.

AAAAAAAAAAARAAAAAQAAAAkAAAAFAAAAYWxpY2U
</details>

The json file consists of two fields: `method_name` and `arguments`.
```json
{
    "method_name": "",
    "arguments": [
        {"type":"String", "value": "alice"}
    ]
}
```

The field `method_name` can be empty for `init` entrypoint method.

More details can be found by running command `help` to see the usage of the tool.