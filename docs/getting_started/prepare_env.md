---
tags:
  - testnet 3
  - parallelchain client
  - tutorial
---

# Prepare Environment

After installation of `pchain_client`, update testnet addresses to program configuration in order to communicate with the testnet. 


If the link above is working, type the command below to configure `pchain_client` to communicate with the testnet (in this example, we use node1):
=== "Linux / macOS"
    ```bash
    ./pchain_client setup networking 
      --standard-api-url https://node-t3-1.digital-transaction.net 
      --rich-api-url https://service-t3.digital-transaction.net/rich_api 
      --analytics-api-url https://service-t3.digital-transaction.net/analytics_api
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe setup config 
      --standard-api-url https://node-t3-1.digital-transaction.net 
      --rich-api-url https://service-t3.digital-transaction.net/rich_api 
      --analytics-api-url https://service-t3.digital-transaction.net/analytics_api
    ```

This command will write a config.json file in `$HOME/.parallelchain/pchain_cli/config.json`. It only needs to be executed once.


To verify that our testnet is live and running, please make sure that the following URL is working by clicking on the link below:

* [https://node-t3-1.digital-transaction.net](https://node-t3-1.digital-transaction.net) 
* [https://node-t3-2.digital-transaction.net](https://node-t3-2.digital-transaction.net) 
* [https://node-t3-3.digital-transaction.net](https://node-t3-3.digital-transaction.net) 
* [https://node-t3-4.digital-transaction.net](https://node-t3-4.digital-transaction.net) 
* [https://service-t3.digital-transaction.net/rich_api](https://service-t3.digital-transaction.net/rich_api) 
* [https://service-t3.digital-transaction.net/analytics_api](https://service-t3.digital-transaction.net/analytics_api) 
* [https://testnet.parallelchain.io/explorer](https://testnet.parallelchain.io/explorer) 