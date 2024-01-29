---
tags:
  - pchain-sdk
  - Smart Contract
  - Cryptographic Functions
---

# Cryptographic Functions

Cryptographic Function refers to cryptographic functions that are supported from execution runtime so that gas cost can be reduced by avoiding implementation inside a contract. Moreover, It helps in providing complicated calculations which are not feasible in the Wasmer environment (e.g. encryption, hashing).

In ParallelChain Mainnet SDK, precompiles are a set of functions that can be called directly. Please be noted that some functions are selected based on the popularity of the blockchain ecosystem, but are not necessary adopted in the implementation of the ParallelChain Mainnet node.

| Function | Description | Gas Cost |
|:---|:---|:---|
| sha256 | SHA256 hash function |  16 x length of the input |
| keccak256 | Keccak-256 hash function | 16 x length of the input |
| ripemd | RIPEMD-160 hash function | 16 x length of the input |
| verify_ed25519_signature | Verification on ed25519 signature | 1,400,000 + 16 x length of the input |

The gas consumption is estimated by the compute costs for precompile functions used by developers to deploy smart contracts on ParallelChain Mainnet.

To use the functions, include the module `crypto` and then call the associate methods. For example,

```rust
use pchain_sdk::crypto;

crypto::sha256(input);
```