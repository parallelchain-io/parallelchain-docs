---
tags:
  - testnet 1.0
  - parallelchain sdk
---

# Using ParallelChain Mainnet Smart Contract Development Kit (SDK)

`Learning outcome`: _Using the methods and macros provided by ParallelChain Mainnet SDK._

## Background Information

Before we dive deep into the structure of a contract and the SDK's semantics, we require some background information about the semantics enforced by ParallelChain Mainnet. The full capabilities of ParallelChain Mainnet continue to grow with active developers 
and a growing community. Let us see how smart contracts are called and processed by a node in the form of transactions.

First, the ParallelChain Light Client (`pchain`) will submit a list of transactions when making a smart contract call to the validating node using [Protocol Buffers](https://developers.google.com/protocol-buffers/) (protobuf). The node's mempool will process 
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

## Structure of a Contract

This section illustrates the frequently used components in a ParallelChain Mainnet Smart Contract. The following section [SDK Semantics](#sdk-semantics) will demonstrate how to use
the methods and macros provided by the SDK.

A ParallelChain Mainnet Smart Contract is a rust crate that imports the SDK. It uses the SDK's features to interact with the blockchain. The folder structure of a typical ParallelChain
Mainnet Smart Contract looks like this:
```bash
my_first_contract
├── src/
│   └── lib.rs # The main source code of your smart contract.  
└── Cargo.toml # You import your packages and the SDK here
```

For more information on rust's crate system, see [Rust Book Chapter 7: Packages and Crates](https://doc.rust-lang.org/stable/book/ch07-01-packages-and-crates.html?highlight=crate#packages-and-crates)

### Importing the SDK into your Smart Contract

For a rust crate to be considered as a qualified ParallelChain Mainnet Smart Contract, it must have the SDK imported into its crate. If you have downloaded any smart contract templates
in [https://github.com/parallelchain-io/example-smart-contracts.git](https://github.com/parallelchain-io/example-smart-contracts.git), the SDK is already imported.
The example `my_first_contract` illustrates this:

`my_first_contract/src/lib.rs`
```rust
use smart_contract::{
  contract_init,
  Transaction
};
```

and in `my_first_contract/Cargo.toml`
```rust
[dependencies]
parallelchain_smart_contract_sdk = { git = "https://github.com/parallelchain-io/parallelchain-sdk" }
```

If you intend to build a smart contract without using any of the templates in [https://github.com/parallelchain-io/example-smart-contracts.git](https://github.com/parallelchain-io/example-smart-contracts.git),
please see ["Creating and Deploying a Smart Contract from Scratch"](/smart_contract_sdk/zero_to_hero) for more information.

### Entrypoint and contract_init: 

In every ParallelChain Mainnet Smart Contract, we need to provide the following entrypoint function:

```rust
#[contract_init]
pub fn contract(tx: Transaction<A>) { } // where A = <TYPE_TO_BE_FED_TO_SDK>
```

The entrypoint name `contract()` is fixed and cannot be changed. `contract()` takes in only one argument: the SDK itself. `Transaction<A>` or `smart_contract::Transaction<A>` can read and write to the `Storage`
plus interact with the state of the same contract. 

The return type for `contract()` can be of type `anyhow::Result` or `None` (like this example). See the subsection ["Returning a Result"](#returning-a-result) on how to achieve 
this.

The `#[contract_init]` attribute macro is exclusively used for the `contract()` entrypoint to generate the necessary code to be a valid ParallelChain Mainnet Smart Contract. `#[contract_init]`
will write the necessary boilerplate code to allow `contract()` to be accessed from the execution engine to `wasmer`. All ParallelChain Mainnet Smart Contracts are expected to use `#[contract_init]`.

As you can observe, the `A` or `<TYPE_TO_BE_FED_TO_SDK>` is the data type intended to be fed into the SDK. The data types accepted are primitive data types such as `String` and `u32` or custom data
types such as `structs` and `enums`. These are explained in detail in the section ["Defining Arguments to the Smart Contract"](#defining-arguments-to-the-smart-contract).

### Returning a Result

Using `Result` is very common in Rust. ParallelChain Mainnet SDK supports the [`anyhow`](https://docs.rs/anyhow/latest/anyhow/) crate, allowing easy idiomatic error handling. A `Result` is an enum type, `Result<T,E>`.
That is, they contain values within variants such as the points below:

- `Ok(T)` - a `Result` variant that has succeeded and contains `T`. 
- `Err(E)` - a `Result` variant that has failed and contains `E`. 
- `T` and `E` refer to [generics](https://doc.rust-lang.org/stable/book/ch10-00-generics.html?highlight=generics#generic-types-traits-and-lifetimes) in rust. They can take in any data type.

`Result` can be used in any function. To use it in `contract()`, the semantics are shown below:

In `your_smart_contract/Cargo.toml`
```rust
[dependencies]
anyhow = "1.0"
```

In `your_smart_contract/src/lib.rs`
```rust
use anyhow::Result;

#[contract_init]
pub fn contract(tx: Transaction<A>) -> Result<()> 
{ 

  Ok(())
} 
```

The `#[contract_init]` macro will rewrite Result<()> into a method provided by the SDK called `return_value()`. This method takes in a byte string as an argument and returns it as a `Receipt`
from execution. See `Receipts` and how to read the status code in ["Transaction Receipt and Status"](../protocol/index.md)

### Defining Arguments to the Smart Contract

As explained in the section ["Returning a Result"](#returning-a-result), you probably understood that `A` or `{TYPE_TO_BE_FED_TO_SDK}` is also a Rust generic. The SDK's `smart_contract::Transaction` struct accepts 
both primitive and custom data types such as structs. However the struct needs to be serialized/deserialized with the [`borsh`](https://borsh.io/) serialization protocol. More details on this in the section
["Serialization Protocols"](#serialization-protocols). 

To access the arguments sent by a smart contract caller, you can use the SDK field `arguments`:
```rust
let an_argument = &tx.arguments;
```

or for fields in a struct
```rust
let a_struct_argument = &tx.argument;
let a_field = &a_struct_argument.a_field;
```

Example 1: An example of how to feed a primitive data type to the smart contract is shown below:
```rust
#[contract_init]
pub fn contract(tx: Transaction<String>)  
{ 
  tx.emit_event(
    format!("Topic: argument in smart contract").as_bytes(),
    format!("The argument to the smart contract is {}", &tx.arguments).as_bytes(),
  );
} 
```

In example 1, we do not have to worry about what the SDK method `emit_event` does. This will be described in detail in ["SDK Semantics"](#sdk-semantics). All you need to know is we are making the smart contract 
emit an event in the block that prints out the arguments to the smart contract. Furthermore, a primitive data type can be easily converted into bytes.

Another example of how to feed a custom data type to the SDK is shown.

Example 2: An example of how to feed a custom data type to the smart contract:
```rust
use borsh::{BorshSerialize, BorshDeserialize};

#[derive(BorshSerialize, BorshDeserialize)]
struct TelephoneContact {
  person: String,
  phone_number: u64,
}

#[contract_init]
pub fn contract(tx: Transaction<TelephoneContact>)  
{ 
  tx.emit_event(
    format!("Topic: argument in smart contract").as_bytes(),
    format!("The argument to the smart contract is person: {} with 
    phone number: {}", &tx.arguments.person, &tx.arguments.phone_number).as_bytes(),
  );
} 
```

As you can observe in Example 2, a struct `TelephoneContact` consists of the fields `person` and `phone_number`. A struct is a custom data type, and we cannot directly convert it to bytes. Therefore we need to use a 
serialization protocol to encode `TelephoneContact` into a byte string. `Borsh` helps us implement traits to serialize the argument data into bytes. We use byte strings because the smart contract arguments can be of 
any data type or size.

What example 2 does is similar to example 1, and the smart contract emits an event that prints out the fields values of `TelephoneContact`. Hence, the smart contract owner will need to inform others of the smart contract 
address and argument format when using `pchain` to make a contract call. 

### Defining Callable Functions through Arguments.

Since arguments to a smart contract can be anything, we can allow enums to call certain specific functions in our smart contract. An example is shown below:
```rust
use borsh::{BorshSerialize, BorshDeserialize};

#[derive(BorshSerialize, BorshDeserialize)]
enum OperationStatus {
  Working,
  Sleeping,
}

#[contract_init]
pub fn contract(tx: Transaction<OperationStatus>)  
{ 
  let operation_status = &tx.arguments;

  match operation_status {
    OperationStatus::Working => working_message(),
    OperationStatus::Sleeping => sleeping_message(),
  }
}

// functions that are not directly callable but require the SDK's arguments field
// to be made callable
fn sleeping_message(tx: &Transaction<OperationStatus>) {
  tx.emit_event(
    "sleeping".to_string().as_bytes(),
    "i am sleeping".to_string().as_bytes(),
  );
}

// functions that are not directly callable but require the SDK's arguments field
// to be made callable
fn working_message(tx: &Transaction<OperationStatus>) {
  tx.emit_event(
    "working".to_string().as_bytes(),
    "i am working".to_string().as_bytes(),
  );
}
```

As you can observe, we can call `working_message()` by supplying the `OperationStatus::Working` variant in the argument to our smart contract.

## SDK Semantics 

This section aims to explain the semantics of how a ParallelChain Mainnet Smart Contract interacts with its environment. There are two main types of interactions provided by the SDK. These are:

- `set`: Mutable action which receives the SDK and acts on the state of the blockchain.   
- `get`: Query action that gains read-only access to the data from the blockchain.
 
### `set` : Writing to `State`

The SDK provides a method called `set` which allows you to interact with `Storage`. This is a low level method which deals with raw byte strings. The usage syntax for this is:
```rust
// in `contract()`
let key = "a_key_in_storage".to_string();
let value: u32 = 5000
// syntax: set(&self, key: &[u8], value: &[u8])
tx.set(&key.as_bytes(), &value.to_be_bytes());
```

As you can see, it is a straightforward process. The `set` method takes in two arguments, which must be raw byte strings (`&[u8]`). The first argument is the `key` that you intend to write to the `Storage`.
The second argument is the value in the `key-value` pair that is converted into byte string. 

If the `key-value` pair you intend to write to `Storage` are primitive types, use `set`. Otherwise, for custom data types, the process is not straightforward. You need to serialize your data into byte string using `Borsh`. However, the SDK provides you with a macro called `#sdk_method_bindgen` to do this for you and create a typed "setter" method. 
See ["#[sdk_method_bindgen]: Macro to Create Typed Setters and Getters"](#sdk_method_bindgen-macro-to-create-typed-setters-and-getters)

### `get`: Querying the `State`

`get` queries the `State`. It is analogous to reading a database. This is a low level method which deals with raw byte strings. The usage syntax for this is:
```rust
// in `contract()`
let key = "a_key_in_storage".to_string();
// syntax: get(&self, key: &[u8]) -> Option<Vec<u8>>
let query_message = tx.get(&key.as_bytes());
// handle the return values from `get`
match query_message {
  Some(q) => {
    tx.emit_event(
      "query_message".to_string().as_bytes(),
      format!("{}", q).as_bytes(),
    );
  },
  None => {
    // the `None` variant is handled and the smart contract will
    // not return a `Receipt` that indicates a `runtime error`
    tx.emit_event(
      "query_message".to_string().as_bytes(),
      "no such key found".to_string().as_bytes(),
    );
  }
}

```

The `get` method takes in one argument, the `key` in `Storage.` This `key` also references a byte string (`&[u8]`). The return type for the `get` method is an 
[`Option`](https://doc.rust-lang.org/stable/book/ch06-01-defining-an-enum.html?highlight=optional%27#the-option-enum-and-its-advantages-over-null-values). If the `key` contains a value, the `value` can be obtained from 
the `Storage`. Otherwise if the `Option` is not handled properly, `wasmer` will throw an error and there will be a `Receipt` indicating a `runtime error`.

If the `key` pair you intend to query to `Storage` are primitive types, use `get`. Otherwise for custom data types, the process is not straightforward. You are required to deserialize your data from byte string into
its original data type using `Borsh`. However, the SDK provides you with a macro called `#sdk_method_bindgen` to do this for you and create a typed "getter" method. 
See ["#[sdk_method_bindgen]: Macro to Create Typed Setters and Getters"](#sdk_method_bindgen-macro-to-create-typed-setters-and-getters)

### `return_value`: Manually Returning a Receipt 

The `contract()` entrypoint function returns a pointer from `wasmer`. The `contract_init` macro automatically performs the smart contract returns for you, if you set the `contract()` entrypoint to return
a result. See ["Returning a Result"](#returning-a-result) for more explanation.

However, should you wish to return the value from the SDK manually, you can use the SDK's `return_value` method to do so. `return_value` behaves like `get` or `set` in the SDK. The syntax is shown below:
```rust
// in `contract()` entrypoint

// as `my_return_value` is a primitive type, we do not need BorshSerialization, we can just
// convert String to Vec<u8>
let my_return_value: String = "I'll be back".to_string();

// the syntax for `return_value` is
// return_value(&self, value: Vec<u8>)
tx.return_value(my_return_value.as_bytes().to_vec());
```

The return value will appear as part of a successfully executed transaction's `Receipts` field. See `Receipts` and how to read the status code in ["Transaction Receipt and Status"](../protocol/index.md).

### `emit_event`: Emitting an Event

The SDK provides a method for emitting events from the smart contract. This is very useful if you wish to understand what is going on throughout the execution of your smart contract. The syntax is shown below:
```rust
// in `contract()` entrypoint

// as `topic` and `value` is a primitive type, we do not need BorshSerialization.
let topic: String = "Terminator Message".to_string();
let value: String = "Hasta La Vista, Baby!".to_string();

// the syntax for `emit_event` is
// emit_event(&self, topic: &[u8], value: &[u8])
tx.emit_event(topic.as_bytes(), value.as_bytes());
```

The resulting message will appear in a successfully executed transaction's `Events` field.

### `#[contract_init]`: Macro to Initialize the Smart Contract

See ["Entrypoint and contract_init"](#entrypoint-and-contract_init)

The usage syntax is shown below:
```rust
#[contract_init]
pub fn contract(tx: Transaction<A>) -> Result<u32> { } // where A = {TYPE_TO_BE_FED_TO_SDK}
```

### `#[sdk_method_bindgen]`: Macro to Create Typed Setters and Getters

A typed "setter" method is a wrapper method around `set` that serializes the `key` and `value` arguments for you before writing. While a typed "getter" method is a wrapper method around `get` that deserializes the 
byte string returned by `get`.

This macro is meant to be used on custom data types such as `structs` and `enums`. Furthermore, the custom data types are required to implement the traits `BorshSerializable` and `BorshDeserializable`. To do this,
we can use the macros provided by `Borsh.` See ["Serialization Protocols"](#serialization-protocols) on how to do this.

To use the macro `sdk_method_bindgen`, you need to apply the attribute macro onto your custom data type. An example is shown below:
```rust
// remember to have the custom data types implement
// the trait BorshSerialize/BorshDeserialize
use borsh::{BorshSerialize, BorshDeserialize};

use smart_contract::{
  Transaction,
  sdk_method_bindgen,
};

#[derive(BorshSerialize, BorshDeserialize)]
#[sdk_method_bindgen]
struct Airline {
  name: String,
  code: String,
}
```

The `sdk_method_bindgen` macro will write the necessary boilerplate code to call typed "getter" and "setter" methods from the SDK. The available methods are shown in the example below:
```rust
// in `contract()` entrypoint 

let american_airlines = Airline { name: "American Airlines", code: "AA"};

// the methods available to the SDK as a result of `sdk_method_bindgen` and its syntax are:
// set_{STRUCT_NAME_IN_SNAKE_CASE}(&self, key: &[u8], value: &{STRUCT_NAME})
// get_{STRUCT_NAME_IN_SNAKE_CASE}(&self, key: &[u8]) -> Option<{STRUCT_NAME}>
tx.set_airline(format!("{}", &american_airlines).as_bytes(), &american_airlines);

// we know that there is a `key` called american_airlines in the blockchain
let airline_from_blockchain = tx.get_airline(format!("{}", &american_airlines).as_bytes()).unwrap();

tx.emit_event(
  "airline_name".to_string().as_bytes(),
  format!(
    "The airline is {} and its code is {}",
    &airline_from_blockchain.name,
    &airline_from_blockchain.code).as_bytes(),
);

```

To summarize, if you use `sdk_method_bindgen` macro and have a struct name called `MyStruct`, the SDK will give you methods that easily interact with the world state.
These methods are:

- `set_my_struct`
- `get_my_struct`

## Serialization Protocols

ParallelChain Mainnet SDK supports [`Borsh`](https://borsh.io/) as its serialization protocol. Custom data types are required to implement the `BorshSerializable` and `BorshDeserializable` trait to use the `get` and
`set` methods by the SDK. 

### Example: Use the macros provided by `Borsh` to implement the serialization/deserialization trait

In `your_smart_contract/Cargo.toml`
```rust
[dependencies]
borsh = "0.9"
```

In `your_smart_contract/src/{YOUR_SOURCE_CODE}.rs`
```rust
use borsh::{BorshSerialize, BorshDeserialize};

// the serialize/deserialize trait can be applied on structs
#[derive(BorshSerialize, BorshDeserialize)]
struct ElectricAppliance {
  lamp: PowerStatus,
  fan: PowerStatus,
}

// and enums too
#[derive(BorshSerialize, BorshDeserialize)]
enum PowerStatus {
  On,
  Off,
}
```

That is a lot to cover. You are ready to head to the next section of this guide. The next section covers building your contract
and making calls to it.
