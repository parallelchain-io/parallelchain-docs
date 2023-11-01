---
tags:
  - wallet
  - browser extension
  - provider API
---

# API Documentation

## Properties

### isXPLL

This property is true if the user has the extension installed.

```js
window.xpll.isXPLL; // => true
```

## Request

The request method is intended as a transport- and protocol-agnostic wrapper function for [Remote Procedure Calls (RPCs)][1].

If resolved, the Promise will resolve with a result per the RPC method’s specification.
If the returned Promise rejects,
it will reject with a `XPLLProviderRpcError` as specified in the [Errors][2] section below.

```ts
interface RequestArguments {
  readonly method: string;
  readonly params?: readonly unknown[] | object;
}
```

```ts
request(data: RequestArguments): Promise<Result>;
// or reject with `XPLLProviderRpcError`
```

### Parameters

- `method`: Indicates which RPC method to call
- `params`: An optional array or object of arguments to pass with the RPC

### Helper Functions

To improve the developer experience,
the Provider also implements a list of helper functions,
which helps you simplify the RPC method call.

## Request Permission

Request [permission][4] to access the user's accounts.

!!! warning
    The request causes a popup to appear.
    You should only request permission in response to a direct user action, such as a button click.

!!! failure
    If the user rejects the request, the `User Rejected Request 4001` error will be thrown.

```ts
interface Permission {
  id: string;
  invoker: string;
  parent_capability: string;
  caveats: {
    type: string;
    value?: unknown;
  }[];
  created_at: number;
}
interface RequestPermissionRequest {
  method: "request_permissions";
}
type RequestedPermissionResult = Permission;
```

=== "RPC"
    ```js
    window.xpll.request({
      method: "request_permissions";
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.requestPermissions();
    ```

## Get Permissions

Return all permissions that have been granted to the wallet.

```ts
interface Permission {
  id: string;
  invoker: string;
  parent_capability: string;
  caveats: {
    type: string;
    value?: unknown;
  }[];
  created_at: number;
}
interface GetPermissionsRequest {
  method: "get_permissions";
}
type GetPermissionsResult = Permission[];
```

=== "RPC"
    ```js
    window.xpll.request({
      method: "get_permissions";
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.getPermissions();
    ```

## Get Accounts

Get the list of [addresses][3] for the accounts owned by the user.

!!! warning
    The `get_accounts` should have permission granted; otherwise, the `User Rejected Request 4001` error will be thrown.
    Please check [Permissions][4] for more detail.


```ts
interface GetAccountsRequest {
  method: "get_accounts";
}

type GetAccountsResult = string[];
```

=== "RPC"
    ```js
    window.xpll.request({
      method: "get_accounts",
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.getAccounts();
    ```

## Get Active Account

Get the [address][3] of the currently active account;
return `undefined` if the user still needs to register.

!!! warning
    The `get_active_account` should have permission granted; otherwise, the `User Rejected Request 4001` error will be thrown.
    Please check [Permission][4] for more detail.

```ts
interface GetActiveAccountRequest {
  method: "get_active_account";
}
type GetActiveAccountResult = string | undefined;
```

=== "RPC"
    ```js
    window.xpll.request({
      method: "get_active_account";
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.getActiveAccount();
    ```

## Get Active Account Balance

Return the balance of the active account in the smallest unit (e.g. [Gray][5]) of the token.

!!! warning
    The `get_active_account_balance` should have permission granted; otherwise,
    the `User Rejected Request 4001` error will be thrown.
    Please check [Permission][4] for more detail.

```ts
interface GetActiveAccountBalanceRequest {
  method: "get_active_account_balance";
}
type GetActiveAccountBalanceResult = string;
```

=== "RPC"
    ```js
    window.xpll.request({
      method: "get_active_account_balance";
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.getActiveAccountBalance();
    ```

## Get Active Account Latest Transactions

Return the latest transactions of the active account.

### Params

- `limit`: The number of transactions to return. The default value is `1`.
- `order`: The order of transactions to return. The default value is `desc`.

!!! warning
    The `get_active_account_latest_transactions` should have permission granted; otherwise,
    the `User Rejected Request 4001` error will be thrown.
    Please check [Permission][4] for more detail.

```ts
interface GetActiveAccountLatestTransactionsRequest {
  method: "get_active_account_latest_transactions";
  params?: {
    limit?: number;
    order?: "asc" | "desc";
  };
}

interface TransactionSummary {
  amount: string;
  block_hash: string;
  commands: string[];
  command: string;
  exit_status?: string;
  gas_consumed: string;
  gas_limit: string;
  hash: string;
  nonce: string;
  number: string;
  signer: string;
  status: number;
  timestamp: number;
  tx_gas_consumption: string;
}
type GetActiveAccountLatestTransactionsResult = TransactionSummary[];
```

