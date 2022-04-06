---
tags:
  - testnet 1.0
  - parallelchain sdk
---

# Creating a Smart Contract from Scratch

This section will guide you from creating your own smart contract to making calls to it.

## Complete Solution
The complete solution to `my_little_pony` smart contract is available at [https://github.com/parallelchain-io/example-smart-contracts.git](https://github.com/parallelchain-io/example-smart-contracts.git). 

You can use this smart contract as a reference.

## Prerequisites

All of these programs below need to be installed before proceeding. Click on the link in each bullet point to learn how to install these programs.

* [`Rust`](./installation.md#linux-and-macos)
* [`Wasm Toolchain`](./installation.md#install-wasm-toolchain): Web Assembly Module Toolkit for smart contracts.
* [`pchain`](./installation.md#using-precompiled-binaries): ParallelChain Light Client to deploy and call smart contracts.

### Create account

See ["Create a new Externally Owned Account (EOA)"](../cli/tutorial.md#create-a-new-externally-owned-account-eoa)

### Add tokens

See ["Request for tokens from the faucet"](../faucet/index.md#request-for-tokens-from-the-faucet)

## Download a Template Smart Contract

Download the template smart contract from this link:
[https://github.com/parallelchain-io/example-smart-contracts.git](https://github.com/parallelchain-io/example-smart-contracts.git). 

The downloaded folder should look like the code snippet below. The whole folder is called a `workspace` in rust.
```bash
parallelchain_sc_template
├── src/
│   └── lib.rs # The main source code of your smart contract.  
└── Cargo.toml # You import your packages and the SDK here
```

## Modify the smart contract and its manifest file

Every ParallelChain Mainnet Smart Contract is a rust crate with the SDK bindings attached. The first thing to do is to modify the name of the smart contract crate.

```bash
mv parallelchain_sc_template my_little_pony
```

Modify the manifest file at `Cargo.toml`:
```diff
[package]
-  name = "parallelchain_sc_template"
+  name = "my_little_pony"
version = "0.1.0"
- authors = ["ParallelChain User <someone@parallelchain.io>"]
+ authors = ["My Name <my_name@example.com>"]
edition = "2021"
...

```

By changing the `name` field in `Cargo.toml`, we are changing the Wasm's file name after building the contract. We expect to find the built contract in `target/wasm32-unknown-unknown/release/my_little_pony.wasm` for the same workspace.

You can also observe that the SDK is already included in the manifest file for you.

```rust
[dependencies]
borsh = "0.9"
smart_contract = { git = "https://github.com/parallelchain-io/parallelchain-sdk" }
```

## Contract Structure

Now, let us look at the main file of the smart contract:

```rust title="my_little_pony/src/lib.rs" linenums="1"
use smart_contract::{
    contract_init,
    sdk_method_bindgen,
    Transaction
};

use borsh::{BorshDeserialize, BorshSerialize};

// REPLACE ARGUMENT STRUCT WITH YOUR OWN IF ANY
#[sdk_method_bindgen]
#[derive(BorshSerialize, BorshDeserialize)]
struct MyStruct {
    // SETUP CONTRACT ARGUMENT
}

// The `contract_init` macro is required to convert the smart contract code
// from idiomatic rust to a contract that is readable and executable in
// ParallelChain Mainnet Fullnode.
#[contract_init]
// REPLACE `A` WITH RUST PRIMITIVE TYPES OR A CUSTOM DATA TYPE SUCH AS `MyStruct` 
pub fn contract(tx: Transaction<A>) {

    // CALL SDK METHODS HERE

    // WRITE CONTRACT STATE
    
    // ADD RESULT RETURN IF REQUIRED
}
```

The code snippet above is a skeleton contract that is ready to be filled in. There are a few points to note:

* [`Borsh`](https://borsh.io/) is imported into the smart contract file. This is a binary serializer and it serializes any custom data type into byte strings. Most of the methods in the SDK take in a byte string reference (`&[u8]`).
* The main entrypoint in the smart contract is called `contract()`. It only takes the SDK as its only argument (`smart_contract::Transaction<A>`). `A` is a rust generic, and it can take in any type. In this context, the concrete type to `A` can be a rust primitive type or a custom data type. 
* `A` is also known as the argument to the smart contract. Its values can be accessed by calling the field `arguments`.
```rust
fn contract(tx: Transaction<String>) {
    ...
    
    let argument = &tx.arguments;
    
    ...
}
```
* The macro `sdk_method_bindgen` provides typed "getter" and "setter" methods to the SDK. This macro is applied to an argument struct to the SDK (`A`). It allows easy interaction to the contract's state without performing much serialization. The `Borsh` serialization macro must also be applied to the argument struct.
* The macro `contract_init` transforms the `contract()` entrypoint from idiomatic rust code to a ParallelChain Mainnet Smart Contract. Standard rust return values will also be transformed into the SDK's `return_value` method and the value is returned as `receipts`.


We will start modifying the code to write a smart contract in the following sections.

## Add an Argument Struct

Let's modify the contract. 

```diff title="my_little_pony/src/lib.rs"
- // REPLACE ARGUMENT STRUCT WITH YOUR OWN IF ANY
- #[sdk_method_bindgen]
- #[derive(BorshSerialize, BorshDeserialize)]
- struct MyStruct {
-     // SETUP CONTRACT ARGUMENT
- }

+ pub struct MyLittlePony {
+     pub name: String,
+     pub age: u32,
+     pub gender: Gender,
+ }
```

We have created a struct called `MyLittlePony` that has the following fields:

* `name`: `String` 
* `age`: `u32`
* `gender`: `Gender`

As we can observe, the `gender` field type is a custom data type which is an [`enum`](https://doc.rust-lang.org/book/ch06-01-defining-an-enum.html). We will need to define this enum in the next section.

### Define an enum

Let's modify the contract to define an enum. 

```diff title="my_little_pony/src/lib.rs"
struct MyLittlePony {
     name: String,
     age: u32,
     gender: Gender,
}

+ pub enum Gender {
+     Female
+     Male
+ }
```

The enum `Gender` is a custom data type that contains variants. In this context, we have two variants: `Female` and `Male.` 

### Add `Borsh` Serializer

Let's modify the contract to add the `Borsh` serializer to the struct and the enum

```diff title="my_little_pony/src/lib.rs"
#[sdk_method_bindgen]
+ #[derive(BorshSerialize, BorshDeserialize)]
struct MyLittlePony {
     name: String,
     age: u32,
     gender: Gender,
}

+ #[derive(BorshSerialize, BorshDeserialize)]
pub enum Gender {
     Female
     Male
}
```
`BorshSerialize` and `BorshDeserialize` are macros that implemented serialization/deserialization traits. The macro `sdk_method_bindgen` requires these traits and will not compile if this requirement is not met. 

### add `sdk_method_bindgen` macro

Let's modify the contract to add the `sdk_method_bindgen` macro.

```diff title="my_little_pony/src/lib.rs"
+ #[sdk_method_bindgen]
#[derive(BorshSerialize, BorshDeserialize)]
struct MyLittlePony {
     name: String,
     age: u32,
     gender: Gender,
}
```

We do not need to add this macro to the enum in the previous section. It is only `MyLittlePony` struct that will be written to the contract's state.

With this macro, there will be two new methods provided by the SDK to quickly write `MyLittlePony` to the blockchain. The method names will have the following syntax
```rust
set_<MY_STRUCT_NAME_IN_SNAKE_CASE>
```

See ["Use the sdk methods from sdk_method_bindgen"](#use-the-sdk-methods-from-sdk_method_bindgen) for more information

We can still use the basic methods provided by the SDK which are `set` and `get`. However. we need to manually serialize and deserialize the data depending on the situation. We will demonstrate how to use `set_my_little_pony` and `get_my_little_pony` in later sections of the document.

## Parse the Arguments from the Smart Contract

As `my_little_pony` smart contract accepts arguments that are of type `MyLittlePony`, we can access the SDK's `arguments` field to be used in smart contract. You can access these values my modifying the smart contract.

```diff title="my_little_pony/src/lib.rs"
// The `contract_init` macro is required to convert the smart contract code
// from idiomatic rust to a contract that is readable and executable in
// ParallelChain Mainnet Fullnode.
#[contract_init]
- // REPLACE `A` WITH RUST PRIMITIVE TYPES OR A CUSTOM DATA TYPE SUCH AS `MyStruct` 
pub fn contract(tx: Transaction<A>) {

-    // CALL SDK METHODS HERE
     // `pony_specification` will be of `MyLittlePony` type. The SDK helps you deserialize
    // the byte string submitted by a smart contract caller into `MyLittlePony`.
+   let pony_specification = &tx.arguments;
    
    ...
}
```

We can now use `pony_specification` and its fields to perform any operations in the smart contract.

### Add an associated function and method

Let us add two functions as an `impl` to `MyLittlePony`. There are two types of functions that can be added as an `impl` to a struct in rust (Click on the bullet points for more information):

* [`Methods`](https://doc.rust-lang.org/book/ch05-03-method-syntax.html#defining-methods)
* [`Associated Functions`](https://doc.rust-lang.org/book/ch05-03-method-syntax.html#associated-functions)

We will add impl blocks.

```diff title="my_little_pony/src/lib.rs"
#[sdk_method_bindgen]
#[derive(BorshSerialize, BorshDeserialize)]
struct MyLittlePony {
     name: String,
     age: u32,
     gender: Gender,
}

+ impl MyLittlePony {
    
+    fn new(tx: Transaction<MyLittlePony>, name: &String, age: u32, gender: Gender) -> Self {
+        let little_pony = MyLittlePony {
+            name.to_owned(),
+            age,
+            gender,
+        };

+        //TODO: we will use the SDK method `set_my_little_pony` generated from the `sdk_method_bindgen` macro
+        // to write little_pony to the contract state in a later section.

+        little_pony

+    }

+    fn neigh(&self) {
+        //TODO: we will use the SDK method `get_my_little_pony` generated from the `sdk_method_bindgen` macro
+        // to get little_pony from the contract state in a later section.
+        // TODO: Add emit event in later section of the document
+    }
+ }
```

There are some points to note:

* `new()` is an associated function as it does not take itself as a receiver. You can call this function without having a struct initialized beforehand. `new()` initializes `MyLittlePony` struct for us from the arguments to the smart contract. To call `new()`, an example statement below is called:
```rust
MyLittlePony::new(tx, "Ginger".to_string(), 100, Gender::Female);
```
* `neigh()` is a method because it takes `MyLittlePony` as a receiver. You can tell that `neigh()` is a method from the keyword `&self` in its signature.  A `MyLittlePony` struct must be initialized before you can use this method. The example below shows how you can use `neigh()`:
```rust
let strong_pony = MyLittlePony { "Bruce".to_string(), 300, Gender::Male };
strong_pony.neigh();
```

## Use the methods from the SDK

### `set`
Let us use the SDK methods to write arbitrary data to the contract state.

```diff title="my_little_pony/src/lib.rs"
// The `contract_init` macro is required to convert the smart contract code
// from idiomatic rust to a contract that is readable and executable in
// ParallelChain Mainnet Fullnode.
#[contract_init]
- // REPLACE `A` WITH RUST PRIMITIVE TYPES OR A CUSTOM DATA TYPE SUCH AS `MyStruct` 
pub fn contract(tx: Transaction<A>) {
+     tx.set(
+        "some".to_string().as_bytes(), 
+        "value".to_string().as_bytes(), 
+      );
}
```

We need to take the arguments to `set` as a byte string.

### `get`

```diff title="my_little_pony/src/lib.rs"
// The `contract_init` macro is required to convert the smart contract code
// from idiomatic rust to a contract that is readable and executable in
// ParallelChain Mainnet Fullnode.
#[contract_init]
- // REPLACE `A` WITH RUST PRIMITIVE TYPES OR A CUSTOM DATA TYPE SUCH AS `MyStruct` 
pub fn contract(tx: Transaction<A>) {
+     // we use `unwrap()` because we just made a call to `set()` to store "value" in the contract state.
+     tx.get(
+        "some".to_string().as_bytes(), 
+      ).unwrap();
}
```

## Use the SDK methods from `sdk_method_bindgen`

In our context, the two new methods provided by the SDK are:
```rust
set_my_little_pony(&self, key: &[u8], value: &MyLittlePony)
get_my_little_pony(&self, key: &[u8]) -> Option<MyLittlePony>
```

### `set_my_little_pony`

We will modify the associated function `new()` to write to the contract state after initializing `MyLittlePony` struct.

```diff title="my_little_pony/src/lib.rs"
impl MyLittlePony {
    
    fn new(tx: Transaction<MyLittlePony>, name: &String, age: u32, gender: Gender) -> Self {
        let little_pony = MyLittlePony {
            name.to_owned(),
            age,
            gender,
        };

        //TODO: we will use the SDK method `set_my_little_pony` generated from the `sdk_method_bindgen` macro
        // to write little_pony to the contract state in a later section.
+       tx.set_my_little_pony(little_pony.name.as_bytes(), &little_pony);
        little_pony

    }

    ...
}
```

### `get_my_little_pony`

We will modify the method `neigh()` to get from the contract state.

```diff title="my_little_pony/src/lib.rs"
impl MyLittlePony {
    
    ...

+    fn neigh(&self, tx: Transaction<MyLittlePony>) {
-        // TODO: we will use the SDK method `get_my_little_pony` generated from the `sdk_method_bindgen` macro
-        // to get little_pony from the contract state in a later section.
+         match tx.get_my_little_pony(self.name.as_bytes()) {
+               Some(pony_name) => {
                   // TODO: Add emit event in later section of the document
+                   pony_name
+               },
+                None => {
                    // TODO: Add emit event in later section of the document
+                }       
+         };

+    }
+ }

    ...
}
```

## Emit event
You can emit events in `contract()` to understand how your smart contract works. Topic and Value can be primitive type or any custom data types. Please note that you will need [BorshSerialization](sdk_tutorial.md#serialization-protocols) for custom data types.

For now, for simplicity, we will use primitive type here. Therefore, we do not need BorshSerialization.

```diff title="my_little_pony/src/lib.rs"
// REPLACE ARGUMENT STRUCT WITH YOUR OWN IF ANY
#[sdk_method_bindgen]
#[derive(BorshSerialize, BorshDeserialize)]
struct MyLittlePony {
    name: String,
    age: u32,
    gender: Gender,
}

impl MyLittlePony {
    ...

    // assoicated function to emit event
    fn neigh(&self) {
-       // TODO: Add emit event in later section of the document
+       let topic: String = "Neigh Message".to_string();
+       let value: String = format!("Pony with name {} neighs", self.name);
+
+       tx.emit_event(topic.as_bytes(), value.as_bytes());
    }
}
```
When we call the function `neigh()`, it will emit an event with the pony name.
The resulting message will appear in a successfully executed transaction's Events field.

## Change `contract()` signature to return a receipt

In rust, you can always return some values in your function to provide feedback to users. You can return any primivite types, structs or even [`anyhow Result`](sdk_tutorial.md#returning-a-result).

```diff title="my_little_pony/src/lib.rs"
// The `contract_init` macro is required to convert the smart contract code
// from idiomatic rust to a contract that is readable and executable in
// ParallelChain Mainnet Fullnode.
#[contract_init]
- // REPLACE `A` WITH RUST PRIMITIVE TYPES OR A CUSTOM DATA TYPE SUCH AS `MyStruct` 
- pub fn contract(tx: Transaction<A>) {
+ pub fn contract(tx: Transaction<A>) -> Result<String> {

    // CALL SDK METHODS HERE

    // WRITE CONTRACT STATE

-    // ADD RESULT RETURN IF REQUIRED
+   Ok(format!("Welcome pony, {}", &strong_pony.name))
}

```
We are now returning a `Ok` Result with a `String`. This returns the string as a Receipt `return_value` from execution. 

## Putting it all together

Now that we have used all of the available tools from the SDK, let us check the source code.

<details>
  <summary>Click to see the whole source code of "my_little_pony"</summary>
```rust title="my_little_pony/src/lib.rs" linenums="1"
/*
Copyright (c) 2022 ParallelChain Lab
This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.
This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
*/

use smart_contract::{
    contract_init,
    sdk_method_bindgen,
    Transaction
};

use anyhow::Result;
use borsh::{BorshDeserialize, BorshSerialize};

// REPLACE ARGUMENT STRUCT WITH YOUR OWN IF ANY
#[sdk_method_bindgen]
#[derive(BorshSerialize, BorshDeserialize)]
pub struct MyLittlePony {
    pub name: String,
    pub age: u32,
    pub gender: Gender,
}

#[derive(BorshSerialize, BorshDeserialize, Clone)]
pub enum Gender {
    Female,
    Male,
}

impl MyLittlePony {
    
    fn new(tx: &Transaction<MyLittlePony>, name: &String, age: u32, gender: &Gender) -> Self {
        let little_pony = MyLittlePony {
            name: name.to_owned(),
            age,
            gender: gender.to_owned(),
        };

        tx.set_my_little_pony(little_pony.name.as_bytes(), &little_pony);

        little_pony

    }

    fn neigh(&self, tx: &Transaction<MyLittlePony>) {

        match tx.get_my_little_pony(self.name.as_bytes()) {
            Some(pony) => {
                let topic: String = "Neigh Message".to_string();
                let value: String = format!("Pony with name {} neighs at age: {}", self.name, pony.age);
                tx.emit_event(topic.as_bytes(), value.as_bytes());
            },
            None => {
                let topic: String = "No Pony".to_string();
                let value: String = format!("Pony not found");
                tx.emit_event(topic.as_bytes(), value.as_bytes());
            }       
        };

    }
}


// The `contract_init` macro is required to convert the smart contract code
// from idiomatic rust to a contract that is readable and executable in
// ParallelChain Mainnet Fullnode.
#[contract_init]
pub fn contract(tx: Transaction<MyLittlePony>) -> Result<String> {
    let pony_specification = &tx.arguments;
    let new_pony = MyLittlePony::new(
        &tx, 
        &pony_specification.name, 
        pony_specification.age, 
        &pony_specification.gender
    );

    Ok(format!("Welcome pony, {}", &new_pony.name))
```
</details>

There are some points to note and recap:

* `contract_init` must be placed on top of the `contract()` entrypoint function. Without it, the contract will not be a ParallelChain Mainnet Smart Contract and compilation of the contract will fail.
* `smart_contract::Transaction` is the imported SDK which we have been using to interact with `storage` in ParallelChain Mainnet.
* `sdk_method_bindgen` provides custom methods to interact with `storage` for any struct such as `MyLittlePony`.
* All structs and enums such as `MyLittlePony` and `Gender` that are intended to be used as inputs/outputs to the smart contract need to be serialized. The same is applicable to structs and enums that interact with `storage`. 
* `emit_event` method is useful for printing some messages from the smart contract. 
* return values are shown in receipts in a transaction.

## Compile contract
After you walkthrough the above steps, your contract should be ready to compile. Building a smart contract is a process to generate a Web Assembly (WASM) binary that will be used to deploy.

See [Building and deploying the contract](build_deploy_contract.md#building-the-contract).

Please note that the generated WASM binary maybe large in file size. It is always good to optimize the WASM binary code size using our optimize tool in order to save money and achieve better performance. 

See [Optimizing WASM binary for code size (optional)](build_deploy_contract.md#optimizing-wasm-binary-for-code-size-optional)

## Deploy Contract

Now, you can publish your smart contract to public by deploying on ParallelChain Mainnet. A contract account will be created with an unique contract address. Whenever people submit transaction with this contract address, it will execcute the contract code that assioiated with this contract account.

See [DeployC: Deploying your Smart Contract](../cli/tutorial.md#deployc-deploying-your-smart-contract)

Please note that whenever there are changes or new function updates in your smart contract, you are required to re-deploy the smart contract and this would result in different contract address. In other words, each contract address is referring a particular version of your smart contract.

## Make calls to contract

Finally, it is time to call your smart contract with the contract address. But before you can acutally submit a EtoC transaction, you need to prepare some informations first. Remember you defined the [argument struct](/smart_contract_sdk/zero_to_hero/#add-an-argument-struct) before, you need to prepare that argument and put it to data field in the transaction.

See [Calling a Contract from an Externally Owned Account](etoc_call.md)

## What is next?

Congratulations, you now know how to write a simple smart contract. Feel free to be creative by modifying `my_little_pony` smart contract. 

Some things that you can try are:

* In `my_little_pony`, add a new field in `MyLittlePony` struct called action to allow functions to be called in the contract.
* 

