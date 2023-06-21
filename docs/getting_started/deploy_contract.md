---
tags:
  - Smart Contract
  - pchain-client
---

This guide will walk you through the process of deploying your own smart contract on ParallelChain Mainnet.

## Deploying a Smart Contract
---

You can deploy the contract using the pchain_client command line tool. You should make sure to obtain your latest account nonce before submitting the transaction.

=== "Linux / macOS"
    ```bash
    ./pchain_client transaction create
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \
    deploy \
    --contract-code <CONTRACT_FILE_PATH_WITH_FILE_NAME> \
    --cbi-verrsion <CBI_VERSION> 
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe transaction create
    --nonce <NONCE> \ 
    --gas-limit <GAS_LIMIT> \
    --max-base-fee-per-gas <MAX_BASE_FEE_PER_GAS> \
    --priority-fee-per-gas <PRIORITY_FEE_PER_GAS> \ 
    deploy \
    --contract-code <CONTRACT_FILE_PATH_WITH_FILE_NAME> \
    --cbi-verrsion <CBI_VERSION> 
    ```

Then you can submit the transaction in the same way as [submitting transfer transaction](./transfer.md#submitting-transaction).

## Checking Contract in State
---

To verify that the smart contract is deployed correctly, you can run command `query` with the flag `contract`. It queries the state of the blockchain and saves the wasm code as `code.wasm` in current directory.
If you want to store contract file your prefered location, you need to provide the flag `destination` to specify the path with your prefered file extension:

=== "Linux / macOS"
    ```bash
    ./pchain_client query contract --address <CONTRACT_ADDRESS>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe query contract --address <CONTRACT_ADDRESS>
    ```