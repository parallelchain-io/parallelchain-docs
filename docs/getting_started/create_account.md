---
tags:
  - testnet 3
  - parallelchain client
  - account
---

# Create Account

## Generate Keypair
---

To create an account, type the following command below:
=== "Linux / macOS"
    ```bash
    ./pchain_client crypto generate-key-pair --name <account name>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe crypto generate-key-pair --name <account name>
    ```

This command generates a set of ed25519_dalek compatible keys. A keypair file in json format is generated in currect directory. These are the keys required for making transactions in the network. Keep these details in a safe place. You cannot retrieve the account details again. An example is shown below:

**IMPORTANT: Keep the keys in a safe place.**

<details><summary>Terminal Output</summary>
```bash
Public Address of your keypair is Dks4-TqCIUA_gLw6RraY2Uokl3wuXBF1_PUjSS8MwF8
```
</details>

You will need the keypair and public key to submit transactions within the testnet.

You can check the account balance using the account address (public key).
=== "Linux / macOS"
    ```bash
    ./pchain_client query account balance --address Dks4-TqCIUA_gLw6RraY2Uokl3wuXBF1_PUjSS8MwF8
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe query account balance --address Dks4-TqCIUA_gLw6RraY2Uokl3wuXBF1_PUjSS8MwF8
    ```
<details><summary>Terminal Output</summary>
```bash
Your value: 0
```
</details>

The balance of a new account is 0. Therefore, you need to request for tokens from faucet.


## Request for tokens from Faucet
---

It is recommended to access the [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer/) for requesting token from Faucet. You can only make a token request once every 30 minutes.

You can also use `curl` command to make a token request in command prompt.

```bash
curl -X POST -F 'address=<YOUR_PUBLIC_KEY>' https://tn3service02.digital-transaction.net/faucet
```

The faucet service submits transaction to transfer amount to the requesting address. `pchain_client` outputs the transaction hash as a Base64URL encoded string if success.

<details><summary>Terminal Output</summary>
```bash
v9AFgNlgJdcA9LRIyGucfCNGmRKH3e6qpKwR1k7XWl8
```