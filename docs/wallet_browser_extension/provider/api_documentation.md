---
tags:
  - wallet
  - browser extension
  - provider API
---

# API Documentation

## Properties

### isXPLL

This property is true if the user has extension installed.

```js
window.xpll.isXPLL; // => true
```

## Request

The request method is intended as a transport- and protocol-agnostic wrapper function for [Remote Procedure Calls (RPCs)][1]

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

- `method`: Method indicates which RPC method to call.
- `params`: Parameters is an optional array or object of arguments to pass with the RPC.

### Helper Functions

To improve the developer experience,
the provider also implemented a list of helper functions,
which helps the user simplify the RPC method call.

## Request Permission

Request [permission][4] to access the user's accounts.

!!! warning
    The request causes a popup to appear.
    You should only request permissions in response to a direct user action, such as a button click.

!!! failure
    If user reject the request, the `User Rejected Request 4001` error will be thrown.

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

Get all permissions granted to the wallet.

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
    Please check [Permission][4] for more detail.


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

get the currently active account's [address][3],
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

Returns the balance of the active account in the smallest unit (e.g. [Gray][5]) of the token.

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

## Get Active Acconut Latest Transactions

Returns the latest transactions of the active account.

### Params

- `limit`: The number of transactions to return. The default value is `1`.
- `order`: The order of transactions to return. The default value is `desc`.

!!! warning
    The `get_active_account_latest_transactions` should have permission granted; otherwise,
    the `User Rejected Request 4001` error will be thrown.
    Please check [Permission][4] for more detail.

```ts
interface GetActiveAcconutLatestTransactionsRequest {
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
type GetActiveAcconutLatestTransactionsResult = TransactionSummary[];
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
    window.xpll.getActiveAcconutLatestTransactions({
      limit: 1,
      order: "desc",
    });
    ```

## Get Current Network

Returns the current network.

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

Transfer tokens to another account, return the transaction hash if the request is successful.

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
    If user reject the request, the `User Rejected Request 4001` error will be thrown.

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

Transfer tokens to another account,
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
      method: "call_contract",
      params: {
        address: "5-2azL_uWn26A46pJ4OoY-Aqlov5w8tL4vqhbetX9gE",
        method: "transfer",
        args: [
          { type: "address", value: "FOg6_UOmsPRs4KOECp3G2UoSS1sUQQH8NSgNa_IkQ_8" },
          { type: "amount", value: "1" },
        ],
      },
    });
    ```

=== "Helper Function"
    ```js
    window.xpll.callContract({
      address: "5-2azL_uWn26A46pJ4OoY-Aqlov5w8tL4vqhbetX9gE",
      method: "transfer",
      args: [
        { type: "address", value: "FOg6_UOmsPRs4KOECp3G2UoSS1sUQQH8NSgNa_IkQ_8" },
        { type: "amount", value: "1" },
      ],
    });
    ```

[1]: ./definition.md#remote-procedure-call-rpc
[2]: ./error.md
[3]: ./definition.md#address
[4]: ./permission.md
[5]: /introduction/xpll/what_is_xpll/#denomination
