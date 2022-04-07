---
tags:
  - testnet 1.0
  - parallelchain sdk
  - references
---

# SDK References

## Structs

### Transaction

`smart_contract::Transaction` is a handle to the SDK containing the parameters of a smart contract call. These parameters are provided by the contract caller. 

You will need to import this handle to use the SDK. This struct includes methods to allow smart contracts to maintain a state that is persistent and conforms to the requirements of a blockchain.

``` rust
pub struct Transaction<A> {
    pub this_block_number: u64,
    pub prev_block_hash: Sha256Hash,
    pub timestamp: u32,
    pub random_bytes: Sha256Hash,
    pub to_address: PublicAddress,
    pub from_address: PublicAddress,
    pub value: u64,
    pub transaction_hash: Sha256Hash,
    pub arguments: A,
}
```

In addition to primitive types in rust, there are some special types specified in ParallelChain Mainnet protocol. These are:

* `Sha256Hash`: A byte string (`Vec<u8>`) that is 32 bytes in length. Used as a datatype for hashing operations.
* `PublicAddress`: A byte string (`Vec<u8>`) that is 32 bytes in length. Used by addresses in ParallelChain.
* `A`: A generic type that is used as an argument to `smart_contract::Transaction`. This is the data type that you need to prepare before populating the `data` field when making a contract call. It can take rust's primitive types or custom data types as its concrete type. See [here](https://doc.rust-lang.org/stable/book/ch10-00-generics.html?highlight=generics#generic-types-traits-and-lifetimes) for more explanation on generics in rust.


The following subsections are associated functions and methods implemented by `smart_contract::Transaction`. All of these associated functions and methods implement the `BorshDeserialize` trait.
``` rust 
impl<A: BorshDeserialize> smart_contract::Transaction<A> {}
```

## Associated Functions

### **new()**

Default constructor for the smart contract. You do not need to call this associated function as it is already implemented by the `#[contract_init]` macro. 

**Function Signature**
``` rust
pub fn new() -> Self
```

**Basic Example**
```rust
// where 'A' is u32
let tx: smart_contract::Transaction<u32> = smart_contract::Transaction::<u32>::new();
```

**Parameters**

* None

**Return type**

`smart_contract::Transaction` with `A` as a concrete type.

**Returns**

SDK handle

## Methods

### **get(&self, key: &[u8])**

`get` returns Some(value) if a non-empty string is stored with key in the world state. If `get` fails, the smart contract terminates and any calls to `set` are not committed. Has a wrapper method that can be used when calling the `#[sdk_method_bindgen]` macro.


**Function Signature**
``` rust
pub fn get(&self, key: &[u8]) -> Option<Vec<u8>>
```

**Basic example**
```rust hl_lines="1"
let value = tx.get("my_key".as_bytes());
match value {
    Some(v) => tx.emit_event(
        format!("topic: get my_key").as_bytes(),
        format!("value: my_key has a value of {}", v).as_bytes()
    ),
    None =>  tx.emit_event(
        format!("topic: get my_key").as_bytes(),
        format!("value: no such key in blockchain").as_bytes()
    ),
}
```

**Parameters**

* `key` (reference to a byte string or `&[u8]`) : This is the key that you intend to query from the blockchain. 

**Return type**

`Option<Vec<u8>>` 

**Returns**

An optional in rust that could contain a value (`Some`) or not (`None`).

### **set(&self, key: &[u8], value: &[u8])**

`set` binds a key to a value in the blockchain. Both arguments to `set` can be of any data type before conversion to a reference of byte string.

**Function Signature**
``` rust
pub fn set(&self, key: &[u8], value: &[u8])
```

**Basic example**
```rust hl_lines="4-9"
let key: u32 = 12345;
let value: String = format!("It is a huge number!");

// the key-value pair is now stored in the contract state, which in turn will be 
// committed to the blockchain.
tx.set(
    key.as_bytes(),
    value.as_bytes(),
);
```

**Parameters**

* `key` (reference to a byte string or `&[u8]`) : This is the key that you intend to set to the blockchain.
* `value` (reference to a byte string or `&[u8]`) : This is the value corresponding to the key you intend to set to the blockchain.

**Return type**

* None

**Returns**

* None

### **emit_event(&self, topic: &[u8], value: &[u8])**

