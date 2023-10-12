# ParallelChain Wallet Extension

ParallelChain Wallet Extension is an extension for managing your XPLL and integrating your DApp.
It allows your DApp to interact with your DApp users' XPLL accounts and make smart contract calls through the extension.

# Features

- Browser Extension
  - Managing XPLL Wallet (Like in [Explorer Wallet](https://explorer.parallelchain.io/))
    - Sending Transactions
    - Staking
  - Triggering Confirmation for smart contract or send token calls
- Inject Global Javascript API into browser for DApps to interact with user wallets
  - Properties
  - Utility methods
  - Events

# Supported Platforms

- Chrome Browser (Minimum version: Chrome 57)
- Firefox Browser (Minimum version: Firefox 57.0)

# Getting Started

## Prerequisites

- Chrome or Firefox browser installed (Refer to above for supported versions)

## Extension Installation

### Chrome

1. Chrome users can only install an approved extension via Chrome Web Store. For the time being, download the `extension-dist-chrome-1.0.zip` file.
2. Unzip it into a folder.
3. Follow the instructions here to load your extension and load the folder when prompted.
   https://developer.chrome.com/docs/extensions/mv3/getstarted/development-basics/#load-unpacked

### Firefox

1. Firefox users can only install an extension that has been verified and "signed" by Mozilla. For the time being, download the `extension-dist-firefox-1.0.zip` file.
2. Unzip it into a folder.
3. Follow the instructions here to load your extension and load the `manifest.json` in the folder when prompted.
   https://extensionworkshop.com/documentation/develop/temporary-installation-in-firefox/

## FAQ

1. After installing the extension, I am still not able to access `window.xpll` in my browser or DApp.

Try refreshing the webpage and make sure the website is a valid http or https website. Restricted sites like `chrome://extensions/` or `about:debugging` would not be able to access the injected API.

2. After pressing the extension icon, nothing happens.

If this is your first time setting up the wallet, a separate notification window will appear. The notification window may appear on other screens if you are using multiple screens.