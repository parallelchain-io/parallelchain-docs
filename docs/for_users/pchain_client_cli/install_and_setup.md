---
tags:
  - Tools
  - CLI
  - pchain-cli-rust
---

# Install and Setup

## Installation

`pchain_client` is an available tool for users on Unix/Linux, MacOS, and Windows operating systems. Simply download the pre-built binary corresponding to your platform and install the `pchain_client`.


### Windows 

1. Download the latest release as a compressed file from Assets of ParallelChain Lab's GitHub [release page](https://github.com/parallelchain-io/pchain-client-cli/releases).
2. Unzip the file to extract the executable `pchain_client.exe`.
3. Open Powershell by pressing *WIN+R* and typing `powershell`.
4. Navigate to the directory where `pchain_client.exe` is located using the `cd` command. For example, if the executable is located at `C:\Development`, type `cd C:\Development`.
5. Follow the instructions in Section [Prepare Environment](#prepare-environment) to get ready for interacting with the blockchain.

### Linux / macOS

The installation process for Linux and macOS is similar. To install `pchain_client`:

1. Download the latest release as a compressed file from Assets of ParallelChain Lab's GitHub [release page](https://github.com/parallelchain-io/pchain-client-cli/releases).

2. Head to the directory where the downloaded file is located and extract it using `tar`. For example:

    === "Linux"
        ```bash
        tar -xvf pchain_client_linux_v0.4.3.tar.gz 
        ```
    === "macOS"
        ```bash
        tar -xvf pchain_client_mac_v0.4.3.tar.gz 
        ```

3. Follow the instructions in Section [Prepare Environment](#prepare-environment) to get ready for interacting with the blockchain.


!!! Tips
    - If you're using macOS and encounter a [GateKeeper](https://support.apple.com/en-gb/guide/security/sec5599b66df/web) message when trying to run pchain_client, you can remove macOS' "GateKeeper" attributes from pchain_client using the following command:
    ```bash
    sudo xattr -rd com.apple.quarantine ./pchain_client
    ```
    This is an elevated action, so you will need to enter your password to continue. `pchain_client` can now be used as normal.

    - You might want to store `pchain_client` in a directory of your choice so that it is easier to follow the commands in the tutorial. For example, we created a folder in our home directory called parallelchain_client:
    ```bash
    $ mkdir -p /home/my_user/parallelchain_client
    $ cp pchain_client /home/my_user/parallelchain_client/
    $ cd /home/my_user/parallelchain_client
    $ ./pchain_client
    ```


!!! note
    If this is your first time using `pchain_client`, you need to set up `$PCHAIN_CLI_HOME` in environment variables to specify the home path. See more [here](https://chlee.co/how-to-setup-environment-variables-for-windows-mac-and-linux/).

## Running pchain_client

### Creating Password

For the first time to use `pchain_client`, you need to create your password for using it. The terminal should prompt you as follows:

```text
First time using ParallelChain Client CLI. Please set up a password to protect your keypairs.
Your password: 
```

This password is only used by the CLI, and **NOT** associated with the blockchain. It is used for encryption and decryption of your keypairs so that the keypairs are stored in your computer more securely. Alternatively, you can skip the password protection by simply pressing **Enter**.

You will be required to enter your password twice. If your password is set successfully, you will see a return message with the `pchain_client` version shown on the console.

=== "Linux / macOS"
    ```bash
    ./pchain_client --version
    ```
=== "Windows PowerShell"
    ```PowerShell
    ./pchain_client.exe --version
    ```

!!! warning
    The password is not sent and saved anywhere. You won't be able to recover the password if you lose it. Please keep your password safe. You will be required to provide this password to submit transactions and manage keypairs later.


## Prepare Environment

### Setting Environmental Variables

Specify the location of the directory for storing your config and keypair by setting the environmental variable `PCHAIN_CLI_HOME`. 

Always remember the location that you set, if you forget the location, it means you forget where your keypair is being placed.

!!! Tips
    For security reasons, you may want to set environmental **temporarily**, so that after you close the terminal session, it will forget the keypair location.

    For convenience reasons, you may alternatively want to set environmental **permanently**. Even in that case, we still suggest you remember the storage location.

The following command will set the environmental variable **temporarily**:

=== "Linux / macOS"
    ```bash
    # For example, "/home/user/pchain_cli_home"
    export PCHAIN_CLI_HOME="<PATH_TO_DIRECTORY>"
    ```
=== "Windows PowerShell"
    ```PowerShell
    # For example, "C:\Users\user\pchain_cli_home"
    $Env:PCHAIN_CLI_HOME="<PATH_TO_DIRECTORY>"
    ```

The following command will set the environmental variable **permanently**:

=== "Linux / macOS"
    ```bash
    ##########################################################
    ## Append this line to $Home/.bashrc and $Home/.profile ##
    ##########################################################
    export PCHAIN_CLI_HOME="<PATH_TO_DIRECTORY>"
    ```
=== "Windows PowerShell (restart shell to pick up change)"
    ```PowerShell
    setx PCHAIN_CLI_HOME "<PATH_TO_DIRECTORY>"
    ```

### Setting Endpoint Interacting with Parallelchain

After installation of `pchain_client`, you had to update the Mainnet / Testnet endpoint to communicate with the Mainnet / Testnet. 

=== "Linux / macOS"
    ```bash
    # Mainnet
    ./pchain_client config setup --url https://pchain-main-rpc02.parallelchain.io

    # Testnet
    ./pchain_client config setup --url https://pchain-test-rpc02.parallelchain.io
    ```
=== "Windows PowerShell"
    ```PowerShell
    # Mainnet
    ./pchain_client.exe config setup --url https://pchain-main-rpc02.parallelchain.io
    
    # Testnet
    ./pchain_client.exe config setup --url https://pchain-test-rpc02.parallelchain.io
    ```

This would check the status of your chosen provider. If `pchain_client` cannot connect to your provider, a warning message will be shown, and the setup fails. You need to set up another URL with the above command again.

A `config.toml` file will be created in the folder specified by the environment variable `PCHAIN_CLI_HOME` upon success. It only needs to be executed once.

Now you can start the journey to play around with `pchain_client`!
