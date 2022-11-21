---
tags:
  - testnet 3
  - parallelchain sdk
  - smart contract
  - world-state
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

The `#[contract]` macro transparently generates code that loads all of a Contract struct's fields from Storage before the execution of an Action or View method, and saves those fields into Contract Storage after an Action or View method returns. All types that implement the `Storage` trait can be used as a Contract field. Out of the box, this includes all Rust primitive types, as well as other commonly used types like `Option<T>`, `Result<T>`, `Vec<T>`, etc. In addition, structs defined by the developer can be made to implement `Storage` by applying the `#[contract_field]` macro on their definitions, provided that all of *their* fields implement Storage:
```rust
#[contract_field]
struct DogToy {
    ...
}
```

If your Contract struct contains fields, then the Contract should export an Init Method to initialize those fields.

### Storage and Collections

Because Storage is so gas-expensive, loading all of a Contract's fields before Method execution and writing them all into Storage after execution typically results in Contracts that are not very economical. For Contracts that do not keep much in Storage, or whose Methods *always* read and write into most fields, this maybe okay, or even ideal, however, some applications cannot avoid keeping a lot of on-chain state, and for these applications eagerly loading and saving fields in every call may be unacceptably expensive.

To solve this, the SDK includes a `pchain_sdk::collections` module. All of the types defined in this module 'lazily' load Storage: they only incur a read or write gas cost when the exact item in the collection is read from or written to. They also offer an API that can make working with large collections of data more convenient.

If your Contract struct contains `collections` fields and you do not have an Init Method that initializes these fields, then your Contract's collections will be initialize to their obvious default values upon their first read/write in an Action method. An example of initializing a collection type:

```rust
struct PrinceTheDog {
    nicknames: Vector<String>
}

impl PrinceTheDog {
    #[init]
    fn init() {
        PrinceTheDog {
            nicknames: Vector::new(),
        }.set();
    }
}
```

#### <u>Cacher (`Cacher<T>`)</u>

Wraps over any non-collections type that implements `Storage` and makes them lazy (all `collections` types are already lazy without Cacher). Cacher implements `Deref`, so `Cacher<T>` can be used *almost* everywhere `T` can be used without any special syntax. 

#### <u>Vector (`Vector<T>`)</u>

Lazily stores a list of items in `Storage`. Vector implements `Index`, `IndexMut`, and has an `iter` method, so most of the things you can do with `std::vec::Vec`, you can probably do with `Vector` too.

#### <u>Maps (`FastMap<K, V>` and `IterableMap<K, V>`)</u>

Collections include two types that store statically-typed mapping between keys and values. The difference between these two types is that IterableMap is, as its name suggests, iterable. i.e., it has the standard library's HashMap's `keys`, `iter`, and `values` sets of methods. This functionality comes at the cost of storing slightly more data in Storage than FastMap. Both types function identically otherwise, down to being able to nest like-Maps together (e.g., `FastMap<T, FastMap<K, V>>`, but *not* `FastMap<T, IterableMap<K, V>>`).

You should use IterableMap if your application absolutely needs to iterate through stored items, otherwise, use FastMap.