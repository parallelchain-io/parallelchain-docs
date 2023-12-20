---
tags:
  - Tools
  - CLI
  - pchain-cli-rust
---


## Manage Account
---

In ParallelChain, an account is uniquely identified by the public key of an Ed25519 keypair. To perform transactions in `pchain_client`, you have the option to either generate new keys or import an existing Ed25519 keypair. In both cases, you will need to provide the password if you have previously set one up.

### Generate New Keypair
This command generates a set of ed25519_dalek compatible keys. A random name will be assigned if you do not provide a name.

=== "Linux / macOS"
    ```bash
    ./pchain_client keys create --keypair-name <KEYPAIR_NAME>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe keys create --keypair-name <KEYPAIR_NAME>
    ```


### Import Existing Keypair
To import your account keypair obtained from ParallelChain Explorer, you can use the following command. Random name will be set if you do not provide a name.

=== "Linux / macOS"
    ```bash
    ./pchain_client keys import --private <PRIVATE_KEY> --public <PUBLIC_KEY/ADDRESS> --keypair-name <KEYPAIR_NAME>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe keys import --private <PRIVATE_KEY> --public <PUBLIC_KEY/ADDRESS> --keypair-name <KEYPAIR_NAME>
    ```

`PRIVATE_KEY` and `PUBLIC_KEY/ADDRESS` are Base64url encoded.

### List Accounts
After creating or importing a keypair, you can utilize the following command to display a list of all the public keys managed within this tool.

=== "Linux / macOS"
    ```bash
    ./pchain_client keys list
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe keys list
    ```