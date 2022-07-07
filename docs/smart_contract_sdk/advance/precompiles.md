---
tags:
  - testnet 2.0
  - parallelchain sdk
  - smart contract
  - precompiles
---

# Precompiles

Precompiles refers to code segment that can be used directly in a contract. As a result, the actual code is not necessary to be included in the contract so that gas cost can be reduced. Moreover, precompiles help in compilcated calculations which are not feasible in Wasmer enviornment (e.g. encryption, hashing).

In ParallelChain Mainnet SDK, precompiles are set of functions that can be called directly. Please be noted that some functions are selected based on popularity of blockchain ecosystem, but not necessary adopted in the implementation of ParallelChain Mainnet node.

| Function | Description | Gas Consumption|
|:---|:---|:---|
| random | psuedo random number | 25 |
| sha256 | SHA256 hash function | 55 x length of the input |
| keccak256 | Keccak-256 hash function | 16 x length of the input |
| keccak512 | Keccak-512 hash function | 16 x length of the input |
| ripemd160 | RIPEMD-160 hash function | 53 x length of the input |
| blake2b | Blake2 Hash Function (BLAKE2b) | 37 x length of the input |
| verify_signature | Verification on ed25519 signature | 14 x length of the input |

The gas consumption is estimated by the compute costs for precompile functions used by developers to deploy smart contracts on ParallelChain Mainnet.

To use the precompile functions, include the module `precompile` and then call the associate methods. For example,

```rust
use pchain_sdk::precompile;
...
precompile::sha256(input);
...
```