---
tags:
  - pchain-sdk
  - tutorial
  - Smart Contract
---

# Example - My Bank

We have introduced macros including `contract`, `contract methods`, and `call` in the contract [Hello Contract](1_hello_contract.md) and accessing values of
fields from storage in the contract [My Little Pony](2_my_little_pony.md). In this chapter, we will put together all the knowledge and implement the bank
smart contract that simulates banking operations with data stored in ParallelChain Mainnet.

Before diving into the writing of a smart contract, let's define the data struct `BankAccount` which will be stored in the storage.

`bank_account.rs` defined the struct `BankAccount` which consists of four fields, `first_name`, `last_name`, `account_id`, and `amount`.

### bank_account.rs: define data struct

```rust
use borsh::{BorshDeserialize, BorshSerialize};

use pchain_sdk::{
    storage,
};

// Note that both the serializer and deserializer macros such as Borsh need to 
// be applied to this struct for it to work.

#[derive(BorshSerialize, BorshDeserialize)]
pub struct BankAccount {
    pub first_name: String,
    pub last_name: String,
    pub account_id: String,
    pub amount: u64,
}
```

We are using **BorshDeserialize** and **BorshSerialize** in the above struct because we will serialize this struct into bytes and store them into the storage. Remember to update the Cargo.toml by adding the crate [borsh](https://crates.io/crates/borsh). We need the crate [base64](https://crates.io/crates/base64) for encoding the corresponding Account ID too. Therefore, add the following two lines under the dependencies section in Cargo.toml:

- base64 = "0.13"
- borsh = "=0.10.2"

Here we are not going to define the `BankAccount` data as a field in contract struct. But we will be able to load and save the data by using `storage::get` and `storage::set` explicitly. Let's add the following two methods for getting and setting the value with a given key. 

`get_bank_account()` retrieves the value (i.e. `BankAccount`) of the given key from storage by using `pchain_sdk::storage::get()`.

`set_bank_account()` stores the value (i.e. `BankAccount`) of the given key to storage by using 
`pchain_sdk::storage::set()`.

### bank_account.rs: accessing storage
```rust
pub fn get_bank_account(key: &[u8]) -> Option<BankAccount> {
    match storage::get(key) {
        Some(raw_result) => {
            let p: Option<BankAccount> =
                match BorshDeserialize::deserialize(&mut raw_result.as_ref()) {
                    Ok(d) => Some(d),
                    Err(_) => None,
                };
            p
        }
        None => None,
    }
}

pub fn set_bank_account(key: &[u8], value: &BankAccount) {
    let mut buffer: Vec<u8> = Vec::new();
    value.serialize(&mut buffer).unwrap();
    storage::set(key, buffer.as_ref());
}
```

Lastly, add the impl of `BankAccount` which includes two methods that perform the actions
of deposit and withdrawal.

### bank_account.rs: impl methods
```rust
impl BankAccount {
    pub fn deposit_to_balance(&mut self, amount_to_add: u64) {
        self.amount += amount_to_add;
    }
    pub fn withdraw_from_balance(&mut self, amount_to_withdraw: u64) -> Option<u64> {
        if amount_to_withdraw <= self.amount {
            self.amount -= amount_to_withdraw;
            Some(self.amount)
        } else {
            None
        }
    }
}
```

After having the `BankAccount` struct ready, it is time to start writing the bank smart contract. `use bank_account::BankAccount;` allows us to use the methods defined in `bank_account.rs`.

The macro `contract` above struct defines the basic structure of the contract which has
only one field, `num_of_account`, indicating the number of accounts associated with this bank. This field has a key `[0]`.

!!! Note
    The key to be stored started with a zero-indexed u8 integer ordered by the fields in the contract struct.

### lib.rs: define contract struct

```rust

use pchain_sdk::{
    contract, contract_methods, call, crypto
};

mod bank_account;

use bank_account::BankAccount;

#[contract]
struct MyBank {
    num_of_account: u64
}
```

Remember that the macro `#[contract_methods]` generates entrypoint methods that can be called in transaction.
We will create the first entrypoint method in `MyBank` impl. Firstly, we need the entrypoint method `open_account()` to create a brand-new account, with the specified `first_name`, `last_name`, `account_id`, and `initial_deposit`.

In `open_account()`, we initialize an instance of `BankAccount`, and store it in the storage directly by invoking `bank_account::set_bank_account()` with its Account ID as a key.

After storing the newly generated account into storage, we have to update the `num_of_account`. Therefore, we obtain the value of the field by doing `MyBank::get_num_of_account()`, like how we get the fields of our little pony in Chapter 2. Similarly, set the updated
value by calling `MyBank::set_num_of_account()`.
 
### lib.rs: open a new account
```rust
#[contract_methods]
impl MyBank {

    /// entrypoint method "open_account"
    #[call]
    fn open_account(
        first_name: String,
        last_name: String,
        account_id: String,
        initial_deposit: u64,
    ) {
        let parsed_account_id= 
        if account_id != "" {
            account_id.to_owned().as_bytes().to_vec()
        } else {

            // Generate a new account id using the base64 encoded sha256 hash
            // of the first and last name concatenated together

            let input = format!("{}{}", &first_name, &last_name).to_string().as_bytes().to_vec();
            crypto::sha256(input)
        };

        // Create a new instance of BankAccount
        let opened_bank_account = BankAccount {
            first_name: first_name.to_owned(),
            last_name: last_name.to_owned(),
            account_id:  base64::encode(parsed_account_id),
            amount: initial_deposit,
        };

        // Calling the functions from bank_account.rs
        bank_account::set_bank_account(
            &opened_bank_account.account_id.as_bytes(),
            &opened_bank_account
        );

        let initial_num_of_account = MyBank::get_num_of_account();
        MyBank::set_num_of_account(initial_num_of_account + 1);

        pchain_sdk::log(
            "bank_account: Open".as_bytes(),
            format!("Successfully opened 
            account for {}, {} 
            with account_id: {}",
            &opened_bank_account.first_name,
            &opened_bank_account.last_name,
            &opened_bank_account.account_id).as_bytes()
        );
    }
}
```

Now, we have successfully implemented an entrypoint method that creates a new bank account, we should support other
basic banking functionalities.

Let's start with checking account balance, users need to know how much money is left in their accounts.

The method `bank_account::get_bank_account()` returns `Option<BankAccount>` by a given `account_id`. If 
`None` is returned, the account does not exist; otherwise, we will be able to obtain the balance of the account by 
accessing the field `amount`.

### lib.rs: query account balance
```rust
#[call]
fn query_account_balance(account_id: String) {
    match bank_account::get_bank_account(account_id.as_bytes()) {
        Some(account) => {
            pchain_sdk::log(
                format!("bank: query_account_balance").as_bytes(),
                format!(
                    "The current balance is: {}", 
                    &account.amount
                ).as_bytes()
            );
        },
        None => {
            pchain_sdk::log(
                format!("bank: query_account_balance").as_bytes(),
                format!("No such account found").as_bytes()
            );
        }
    }
}

```

Lastly, finish up the functionalities of the bank by completing the implementation of `withdraw_money()` and 
`deposit_money()` using the methods mentioned in all previous sections.

### lib.rs: withdrawal and deposit
```rust
#[call]
fn withdraw_money(account_id: String, amount_to_withdraw: u64) {
    match bank_account::get_bank_account(account_id.as_bytes()) {
        Some(mut account) => {
            match account.withdraw_from_balance(amount_to_withdraw) {
                Some(balance) => {

                    // update the world state
                    bank_account::set_bank_account(account_id.as_bytes(), &account);

                    pchain_sdk::log(
                        format!("bank: withdraw_money").as_bytes(),
                        format!("The updated balance is: \n
                        Name: {} {}\n
                        Account Number: {}\n
                        Balance: {}", 
                        &account.first_name,
                        &account.last_name,
                        &account.account_id,
                        &balance).as_bytes()
                    );
                }
                None => pchain_sdk::log(
                    format!("bank: withdraw_money").as_bytes(),
                    format!("You do not have enough funds to withdraw from this account.").as_bytes()
                ),
            }
        },
        None => pchain_sdk::log(
            format!("bank: withdraw_money").as_bytes(),
            format!("No such account found").as_bytes()
        ),
    };
}
```

```rust
#[call]
fn deposit_money(account_id: String, amount_to_deposit: u64) {
    match bank_account::get_bank_account(account_id.as_bytes()) {
        Some(mut account) => {
            account.deposit_to_balance(amount_to_deposit);

            // update the world state
            bank_account::set_bank_account(account_id.as_bytes(), &account);

            pchain_sdk::log(
                format!("bank: deposit_money").as_bytes(),
                format!("The updated balance is: \nName: {} {}\nAccount Number: {}\nBalance: {}", 
                &account.first_name,
                &account.last_name,
                &account.account_id,
                &account.amount).as_bytes()
            );
        },
        None => pchain_sdk::log(
            format!("bank: query_account_balance").as_bytes(),
            format!("No such account found").as_bytes()
        ),
    };
}
```

