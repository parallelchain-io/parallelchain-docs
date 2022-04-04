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

## Return testnet tokens

Testnet tokens are distinct from actual coins, and they do not have any monetary value. When you have finished using your test tokens, please return your remaining testnet tokens to the parallalchain account so that other members of the community can use them. 

Please return your test tokens to this address:
```
bvxhscIRoJvmu2JQrJyTakMBufl1ogN4py84rtZl7sA=
```
Use below `pchain` [command](/cli/#transfer-token) to return tokens
```
pchain submit tx \
--from-address <YOUR_ACCOUNT_ADDRESS> \
--to-address bvxhscIRoJvmu2JQrJyTakMBufl1ogN4py84rtZl7sA= \
--value <TOKEN_AMOUNT> \
--tip 0 \
--gas-limit 50000 \
--gas-price 1 \
--data null \
--nonce <YOUR_ACCOUNT_NONCE> \
--keypair <YOUR_ACCOUNT_KEYPAIR>
```