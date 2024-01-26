---
tags:
  - pchain-compile
  - Contract Publishing Service
---

# Contract Publishing Service

## What is contract publishing service?
---

The Contract Publishing Service is a platform that allows smart contract developers to **verify** their deployed source code on ParallelChain Mainnet.

## How does it work?
---

The Contract Publishing Service follows these three steps:

1. The service generates identity verification bytes using the contract address and transaction hash provided by the user.
2. The user signs the ID bytes with the help of the ParallelChain client using the instructions provided on the front end, which generates a signature.
3. The backend accepts the signature, GitHub link, and commit SHA of the source code. It builds the source code using `pchain_compile` and verifies the generated binary against the existing code on ParallelChain Mainnet. If the contract is successfully verified, the service returns a success status code to the user.

## About the word "Verify"
---

The contract publishing service in ParallelChain Mainnet relies on the user to provide a correct Github link and commit SHA of the source code. If the user provides incorrect information, the verification process may fail or result in a false positive. Additionally, the service only *verifies* the **binary code** generated from the source code, and does *not verify* the **actual logic and functionality** of the smart contract. Therefore, it is still important for smart contract developers to thoroughly test and audit their code before deployment to ensure its correctness and security.