=== "RPC"
    ```js
    window.xpll.request({
      method: "get_active_account_latest_transactions",
      params: {
        limit: 1,
        order: "desc",
      },
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.getActiveAccountLatestTransactions({
      limit: 1,
      order: "desc",
    });
    ```

## Get Current Network

Return the current network.

```ts
interface GetCurrentNetworkRequest {
  method: "get_current_network";
}
type GetCurrentNetworkResult = string | undefined;
```

=== "RPC"
    ```js
    window.xpll.request({
      method: "get_current_network";
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.getCurrentNetwork();
    ```

## Send Token

Transfer tokens to another account; return the transaction hash if the request is successful.

### Params

- `amount`: The amount of tokens to transfer in the smallest unit (e.g. [Gray][5]) of the token.
- `recipient`: The address of the recipient account.
- `nonce`: The nonce of the transaction.
  If not provided, the nonce will be calculated automatically.
- `gas_limit`: The gas limit of the transaction.
  If not provided, the gas limit will be calculated automatically.
- `max_base_fee_per_gas`: The maximum base fee per gas of the transaction. The default value is `8`.
- `priority_fee_per_gas`: The priority fee per gas of the transaction. The default value is `0`.

!!! warning
    The request causes a popup to appear.
    You should only request permissions in response to a direct user action, such as a button click.

!!! failure
    If user rejects the request, the `User Rejected Request 4001` error will be thrown.

```ts
interface SendTokenRequest {
  method: "send_token";
  params: {
    amount: string;
    recipient: string;
    nonce?: string;
    gas_limit?: string;
    max_base_fee_per_gas?: string;
    priority_fee_per_gas?: string;
  };
}
export type SendTokenResult = string; // transaction hash
```

=== "RPC"
    ```js
    window.xpll.request({
      method: "send_token",
      params: {
        recipient: "FOg6_UOmsPRs4KOECp3G2UoSS1sUQQH8NSgNa_IkQ_8",
        amount: "1",
        gas_limit: "166350",
        nonce: "17",
        max_base_fee_per_gas: "8",
        priority_fee_per_gas: "0",
      },
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.sendToken({
      recipient: "FOg6_UOmsPRs4KOECp3G2UoSS1sUQQH8NSgNa_IkQ_8",
      amount: "1",
    });
    ```

## Call Contract

Call the method except the view method of the contract;
return the transaction hash if the request is successful.

### Params

- `method`: The method of the contract.
- `address`: The [address][3] of the contract.
- `args`: The arguments of the contract's method.
- `nonce`: The nonce of the transaction.
  If not provided, the nonce will be calculated automatically.
- `gas_limit`: The gas limit of the transaction.
  If not provided, the gas limit will be calculated automatically.
- `max_base_fee_per_gas`: The maximum base fee per gas of the transaction. The default value is `8`.
- `priority_fee_per_gas`: The priority fee per gas of the transaction. The default value is `0`.

!!! warning
    The request causes a popup to appear.
    You should only request permissions in response to a direct user action, such as a button click.

### Arguments Serialization

The arguments of the contract's method should be serialized as [Uint8Array][6]
before passing to the `call_contract` method.

The primitive types are serialized based on [Borsh binary serialization format][7]. You can use the library [borsh-js][10] to help you serialize the arguments.

The address is base64 encoded string. 
For more details about base64 serialization, please check [MDN][9].

The option type is also based on [Borsh binary serialization format][7]. 
For instance, `Option<PublicAddress>` is serialized as:

```js
const address = 'ggYKJ-RGIfs2YVzAW2W5KV39NpPSUIzabo7m0dSC_Hs'; 
const addressBytes = new TextEncoder().encode(address); // PublicAddress
const optionBytes = new Uint8Array([1, ...addressBytes]); // Option<PublicAddress>
```

The following example shows how to serialize the arguments:

Assume that you want to call the [transfer_from][8] method 
of the `mTcm1Gi520O02C_fuCamRTWVdi2FBBudePWEA-55bT8` PRFC1 contract.

```js
import * as borsh from 'borsh';

// PublicAddress
const fromAddress = 'YChzIE0ZGwKuuJSSVugZ-SlY3RvBzzjjz3__VkftgCY';
// Option<PublicAddress>
const toAddress = 'ggYKJ-RGIfs2YVzAW2W5KV39NpPSUIzabo7m0dSC_Hs'; 
// u64
const value = '0.001'; 

// helper functions for serialization
const base64ToBytes = (text) => new TextEncoder().encode(text);
const option = (bytes) => new Uint8Array([1, ...bytes]);

window.xpll.request({
    method: 'call_contract',
    params: {
        address: 'mTcm1Gi520O02C_fuCamRTWVdi2FBBudePWEA-55bT8',
        method: 'transfer_from',
        args: [
            base64ToBytes(fromAddress),
            option(base64ToBytes(toAddress)),
            borsh.serialize('u64', value),
        ],
    }
});
```

