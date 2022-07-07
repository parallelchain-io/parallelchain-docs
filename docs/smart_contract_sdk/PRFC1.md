# ParallelChain F Request for Comments 1 (PRFC 1)

| PRFC | Title | Author | Version | Date First Published |
| --- | ----- | ---- | --- | --- |
| 1   | Fungible Token Standard | ParallelChain Lab | 1 | July 7th, 2022 | 

## Summary

ParallelChain RFC 1 defines a standard interface for fungible tokens implemented as ParallelChain F smart contracts. “Fungible tokens” is taken here to have the same meaning as in Ethereum's ERC 20, namely a set of identical, transferable entities. XPLL, for example, is a fungible token.

A standard contract interface for fungible tokens allows more seamless interoperability, since applications can make the simplifying assumption that all PRFC 1-implementing contracts always export the same, named set of Methods (they may export more).

The below section lists the set of methods that all smart contracts that want to be PRFC 1-compliant must implement, as well as  the behavior that each method must exhibit. Required behavior involves emitting certain events. These are also listed and described.

## Required Methods 

Note: the following uses syntax from Rust (version 1.59.0).

**ACTION** 

`fn transfer(to_address: protocol_types:Public_Address, value: u64)`

'transfer' transfers 'value' tokens to address 'to_address' from the account identified by txn.from_address.

Transfer must panic if txn.from_address's balance is less than amount.

Event `Transfer` must be emitted if 'transfer' is successful.


**ACTION** 

`fn transfer_from(from_address: protocol_types:Public_Address, to_address: protocol_types:Public_Address, value: u64)`

'transfer_from' transfers 'amount' tokens to address 'to_address' on behalf of ‘owner’.

transfer_from must panic if get_allowance_from(owner) < value.

Event `Transfer` must trigger if ‘transfer_from’ is successful. Note that the value of the `Transfer` event should contain the owner address, not txn.from_address.


**ACTION** 

`fn set_allowance(spender: PublicAddress, value: u64)`

'set_allowance' allows 'spender' to withdraw from your account multiple times, up to 'value' amount. If this function is called again it overwrites the current allowance with 'value'.

set_allowance must panic if txn.from_address's balance is less than 'value'.

Event `SetAllowance` must be emitted if set_allowance is successful.


**VIEW** 

`fn get_allowance(owner: PublicAddress, spender: PublicAddress) -> u64`

‘get_allowance’ returns the number of tokens currently in spender's allowance that they can spend on ‘owner’s’ behalf.


**VIEW**

`fn total_supply() -> u64`

'total_supply' returns the total token supply. ‘Supply’ here means the sum of token balances over *all* addresses at any given point of time. Total supply may increase, for example, when new tokens are minted, or decrease, for example, when tokens are burned. PRFC 1 has no opinion about how to implement minting and burning, nor does not require that contracts implement them.


**VIEW** 

`fn balance_of(address: PublicAddress) -> u64`

'balance_of' queries the balance for an owner account 'address'.

## Required Events

`Transfer`

| Field | Value |
| ----- | ----- |
| Topic | `0u8` |
| Value | `owner_address: PublicAddress \|\| recipient_address: PublicAddress` |

Gets emitted on successful token transfer through methods 'transfer' and 'transfer_from'.

`SetAllowance`

| Field | Value |
| ----- | ----- |
| Topic | `1u8` |
| Value | `owner_address: PublicAddress \|\| spender_address: PublicAddress` |

Gets triggered on successful delegation of tokens through method 'set_allowance'. 

## Optional Methods 

**VIEW**

`fn name() -> String`

'name' returns the name of the token.

**VIEW**

`fn symbol() -> String`

'symbol' returns the symbol of the token.

**VIEW**

`fn decimals() -> u8`

'decimals' returns the number of decimals that should be divided from a token amount (such as that returned by method 'balance_of') to get its user representation. i.e., `n` means to divide the token amount by `10^n`.

## Implementation 

An example smart contract implementing the token standard is available.
See [here](https://github.com/parallelchain-io/example-smart-contracts).

## License

[GNU Public License 3](https://www.gnu.org/licenses/gpl-3.0.en.html).