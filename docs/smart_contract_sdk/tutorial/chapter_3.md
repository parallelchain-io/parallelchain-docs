---
tags:
  - pchain-sdk
  - tutorial
  - Smart Contract
---

# Chapter 3
We have introduced macros including `contract`, `contract methods`, and `call` in Chapter 1 and accessing values of
fields from storage in Chapter 2. In this chapter, we will put together all the knowledge and implement the bank
smart contract that simulates banking operations with data stored in ParallelChain Mainnet. 

Before diving into the writing of a smart contract, let's build the data struct `BankAccount` using the `sdk_method_bindgen`
macro provided by Parallelchain Mainnet Smart Contract SDK.

`bank_account.rs` defined the `BankAccount` data struct which consists of four fields, `first_name`, `last_name`, `account_id`, and `amount`. All these fields will be initialized to 0 or empty upon deployment.

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

After defining the data struct, add the following two functions which are responsible for
loading and storing the value with a given key accordingly. 
> **Note**
> The key to be stored is an u8 integer ordered by the index of the fields, e.g. `first_name`
has key [0].

`get_bank_account()` retrieve the value of the given key using `pchain_sdk::storage::get()`,
deserialize the result and return an `Option`.

`set_bank_account()` stores the given key-value pair in the storage using 
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

---

After having the `BankAccount` struct ready, it is time to start writing the bank smart contract. `use bank_account::BankAccount;` allows us to use the methods defined in `bank_account.rs`.

The macro `contract` on the data struct allows loading/storing fields from/into the world state. The contract struct, `MyBank`, has
only one field, `num_of_account`, indicating the number of accounts associated with this bank. As mentioned in the previous
section, the key of `num_of_account` in the storage will be [0] according to its index.


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

As mentioned in Chapter 1, the macro `#[contract_methods]` generates entrypoint methods that can be called in transaction.
We will create the first entrypoint method in `MyBank` impl. Firstly, we need the entrypoint method `open_account()` to create a brand-new account, with the specified `first_name`, `last_name`, `account_id`, and `initial_deposit`.

In `open_account()`, we initialize an instance of `BankAccount`, and store it in the storage directly by invoking `bank_account::set_bank_account`. 

After storing the newly generated account into storage, we have to update the `num_of_account`. Therefore, we obtain the value of the field by doing `MyBank::get_num_of_account()`, like how we get the fields of our pony in Chapter 2. Similarly, store the updated
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

Now, we have successfully created a function that creates a new bank account, we should support other
basic banking functionalities.

Let's start with checking account balance, users need to know how much money is left in their accounts.

By calling `bank_account::get_bank_account()` with a given `account_id`, `Option<BankAccount>` will be returned. If 
`None` is returned, the account does not exist; otherwise, we will be able to obtain the balance of the account by 
accessing the value of the field `amount`.

### lib.rs: query account balance
```rust
#[call]
fn query_account_balance(account_id: String) {
    match bank_account::get_bank_account(account_id.as_bytes()) {
        Some(balance) => {
            pchain_sdk::log(
                format!("bank: query_account_balance").as_bytes(),
                format!(
                    "The current balance is: {}", 
                    &balance.amount
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
`deposit_money` using the methods mentioned in all previous sections.

### lib.rs: withdrawal and deposit
```rust
#[call]
fn withdraw_money(account_id: String, amount_to_withdraw: u64) {
    match bank_account::get_bank_account(account_id.as_bytes()) {
        Some(mut query_result) => {
            match query_result.withdraw_from_balance(amount_to_withdraw) {
                Some(balance) => {

                    // update the world state
                    bank_account::set_bank_account(account_id.as_bytes(), &query_result);

                    pchain_sdk::log(
                        format!("bank: withdraw_money").as_bytes(),
                        format!("The updated balance is: \n
                        Name: {} {}\n
                        Account Number: {}\n
                        Balance: {}", 
                        &query_result.first_name,
                        &query_result.last_name,
                        &query_result.account_id,
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
        Some(mut query_result) => {
            query_result.deposit_to_balance(amount_to_deposit);

            // update the world state
            bank_account::set_bank_account(account_id.as_bytes(), &query_result);

            pchain_sdk::log(
                format!("bank: deposit_money").as_bytes(),
                format!("The updated balance is: \nName: {} {}\nAccount Number: {}\nBalance: {}", 
                &query_result.first_name,
                &query_result.last_name,
                &query_result.account_id,
                &query_result.amount).as_bytes()
            );
        },
        None => pchain_sdk::log(
            format!("bank: query_account_balance").as_bytes(),
            format!("No such account found").as_bytes()
        ),
    };
}
```

