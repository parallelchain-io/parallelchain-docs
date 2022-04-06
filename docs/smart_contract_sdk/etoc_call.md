---
tags:
  - testnet 1.0
  - parallelchain sdk
---

# Calling a Contract from an Externally Owned Account

`Learning outcome`: _Interact with the smart contract using your account._

You need to perform some data preparation to feed arguments in your contract call if there are any. The light client accepts data as a byte string. In rust these types are `Vec<u8>` or `&[u8]` depending on the context of usage in rust.

Serialization is required for custom data types in rust such as `structs` and `enums`. If the data type you intend to send to the contract are rust primitive types, you can use any method provided by rust to convert such data type into a byte string. See ["Rust's Primitive Types"](https://doc.rust-lang.org/std/#primitives) on how to achieve this.

We recommend that you read the following two sections:

* [Publish the smart contract to public](#publish-the-smart-contract-to-public)
* [Extra: Preparing the data for serialization](#extra-preparing-the-data-for-serialization)

to facilitate the process of making calls to a smart contract. If you do not need any serialization and just want to make a simple smart contract call, you can skip to ["Calling the contract"](#calling-the-contract).


## Publish the Smart Contract to Public
Before allowing other people to interact with your smart contract, some items need to be known beforehand:

* smart contract's address
* callable functions and arguments format of smart contract

When using `pchain` to deploy a smart contract, we get the smart contract's address. For example:
```
Contract address: "yE/kZn44llWcFmit6BO/nXwe8vDAnpRil1rmryEomTA="
Hash of tx: "nhFOKjL51gbkbSg2ATBzka36y79izKQuLFFfV2k8DkI="
Status 200
Response "Your request has been received."
```
 
You can publish your contract address, argument format (See ["Defining Arguments to the Smart Contract"](../sdk_tutorial.md#defining-arguments-to-the-smart-contract)) and callable functions (See ["Defining Callable Functions through Arguments"](../sdk_tutorial.md#defining-callable-functions-through-arguments)) through offline channels so that other people can call your smart contract. 

Do not forget to open a discussion page to share your smart contract address and arguments at [ParallelChain Mainnet SDK Discussions](https://github.com/parallelchain-io/parallelchain-sdk/discussions).


## Calling the contract

See ["EtoC: Making calls to your Smart Contract"](../cli/tutorial.md#etoc-making-calls-to-your-smart-contract)

Consider a smart contract with String as input argument as below: 
```rust
fn contract(tx: Transaction<String>)
``` 

It requires a string as input value when calling from the client program `pchain`, which should be specified in the option `--data`:

```bash hl_lines="3 8"
pchain submit tx \
--from-address <YOUR_ACCOUNT_ADDRESS> \
--to-address <CONTRACT_ADDRESS> \
--value 0 \
--tip 0 \
--gas-limit <GAS_LIMIT> \
--gas-price 1 \
--data <BASE64_ENCODED_ARGUMENT> \
--nonce <YOUR_ACCOUNT_NONCE> \
--keypair <YOUR_ACCOUNT_KEYPAIR>
```

Where:

* `<CONTRACT_ADDRESS>` is the address of the smart contract being called.
* `<BASE64_ENCODED_ARGUMENT>` is the input value in format of encoded base64 string. For example, the string `hello world` could be encoded to **Borsh serialized** base64 encoded string `CwAAAGhlbGxvIHdvcmxk` which is the direct input to the option `--data`:

```bash hl_lines="6"
pchain submit tx \
--from-address 2oQRJJZ62AQteFw632lGYUVeEiHOaNn8oqLf3Q89jDU= \
--to-address Ou0NCikd0A0dohxMjMmpidnZqxQKww4vHV8o1vRBKpI= \
--value 0 \
--tip 0 \
--gas-limit 500000 \
--gas-price 1 \
--data CwAAAGhlbGxvIHdvcmxk \
--nonce 1 \
--keypair GzUaTtMz2pJkGDWGj9PeXQC0+P5ne2kX/pIyzTOKCuLahBEklnrYBC14XDrfaUZhRV4SIc5o2fyiot/dDz2MNQ==
```
_Please note the gas limit specified in `--gas-limit` should be at least 500000 gas for EtoC transaction._

We should get a successful request as shown below:
```
Hash of tx: "klaDUxdvFu0aVOg6Uu7RZSMuAOYWPMzOBeYJgtJo9c8="
Status 200
Response "Your request has been received."
```

To convert from string `hello world` to `CwAAAGhlbGxvIHdvcmxk`, please continue reading the section below. (["Extra: Preparing the data for serialization"](#extra-preparing-the-data-for-serialization))

## Extra: Preparing the data for serialization
The input argument to a smart contract can either be rust's primitive types or custom data types such as `structs` and `enums`. Do remember that custom data types have to be serialized/deserialized with the [`borsh`](https://borsh.io/) serialization protocol (See more on section ["Defining Arguments to the Smart Contract"](sdk_tutorial.md#defining-arguments-to-the-smart-contract)).


In `your_smart_contract/Cargo.toml`, import the required depencies to continue with the tutorial.
```rust
[dependencies]
base64 = "0.13"
borsh = "0.9"
```

### For arguments that are of rust's primitive types
Example 1: An example of the smart contract that takes the primitive type `String` as its argument type is shown below:
```rust hl_lines="2"
#[contract_init]
pub fn contract(tx: Transaction<String>)  
{ 
  tx.emit_event(
    format!("Topic: argument in smart contract").as_bytes(),
    format!("The argument to the smart contract is {}", &tx.arguments).as_bytes(),
  );
} 
```
In order to call the above smart contract, you need to prepare the argument in `String`
```rust
use borsh::{BorshSerialize, BorshDeserialize};
use base64;

fn main() {
  let args = "sample string argument".to_string();
  ...
```

Perform Borsh Serialization on your string argument
```rust
let mut buffer: Vec<u8> = Vec::new();
args.serialize(&mut buffer).unwrap();
```
Do [Base64 encoding](https://docs.rs/base64/latest/base64/) on serialized buffer and return a new string for your transaction.
```rust
let final_args = base64::encode(buffer);
```

Compile and run the code. You should be able to see the base64-encoded argument string on the console.
```bash
cargo build --release
cargo run
```
<details><summary>Terminal Output</summary>
```bash
FgAAAHNhbXBsZSBzdHJpbmcgYXJndW1lbnQ=
```
</details>

Full code : 
```rust
use borsh::{BorshSerialize, BorshDeserialize};
use base64;

fn main() {
  let args = "This is string argument".to_string();
    
  let mut buffer: Vec<u8> = Vec::new();
  args.serialize(&mut buffer).unwrap();
    
  let final_args = base64::encode(buffer);
  println!("{}", final_args);
}
```

### For custom data types such as `structs` or `enums`.
Example 2: An example of the smart contract takes custom data type `TelephoneContact` in argument
```rust hl_lines="3-7 10"
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

To call the above smart contract, you need to prepare the argument in `TelephoneContact`.
```rust
use borsh::{BorshSerialize, BorshDeserialize};
use base64;

#[derive(BorshSerialize, BorshDeserialize)]
struct TelephoneContact {
  person: String,
  phone_number: u64,
}

fn main() {
  let args = TelephoneContact{
    person: "Anna",
    phone_number: 998877665544,
  };
  ...
```

Do Borsh Serialization on `TelephoneContact` argument
```rust
let mut buffer: Vec<u8> = Vec::new();
args.serialize(&mut buffer).unwrap();
```
Do [Base64 encoding](https://docs.rs/base64/latest/base64/) on serialized buffer and return a new string for your transaction.
```rust
let final_args = base64::encode(buffer);
```

Compile and run the code. You should be able to see the base64-encoded argument string on the console.
```bash
cargo build --release
cargo run
```

Full code : 
```rust
use borsh::{BorshSerialize, BorshDeserialize};
use base64;

#[derive(BorshSerialize, BorshDeserialize)]
struct TelephoneContact {
  person: String,
  phone_number: u64,
}

fn main() {
  let args = TelephoneContact{
    person: "Anna",
    phone_number: 998877665544,
  };
    
  let mut buffer: Vec<u8> = Vec::new();
  args.serialize(&mut buffer).unwrap();
    
  let final_args = base64::encode(buffer);
  println!("{}", final_args);
}
```