Sometimes, it's hard to serialize the arguments by yourself. So, the Provider also provides a handy way to help you serialize the arguments:
by passing the arguments as an array of objects, as seen below.

```ts
interface UintArgument {
    type: 'u8' | 'u16' | 'u32' | 'u64' | 'u128';
    value: string | number;
}

interface IntArgument {
    type: 'i8' | 'i16' | 'i32' | 'i64' | 'i128';
    value: string | number;
}

interface FloatArgument {
    type: 'f32' | 'f64';
    value: string | number;
}

interface BoolArgument {
    type: 'bool';
    value: boolean;
}

interface StringArgument {
    type: 'string';
    value: string;
}
interface Base64Argument {
    type: 'base64';
    value: string;
}
```

The following example shows how to serialize the arguments:

```js
window.xpll.request({
    method: 'call_contract',
    params: {
        address: 'mTcm1Gi520O02C_fuCamRTWVdi2FBBudePWEA-55bT8',
        method: 'transfer_from',
        args: [
            { type: 'base64', value: 'YChzIE0ZGwKuuJSSVugZ-SlY3RvBzzjjz3__VkftgCY' },
            { type: 'base64', value: 'ggYKJ-RGIfs2YVzAW2W5KV39NpPSUIzabo7m0dSC_Hs' },
            { type: 'u64', value: '0.001' }
        ],
    }
});
```

```ts
interface CallContractRequest {
  method: "call_contract";
  params: {
    method: string;
    address: string;
    args: (
      | {
          value: string;
          type: "address";
        }
      | {
          value: string | number;
          type: "amount";
        }
    )[];
    gas_limit?: string;
    nonce?: string;
    max_base_fee_per_gas?: string;
    priority_fee_per_gas?: string;
  };
}
export type CallContractResult = string; // transaction hash
```

=== "RPC"
    ```js
    window.xpll.request({
        method: 'call_contract',
        params: {
            address: 'mTcm1Gi520O02C_fuCamRTWVdi2FBBudePWEA-55bT8',
            method: 'transfer_from',
            args: [
                { type: 'base64', value: 'YChzIE0ZGwKuuJSSVugZ-SlY3RvBzzjjz3__VkftgCY' },
                { type: 'base64', value: 'ggYKJ-RGIfs2YVzAW2W5KV39NpPSUIzabo7m0dSC_Hs' },
                { type: 'u64', value: '0.001' }
            ],
        }
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.callContract({
        address: 'mTcm1Gi520O02C_fuCamRTWVdi2FBBudePWEA-55bT8',
        method: "transfer_from",
        args: [
            { type: 'base64', value: 'YChzIE0ZGwKuuJSSVugZ-SlY3RvBzzjjz3__VkftgCY' },
            { type: 'base64', value: 'ggYKJ-RGIfs2YVzAW2W5KV39NpPSUIzabo7m0dSC_Hs' },
            { type: 'u64', value: '0.001' }
        ],
    });
    ```

## Watch Asset

Allows developers to request a specified asset be tracked in the wallet.

### Params

- `type`: The type string is the commonly accepted name of the interface implemented by the asset’s contract, e.g. PRFC1.
- `address`: The [address][3] of the contract.

!!! warning
    The request causes a popup to appear.
    You should only request permissions in response to a direct user action, such as a button click.

!!! failure
    If the user rejects the request, the `User Rejected Request 4001` error will be thrown.


```ts
interface WatchAssetRequest {
    method: "watch_asset";
    params: {
        type: "PRFC1";
        address: string;
    };
}
type WatchAssetResult = boolean;
```

=== "RPC"
    ```js
    window.xpll.request({
        method: 'watch_asset',
        params: {
            type: 'PRFC1',
            address: 'xXAbY4DmeOHHuRCmE9dXczV38BoO-CTKYeWra8YOlLw'
        }
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.watchAsset({
        type: 'PRFC1',
        address: 'xXAbY4DmeOHHuRCmE9dXczV38BoO-CTKYeWra8YOlLw'
    });
    ```

[1]: ../definition.md#remote-procedure-call-rpc
[2]: ./error.md
[3]: ../definition.md#address
[4]: ./permission.md
[5]: ../../../introduction/xpll/what_is_xpll.md#denomination
[6]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array
[7]: https://borsh.io/
[8]: https://github.com/parallelchain-io/prfcs/blob/master/PRFCS/prfc-1.md#transfer_from
[9]: https://developer.mozilla.org/en-US/docs/Glossary/Base64
[10]: https://github.com/near/borsh-js

