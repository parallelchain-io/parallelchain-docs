# ParallelChain F Request for Comments 1 (PRFC 1)

| PRFC | Title | Author | Version | Date First Published |
| --- | ----- | ---- | --- | --- |
| 1   | Fungible Token Standard | ParallelChain Lab | 3 | July 23rd, 2022 | 

## Summary

ParallelChain RFC 1 defines a standard interface for fungible tokens implemented as ParallelChain F smart contracts. “Fungible tokens” is taken here to have the same meaning as in Ethereum's ERC 20, namely a set of identical, transferable entities. XPLL, for example, is a fungible token.

A standard contract interface for fungible tokens allows more seamless interoperability, since applications can make the simplifying assumption that all PRFC 1-implementing contracts always export the same, named set of Methods (they may export more).

The below section lists the set of methods that all smart contracts that want to be PRFC 1-compliant must implement, as well as  the behavior that each method must exhibit. Required behavior involves emitting certain events. These are also listed and described.

## Required types 

```rust
struct Token {
    name: String,
    symbol: String,

    // the number of decimals that should be divided from a token amount (such as that returned by method
    // 'balance_of') to get its user representation. i.e., `n` means to divide the token amount by `10^n`.
    decimals: u8,

    // ‘Supply’ here means the sum of token balances over *all* addresses at any given point of time.
    // Total supply may increase, for example, when new tokens are minted, or decrease, for example,
    // when tokens are burned.
    total_supply: u64
}
```

## Required Views 

The following uses syntax from Rust (version 1.59.0).

### `fn token() -> Token`

Returns information about the Token implemented by this contract.

### `fn allowance(owner: PublicAddress, spender: PublicAddress) -> u64`

Returns the number of tokens currently in spender's allowance that they can spend on ‘owner’s’ behalf.

### `fn balance_of(address: PublicAddress) -> u64`

Queries the balance for an owner account 'address'.


## Required Actions

### `fn transfer(to_address: Option<Public_Address>, amount: u64)`

'transfer' transfers 'amount' tokens to address 'to_address' from the account identified by txn.from_address. If `to_address` is None, this burns the amount.

Transfer must panic if txn.from_address's balance is less than amount.

Event `Transfer` must be emitted if 'transfer' is successful.


### `fn transfer_from(from_address: PublicAddress, to_address: Option<PublicAddress>, value: u64)`

'transfer_from' transfers 'amount' tokens to address 'to_address' on behalf of ‘owner’. If `to_address` is None, this burns the amount.

transfer_from must panic if get_allowance_from(owner) < value.

Event `Transfer` must trigger if ‘transfer_from’ is successful. Note that the value of the `Transfer` event must contain the owner address, not txn.from_address.


### `fn set_allowance(spender: PublicAddress, value: u64)`

'set_allowance' allows 'spender' to withdraw from your account multiple times, up to 'value' amount. If this function is called again it overwrites the current allowance with 'value'.

set_allowance must panic if txn.from_address's balance is less than 'value'.

Event `SetAllowance` must be emitted if set_allowance is successful.

## Required Events

In this section, `++` denotes bytes concatenation.

### `Transfer`

| Field | Value |
| ----- | ----- |
| Topic | `0u8 ++ owner_address ++ recipient_address: Option<PublicAddress>`  |
| Value | `amount` |

Gets emitted on successful token transfer through methods 'transfer' and 'transfer_from'.

### `SetAllowance`

| Field | Value |
| ----- | ----- |
| Topic | `1u8 ++ owner_address ++ spender_address` |
| Value | `amount` |

Gets triggered on successful delegation of tokens through method 'set_allowance'. 

## License

[GNU Public License 3](https://www.gnu.org/licenses/gpl-3.0.en.html).