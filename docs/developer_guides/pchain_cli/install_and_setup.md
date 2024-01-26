---
tags:
  - Tools
  - CLI
  - pchain-cli-rust
---

# Install and Setup

## Installation
---

`pchain_client` is an available tool for users on Unix/Linux, MacOS, and Windows operating systems. Simply download the pre-built binary corresponding to your platform and install the `pchain_client`.

Here are the simple steps to install `pchain_client`:

- Open a web browser and go to [release page](https://github.com/parallelchain-io/pchain-client-cli/releases).
- Follow the link to download pre-built binary available for your platform.
- Run the downloaded file.

**NOTE:**
If this is your first time using `pchain_client`, you need to setup `$PCHAIN_CLI_HOME` in environment variables to specify the home path. See more [here](https://chlee.co/how-to-setup-environment-variables-for-windows-mac-and-linux/).

## Running pchain_client
---

Upon first use of `pchain_client`, you will be prompted to set up a password to protect your account keypairs. Please note that this password can be different from the password you used in ParallelChain Explorer. Alternatively, you can skip the password protection by simply pressing Enter.

=== "Linux / macOS"
    ```bash
    ./pchain_client --version
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe --version
    ```
You will be required to enter your password twice. If your password is set successfully, you will see a return message with `pchain_client` version shown on console.

**WARNING:**
The password is not sent and saved anywhere. You won't be able to recover the password if you lose it. Please keep your password safe. You will be required to provide this password to submit transactions and manage keypairs later.


## Prepare Environment
---

Before you can submit transactions or query information on ParallelChain, you need to setup your own choice of ParallelChain RPC API provider URL.

=== "Linux / macOS"
    ```bash
    ./pchain_client config setup --url <URL>
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe config setup --url <URL>
    ```

This would check the status of your chosen provider. If `pchain_client` cannot connect to your provider, a warning message will be shown, and setup fails. You need to set up another url with the above command again.