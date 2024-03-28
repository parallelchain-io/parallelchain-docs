---
tags:
  - Tools
  - CLI
  - pchain-cli-rust
---

`pchain_client` allows you to query different data from the ParallelChain, not just Transaction or Account related information, but also details of Validators and Stake Pool in ParallelChain network. 

Use `pchain_client query --help` to check the full list available to query.

### Check Account Related Information
To check Externally Owned Accounts (EOA) information such as balance and nonce, your account address (public key) is always needed.

Commands:
=== "Linux / macOS"
    ```bash
    ./pchain_client query balance --address <ADDRESS>
    ./pchain_client query nonce --address <ADDRESS>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe query balance --address <ADDRESS>
    ./pchain_client.exe query nonce --address <ADDRESS>
    ```


For Contract Account, you can use another command to download the contract code binary file (wasm).

Command:

=== "Linux / macOS"
    ```bash
    ./pchain_client query contract --address <ADDRESS>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe query contract --address <ADDRESS>
    ```

### Get Transaction with Receipt
In [Submit Transaction to ParallelChain](./creating_transaction.md#submit-transaction-to-parallelchain) section, after you successfully make transaction on ParallelChain, you should receive the transaction hash (tx_hash) in the response. This hash is the identity of your transaction. You can always retrieve the transaction details with receipt by the transaction hash.

Command:

=== "Linux / macOS"
    ```bash
    ./pchain_client query tx --hash <TX_HASH>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe query tx --hash <TX_HASH>
    ```

If you just want to get the receipt, you can use following command

=== "Linux / macOS"
    ```bash
    ./pchain_client query receipt --hash <TX_HASH>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe query receipt --hash <TX_HASH>
    ```

### Get Deposit and Stake
You can query deposit or stake amount of an account from a specific pool stored in Network Account.

Commands:

=== "Linux / macOS"
    ```bash
    ./pchain_client query deposit --operator <OPERATOR> --owner <OWNER>
    ./pchain_client query stake --operator <OPERATOR> --owner <OWNER>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe query deposit --operator <OPERATOR> --owner <OWNER>
    ./pchain_client.exe query stake --operator <OPERATOR> --owner <OWNER>
    ```
 
