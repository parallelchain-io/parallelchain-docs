---
tags:
  - testnet 3
  - parallelchain client
  - transaction
---

# Transaction Parameters

Although the three transactions appear similar when using `pchain_client`, there are notable differences in their argument values. The table below shows this:

|        Fields       |  EtoE                         |   DeployC                |       EtoC               |
| ------------------- |-------------------------------| ------------------------ |------------------------- |
|   `to_address`      | Destination account address   | null                     | Contract address         |
|   `value`           | The token amount to transfer  | Set to 0                 | Set to 0                 |
|   `gas_limit`       | Set to 50000                  | At least CODE_SIZE * 105 | At least 500000 gas      |
|   `data`            | null                          |  Relative path to your smart contract | Base64 encoded of the contract arguments|
|   `deploy_args`     |                           | optional Base64 encoded of the contract arguments | optional Base64 encoded of the contract arguments|
