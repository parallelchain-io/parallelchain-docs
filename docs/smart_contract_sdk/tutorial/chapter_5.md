
---
tags:
  - pchain-sdk
  - tutorial
  - Smart Contract
---

# Chapter 5

In the previous chapters, we have understood how we write contract methods and access storage in the world state.
In this chapter, we are going to demonstrate the functionality of collections provided by `pchain-sdk`.

`pchain-sdk` provides several collection structures that are designed for gas efficiency:

- Cacher: allows lazy initialization
- Vector: lazily stores a list of items
- FastMap: lazily stores items into a key-value map
- IterableMap: lazily stores items into an iterable key-value map

We create the `MyCollections` struct which includes all these collection structures.


### lib.rs
```rust
use pchain_sdk::{
    contract, contract_methods, call, Cacher, collections::{Vector, FastMap, IterableMap}
};

type Address = [u8; 32];

#[contract]
pub struct MyCollections {
    lazy_cat: Cacher<String>,
    pretty_numbers: Vector<i32>,
    address_resolver: FastMap<Address, String>,
    prices: IterableMap<String, u32>
}
```

---

### Cacher
`Cacher` is a data wrapper to support Lazy Read and Lazy Write to Contract Storage.

```rust
#[contract_methods]
impl MyCollections {
     
    /// Here we use the receiver `&self` without loading data before executing this method. 
    #[call]
    fn meow(&self) -> String {
        // Actual loading happens here. 
        // Dereference as immutable and invoke a function as &String
        self.lazy_cat.to_uppercase()
    }
 
    /// Here we use the receiver `&mut self` without loading data before executing this method. 
    #[call]
    fn feed(&mut self, data: String) {
        // Dereference as mutable and then assign a value to it
        self.lazy_cat.set(data);
        // Actual saving happens after this method
    }
}
```

---

### Vector

``` rust
/// Here we use the receiver `&self` without loading data before executing this method. 
#[call]
fn pick(&self, index: usize) -> Option<i32> {
    // Actual loading happens here. 
    // Dereference as immutable and call functions from iterator
    self.pretty_numbers.iter().nth(index).map(|value| *value)
}
 
/// Here we use the receiver `&mut self` without loading data before executing this method. 
#[call]
fn push(&mut self, num: i32) -> usize {
    // Dereference as mutable and then set a value to it
    self.pretty_numbers.push(&num);
    self.pretty_numbers.len()
    // Actual saving happens after this method
}
```

---

### FastMap
```rust
/// Here we use the receiver `&self` without loading data before executing this method. 
#[call]
fn resolve(&self, address: Address) -> Option<String> {
    // Actual loading happens here. 
    // Dereference as immutable and call functions from key-value map
    self.address_resolver.get(&address)
}

/// Here we use the receiver `&mut self` without loading data before executing this method. 
#[call]
fn add_record(&mut self, address: Address, name: String) {
    // Dereference as mutable and then insert a value
    self.address_resolver.insert(&address, name)
    // Actual saving happens after this method
}
```

---

### IterableMap
```rust
/// Here we use the receiver `&self` without loading data before executing this method. 
#[call]
fn price(&self, item: String) -> Option<u32> {
    // Actual loading happens here. 
    // Dereference as immutable and call functions from key-value map
    self.prices.get(&item)
}

/// Here we use the receiver `&mut self` without loading data before executing this method. 
#[call]
fn set_price(&mut self, item: String, price: u32) {
    // Dereference as mutable and then insert a value
    self.prices.insert(&item, price);
    // Actual saving happens after this method
}

/// This method the iterable-map to calculate the sum of the values by iterating each item.
#[call]
fn total_price(&self) -> u32 {
    self.prices.values().sum()
}

```