---
tags:
  - testnet 3
  - parallelchain client
  - transaction
---

# Deploy Contract

---

## Deploy by using Client

We will walk through the process of deploying your own smart contract. In this section, we will use the `HelloContract` smart contract. Remember to get your latest account nonce before submitting transaction.

Deploy the contract using `pchain_client`
=== "Linux / macOS"
    ```bash
    ./pchain_client submit tx 
    --to-address <RECEIVER_ACCOUNT_ADDRESS> \
    --value null \
    --tip 0 \
    --gas-limit <GAS_LIMIT> \
    --gas-price 1 \
    --data <RELATIVE_PATH_TO_WASM_BINARIES> \
    --nonce <ACCOUNT_NONCE> \
    --keypair-name <KEYPAIR_NAME>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe submit tx 
    --to-address <RECEIVER_ACCOUNT_ADDRESS> \
    --value null \
    --tip 0 \
    --gas-limit <GAS_LIMIT> \
    --gas-price 1 \
    --data <RELATIVE_PATH_TO_WASM_BINARIES> \
    --nonce <ACCOUNT_NONCE> \
    --keypair-name <KEYPAIR_NAME>
    ```

The real world example code used will be denoted like this:
<details><summary>Click to view real world example of a DeployC transaction</summary>
Do not blindly copy the values in the real world example 
```bash
./pchain_client submit tx \
--to-address null \
--value 0 \
--tip 0 \
--gas-limit 10000000 \
--gas-price 1 \
--data ../wasm32-unknown-unknown/release/minified-bank.wasm \
--nonce 1 \
--keypair-name user
```
</details>

You should get the `contract address` and `transaction hash` if the command has succeeded. 

<details><summary>Terminal Output</summary>
```bash
Signature of tx: "aC7xyBatLpvkuRrVnBQ813aOprOXrT9_Gl2s5g-HOE6ihBvU8Cnx2mqRg3SbYMcDCmeRSnFk1uir8rYLthxWAw"
Hash of tx: "m6ef1Ipm3d8yxs2LrBnwwOh8retD8gW5k69O_hcFRM4"
Submit Transaction {
  "to_address": "null",
  "value": 0,
  "tip": 0,
  "gas_limit": 1000000,
  "gas_price": 1,
  "data": "../wasm32-unknown-unknown/release/minified-bank.wasm",
  "deploy_args": "",
  "nonce": 1,
  "keypair_name": "user"
}
Status 202
Response "Transaction added to mempool."
```
"Status 202" is a HTTP response status codes. It implies that your transaction request was successfully sent to our node to process. See <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status">here</a> for details.
</details>

A smart contract is correctly deployed if you get a stream of bytes in the terminal through the `contract-code` flag.
=== "Linux / macOS"
    ```bash
    ./pchain_client query account contract-code --address <CONTRACT_ADDRESS>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe query account contract-code --address <CONTRACT_ADDRESS>
    ```

<details><summary>Click to view real world example to query a smart contract's code</summary>
```bash
./pchain_client query account contract-code --address Ns9DuNe8aS5QISfCyjEoAcZq20OVr2nKQTKsYGmo/Jw=
```
</details>

<details><summary>Terminal Output</summary>
```bash
Your result is saved to binary file `contract-code.bin` in same directory
```
</details>

If there is no result from querying the smart contract code, your transaction might have failed with a specific status code. You can check the transaction 
history with `transaction hash` using `pchain_client`, or check at [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer)
=== "Linux / macOS"
    ```bash
    ./pchain_client query blocks --tx-hash <TRANSACTION_HASH> --limit 1 --order desc
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe query blocks --tx-hash <TRANSACTION_HASH> --limit 1 -order desc
    ```
<details><summary>Click to view real world example to query block</summary>
```bash
./pchain_client query blocks --tx-hash JOKxDTNqlMUjZ65DDxONrRkOjPOKqW3zO44F48SJOec= --limit 1 --order desc
```
</details>
<details><summary>Terminal Output (Warning: Very Long Code)</summary>
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
            to_address: "UFG7PkGOOu4tdg9sYnplB1jlwClwuHimirA3S/kOhVw=",
            value: 50,
            tip: 0,
            gas_limit: 50000,
            gas_price: 1,
            data: "",
            n_txs_on_chain_from_address: 10,
            hash: "5Vz-d6XqCdtm6FKkZ4IvCrmVWa2y5oUsB7rLF-a8E4A",
            signature: "ioMFMSC4POjTtcQ4mTneo9pGEyWf14hPEou+XY6DF1gwmQSEX5DbrwmmR21ffCTG3IZmC90HZeUxSFz+RSOyDw==",
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
Please refer to ["Transaction Status Code"](../protocol/index.md#status-code) section for details.


## Deploy with arguments

ParallelChain Client supports calling a contract during deployment. This executing method in the contract is called `init` entrypoint method. This method is like general methods in a contract that accepts inputs of method arguments. Client supports including submitting arguments along with the deploying transaction.

Take below `init` method as example:

```rust
    #[init]
    fn new(hello_msg : String) {
        pchain_sdk::emit_event(
            "topic: Init".as_bytes(), 
            format!("{}", hello_msg).as_bytes()
        );
    }
```

The method takes a string as argument. It can be specfied in the field `<DEPLOY_ARGS>` when submitting transaction.

```bash
./pchain_client submit tx \
--to-address null \
--value 0 \
--tip 0 \
--gas-limit 10000000 \
--gas-price 1 \
--data ../wasm32-unknown-unknown/release/minified-bank.wasm \
--deploy-args AAAAAAAAAAARAAAAAQAAAAkAAAAFAAAAYWxpY2U
--nonce 1 \
--keypair-name user
```

`<DEPLOY_ARGS>` is base64 urlencoded string. Client also provides tool to parse a json file to required input string. 

```bash
./pchain_client parse call-data --json-file ./call_data.json 
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