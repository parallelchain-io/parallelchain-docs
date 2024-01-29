---
tags:
  - pchain-sdk
  - Smart Contract
  - World State
---


# Contract Storage

Contracts can use Storage to persist data between calls. The simplest way to read and write data into Storage is to add fields to the Contract struct:

```rust
#[contract]
struct PrinceTheDog {
    age: u8, 
    breed: String,
    hungry: bool,
    toy: DogToy,
}
```

The `#[contract]` macro transparently generates code that loads all of a Contract struct's fields from Storage before the execution of the contract method and saves those fields into Contract Storage after the contract method returns. All types that implement the `Storage` trait can be used as a Contract field. Out of the box, this includes all Rust primitive types, as well as other commonly used types like `Option<T>`, `Result<T>`, `Vec<T>`, etc. In addition, structs defined by the developer can be made to implement `Storage` by applying the `#[contract_field]` macro on their definitions, provided that all of *their* fields implement Storage:

```rust
#[contract_field]
struct DogToy {
    ...
}
```

## Accessing Storage
---

SDK provides functions to access Storage by read-write operations. The function `get_network_state` is particularly used to get the state from the Network Account of the blockchain.

Related APIs in `pchain_sdk::storage`:

```rust
/// Gets the value, if any, associated with the provided key in this Contract Storage.
pub fn get(key: &[u8]) -> Option<Vec<u8>>;
/// Gets the value, if any, associated with the provided key in Network Account's Storage.
pub fn get_network_state(key: &[u8]) -> Option<Vec<u8>>;
/// Binds the provided key to the provided value in this Contract's Storage.
pub fn set(key: &[u8], value: &[u8]);
```

## Storage and Collections
---

Because Storage is so gas-expensive, loading all of a Contract's fields before Method execution and writing them all into Storage after execution typically results in Contracts that are not very economical. For Contracts that do not keep much in Storage, or whose Methods *always* read and write into most fields, this may be okay, or even ideal, however, some applications cannot avoid keeping a lot of on-chain states, and for these applications eagerly loading and saving fields in every call may be unacceptably expensive.

To solve this, the SDK includes a `pchain_sdk::collections` module. All of the types defined in this module 'lazily' load Storage: they only incur a read or write gas cost when the exact item in the collection is read from or written to. They also offer an API that can make working with large collections of data more convenient.

```rust
#[contract]
struct PrinceTheDog {
    nicknames: Vector<String>
}
```

### <u>Cacher (`Cacher<T>`)</u>

Wraps over any non-collections type that implements `Storage` and makes them lazy (all `collections` types are already lazy without Cacher). Cacher implements `Deref`, so `Cacher<T>` can be used *almost* everywhere `T` can be used without any special syntax. 

In the example below, the contract field `lazy_cat` is a `Cacher` of `String`. Passing the immutable receiver `&self` to the method does not immediately load its data from the world state until dereferencing the field `lazy_cat`. In the method`set_cat_name`, the calling function `set` does not immediately save its data to the world state until the end of this method call.

```rust
#[contract]
struct Pet {
    lazy_cat: Cacher<String>
}

#[contract_methods]
impl Pet { 

    //...

    #[call]
    fn cat_name(&self) -> String {
        self.lazy_cat.to_uppercase()
    }

    #[call]
    fn set_cat_name(&mut self, name: String) {
        self.lazy_cat.set(name);
    }

    // ...
}
```

### <u>Vector (`Vector<T>`)</u>

Lazily stores a list of items in `Storage`. Vector implements `Index`, `IndexMut`, and has an `iter` method, so most of the things you can do with `std::vec::Vec`, you can probably do with `Vector` too.

In the below example, the contract field `nicknames` is a `Vector` of `String`. It loads data from the world state only when it is needed to be read, and stores data after the method call. It calls `iter()` to get an iterator as `std::vec::Vec` and cal `push()` to save data to the world state. Besides, it supports indexing for accessing particular indexed elements.

```rust
#[contract]
struct Me {
    nicknames: Vector<String>
}

#[contract_methods]
impl Me { 
    
    // ...

    #[call]
    fn my_name_is(&self, name: String) -> bool {
        self.nicknames.iter().any(|n| n == &name)
    }

    #[call]
    fn call_me(&mut self, name: String) {
        self.nicknames.push(&name);
    }

    // ...
}
```

### <u>Maps (`FastMap<K, V>` and `IterableMap<K, V>`)</u>

Collections include two types that store statically typed mapping between keys and values. The difference between these two types is that IterableMap is, as its name suggests, iterable. i.e., it has the standard library's HashMap's `keys`, `iter`, and `values` sets of methods. This functionality comes at the cost of storing slightly more data in Storage than FastMap. Both types function identically otherwise, down to being able to nest like-Maps together (e.g., `FastMap<T, FastMap<K, V>>`, but *not* `FastMap<T, IterableMap<K, V>>`).

You should use IterableMap if your application needs to iterate through stored items, otherwise, use FastMap.

#### Example - FastMap<K, V\>

```rust
type Address = [u8; 32];

#[contract]
struct NameResolution {
    name_mapping: FastMap<Address, String>
}

#[contract_methods]
impl NameResolution { 
    
    // ...

    #[call]
    fn resolve(&self, address: Address) -> Option<String> {
        self.name_mapping.get(&address)
    }

    #[call]
    fn add_record(&mut self, address: Address, name: String) {
        self.name_mapping.insert(&address, name)
    }

    // ...
}
```

#### Example - IterableMap<K, V\>


```rust

#[contract]
struct Market {
    prices: IterableMap<String, u32>
}

#[contract_methods]
impl Market {

    // ...

    #[call]
    fn price(&self, item: String) -> Option<u32> {
        self.prices.get(&item)
    }

    #[call]
    fn set_price(&mut self, item: String, price: u32) {
        self.prices.insert(&item, price);
    }

    #[call]
    fn total_price(&self) -> u32 {
        self.prices.values().sum()
    }

    // ...
}
```