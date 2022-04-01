---
tags:
  - testnet 1.0
  - parallelchain sdk
---

# Calling a Contract from an Externally Owned Account

`Learning outcome`: _Interact with the smart contract using your account._

In ParallelChain Mainnet, there are two kinds of accounts. `Externally Owned Account(EOA)` refers to an account with a public and private key pair that holds your tokens and nonce. The address of EOA is refers to its public key. The second type is `Contract Account` which is associated with smart contract code. Furthermore, the contract account does not have a private key. The address of contract account is generated when a smart contract is deployed.


## Publish the smart contract to public
After deploying the contract, you should be able to obtain the contract address if the deploy command has succeeded. 

You can publish your contract address, argument format (See ["Defining Arguments to the Smart Contract"](../sdk_and_writing_contract/#defining-arguments-to-the-smart-contract)) and callable functions (See ["Defining Callable Functions through Arguments"](../sdk_and_writing_contract/#defining-callable-functions-through-arguments)) through offline channels so that other people can call your smart contract. Do not forget to open a discussion page to share your smart contract address and arguments at [ParallelChain Mainnet SDK Discussions](https://github.com/parallelchain-io/parallelchain-sdk/discussions).

## Preparing the data for serialization
Before you call a smart contract, you need to prepare the arguments for the contract first. The argument of each contract is dependent on how it is defined in the smart contract. The argument can either be rust primitive types or custom data types such as `structs` and `enums`. Do remember that custom data types have to be serialized/deserialized with the [`borsh`](https://borsh.io/) serialization protocol (See more on section ["Defining Arguments to the Smart Contract"](../sdk_and_writing_contract/#defining-arguments-to-the-smart-contract)).

### For primitive types argument
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
```rust hl_lines="3 4 5 6 7 10"
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

## Calling the contract

See ["EtoC: Making calls to your Smart Contract"](../cli/real_world_walkthrough.md#etoc-making-calls-to-your-smart-contract)
