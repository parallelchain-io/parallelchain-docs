---
tags:
  - wallet
  - browser extension
  - provider API
---

# Events

The Provider will be implemented per the Node.js [EventEmitter][eventemitter] API
with the following event handling methods:

- `on`
- `off`
- `addListener`
- `removeListener`

[eventemitter]: https://nodejs.org/api/events.html#class-eventemitter

## Example Usage

```js
function handleActiveAccountChanged(accountAddress) {
  // handle active account
}
// subscribe the activeAccountChanged event
window.xpll.on("activeAccountChanged", handleActiveAccountChanged);
// unsubscribe the activeAccountChanged event
window.xpll.off("activeAccountChanged", handleActiveAccountChanged);
```

## Event Message Type

When emitted, the event will be emitted with an object argument of the following form:

```ts
interface ProviderMessage {
  readonly type: string;
  readonly data: unknown;
}
```

## Network Changed

If the network that the Provider is connected to is changed,
   the Provider will emit the event named `networkChanged` with value `network_id: string`.

```ts
interface NetworkChangedEvent {
  readonly type: "networkChanged";
  readonly data: string;
}
```

## Active Account Changed

If the current active account is changed,
the Provider will emit the event named `activeAccountChanged` with value `account_address: string`.

```ts
interface ActiveAccountChangedEvent {
  readonly type: "activeAccountChanged";
  readonly data: string;
}
```

!!! warning
    The `activeAccountChanged` event will only fire after permission has been granted.
    Please check [Permissions][permission] for more details.

[permission]: ./permission.md
