---
tags:
  - testnet 2.0
  - parallelchain light client
  - account
---

# Create Account

## Generate Keypair
---

To create an account, type the following command below:
=== "Linux / macOS"
    ```bash
    ./pchain crypto generate-key-pair
    ```
=== "Windows"
    ```PowerShell
    pchain.exe crypto generate-key-pair
    ```

This command generates a set of ed25519_dalek compatible keys. A keypair file in json format is generated in currect directory. These are the keys required for making transactions in the network. Keep these details in a safe place. You cannot retrieve the account details again. An example is shown below:

**IMPORTANT: Keep the keys in a safe place.**

<details><summary>Terminal Output</summary>
```bash
keypair.json saved in current directory.
```
</details>

You will need the keypair and public key to submit transactions within the testnet.

You can check the account balance using the account address (public key).
=== "Linux / macOS"
    ```bash
    ./pchain query account balance --address /5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs=
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query account balance --address /5orENuI/htbwtAyu+3t6rYn90q3vly1yVdosBHuNSs=
    ```
<details><summary>Terminal Output</summary>
```bash
0
```
</details>

The balance of a new account is 0. Therefore, you need to request for tokens from faucet.


## Request for tokens from Faucet
---

It is recommended to access the [ParallelChain Testnet Explorer](https://testnet.parallelchain.io/explorer/) for requesting token from Faucet. You can only make a token request once every 30 minutes.

You can also use `curl` command to make a token request in command prompt.

```bash
curl -X POST -F 'address=<YOUR_PUBLIC_KEY>' https://testnet2.digital-transaction.net/faucet
```

You can check for the new balance of your account using `pchain` or `curl`

```bash
curl --get https://node1.digital-transaction.net/account/<YOUR_PUBLIC_KEY>/balance?proof=?
```

or using `pchain`
=== "Linux / macOS"
    ```bash
    ./pchain query account balance --address <YOUR_PUBLIC_KEY>
    ```
=== "Windows"
    ```PowerShell
    pchain.exe query account balance --address <YOUR_PUBLIC_KEY>
    ```
