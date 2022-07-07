---
tags:
  - testnet 2.0
  - parallelchain light client
  - tutorial
---

# Prepare Environment

After installation of `pchain`, update testnet addresses to program configuration in order to communicate with the testnet. 


If the link above is working, type the command below to configure `pchain` to communicate with the testnet (in this example, we use node1):
=== "Linux / macOS"
    ```bash
    ./pchain set config 
      --target-url https://node1.digital-transaction.net 
      --rich-api-url https://node1.digital-transaction.net/api 
      --analytics-api-url https://node1.digital-transaction.net/analytics
    ```
=== "Windows"
    ```PowerShell
    pchain.exe set config 
      --target-url https://node1.digital-transaction.net 
      --rich-api-url https://node1.digital-transaction.net/api 
      --analytics-api-url https://node1.digital-transaction.net/analytics
    ```

This command will write a config.json file in `$HOME/.parallelchain/pchain_cli/config.json`. It only needs to be executed once.


To verify that our testnet is live and running, please make sure that the following URL is working by clicking on the link below:

* [https://node1.digital-transaction.net](https://node1.digital-transaction.net) 
* [https://node1.digital-transaction.net/api](https://node1.digital-transaction.net/api)
* [https://node1.digital-transaction.net/analytics](https://node1.digital-transaction.net/analytics)  