`emit_event` posts a message in the event fields in a `block`. You will need to use `pchain` to query a block and view the `events` field when `emit_event` is called. It can take any data types before conversion to a byte string.

**Function Signature**
``` rust
pub fn emit_event(&self, topic: &[u8], value: &[u8])
```

**Basic example**
```rust hl_lines="1-4"
tx.emit_event(
        format!("topic: a topic").as_bytes(),
        format!("value: The smart contract is talking about a topic").as_bytes()
);
```

**Parameters**

* `topic` (reference to a byte string or `&[u8]`) : This is the topic you intend to emit as an event in the `block`.
* `value` (reference to a byte string or `&[u8]`) : This is the value corresponding to the topic.

**Return type**

* None

**Returns**

* None

### **return_value(&self, value: Vec<u8\>)**

`return_value` returns any values in the smart contract back in the form of `receipts` of a block. This method is not required to be used when the `#[contract_init]` macro is being used on the `contract()` entrypoint function. If multiple calls to `return_value` is made, only the first call to `return_value` will be included in the block.

**Function Signature**
``` rust
pub fn return_value(&self, value: Vec<u8>)
```

**Basic example**
```rust hl_lines="2"
let some_return_value: u64 = 1_000_000;
tx.return_value(some_return_value.as_bytes().to_vec());
```

**Parameters**

* `value` (owned byte string or `Vec<u8>`) : The value to return as receipts in a block.

**Return type**

* None

**Returns**

* None

## Macros

### **contract_init**
`contract_init` transforms idiomatic rust smart contracts into ParallelChain Mainnet Smart Contracts.

This macro expects the trait `BorshSerialize/BorshDeserialize` to be implemented on a smart contract argument that is a struct or enum. See [https://docs.rs/borsh/0.9.3/borsh/index.html](https://docs.rs/borsh/0.9.3/borsh/index.html) on how to implement these traits.

**Macro**
``` rust
#[contract_init]
```

**Basic example**
```rust hl_lines="6"
// `contract_init` must be used if you intend to write
// the smart contract in idiomatic rust code.
//
// The return value of the entrypoint function `Result<u32>` will be emitted
// as protocol_types::Transaction::Receipts.
#[contract_init]
pub fn contract(tx: Transaction<MyArgument>) -> Result<u32> {
 
  //initialize MyArgument
  let my_argument = MyArgument { 
    first_argument: String::from("Hello ParallelChain"),
    second_argument: 555,
  }; 
 
  Ok(my_argument.second_argument)
}
```

### **sdk_method_bindgen**

`sdk_method_bindgen` provides “convenience methods” to interact with the world state using custom data types (structs).
These convenience methods are explicitly known as “typed methods” from the SDK.

This macro expects the trait `BorshSerialize/BorshDeserialize` to be implemented on a smart contract argument that is a struct or enum. See [https://docs.rs/borsh/0.9.3/borsh/index.html](https://docs.rs/borsh/0.9.3/borsh/index.html) on how to implement these traits.

**Macro**
``` rust
#[sdk_method_bindgen]
```

**Basic example**
```rust
// add in the BorshSerialize/BorshDeserialize macros to 
// automatically implement the traits required for the
// `sdk_method_bindgen` macro.
#[derive(BorshSerialize, BorshDeserialize)]
#[sdk_method_bindgen]
struct MyArgument {
  first_argument: String,
  second_argument: u32,
}
// the code illustrates the "typed methods" available to the 
// smart contract developer after this macro is used 
#[contract_init]
pub fn contract() {
 
  let tx = smart_contract::Transaction::<MyArgument>::new();
 
  //initialize MyArgument
  let my_argument = MyArgument { /.. / }; 
 
  // This is the "typed_set" method to bind a key to a value that is of a 
  // custom data type. See the `set` method of the SDK for more information. 
  // Note: the naming convention of this "typed_method" is in snake case.
  // It takes in a byte strting as the key and the custom data type itself as 
  // its value.
  tx.set_my_argument(key, value);
   
  // This is the "typed_get" method get a custom data type from the
  // world state. Some(value) is returned if a non-empty value
  // in the world state. See the `get` method of the SDK for more 
  // information. Note: the naming convention of this "typed_method" 
  // is in snake case. It takes in a byte strting as the key and the 
  // custom data type itself as its value.
  tx.get_my_argument(key);
}
```