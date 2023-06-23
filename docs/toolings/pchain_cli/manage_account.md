---
tags:
  - Tools
  - CLI
  - pchain-cli-rust
---


## Manage Account
---

In ParallelChain, an account is identified by the public key of Ed25519 keypair. You can either generate new keys or import your existing Ed25519 keypair to make transactions in `pchain_client`. Both operations require password (if you setup before).

### Generate New Keypair
This command generates a set of ed25519_dalek compatible keys. Random name will be set if you do not provide a name.
```sh
pchain_client keys create --name <NAME>
```

### Import Existing Keypair
If you have already got keys from ParallelChain Explorer, you can import your account keypair with this command. Random name will be set if you do not provide a name.
```sh
pchain_client keys import --private <PRIVATE_KEY> --public <PUBLIC_KEY> --name <NAME>
```

`PRIVATE_KEY` and `PUBLIC_KEY` are Base64url encoded.

### List Accounts
After creating or adding keypair, you can check it using the following command to list out all public keys managed in this tool.
```sh
pchain_client keys list
```
