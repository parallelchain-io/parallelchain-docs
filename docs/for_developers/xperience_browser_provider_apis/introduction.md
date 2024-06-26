---
tags:
  - wallet
  - browser extension
  - provider API
---

# Introduction to Xperience Browser Provider APIs

The Xperience Browser Provider API is a Javascript API designed to deliver a friendly developer experience and consistency across clients and applications. 

Xperience Browser Extension injects a global Javascript API into the browser using the `#!js window.xpll` [provider][1] object. 

This API is specified by [EIP-1193][2], which promotes wallet interoperability by preventing conflicting interfaces and behaviours between wallets. 

Xperience Browser Provider API is designed to be minimal, event-driven, and agnostic of transport and [RPC][3] protocols. Its functionality is easily extended by defining new RPC methods and message event types.

## Installing Xperience Browser Extension

The extension currently only works on Chrome. Chrome user can install the extension via the [Chrome Web Store](https://chromewebstore.google.com/detail/xperience-browser-extensi/gpfllmjckejjhmmdmgbgmclmhopekjpf). 

If necessary, refer to [these instructions](https://support.google.com/chrome_webstore/answer/2664769?hl=en) on installing Chrome extensions.

## FAQ

### After installing the extension, I am still not able to access `window.xpll` in my browser or dapp.

Try refreshing the webpage and make sure the website is a valid http or https website. Restricted sites like `chrome://extensions/` or `about:debugging` will not be able to access the injected API.

### After clicking the extension icon, nothing happens.

If this is your first time setting up the wallet, a separate notification window will appear. If you are using multiple screens, check if the notification window has appeared on another screen.




[1]: definitions.md#provider
[2]: https://eips.ethereum.org/EIPS/eip-1193
[3]: ../xperience_browser_provider_apis/definitions.md#remote-procedure-call-rpc
[4]: https://developer.chrome.com/docs/extensions/mv3/getstarted/development-basics/#load-unpacked