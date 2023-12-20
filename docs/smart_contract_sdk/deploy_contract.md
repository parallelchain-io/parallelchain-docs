---
tags:
  - Smart Contract
  - pchain-client
---

This guide will walk you through the process of deploying your smart contract on ParallelChain Mainnet.

## Deploying a Smart Contract
---

You can deploy the contract using the pchain_client command line tool. You should make sure to obtain your latest account nonce before submitting the transaction.

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create
    <--v1|--v2> \
    --nonce <NONCE> \
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    deploy \
    --contract-code <WASM_BINARY_PATH> \
    --cbi-version <CBI_VERSION> 
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe transaction create `
    <--v1|--v2> `
    --nonce <NONCE> `
    --gas-limit <GAS_LIMIT> `
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> `
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> `
    deploy `
    --contract-code <WASM_BINARY_PATH> `
    --cbi-version <CBI_VERSION> 
    ```

Then you can submit the transaction in the same way as [submitting transfer transaction](../getting_started/transfer.md#submitting-transaction).

## Checking Contract in State
---

To verify that the smart contract is deployed correctly, you can run the command `query` with the flag `contract`. It queries the state of the blockchain and saves the wasm code as `code.wasm` in the current directory.
If you want to store the contract file in your preferred location, you need to provide the flag `destination` to specify the path with your preferred file extension:

=== "Linux / macOS"
    ```bash
    ./pchain_client query contract --address <CONTRACT_ADDRESS>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe query contract --address <CONTRACT_ADDRESS>
    ```