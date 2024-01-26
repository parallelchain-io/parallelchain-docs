---
tags:
  - wallet
  - browser extension
  - provider API
---

# Errors

The Provider will reject any RPC request with an error object if the RPC request cannot be fulfilled.
Every error object should have a `code` property that indicates the error type that occurred.

## Error Type

```ts
interface XPLLProviderRpcError extends Error {
  code: number;
  data?: unknown;
}
```

!!! tip
    `XPLLProviderRpcError` is inherited from the native [`Error`][error] object.
    Since `Error` is a serializable object, it can be cloned with [structuredClone()][structuredClone] or copied between [Workers][worker] using [postMessage()][postMessage].

## Provider Errors

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

> [`4900`] is intended to indicate that the Provider is disconnected *from all chains*,
> while [`4901`] is intended to indicate that the Provider is disconnected *from a specific chain only*.
> In other words, [`4901`] implies that the Provider is *connected to other chains, just not the requested one*.

[error]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error
[structuredClone]: https://developer.mozilla.org/en-US/docs/Web/API/structuredClone
[postMessage]: https://developer.mozilla.org/en-US/docs/Web/API/Worker/postMessage
[worker]: https://developer.mozilla.org/en-US/docs/Web/API/Worker
