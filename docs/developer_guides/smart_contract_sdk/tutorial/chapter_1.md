---
tags:
  - pchain-sdk
  - tutorial
  - Smart Contract
---

# Chapter 1

In this chapter, we will go through the steps of creating a simple smart contract with the help of `pchain-sdk`.

Firstly, we have to prepare the `Cargo.toml`, which specifies the name, version, and year of edition of the smart contract. We need to use `pchain-sdk` for this smart contract development, so remember to add `pchain-sdk = { version = "LATEST_VERSION"}` under dependencies.
### Cargo.toml
```toml
[package]
name = "hello_contract"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[lib]
crate-type = ["cdylib"]

[dependencies]
pchain-sdk = { version = "LATEST_VERSION"}
```

---

After preparing `Cargol.toml`, we can now start preparing the smart contract.  

`#[contract]` defines basic struct as a programming model of a contract. 
Fields are data representations of contract storage.

`#[contract_methods]` defines impl for the contract struct. 
Methods declared in the impl are callable by Transaction Command Call if their visibility is `pub`.

`#[call]` macro applies to impl methods for the contract method call. 

In the file, we have added a method called `hello()`, `pchain_sdk::log()` logs the key and value and stores them into a tmp directory when the method is invoked.

### lib.rs
```rust
use pchain_sdk::{
    contract, contract_methods, call, storage, log, 
};

#[contract]
struct HelloContract {}

#[contract_methods] 
impl HelloContract {
    
    #[call]
    fn hello() {
        pchain_sdk::log(
            "topic: Hello".as_bytes(), 
            "Hello, Contract".as_bytes()
        );
    }
}
```

---

Next, we can add two other methods, which illustrate how we can set or read values from the storage.

```rust
#[call]
fn hello_set_many() {
    for i in 1..10{
        let key = format!("hello-key-{}", i);
        let value = vec![0_u8; 1024*10]; //10KB
        storage::set(key.as_bytes(), &value);
    }
}

#[call]
fn hello_read_many() {
    for i in 1..10{
        let key = format!("hello-key-{}", i);
        let value = storage::get(key.as_bytes());
        if value.is_some(){
            log(
                "topic: Hello read".as_bytes(), 
                format!("key: {}, len: {}", key, value.unwrap().len()).as_bytes()
            );
        }
    }
}

```