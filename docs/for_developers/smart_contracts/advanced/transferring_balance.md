---
tags:
  - mainnet
  - testnet 4
  - parallelchain sdk
  - smart contract
  - internal transaction
---

# Transferring Balance

`pchain_sdk::transfer` transfers the balance from the Contract Account to another Account and returns the balance of the recipient after the transfer.

Related API in `pchain_sdk::internal`:

```rust
/// Transfer the balance amount to another address. 
fn transfer(recipient: PublicAddress, amount: u64);
```

!!! Notes
    - Balance is deducted from the contract, but not the caller's account.
    - Gas cost is deducted from the caller's account.

Example:

```rust
/// Use method transfer() to send tokens from this contract balance to a specific address.
#[call]
fn send_tokens(value :u64){
    let contract_address = Base64URL::decode("-jUt6jrEfMRD1JM9n6_yAASl2cwsc4tg1Bqp07gvQpU").unwrap().try_into().unwrap();
    pchain_sdk::transfer(
        contract_address,
        value
    );
}
```