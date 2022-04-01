---
tags:
  - testnet 1.0
  - Faucet
---

# Faucet

## Request for tokens from the faucet

The faucet URL is located at [ParallelChain Testnet Faucet](https://testnet.parallelchain.io/explorer/faucet). You can only make a token request once every 30 minutes.

You can check for the new balance of your account using `pchain` or `curl`

```bash
curl -X GET https://testnet1.digital-transaction.net/account/balance/?address=<YOUR_PUBLIC_KEY>
```

or using `pchain`

```bash
$ pchain query account balance --address <YOUR_PUBLIC_KEY>
```
