---
tags:
  - pchain-sdk
  - Smart Contract
  - Cryptographic Functions
---

# Cryptographic Functions

Cryptographic Function refers to cryptographic functions that are supported from execution runtime so that gas cost can be reduced by avoiding implementation inside a contract. Moreover, It helps in providing complicated calculations which are not feasible in the Wasmer environment (e.g. encryption, hashing).

Please be noted that some functions are selected based on the popularity of the blockchain ecosystem, but are not necessary adopted in the implementation of the ParallelChain Mainnet node.

| Function | Description | Gas Cost |
|:---|:---|:---|
| sha256 | SHA256 hash function |  16 x length of the input |
| keccak256 | Keccak-256 hash function | 16 x length of the input |
| ripemd | RIPEMD-160 hash function | 16 x length of the input |
| verify_ed25519_signature | Verification on ed25519 signature | 1,400,000 + 16 x length of the input |

The gas consumption is estimated by the compute costs for precompile functions used by developers to deploy smart contracts on ParallelChain Mainnet.

Related APIs in `pchain_sdk::crypto`:

```rust
/// Computes the SHA256 digest (32 bytes) of arbitrary input.
fn sha256(input: Vec<u8>) -> Vec<u8>;
/// Computes the Keccak256 digest (32 bytes) of arbitrary input.
fn keccak256(input: Vec<u8>) -> Vec<u8>;
/// Computes the RIPEMD160 digest (20 bytes) of arbitrary input.
fn ripemd(input: Vec<u8>) -> Vec<u8>;
/// Returns whether an Ed25519 signature was produced by a specified by a specified address over some specified message.
/// Contract call fails if the input `address` or `signature` is not valid.
fn verify_ed25519_signature(input: Vec<u8>, signature: Vec<u8>, address: Vec<u8>) -> bool;
```