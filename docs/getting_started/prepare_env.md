---
tags:
  - pchain-client
  - mainnet
  - testnet
  - explorer
---

## Setting Environmental Variables
---

Specify the location for storing your config and keypair by setting the environmental variable `PCHAIN_CLI_HOME`.

Always remember the location that you set, if you forget the location, it means you forget where your keypair is being placed.

!!! Tips
    For security reasons, you may want to set environmental **temporarily**, so that after you close the terminal session, it will forget the keypair location.

The following command will set the environmental variable **temporarily**:

=== "Linux / macOS"
    ```bash
    export PCHAIN_CLI_HOME="<PATH_TO_CONFIG>"
    ```
=== "Windows PowerShell"
    ```PowerShell
    $Env:PCHAIN_CLI_HOME="<PATH_TO_CONFIG>"
    ```
=== "Windows Command Prompt"
    ```PowerShell
    # No parentheses required
    set PCHAIN_CLI_HOME=<PATH_TO_CONFIG>
    ```

For convenience reasons, you may alternatively want to set environmental **permanently**. Even in that case, we still suggest you remember the storage location.

The following command will set the environmental variable **permanently**:

=== "Linux / macOS"
    ```bash
    ##########################################################
    ## Append this line to $Home/.bashrc and $Home/.profile ##
    ##########################################################
    export PCHAIN_CLI_HOME="<PATH_TO_CONFIG>"
    ```
=== "Windows (restart shell to pick up change)"
    ```PowerShell
    setx PCHAIN_CLI_HOME "<PATH_TO_CONFIG>"
    ```

## Creating Password
---

For the first time to use `pchain_client`, you need to create your password for using it. The terminal should prompt you as follows:

```text
First time using ParallelChain Client CLI. Please set up a password to protect your keypairs.
Your password: 
```

This password is only used by the CLI, and **NOT** associated with the blockchain. It is used for encryption and decryption of your keypairs so that the keypairs are stored in your computer more securely.

## Setting Endpoint Interacting with Parallelchain
---

After installation of `pchain_client`, you had to update the Mainnet / Testnet endpoint to communicate with the Mainnet / Testnet. 

=== "Linux / macOS"
    ```bash
    # Mainnet
    ./pchain_client config setup --url https://pchain-main-rpc02.parallelchain.io
    # Testnet
    ./pchain_client config setup --url https://pchain-test-rpc02.parallelchain.io
    ```
=== "Windows"
    ```PowerShell
    # Mainnet
    pchain_client.exe config setup --url https://pchain-main-rpc02.parallelchain.io
    # Testnet
    pchain_client.exe config setup --url https://pchain-test-rpc02.parallelchain.io
    ```

This command will write a `config.toml` file in the folder specified by the environment variable `PCHAIN_CLI_HOME`. It only needs to be executed once.

Now you can start the journey to play around with `pchain_client`, but there is still one thing you had to put in mind first.
