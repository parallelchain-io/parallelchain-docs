---
tags:
  - pchain-client
  - Call
  - Smart Contract
---


## Preparing Contract Method Arguments
---

Suppose you already deployed a contract that contains a `call` method as follows:

```rust
    #[call]
    fn hello_from(name: String) -> u32 {
        pchain_sdk::log(
            "topic: Hello From".as_bytes(), 
            format!("Hello, Contract. From: {}", name).as_bytes()
        );
        name.len() as u32
    }
```

To invoke this contract in the blockchain by using `pchain_client`, you can prepare a JSON file that contains list of arguments and matches with the `call` method. For example, this `call` method takes a string argument. Then, the content of the JSON file should be as follows:

```json
{
    "arguments": [
        {"argument_type":"String", "argument_value": "alice"}
    ]
}
```

This JSON file will be used in subcommand `call` as mentioned the subsequent sections in this page.

## Calling Contract
---

To call a smart contract, submit a transaction with your account nonce and contract address using the `pchain_client`.

Here is the command to call a contract:

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    call \
    --target <CONTRACT_ADDRESS> \   
    --method <contract_METHOD> \
    --argument <CALL_ARGUMENT_FILE_PATH_WITH_FILE_NAME>
    --amount <AMOUNT_TO_CONTRACT> \
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe transaction create \
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    call \
    --target <CONTRACT_ADDRESS> \   
    --method <contract_METHOD> \
    --argument <CALL_ARGUMENT_FILE_PATH_WITH_FILE_NAME>
    --amount <AMOUNT_TO_CONTRACT> \
    ```

The gas limit required for the transaction depends on the complexity of the smart contract. For safety reasons, you can always set a higher gas limit. You can also test contract call on testnet to reassure.

Then you can submit the transaction in the same way as [submitting transfer transaction](./transfer.md#submitting-transaction).

To query the resulting receipt of the transaction, 

=== "Linux / macOS"
    ```bash
    ./pchain_client query tx --hash <TRANSACTION_HASH> 
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe query tx --hash <TRANSACTION_HASH>
    ```

The commands stored in `transaction` and command receipts in `receipt` are following the same order. That means you can always find the corresponding transaction from a command receipt.

## Parse Call Result
---

To parse the response from the contract method, represented in field named `return value` , which is in `CallResult` format, you can use the `parse call-result` command in ParallelChain Client.

For example, if the contract method returns a u32 integer, the `return value` is "BAAAAAUAAAA" you can parse the `CallResult` data structure using the `--data-type u32` flag:

=== "Linux / macOS"
    ```bash
    pchain_client parse call-result --value BAAAAAUAAAA --data-type u32
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe parse call-result --value BAAAAAUAAAA --data-type u32
    ```

The output will be the parsed value of the `CallResult`, which in this case is `4`. For more details, you can use the `help` command to see the usage of the tool or take a look at the example `argument,json`.