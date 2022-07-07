---
tags:
  - testnet 2.0
  - parallelchain light client
  - transaction
---

# Transfer Tokens

Before you transfer token to someone, you need to know the `nonce` of your account. It is important to keep track of this number. You will need this number to submit transaction.
=== "Linux / macOS"
    ```bash
    ./pchain query account nonce --address <YOUR_ACCOUNT_ADDRESS>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query account nonce --address <YOUR_ACCOUNT_ADDRESS>
    ```
<details><summary>Click to view real world example</summary>

Do not blindly copy the values in the real world example
```bash
./pchain query account nonce --address /5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs=
```
</details>
<details><summary>Terminal Output</summary>
```bash
0
```
</details>

Transfer tokens from your account to the dummy acccount using `pchain`
=== "Linux / macOS"
    ```bash
    ./pchain submit tx \
    --from-address <FROM_ACCOUNT_ADDRESS> \
    --to-address <DUMMY_ACCOUNT_ADDRESS> \
    --value <AMOUNT_TO_TRANSFER> \
    --tip 0 \
    --gas-limit 50000 \
    --gas-price 1 \
    --data null \
    --nonce <ACCOUNT_NONCE> \
    --path-to-keypair-json <ACCOUNT_KEYPAIR>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe submit tx \
    --from-address <FROM_ACCOUNT_ADDRESS> \
    --to-address <DUMMY_ACCOUNT_ADDRESS> \
    --value <AMOUNT_TO_TRANSFER> \
    --tip 0 \
    --gas-limit 50000 \
    --gas-price 1 \
    --data null \
    --nonce <ACCOUNT_NONCE> \
    --path-to-keypair-json <ACCOUNT_KEYPAIR>
    ```
The real world example code used will be denoted like this:

<details><summary>Click to view real world example</summary>

Do not blindly copy the values in the real world example
```bash
./pchain submit tx \
--from-address 3_AI1h9ODjUJnKALkFmcxzfFXAVjDuMqgN8hwd03c3g \
--to-address VGb9nPP0pyi7_TDGWoOcgsXQWHxWp-Yt7vFFnIcN9lA \
--value 50 \
--tip 0 \
--gas-limit 50000 \
--gas-price 1 \
--data  null \
--nonce 0 \
--path-to-keypair-json ./keypair.json
```
</details>
<details><summary>Terminal Output</summary>
```bash
Signature of tx: "aC7xyBatLpvkuRrVnBQ813aOprOXrT9_Gl2s5g-HOE6ihBvU8Cnx2mqRg3SbYMcDCmeRSnFk1uir8rYLthxWAw"
Hash of tx: "m6ef1Ipm3d8yxs2LrBnwwOh8retD8gW5k69O_hcFRM4"
Submit Transaction {
  "from_address": "3_AI1h9ODjUJnKALkFmcxzfFXAVjDuMqgN8hwd03c3g",
  "to_address": "VGb9nPP0pyi7_TDGWoOcgsXQWHxWp-Yt7vFFnIcN9lA",
  "value": 50,
  "tip": 0,
  "gas_limit": 50000,
  "gas_price": 1,
  "data": "",
  "deploy_args": "",
  "nonce": 114,
  "path_to_keypair_json": "../keypair.json"
}
Status 202
Response "Transaction added to mempool."
```
"Status 202" is a HTTP response status codes. It implies that your transaction request was successfully sent to our node to process. See <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status">here</a> for details.
</details>

You can check the transaction 
history with `transaction hash` using `pchain`, or check at [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer)
=== "Linux / macOS"
    ```bash
    ./pchain query blocks --tx-hash <TRANSACTION_HASH> --size 1
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query blocks --tx-hash <TRANSACTION_HASH> --size 1
    ```
<details><summary>Click to view real world example</summary>

Do not blindly copy the values in the real world example
```bash
./pchain query blocks --tx-hash /dg7QxEUKDApdO/OuOufGewo1r6OO/XXLsFay65ozlg=
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
    receipts: [
        Receipt {
            status_code: [
                0,
            ],
            gas_consumed: 44200,
            return_value: [],
            events: [],
        },
    ],
}
```
</details>
