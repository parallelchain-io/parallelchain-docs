---
tags:
  - testnet 3
  - parallelchain sdk
  - smart contract
  - metadata
---

# Contract Metadata

Contracts live in the World State as WASM bytecode. Like most bytecode, these are not designed to be human-readable. This means that it's virtually impossible for potential users of your Contract to discover the callable interface (set of exported Methods) of your Contract by querying for your Contract's code in the World State. This is where Contract Metadata comes in:

```rust
#[contract_methods(meta)]
impl PrinceTheDog {
    ...
}
```

Passing the 'meta' option to the `#[contract_methods]` attribute causes code to be generated during expansion that enables users to get a String from the 'get metadata' route of the Standard HTTP API that describes the contract's callable interface.


Example of using `pchain_client` to query metadata by a given contract address.
```bash
pchain_client query account contract-metadata --address oiFs9jr2godPkNJh4IktbIUUUEiPwCtb0VRlLJGDAVE
```

Output:
```rust
pub trait PRFC2ImplementorActions {
  fn transfer(token_id :String, to_address :pchain_types::PublicAddress);
  fn transfer_from(from_address :pchain_types::PublicAddress, to_address :pchain_types::PublicAddress, token_id :String);
  fn set_spender(token_id :String, spender_address :pchain_types::PublicAddress);
  fn set_exclusive_spender(spender_address :pchain_types::PublicAddress);
}
pub trait PRFC2ImplementorViews {
  fn collection() -> Collection;
  fn tokens_owned(address :pchain_types::PublicAddress) -> u64;
  fn get_owner(token_id :String) -> pchain_types::PublicAddress;
  fn get_spender(token_id :String) -> pchain_types::PublicAddress;
  fn get_exclusive_spender(for_address :pchain_types::PublicAddress) -> pchain_types::PublicAddress;
}
```


