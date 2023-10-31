# ParallelChain Wallet Extension

ParallelChain Wallet Extension is a browser extension that allows you to integrate your [dapp](./definition.md#provider) ( with [ParallelChain Wallet](../concepts/wallet.md).

This enables your dapp to interact with your dapp users' XPLL accounts, to:

- Send [transactions](../concepts/transaction.md#transaction)
  - [Stake](../concepts/staking/what_is_staking.md) XPLL
- Trigger confirmation for [smart contract](../concepts/smartcontract.md) calls 

Start by reading about the [ParallelChain Wallet Provider API](./provider/intro.md).

## Supported Platforms

- Chrome browser (Minimum version: Chrome 57)
- Firefox browser (Minimum version: Firefox 57.0)

## Getting Started

### Prerequisites

- Chrome or Firefox browser installed (Refer to above for supported versions)

### Extension Installation

#### Chrome

##### Chrome Web Store

Chrome users can install the extension via the Chrome Web Store.

##### Manual Installation

1. Download `parallelchain_wallet_extension-0.0.1-chromium.zip`.
2. Unzip the file into a folder.
3. Follow the [instructions here][1] to load the extension. Load the folder when prompted.

[1]: https://developer.chrome.com/docs/extensions/mv3/getstarted/development-basics/#load-unpacked

#### Firefox

##### Firefox Add-ons

Firefox users can install the extension via Firefox Add-ons.

##### Manual Installation

1. Download `parallelchain_wallet_extension-0.0.1-firefox.zip`.
2. Unzip the file into a folder.
3. Follow the [instructions here][2] to load the extension. Load `manifest.json` in the folder when prompted.

[2]: https://extensionworkshop.com/documentation/develop/temporary-installation-in-firefox/

## FAQ

### After installing the extension, I am still not able to access `window.xpll` in my browser or dapp.

Try refreshing the webpage and make sure the website is a valid http or https website. Restricted sites like `chrome://extensions/` or `about:debugging` will not be able to access the injected API.

### After clicking the extension icon, nothing happens.

If this is your first time setting up the wallet, a separate notification window will appear. If you are using multiple screens, check if the notification window has appeared on another screen.
