---
tags:
  - wallet
  - browser extension
  - provider API
---

# Permissions

Wallets mediate interactions between untrusted applications and users’ keys by requesting and getting user permissions.

## Permission Type

```ts
interface Caveat {
  type: string;
  value: unknown;
}

interface Permission {
  invoker: string;
  parentCapability: string;
  caveats: Caveat[];
}
```

## Properties

- The `invoker` is a URI used to identify the source of the current dapp (e.g. `https://your-site.com/`).
- The term `parentCapability` refers to the method that is being permitted (e.g. `xpll_accounts`).
- The caveats array represents the specific restrictions applied to the permitted method.
- The type of a `Caveat` is a string, and the `value` is an arbitrary JSON value.
- The value of a `Caveat` is only meaningful in the context of the type of the `Caveat`.

## Request Permission

Request permission to access the user's accounts.

Check out the API documentation [Request Permissions](./api_documentation.md#request-permission) for details.

## Get Permissions

Return all permissions that have been granted to the wallet.

Check out the API documentation for [Get Permissions](./api_documentation.md#get-permissions) for details.
