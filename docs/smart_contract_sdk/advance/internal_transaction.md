---
tags:
  - testnet 3
  - parallelchain sdk
  - smart contract
  - internal transaction
---

# Transferring Balance

`pchain_sdk::pay` transfers balance from the Contract Account to another Account and returns the balance of the recipient after the transfer.


See the example smart contract "ContractProxy" in [chapter_3](https://github.com/parallelchain-io/example-smart-contracts) of repository of Parallelchain Lab.

```rust
/// ### Lesson 4:
/// use method pay() to send tokens from this contract balance to specific address.
#[action]
fn send_tokens(value :u64){
    let contract_address = pchain_types::Base64URL::decode("-jUt6jrEfMRD1JM9n6_yAASl2cwsc4tg1Bqp07gvQpU").unwrap().try_into().unwrap();
    pchain_sdk::pay(
        contract_address,
        value
    );
}
```