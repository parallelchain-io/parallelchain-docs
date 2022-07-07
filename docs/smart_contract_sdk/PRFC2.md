# ParallelChain F Request for Comments 2 (PRFC 2)

| PRFC | Title | Author | Version | Date First Published |
| --- | ----- | ---- | --- | --- |
| 2   | Non-Fungible Token Standard | ParallelChain Lab | 1 | July 7th, 2022 | 

## Summary 
  
ParallelChain F Request for Comments 2 defines a standard interface for non-fungible tokens implemented as ParallelChain F smart contracts. "Non-Fungible Tokens" or NFTs is taken here to have the same meaning as in Ethereum's ERC-721, namely a set of transferable entities on a blockchain with identification metadata unique to its creator. For example, a deed for a plot of land is a non-fungible token, since a deed identifies a singular, unique plot of land (i.e., no two deeds identifies the same plot of land).

A standard contract interface for non-fungible tokens allows more seamless interoperability, since applications can make the simplifying assumption that all PRFC 2-implementing contracts always export the same, named set of Methods (they may export more).

The below section lists the set of methods that all smart contracts that want to be PRFC 2-compliant must implement, as well as the behavior that each defined method must exhibit. Required behavior involves emitting certain events. These are also listed and described.


## Glossary

**Spender**: An account that approved (using method `set_spender` or `set_exclusive_spender`) to transfer tokens on behalf of an owner. Any token can have at most one Spender.

**Exclusive Spender**: A spender that is approved to transfer *all* tokens owned by an owner. An account can be made an Exclusive Spender either through a single call to `set_exclusive_spender` (preferred), or multiple calls to `set_spender`.  

## Required Methods  

Note: the following uses syntax from Rust (version 1.59.0).

**VIEW**

`fn tokens_owned(address: protocol_types::PublicAddress) -> u64`

'tokens_owned' returns the number of tokens owned by an account.

**VIEW** 

`fn get_owner(token_id: String) -> Option<protocol_types::PublicAddress>`

'get_owner' returns the address of the account that owns the token identified by `token_id`, or `None` if the token is not owned by anybody.

'get_owner' must panic if 'token_id' does not identify a token.

**VIEW** 
  
`fn get_spender(token_id: String) -> Option<protocol_types::PublicAddress>` 

'get_spender' returns the address of the account that is the spender (exclusive, or not) of the token identified by `token_id`, or `None` if no account is the spender of the token. 

'get_spender' must panic if 'token_id' does not identify a token.

**VIEW**

`fn get_exclusive_spender(for_address: protocol_types::PublicAddress) -> Option<protocol_types::PublicAddress>`

'get_exclusive_spender' returns the address of the account that is the exclusive spender for all tokens owned by `for_address`, or `None` if no account is the exclusive spender for the account.

**ACTION**

`fn transfer(token_id: String, to_address: protocol_types::PublicAddress)`

'transfer' transfers the token identified by `token_id` from the account identified by `txn.from_address` (its owner), to the account identified by `to_address`.

'transfer' must panic if get_owner(token_id) != `txn.from_address`.


Event `Transfer` must trigger if 'transfer' is successful.

**ACTION** 

`fn transfer_from(from_address: protocol_types::PublicAddress, to_address: protocol_types::PublicAddress, token_id: String)`

'transfer_from' transfers the token identified by `token_id` from the account identified by `from_address`, to the account identified by `to_address`, on behalf of the account identified by get_owner(token_id).

'transfer_from' must panic if: 
1. get_spender(token_id) != `txn.from_address`.
2. get_owner(token_id) != Some(`from_address`).
3. Or, if evaluating 1. or 2. causes a panic.

Event `Transfer` must trigger if ‘transfer_from’ is successful. 

**ACTION** 

`fn set_spender(token_id: String, spender_address: protocol_types::PublicAddress, approved: bool)`

If approved, 'set_spender' gives the account identified by `spender_address` the right to transfer the token identified by `token_id` on behalf of its owner, `txn.from_address`. If !approved, set_spender revokes the right of `get_spender(token_id)` to transfer the token. In the latter case, `spender_address` need not be equals to `get_spender(token_id)` in order for the call to succeed.

If get_spender(token_id) == `spender_address`, this Method is a no-op.

'set_spender' must panic if:
1. get_owner(token_id) != `txn.from_address`.
2. get_exclusive_spender(`txn.from_address`) is Some and != `spender_address`.
3. Or, if evaluating 1. causes a panic.

Event `SetSpender` must trigger if 'set_spender' is successful.

**ACTION** 

`fn set_exclusive_spender(spender_address: protocol_types::PublicAddress, approved: bool)`

'set_exclusive_spender' gives the account identified by `spender_address` the right to transfer *all* tokens owned by `txn.from_address`. Calling this method must have the same side effects (except events emitted) as calling `set_spender` for every `token_id` owned by `txn.from_address`, with the same `spender_address` and `approved`.

Event `SetExclusiveSpender` must trigger if 'set_exclusive_spender' is successful.
     
## Required Events

`Transfer`

| Field | Value |
| ----- | ----- |
| Topic | `0u8` |
| Value | `owner_address: PublicAddress \|\| recipient_address: PublicAddress \|\| token_id: String` |

Gets trigerred on successful call to methods 'transfer', or 'transfer_from'.

`SetSpender`

| Field | Value |
| ----- | ----- |
| Topic | `1u8` |
| Value | `spender_address: PublicAddress \|\|  token_id: String \|\| appproved: bool (0u8/1u8)` |

Gets triggered on successful call to method 'set_spender'.

`SetExclusiveSpender`

| Field | Value |
| ----- | ----- |
| Topic | `2u8` |
| Value | `owner_address: PublicAddress \|\| spender_address: PublicAddress \|\| approved: bool (0u8/1u8)` |

Gets triggered on successful call to method 'set_exclusive_spender'. 

## Optional Methods 

**VIEW** 

`fn name() -> String`
  
'name' returns the name of the token.

**VIEW** 

`fn symbol() -> String`

'symbol' returns the symbol of the token.

**ACTION**

`fn identify_token(token_id: String) -> bool`

'identify_token' returns a boolean depicting whether the token is identifiable on ParallelChain F.




## Implementation 

An example smart contract portraying the token standard has been shown in prfc2/.
See [here](https://github.com/parallelchain-io/example-smart-contracts)

## Copyright
  
[GNU Public License 3](https://www.gnu.org/licenses/gpl-3.0.en.html).
