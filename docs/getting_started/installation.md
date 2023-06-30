---
tags:
  - pchain-client
---

# Installation

ParallelChain client (`pchain_client`) is a command-line tool that allows you to connect and interact with the ParallelChain Mainnet / Testnet. `pchain_client` supports both Windows and Linux/macOS. In this tutorial, we will provide you with step-by-step instructions on how to install and use `pchain_client` to transfer tokens, deploy smart contracts, and call contracts.

## Installation Instructions
---

### Windows 

1. Download the latest release as a compressed file from Assets of ParallelChain Lab's GitHub [release page](https://github.com/parallelchain-io/pchain-client-cli).
2. Unzip the file to extract the executable `pchain_client.exe`.
3. Open Command Prompt by pressing *WIN+R* and typing `cmd`.
4. Navigate to the directory where `pchain_client.exe` is located using the `cd` command. For example, if the executable is located at `C:\Development`, type `cd C:\Development`.
5. Follow the instructions in section [Prepare Environment](prepare_env.md) to get ready for interacting with the blockchain.

### Linux / macOS

The installation process for Linux and macOS is similar. To install pchain_client:

1. Download the latest release as a compressed file from Assets of ParallelChain Lab's GitHub [release page](https://github.com/parallelchain-io/pchain-client-cli).

2. Head to the directory where the downloaded file is located and extract it using `tar`. For example:

    === "Linux"
        ```bash
        tar -xvf parallelchain-client_v0.4.0_linux.tar.xz
        ```
    === "macOS"
        ```bash
        tar -xvf parallelchain-client_v0.4.0_mac.tar.xz
        ```

3. Follow the instructions in section [Prepare Environment](prepare_env.md) to get ready for interacting with the blockchain.


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
