---
tags:
  - wallet
  - browser extension
  - provider API
---

# Permission

Wallets are responsible for mediating interactions between untrusted applications
and users’ keys through appropriate user consent.

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
