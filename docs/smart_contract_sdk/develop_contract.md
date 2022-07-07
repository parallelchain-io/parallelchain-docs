---
tags:
  - testnet 2.0
  - parallelchain sdk
  - smart contract
  - rust
---

# Develop Contract

## Background Information
---

Before we dive deep into the structure of a contract and the SDK's semantics, we require some background information about the semantics enforced by ParallelChain Mainnet. The full capabilities of ParallelChain Mainnet continue to grow with active developers 
and a growing community. Let us see how smart contracts are called and processed by a node in the form of transactions.

First, the ParallelChain Light Client (`pchain`) will submit a list of transactions when making a smart contract call to the validating node using Proprietary serialization scheme [pchain-types](https://github.com/parallelchain-io/parallelchain-protocol-types). The node's mempool will process 
the list of transactions and check for validity and correctness. 

This validation check ensures that the transactions are:

- correctly formatted 
- signed by an account with enough gas fees to pay for the transactions

Therefore, transactions that fail the validation check may be included in a block. Future updates to ParallelChain Mainnet will address this issue. The transactions are then packed (typically every 5 seconds) in a block and sent to the execution engine in the 
node to execute the transactions. 

The executor will call `wasmer` (Web Assembly Engine) which provides an isolated context to perform the execution. This enables the smart contract code to read the current state of the blockchain and interact with it. However, the execution results are temporarily 
stored and subject to further checks before the block may be committed or rolled back on error. 

`wasmer` also computes the gas fees through a metering module. The metering module in `wasmer` is reponsible for limiting the execution to the amount of gas paid for by the `value/tip`. Each transaction returns a result in the form of receipts along with event
logs. The receipts, events, transactions and block itself will contain its own hash (or merkle proof) which is included in the same block.


## Smart Contract Development Kit
---

Smart Contract can be created by using [ParallelChain SDK](https://github.com/parallelchain-io/parallelchain-sdk). Example contract can be found in [example-smart-contracts](https://github.com/parallelchain-io/example-smart-contracts).

A ParallelChain Mainnet Smart Contract is a rust crate that imports the SDK. It uses the SDK's features to interact with the blockchain. The folder structure of a typical ParallelChain
Mainnet Smart Contract looks like this:
```bash
my_first_contract
├── src/
│   └── lib.rs # The main source code of your smart contract.  
└── Cargo.toml # You import your packages and the SDK here
```

For more information on rust's crate system, see [Rust Book Chapter 7: Packages and Crates](https://doc.rust-lang.org/stable/book/ch07-01-packages-and-crates.html?highlight=crate#packages-and-crates)

Please specify the dependency in `Cargo.toml` for using SDK by fetching from [crates.io](https://crates.io/) or repository in Github.

=== "Crates.io"
    ```rust
    [dependencies]
    pchain-sdk = "0.2.0"
    ```

=== "Github"
    ```rust
    [dependencies]
    pchain-sdk = { git = "https://github.com/parallelchain-io/parallelchain-sdk" }
    ```

## Smart Contract Programming Model
---

ParallelChain considers smart contract as a `struct` in `rust` programming language. A `struct` can have methods which are created by implementing `impl` for it. Hence, those methods are called entrypoint methods because they are the bodies of contract execution.

- **Entrypoint**: a starting point of contract execution. There are few kinds of entrypoints:
    - **Init**: entrypoint method that could be called during contract deployment transaction.
    - **Actions**: entrypoint method that is called in EtoC transaction.
    - **Views**: entrypoint method that is limited to executing read-only operaions
- **Contract MetaData**: descriptive string to describe the entrypoint methods inside a contract.
- **Contract Storage**: data pool that is accessible to the contract. It is represented as a key-value storage in world-state.


## Define Contract Entrypoint Method
---

ParallelChain Mainnet recogizes the entrypoint methods `init`, `actions` and `views`. In `rust` code, they are specified with method name and keyword `extern "C"` under macro `#[no_mangle]`. For example, 

```rust 
#[no_mangle]
pub extern "C" fn actions() {
  ...
}
```

ParallelChain SDK provides macros `contract` and `action` to provide better development experience. The below code illustrates a very simple contract to start with.

```rust
use smart_contract::{
    contract, action
}

#[contract]
struct MyContract {}

#[contract]
impl MyContract {

  #[action]
  fn hello_world() {

  }
}
```


The first macro `contract` on `struct` declares [Contract Storage](#contract-storage) for the contract.

The second macro `contract` on `impl` declares the entrypoint methods that can be executed. Only methods with macros `init`, `action` and `view` are recognized as entrypoint methods. Otherwise, methods are not visible to ParallelChain mainnet blockchain.

Hence, the above example code creates an entrypoint to execute the method `hello_world` when the contract is called by specifying the method name as `hello_world`. It is possible to define more than one `action` and `view` methods (See [Entrypoints](./advance/entrypoints.md)).

## Contract Storage
---

Contract can perform set and get of data to its own world-state just like an object can access and modify its own fields in common programming languages.
The concept is applied here to provide a structure that is familiar to contract developers. The macro `#[contract]` applied to a struct will create getter and setter methods for the fields inside.

```rust
#[contract]
struct MyContract{
    data: i32
}
```

In the body of smart contract, it can access the data and update it without explicity calling pchain_sdk::Transaction::get and pchain_sdk::Transaction::set, take care of name of the key and arguments parsing.
In this model,

- Key is an index in u8 integer format (hence, the maximum number of fields is 256). The above example, the key will be [0x0]. The order of intex is as same as the order of fields defined in the struct.
- Value are [borsh-serializable](https://crates.io/crates/borsh) and [borsh-deserializable](https://crates.io/crates/borsh)
- Value is primitive types (i8, u8, i16, u16, i32, u32, i64, u64, i128, u128, usize, String, bool, Vec<T> where T is a primitive type)


## Contract Metadata
---

Contract MetaData is descriptive information about the contract's entrypoint methods. It is represented as a trait of the contract.
Example:
```rust
pub trait HelloContract {
    fn hello();
    fn hello_from(name:String) -> u32;
}
```

Smart contract developers can share or even publish this information to the public so that others can interact with the contract in a proper way.
To enable this feature in contract, add keyword "meta" as attribute to the contract macro.

```rust
#[contract(meta)]
impl MyContract {
    ...
}
```
Under the hood, SDK generates a static slice variable __contract_metadata__ terminated by character '\0'. Its data resides in the memory section of the wasm code, which can be recognized by ParallelChain mainnet nodes.

## Input Arguments
---

In section [Call Contract (EtoC)](../getting_started/call_contract.md), the `data` field is vector of bytes composited of leading 4 bytes as format version number and the serialized bytes by using [pchain-types](https://crates.io/crates/pchain-types) on a struct `CallData`.

```
[format version (4 bytes)][raw content (bytes)]
```

```rust
pub struct CallData {
    /// function name of contract with entrypoint methods. Empty string indicates the contract without entrypoint methods
    pub method_name :String,

    /// arguments to function (entrypoint method)
    /// In contract with entrypoint methods, the arguments should be deserialized to vector of Vec<u8> and then pass as function arguments
    pub arguments :Vec<u8>
}
```

If the name of the entrypoint method is "hello", the value of field `method_name` should set to `"hello".to_string()`.

The field `arguments` is [Borsh](https://crates.io/crates/borsh) serialized bytes on `Vev<Vec<u8>>` where it is vector of bytes. The "bytes" are also Borst serialized from the input argument of the method according to its data type.

For example,
```rust
fn entrypoint_1(data: i32, name :String) { ...
```

The first "bytes" in `Vec<Vec<u8>>` is borsh serialized `i32` while the second "bytes" is borsh serialized `String`.

The simple `rust` program illustrates how to construct input arguments.

```rust
    // The expected name of entrypoint method
    let method_name = "entrypoint_1".to_string();
    // The input arguments to method entrypoint_1(data: i32, name :String)
    let data: i32 = 123;
    let name: String = "name".to_string();

    // First 4 bytes for format version
    let version_bs = (0 as u32).to_le_bytes().to_vec();

    let mut vec_arguments: Vec<Vec<u8>> = vec![];

    // For first argument `data`
    let mut args_bs:Vec<u8> = vec![]; 
    data.serialize(&mut args_bs).unwrap();
    vec_arguments.push(args_bs);

    // For second argument `name`
    let mut args_bs:Vec<u8> = vec![]; 
    name.serialize(&mut args_bs).unwrap();
    vec_arguments.push(args_bs);

    // Make CallData::arguments
    let mut arguments_bs :Vec<u8> = vec![];
    vec_arguments.serialize(&mut arguments_bs).unwrap();

    // Create CallData
    let call_data = CallData{
        method_name: method_name,
        arguments: arguments_bs
    };
    // Serialize by pchain-types
    let call_data_bs = CallData::serialize(&call_data);

    // the transaction data
    let tx_data = [version_bs, call_data_bs].concat();

    // Encode into Base64 data for submitting transaction
    assert_eq!(base64::encode(tx_data), "AAAAAAwAAAAYAAAAZW50cnlwb2ludF8xAgAAAAQAAAB7AAAACAAAAAQAAABuYW1l");
```

As a result, the field `data` is set to "AAAAAAwAAAAYAAAAZW50cnlwb2ludF8xAgAAAAQAAAB7AAAACAAAAAQAAABuYW1l" when submitting transaction:

```
pchain submit tx \
--from-address <YOUR_ACCOUT_ADDRESS> \
...
--data AAAAAAwAAAAYAAAAZW50cnlwb2ludF8xAgAAAAQAAAB7AAAACAAAAAQAAABuYW1l \
...
```

## Return Value
---

The return value from a contract call is in format of `Vec<u8>`. It is borsh-seralizable so that it is up to developer to design the response data structure.

```rust
pub struct Callback {
    return_val :Vec<u8>
}
```

## Basic functions
---

### Transaction::set
The SDK provides a method called set which allows you to interact with Storage. This is a low level method which deals with raw byte strings. 

Example:
```rust
let key = "a_key_in_storage".to_string();
let value: u32 = 5000
Transaction::set(&key.as_bytes(), &value.to_be_bytes());
```

The `set` method takes in two arguments, which must be raw byte strings (&[u8]). The first argument is the key that you intend to write to the Storage. The second argument is the value in the key-value pair that is converted into byte string. 

### Transaction::get

The `get` method queries the State. It is analogous to reading a database. This is a low level method which deals with raw byte strings.

Example:
```rust
let key = "a_key_in_storage".to_string();
Transaction::get(&key.as_bytes());
```

The get method takes in one argument, the key in Storage. This key also references a byte string (&[u8]). The return type for the get method is an `Option`. If the key contains a value, the value can be obtained from the [Contract Storage](#contract-storage). Otherwise if the `Option` is not handled properly, `wasmer` runtime will throw an error and there will be a Receipt indicating a runtime error (see [Status Code](../getting_started/status_code.md)).


### Transaction::emit_event

The `emit_event` method emits events from the smart contract. This is very useful if you wish to understand what is going on throughout the execution of your smart contract.

Example:
```rust
let topic: String = "Terminator Message".to_string();
let value: String = "Hasta La Vista, Baby!".to_string();
Transaction::emit_event(topic.as_bytes(), value.as_bytes());
```

The resulting message will appear in a successfully executed transaction's Events field.

### Transaction::return_value

The `return_value` method save value as return value in the Transaction Receipt.

Example:
```rust
let my_return_value: String = "I'll be back".to_string();
Transaction::return_value(my_return_value.as_bytes().to_vec());
```

The return value will appear as part of a successfully executed transaction's [Receipts](../concepts/transaction.md) field.
