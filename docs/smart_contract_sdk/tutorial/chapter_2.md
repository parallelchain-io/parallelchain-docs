---
tags:
  - pchain-sdk
  - tutorial
  - Smart Contract
---

# Chapter 2

In Chapter 2, we will implement a smart contract called `MyLittlePony`, to demonstrate how a contract can:

- define entrypoint methods
- define fields as data in contract storage


From the last chapter, we have learned that the macro `contract` on struct allows getting or setting data from or into the [world state](/concepts/storage/#world-state). 
The key to be stored started with a zero-indexed u8 integer ordered by the fields in the contract struct.

In this chapter, we first create a struct, `MyLittlePony`, that consists of `name`, `age`, and `gender`. In this case, `name` has key `[0]` while `age` has key `[1]`.


### lib.rs: define MyLittlePony
```rust
use pchain_sdk::{
    contract, contract_methods, call, contract_field
};

#[contract]
pub struct MyLittlePony {
    name: String,
    age: u32,
    gender: Gender,
}
```

Next, we need to declare the `Gender` struct, which is the type of the `gender` field. To use the nested
struct in the contract struct, we need the `contract_field` macro to define the key of its fields in canonical format.
For instance, `gender` should have a key started with `[2]` in the contract `MyLittlePony`, so the `name` in `Gender` struct has a key `[2][0]`.

### lib.rs: define Gender
```rust
#[contract_field]
struct Gender {
    name: String,
    description: String
}
```

---

After getting all the structs ready, we should start implementing the contract methods. This smart
contract should provide three method calls:

- self_introduction()
- grow_up()
- change_pony()


Firstly, `self_introduction()` uses receiver `&self` to load all data before executing this method.
All data will be loaded to the receiver self from the world state. In this way, we can obtain all the values of the fields in the contract, including the fields in the `Gender` struct.

### lib.rs: load data with &self
```rust
#[contract_methods]
impl MyLittlePony {
    
    #[call]
    fn self_introduction(&self) -> String {
        format!("Hi, I am {}. Age of {}. I am {} that means {}.",
            self.name, self.age, self.gender.name, self.gender.description)
    }
}
```

In the next method, we are going to illustrate how we can use contract getter and setter to access the data in the world state. The advantage of this is that the Write [gas cost](/concepts/gas/) is smaller compared to what we did in `self_introduction()`
because only one key-value pair (i.e. `age`) is involved.

Instead of passing `&self` as an argument, simply do `Self::get_<field_name>()` to get the value and 
`Self::set_<field_name>()` to set updated value.

### lib.rs: getter and setter
```rust
#[call]
fn grow_up() {
    let age = Self::get_age();
    Self::set_age(age+1)
}
```

Lastly, we want to change our little pony, so we use a mutable receiver `&mut self` to load data before
executing this method, then store all data after execution. However, we should be cautious when using
a mutable receiver as it is expensive to load and store since it mutates all key-value pairs (i.e. `name`, `age` and `gender`) in the world state.

### lib.rs: load data with a mutable receiver
```rust
#[call]
fn change_pony(&mut self, name: String, age: u32, gender_name: String, description: String) {
    pchain_sdk::log(
        "update_gender".to_string().as_bytes(), 
        format!("update name:{} description: {}", name, description).as_bytes());
    self.name = name;
    self.age = age;
    self.gender.name = gender_name;
    self.gender.description = description;
}
```

---

**Now you should have learned how to get and set contract fields in your smart contract.**