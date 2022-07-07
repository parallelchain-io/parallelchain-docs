---
tags:
  - testnet 2.0
  - parallelchain sdk
  - smart contract
  - internal transaction
---

# Internal Transaction

Internal Transaction refers to transfer of tokens from a contract to another address. 

- Tokens being transferred are from the Contract instead of an External Owned Account.
- Gas is consumed in External Owned Account.
- No transaction receipt.
- Balance is adjusted only after success of the entire transaction.

SDK provides function `Transaction::pay` to transfer tokens from contract to another address.

See the example smart contract [contract_proxy](https://github.com/parallelchain-io/example-smart-contracts):
```rust
/// ### Lesson 4:
/// use method pay() to send tokens from this contract balance to specific address.
#[action]
fn send_tokens(value :u64){
    Transaction::pay(
        smart_contract::decode_contract_address("b12EJf3vd+UESLj4ZCBVSOqruUAgseQ6zVa3RW2Jcqg=".to_string()),
        value
    );
}
```