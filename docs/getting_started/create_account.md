---
tags:
  - pchain-client
  - account
  - mainnet
  - testnet 4
  - faucet
---

## Generating Keypair
---

To create an account, type the following command below:
=== "Linux / macOS"
    ```bash
    ./pchain_client keys create --name <NAME>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe keys create --name <NAME>
    ```

You will be asked to input your password to save the new keypair.

This command generates a set of ed25519_dalek compatible keys. A keypair file in json format is created in the folder specified by the environmnet variable `PCHAIN_CLI_HOME`. These are the keys required for making transactions in the network. Its public key is the public address of your account. You can view it as follows:

=== "Linux / macOS"
    ```bash
    ./pchain_client keys list
    ```
=== "Windows"
    ```PowerShell
    ./pchain_client.exe keys list
    ```

<details><summary>Terminal Output</summary>
```bash
Keypair Name (First 50 char)                        Public key 
-------------------------                           ------------------------- 
doc                                                 mF0a_r31a_cNNnKbngoUGUuy71V_l872yGxy7iwIBwA
```
</details>

You will need the keypair and public key to submit transactions.

You can check the account balance using the account address (public key).
=== "Linux / macOS"
    ```bash
    ./pchain_client query balance --address <PUBLIC_KEY>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe query balance --address <PUBLIC_KEY>
    ```
<details><summary>Terminal Output</summary>
```bash
0
```
</details>

The balance of a new account is 0. Therefore, you need to request for tokens from faucet.

## Exporting Keypair

Right now, the key pair is only in your machine. It will go away if your machine somehow breaks. It may also be lost if you forget your password. 

To secure your keypair and money, you must export it and save it to somewhere secure, or at least make some copy of it.

=== "Linux / macOS"
    ```bash
    ./pchain_client keys export --name <NAME>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe keys export --name <NAME>
    ```

You will be asked to input your password to export the keypair. The keypair will be saved at current directory, in json, with same name of the keypair itself.

## Adding Keypair

To add your own keypair, type the following command below:
=== "Linux / macOS"
    ```bash
    ./pchain_client keys add --public-key <PUBLIC_KEY> --private-key <PRIAVATE_KEY> --name <NAME>
    ```
=== "Windows"
    ```PowerShell
    pchain_client.exe keys add --public-key <PUBLIC_KEY> --private-key <PRIAVATE_KEY> --name <NAME>
    ```

You will be asked to input your password to add the new keypair.

## Request for tokens from Faucet (Testnet)
---

Right now, you have your own keypair (account) to associate with the blockchain. But the account should have empty balance. You can use command `curl` to make a token request in command line.

<!-- ```bash
curl --location --request POST 'https://testnet4-service01.digital-transaction.net/faucet/request_tokens' \
    --header 'Content-Type: application/json' \
    --data-raw '{"address":"<YOUR_ADDRESS>"}'
``` -->

```bash
curl --location --request POST <TESTNET_REQUEST_TOKEN_URL> \
    --header 'Content-Type: application/json' \
    --data-raw '{"address":"<YOUR_ADDRESS>"}'
```

> **Note:** TESTNET_REQUEST_TOKEN_RUL will be released soon.

If your request is successfully submitted, it outputs success message:

```text
"Your token transfer request has been submitted to the queue. It will be processed shortly. Please check your wallet for updated balance in a few minutes."
```

The token request can only be made once every 30 minutes. You will receive a message if you request again within the time period:
```text
"Your have requested tokens within last 30 minutes. Please try again later."
```

The faucet service transfers an amount of tokens to the requesting addresses in batch. Therefore, it can take up to few minutes for you to receive your tokens depending on how full is the buffer of requests.
