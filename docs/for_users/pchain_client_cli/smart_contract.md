---
tags:
  - Tools
  - CLI
  - pchain-cli-rust
---


## Smart Contract

Smart contracts are computer programs that are stored on a blockchain. You need to provide some necessary information such as contract address, method name, and arguments in order to invoke method of the contract.

### Retrieve Contract Address
After you deploy the contract in a transaction, you should receive the contract address together with transaction hash. If you want to deploy contract and call method in the SAME transaction, it is possible to compute the contract address in advance.

You need to provide the account address and nonce when deploying the contract.

Command:

=== "Linux / macOS"
    ```bash
    ./pchain_client parse contract-address --address <ADDRESS> --nonce <NONCE>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe parse contract-address --address <ADDRESS> --nonce <NONCE>
    ```

### Prepare Contract Method Arguments File
When you make a contract call that modifies or views state, the contract method may expect arguments. You need to provide arguments by JSON file(.json) with `transaction create call` or `query view` commands.

Example:
For a contract method that accepts 3 arguments (String, Vec<i16> , boolean)
```sh
{
    "arguments": [
        {"argument_type": "String", "argument_value": "Yuru Camp"},
        {"argument_type": "Vec<i16>", "argument_value":"[-1, 20]"},
        {"argument_type": "bool", "argument_value": "true"}
    ]
}
```
Each object in an arguments array consists of two fields, `argument_type` and `argument_value`.
Here are some acceptable types and values.

| Type        | Description                                   | Example                              |
|-------------|-----------------------------------------------|--------------------------------------|
| `i8`        | The 8-bit signed integer type                 | "-128"                               |
| `i16`       | The 16-bit signed integer type                | "-32768"                             |
| `i32`       | The 32-bit signed integer type                | "-2147483648"                        |
| `i64`       | The 64-bit signed integer type                | "-9223372036854775808"               |
| `u8`        | The 8-bit unsigned integer type               | "255"                                |
| `u16`       | The 16-bit unsigned integer type              | "65535"                              |
| `u32`       | The 32-bit unsigned integer type              | "4294967295"                         |
| `u64`       | The 64-bit unsigned integer type              | "18446744073709551615"               |
| `String`    | String                                        | "\"This is test string\""            |
| `bool`      | Boolean                                       | "true" or "false"                    |
| `Vec<TYPE>` | Array with specific type and arbitrary length | "[65535,6535]" , "[true,false,true]" |
| `[5]`       | Array with specific length                    | "[1,2,3,4,5]"                        |
