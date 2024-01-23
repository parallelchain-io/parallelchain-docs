---
tags:
  - wallet
  - browser extension
  - provider API
---

# Definitions

## Dapp

A decentralized application (dapp) is an application built on a decentralized network that combines a smart contract and a front-end user interface.

## Provider

A JavaScript object made available to a consumer, that provides access to a wallet using a Client.

## Client

An endpoint that receives Remote Procedure Call (RPC) requests from the Provider and returns their results.

## Wallet

An end-user application that manages private keys, performs signing operations,
and acts as a middleware between the Provider and the Client.

## Remote Procedure Call (RPC)

A Remote Procedure Call (RPC), is any request submitted to a Provider for some procedure
that is to be processed by a Provider, its Wallet, or its Client.

## Address

An address is a [base64url][1] encoded string representation of a public key.
It is used to identify users in the network.

## Active Account

An active account is an account that is currently selected by the user in the wallet.

[1]: https://datatracker.ietf.org/doc/html/rfc4648#section-5