---
tags:
  - Tools
  - CLI
  - pchain-cli-rust
---

# Install and Setup

## Installation
---

The `pchain_client` tool is accessible to users operating on Unix/Linux, MacOS, and Windows systems. To install `pchain_client`, simply download the pre-built binary that corresponds to your specific platform and proceed with the installation. Here are the straightforward steps to follow:

1. Open a web browser and navigate to the [release page](https://github.com/parallelchain-io/pchain-client-cli/releases).
2. Locate and download the pre-built binary that is compatible with your platform.
3. Execute the downloaded file to initiate the installation process.

Please note that if this is your first time using `pchain_client`, it is necessary to set up the `$PCHAIN_CLI_HOME` environment variable to specify the home path. For detailed instructions on setting up environment variables on Windows, Mac, and Linux systems, please refer to this resource: [here](https://chlee.co/how-to-setup-environment-variables-for-windows-mac-and-linux/).

## Running pchain_client
---
To check if `pchain_client` is intalled properly, run the following command:
=== "Linux / macOS"
    ```bash
    ./pchain_client --version
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe --version
    ```

Upon first use of `pchain_client`, you will be prompted to set up a password to protect your account keypairs. Please note that this password can be different from the password you used in ParallelChain Explorer. Alternatively, you can skip the password protection by simply pressing Enter.

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