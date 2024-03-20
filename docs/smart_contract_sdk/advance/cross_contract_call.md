---
tags:
  - pchain-sdk
  - Smart Contract
  - Cross Contract Call
---

# Cross Contract Call

The SDK includes a pair of functions to make Contract-To-Contract internal calls:

- `call` and `call_untyped`

It does the obvious: to call a method in a specified Contract with the given arguments.

Related APIs in `pchain_sdk::internal`:

```rust
/// A call to contract. The caller should already know the data type of return value from the function call.
/// It returns Option of T where T is return value from the function. 
/// If data type T is different from the actual return value type of the function, None is returned.
fn call<T: borsh::BorshDeserialize>(address: PublicAddress, method_name: &str, arguments: Vec<u8>, value: u64) -> Option<T>;

/// A call to contract, with vector of bytes as return type.
/// It returns Option of Vec of bytes. Interpretation on the bytes depends on caller
fn call_untyped(contract_address: PublicAddress, method_name: &str, arguments: Vec<u8>, value: u64) -> Option<Vec<u8>>;
```
Check out the [Tutorial 4](/smart_contract_sdk/tutorial/chapter_4) for the tutorial that illustrates the details by example smart contract [ContractProxy](https://github.com/parallelchain-io/example-smart-contracts/blob/main/chapter_4/src/lib.rs).