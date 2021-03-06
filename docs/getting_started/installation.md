---
tags:
  - testnet 2.0
  - parallelchain light client
  - tutorial
---

# Installation

## Introduction
---

Parallelchain light client (`pchain`) is a command-line tool for you to connect and interact with the ParallelChain Mainnet. `pchain` supports Windows and Linux/MacOs.

Throughout the section [Getting Started](installation.md), we will describe how to transfer tokens, deploy smart contract and call contract with real world examples by using `pchain`. You can find all `pchain` commands with sample outputs in the [References](references.md) section.

## For Windows 
---

Download the compressed zip file from [https://cms.parallelchain.io/parallelchain-very-light_win_v0.1.0.zip](https://cms.parallelchain.io/parallelchain-very-light_win_v0.1.0.zip)

Unzip the file to extract the executable `pchain.exe`. 

![Screenshot](../screen/win_install_1.png)

To open Command Prompt, type *WIN+R* and input `cmd`:

![Screenshot](../screen/win_install_2.png)

Head to the directory where `pchain.exe` is located via `cd`. For example, the executable is located at C:\Development:

![Screenshot](../screen/win_install_3.png)

Run the command `pchain.exe` and see usage page.

Congratulations. You have successfully installed `pchain` and are ready to proceed to ["Prepare Environment"](prepare_env.md)

### For Powershell User

This section describes using `PowerShell` as the command line utility to install ParallelChain Light Client. 

Open up `PowerShell` using the `run` keyboard shortcut. That is *WIN+R* and type in `powershell` to proceed. 

Unzip the compressed zip file by `Expand-Archive`. Please specify the source path and destination path for your command parameters:

-  `<SOURCE_PATH>`: the directory where `parallelchain-very-light_win_v0.1.0.zip` is located.
-  `<DESTINATION_PATH>`: the directory you intend to install `pchain`. 

```PowerShell
Expand-Archive -LiteralPath 'C:\<SOURCE_PATH>\parallelchain-very-light_win_v0.1.0.zip' -DestinationPath 'C:\<DESTINATION_PATH>\parallelchain-very-light_win_v0.1.0.exe'
```

To switch the operating mode of `PowerShell` from a normal mode to administrator mode:
```PowerShell
Start-Process powershell -Verb runAs
```

Head to the destination directory where `parallelchain-very-light_win_v0.1.0.exe` is extracted:
```PowerShell
Set-Location C:\<DESTINATION_PATH>\
```

In the destination directory (`<DESTINATION_PATH>`), it is suggested to rename the binary to `pchain` so that it becomes easier to follow this guide:
```PowerShell
Rename-Item -Path 'parallelchain-very-light_win_v0.1.0.exe' -NewName 'pchain.exe'
```

Run the command `pchain` to see if it launches.
```PowerShell
pchain.exe
```
<details>
  <summary>To verify that the Light Client works</summary>
    pchain is now an executable from anywhere on your system
    ```bash
    verylight 0.1.0
    <Parallel Chain Lab>
    Parallel Chain Mainnet verylight
    ```
</details>

Congratulations. You have successfully installed `pchain` and are ready to proceed to ["Prepare Environment"](prepare_env.md)

## For Linux / macOS
---

In this section, most of the commands between the two operating systems are the same. If there are any differences, a
tab that states a command for a particular operating system will be shown as below:

=== "Linux"
    ```bash
    echo "This is a linux command"
    ```
=== "macOS"
    ```bash
    echo "This is a macOS command"
    ```

To download the precompiled compressed binaries, use:

=== "Linux"
    ```bash
    wget https://cms.parallelchain.io/parallelchain-very-light_linux_v0.1.0.tar.xz
    ```
=== "macOS"
    ```bash
    curl -O https://cms.parallelchain.io/parallelchain-very-light_mac_x86_v0.1.0.tar.xz
    ```

To extract the client program, head to the directory where the downloaded file `parallelchain-very-light_linux_v0.1.0.tar.xz` or  `parallelchain-very-light_mac_x86_v0.1.0.tar.xz` is located and extract via `tar`:
=== "Linux"
    ```bash
    tar -xvf parallelchain-very-light_linux_v0.1.0.tar.xz 
    ```
=== "macOS"
    ```bash
    tar -xvf parallelchain-very-light_mac_x86_v0.1.0.tar.xz
    ```

Rename the client program to `pchain` so that it becomes easier to follow this guide:
```bash
mv parallelchain-very-light_linux_v0.1.0 pchain
```

Run the command `pchain` to see if it launches.
=== "Linux"
    ```bash
    ./pchain
    ```
=== "macOS"
    ```bash
    ./pchain
    ```

    !!! Tip
        **For macOS users**: _Newer versions of macOS contain extra security verification steps configured by default. This is called [GateKeeper](https://support.apple.com/en-gb/guide/security/sec5599b66df/web). This usually happens when you download the binaries from a browser instead of using the `curl` tool as mentioned in the previous step. In case you found a message like this when calling `pchain`_:
        ```bash
        ./pchain
        ```
        ![Screenshot](../screen/macos_gatekeeper_1.png)

        then you can remove macOS' "GateKeeper" attributes from `pchain` by this command and run pchain as normal
        ```bash
        sudo xattr -rd com.apple.quarantine ./pchain
        ```

        This is an elevated action, so you will need to enter your password to continue. `pchain` can now be used as normal.

<details>
  <summary>To verify that the Light Client works</summary>
    ```bash
    verylight 0.1.0
    <Parallel Chain Lab>
    Parallel Chain Mainnet verylight
    ```
</details>

Congratulations. You have successfully installed `pchain`.

!!! Tip

    You might want to store `pchain` in a directory of your choice so that it is easier to follow the commands in the tutorial. For example, we created a folder in our home directory called parallelchain_client:
    ```bash
    $ mkdir -p /home/my_user/parallelchain_client
    $ cp pchain /home/my_user/parallelchain_client/
    $ cd /home/my_user/parallelchain_client
    $ ./pchain
    ```

    So from now on, when you see a command like this in linux/macOS:
    ```bash
    ./pchain
    ```

    It means that `pchain` shall be executed from the directory you stored `pchain` in.

