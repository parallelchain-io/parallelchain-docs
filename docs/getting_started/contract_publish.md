---
tags:
  - testnet 3
  - parallelchain compile
  - Contract Publishing Service
---

# Contract Publishing Service

## What is contract publishing service?

The ParallelChain F smart contract publishing service provides a platform for smart contract developers to **verify** their deployed source code on ParallelChain F. 


## How does it work?

 The service performs three steps.

1. The service generates identity verification bytes using the contract address and transaction hash provided by the user.
2. The user then signs the ID bytes with the help of our parallelchain client using the instructions on the front end which generates a signature.
3. The back end accepts the signature, github link and commit SHA of the source code, builds the source code with `pchain_compile` and verifies the generated binary against that existing on ParallelChain F. The service returns Status Code 200 
   to the user if the contract is successfully verified.

