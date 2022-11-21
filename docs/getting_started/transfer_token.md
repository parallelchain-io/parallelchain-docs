---
tags:
  - testnet 3
  - parallelchain light client
  - transaction
---

# Transfer Tokens

Before you transfer token to someone, you need to know the `nonce` of your account. It is important to keep track of this number. You will need this number to submit transaction.
=== "Linux / macOS"
    ```bash
    ./pchain_client query account nonce --address <YOUR_ACCOUNT_ADDRESS>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe query account nonce --address <YOUR_ACCOUNT_ADDRESS>
    ```
<details><summary>Click to view real world example</summary>

Do not blindly copy the values in the real world example
```bash
./pchain_client query account nonce --address Dks4-TqCIUA_gLw6RraY2Uokl3wuXBF1_PUjSS8MwF8
```
</details>
<details><summary>Terminal Output</summary>
```bash
Your value: 0
```
</details>

Transfer tokens from your account to an acccount using `pchain_client`
=== "Linux / macOS"
    ```bash
    ./pchain_client submit tx 
    --to-address <RECEIVER_ACCOUNT_ADDRESS> \
    --value <AMOUNT_TO_TRANSFER> \
    --tip 0 \
    --gas-limit 500000 \
    --gas-price 1 \
    --data null \
    --nonce <ACCOUNT_NONCE> \
    --keypair-name <KEYPAIR_NAME>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe submit tx \
    --to-address <RECEIVER_ACCOUNT_ADDRESS> \
    --value <AMOUNT_TO_TRANSFER> \
    --tip 0 \
    --gas-limit 500000 \
    --gas-price 1 \
    --data null \
    --nonce <ACCOUNT_NONCE> \
    --keypair-name <KEYPAIR_NAME>
    ```
The real world example code used will be denoted like this:

<details><summary>Click to view real world example</summary>

Do not blindly copy the values in the real world example
```bash
./pchain_client submit tx \
--to-address VGb9nPP0pyi7_TDGWoOcgsXQWHxWp-Yt7vFFnIcN9lA \
--value 50 \
--tip 0 \
--gas-limit 50000 \
--gas-price 1 \
--data  null \
--nonce 0 \
--keypair-name User
```
</details>
<details><summary>Terminal Output</summary>
```bash
Signature of tx: "aC7xyBatLpvkuRrVnBQ813aOprOXrT9_Gl2s5g-HOE6ihBvU8Cnx2mqRg3SbYMcDCmeRSnFk1uir8rYLthxWAw"
Hash of tx: "m6ef1Ipm3d8yxs2LrBnwwOh8retD8gW5k69O_hcFRM4"
Submit Transaction {
  "to_address": "VGb9nPP0pyi7_TDGWoOcgsXQWHxWp-Yt7vFFnIcN9lA",
  "value": 50,
  "tip": 0,
  "gas_limit": 500000,
  "gas_price": 1,
  "data": "",
  "deploy_args": "",
  "nonce": 114,
  "keypair_name": "user"
}
Status 202
Response "Transaction added to mempool."
```
"Status 202" is a HTTP response status codes. It implies that your transaction request was successfully sent to our node to process. See <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status">here</a> for details.
</details>

You can check the transaction 
history with `transaction hash` using `pchain_client`, or check at [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer)
=== "Linux / macOS"
    ```bash
    ./pchain_client query blocks --tx-hash <TRANSACTION_HASH> --limit 1 --order desc
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe query blocks --tx-hash <TRANSACTION_HASH> --limit 1 --order desc
    ```
<details><summary>Click to view real world example</summary>

Do not blindly copy the values in the real world example
```bash
./pchain_client query blocks --tx-hash m6ef1Ipm3d8yxs2LrBnwwOh8retD8gW5k69O_hcFRM4
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
