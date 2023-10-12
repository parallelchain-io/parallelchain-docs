---
tags:
  - wallet
  - browser extension
  - provider API
---

# ParallelChain Wallet Provider API

A Javascript Wallet Provider API for consistency across clients and applications.

## Introduction

A common convention in the [“dapp”][2] ecosystem is for key management software [“wallets”][3]
to expose their API via a JavaScript object in the web page.
This object is called “[Provider](#provider)”.

Historically, Provider implementations have exhibited conflicting interfaces and behaviors between wallets.
The [EIP-1193][1] formalizes the Provider API to promote wallet interoperability.
The API is designed to be minimal, event-driven, and agnostic of transport and RPC protocols.
Its functionality is easily extended by defining new RPC methods and message event types.

The ParallelChain Provider implementation based on [EIP-1193][1]
with additional help to provide a friendly developer experience.

After the ParallelChain wallet extension has been installed,
provider has been made available as `#!js window.xpll` in web browsers.

## Definition

#### Provider

A JavaScript object made available to a consumer, that provides access to wallet by means of a Client.

#### Client

An endpoint that receives Remote Procedure Call (RPC) requests from the Provider, and returns their results.

#### Wallet

An end-user application that manages private keys, performs signing operations,
and acts as a middleware between the Provider and the Client.

#### Remote Procedure Call (RPC)

A Remote Procedure Call (RPC), is any request submitted to a Provider for some procedure
that is to be processed by a Provider, its Wallet, or its Client.

#### Address

An address is a base64 encoded string representation of a public key.

## Properties

### isXPLL

This property is true if the user has extension installed.

```js
window.xpll.isXPLL; // => true
```

## Request

The request method is intended as a transport- and protocol-agnostic wrapper function for [Remote Procedure Calls (RPCs)](#remote-procedure-call-rpc).

If resolved, the Promise will resolve with a result per the RPC method’s specification.
If the returned Promise rejects,
it will reject with a `XPLLProviderRpcError` as specified in the [Errors](#errors) section below.

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
  Each method name correspond to a method specified in the [API Documentation](#api-documentation) section.
- `params`: Parameters is an optional array or object of arguments to pass with the RPC.

### Helper Functions

To improve the developer experience,
the provider also implemented a list of helper functions,
which helps the user simplify the RPC method call.

### API Documentation

#### Get Accounts

Get the list of [addresses](#address) for the accounts owned by the user.

!!! warning
    The `get_accounts` should have permission granted; otherwise, the `User Rejected Request 4001` error will be thrown.
    Please check [Permission](#permission) for more detail.

```ts
interface GetAccountsRequest {
  method: "get_accounts";
}

type GetAccountsResult = string[];
```

##### Example of usage

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

#### Get Active Account

get the currently active account's [address](#address),
return `undefined` if the user still needs to register.

!!! warning
    The `get_active_account` should have permission granted; otherwise, the `User Rejected Request 4001` error will be thrown.
    Please check [Permission](#permission) for more detail.

```ts
interface GetActiveAccountRequest {
  method: "get_active_account";
}
type GetActiveAccountResult = string | undefined;
```

##### Example of usage

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

#### Get Active Account Balance

Returns the balance of the active account in the smallest unit (e.g. [Gray][gray]) of the token.

!!! warning
    The `get_active_account_balance` should have permission granted; otherwise,
    the `User Rejected Request 4001` error will be thrown.
    Please check [Permission](#permission) for more detail.

```ts
interface GetActiveAccountBalanceRequest {
  method: "get_active_account_balance";
}
type GetActiveAccountBalanceResult = string;
```

##### Example of usage

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

#### Get Active Acconut Latest Transactions

Returns the latest transactions of the active account.

##### Params

- `limit`: The number of transactions to return. The default value is `1`.
- `order`: The order of transactions to return. The default value is `desc`.

!!! warning
    The `get_active_account_latest_transactions` should have permission granted; otherwise,
    the `User Rejected Request 4001` error will be thrown.
    Please check [Permission](#permission) for more detail.

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

##### Example of usage

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

#### Get Current Network

Returns the current network.

```ts
interface GetCurrentNetworkRequest {
  method: "get_current_network";
}
type GetCurrentNetworkResult = string | undefined;
```

##### Example of usage

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

#### Request Permission

Request [permission](#permission) to access the user's accounts.

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

##### Example of usage

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

#### Get Permissions

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

##### Example of usage

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

#### Send Token

Transfer tokens to another account, return the transaction hash if the request is successful.

##### Params

- `amount`: The amount of tokens to transfer in the smallest unit (e.g. [Gray][gray]) of the token.
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

##### Example of usage

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

#### Call Contract

Transfer tokens to another account,
return the transaction hash if the request is successful.

##### Params

- `method`: The method of the contract.
- `address`: The [address](#address) of the contract.
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

##### Example of usage

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

## Errors

The Provider will reject any RPC request with an error object if the RPC request cannot be fulfilled.
Every error object should have a `code` property that indicates the error type occurred.


### Error Type

```ts
interface XPLLProviderRpcError extends Error {
  code: number;
  data?: unknown;
}
```

!!! tip
    The `XPLLProviderRpcError` is inherited from native [`Error`][error] object.
    Since Error is a serializable object, 
    so it can be cloned with [structuredClone()][structuredClone] or copied between [Workers][worker] using [postMessage()][postMessage].

[error]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error
[structuredClone]: https://developer.mozilla.org/en-US/docs/Web/API/structuredClone
[postMessage]: https://developer.mozilla.org/en-US/docs/Web/API/Worker/postMessage
[worker]: https://developer.mozilla.org/en-US/docs/Web/API/Worker

### Provider Errors

| Status code |              Name              |                               Description                                |
| :---------: | :----------------------------: | :----------------------------------------------------------------------: |
|    4001     |     User Rejected Request      |                      The user rejected the request.                      |
|    4100     |          Unauthorized          | The requested method and/or account has not been authorized by the user. |
|    4200     |       Unsupported Method       |           The Provider does not support the requested method.            |
|    4900     |          Disconnected          |              The Provider is disconnected from all chains.               |
|    4901     |       Chain Disconnected       |          The Provider is not connected to the requested chain.           |
|   -32700    |          Parse error           |                               Invalid JSON                               |
|   -32600    |        Invalid request         |                    JSON is not a valid request object                    |
|   -32601    |        Method not found        |                          Method does not exist                           |
|   -32602    |         Invalid params         |                        Invalid method parameters                         |
|   -32603    |         Internal error         |                         Internal JSON-RPC error                          |
|   -32000    |         Invalid input          |                      Missing or invalid parameters                       |
|   -32001    |       Resource not found       |                       Requested resource not found                       |
|   -32002    |      Resource unavailable      |                     Requested resource not available                     |
|   -32003    |      Transaction rejected      |                       Transaction creation failed                        |
|   -32004    |      Method not supported      |                        Method is not implemented                         |
|   -32005    |         Limit exceeded         |                      Request exceeds defined limit                       |
|   -32006    | JSON-RPC version not supported |              Version of JSON-RPC protocol is not supported               |

> [`4900`] is intended to indicate that the Provider is disconnected from all chains,
> while [`4901`] is intended to indicate that the Provider is disconnected from a specific chain only.
> In other words, [`4901`] implies that the Provider is connected to other chains, just not the requested one.

## Events

The provider be implemented per the Node.js [EventEmitter][eventemitter] API
with the following event handling methods:

- `on`
- `off`
- `addListener`
- `removeListener`

[eventemitter]: https://nodejs.org/api/events.html#class-eventemitter

### Example Usage

```js
function handleActiveAccountChanged(accountAddress) {
  // handle active account
}
// subscript the activeAccountChanged event
window.xpll.on("activeAccountChanged", handleActiveAccountChanged);
// unsubscript the activeAccountChanged event
window.xpll.off("activeAccountChanged", handleActiveAccountChanged);
```

### Event Message Type

When emitted, the event will be emitted with an object argument of the following form:

```ts
interface ProviderMessage {
  readonly type: string;
  readonly data: unknown;
}
```

#### Network Changed

If the network which the Provider is connected was changed,
   the Provider will emit the event named `networkChanged` with value `network_id: string`.

```ts
interface NetworkChangedEvent {
  readonly type: "networkChanged";
  readonly data: string;
}
```

#### Active Account Changed

If the current active account was changed,
the Provider will emit the event named `activeAccountChanged` with value `account_address: string`.

```ts
interface ActiveAccountChangedEvent {
  readonly type: "activeAccountChanged";
  readonly data: string;
}
```

!!! warning
    The `activeAccountChanged` event only fired after permission has been granted.  
    Please check [Permission](#permission) for more detail.

## Permission

Wallets are responsible for mediating interactions between untrusted applications
and users’ keys through appropriate user consent.

### Permission Type

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

#### Properties

- The `invoker` is a URI used to identify the source of the current dapp (e.g. `https://your-site.com/`).
- The term `parentCapability` refers to the method that is being permitted (e.g. `xpll_accounts`).
- The caveats array represents the specific restrictions applied to the permitted method.
- The type of a `Caveat` is a string, and the `value` is an arbitrary JSON value.
- The value of a `Caveat` is only meaningful in the context of the type of the `Caveat`.


[1]: https://eips.ethereum.org/EIPS/eip-1193
[2]: https://en.wikipedia.org/wiki/Decentralized_application
[3]: https://en.wikipedia.org/wiki/Cryptocurrency_wallet
[gray]: /introduction/xpll/what_is_xpll/#denomination
