---
tags:
  - testnet 1.0
  - parallelchain light client
  - tutorial
---

# Installation

`Learning outcome`: _Setup and install `pchain` on your system._

## For Windows 

This section will use `PowerShell` as the command line utility to install ParallelChain Light Client. Open up `PowerShell` using the `run` keyboard shortcut. That is *Ctrl+R* and type in `powershell` to proceed. 

Download the binaries from [https://cms.parallelchain.io/parallelchain-cli_win_x64_v0.1.0.zip](https://cms.parallelchain.io/parallelchain-cli_win_x64_v0.1.0.zip)

Unzip the compressed zip file by using the following `PowerShell` command. Make sure that you replace the two variables below with your own.
* `<SOURCE_PATH>`: the directory where `parallelchain-cli_win_x64_v0.1.0.zip` is located.
* `<DESTINATION_PATH>`: the directory you intend to install `pchain`. 
```PowerShell
Expand-Archive -LiteralPath 'C:\<SOURCE_PATH>\parallelchain-cli_win_x64_v0.1.0.zip' -DestinationPath 'C:\<DESTINATION_PATH>\parallelchain-cli_win_x64_v0.1.0.exe'
```

Switch the operating mode of `PowerShell` from a normal mode to administrator mode with this command:
```PowerShell
Start-Process powershell -Verb runAs
```

Head to the destination directory where `parallelchain-cli_win_x64_v0.1.0.exe` is extracted:
```PowerShell
Set-Location C:\<DESTINATION_PATH>\
```

In the destination directory (`<DESTINATION_PATH>`), we need to rename the binary to `pchain` so that it is easier to follow this guide:
```PowerShell
Rename-Item -Path 'parallelchain-cli_win_x64_v0.1.0.exe' -NewName 'pchain.exe'
```

Type the command `pchain` to see if it launches.
```PowerShell
./pchain.exe
```
<details>
  <summary>To verify that the Light Client works</summary>
    `pchain` is now an executable from anywhere on your system
    ```bash
    verylight 0.1.0
    <Parallel Chain Lab>
    Parallel Chain Mainnet verylight
    ```
</details>

Congratulations. You have successfully installed `pchain` and are ready to proceed to the following sections.

## For Linux

You can download the precompiled compressed binaries using the terminal command below:
```bash
wget https://cms.parallelchain.io/parallelchain-cli_linux_v0.1.0.tar.xz
```

After downloading the file `parallelchain-cli_linux_v0.1.0.tar.xz`, head to the directory where your binaries are located. 

Type this command to extract and use the light client immediately. **The following two commands require privileged access; you can look at `Administrative Action in Linux` at the bottom of this section on allowing these types of actions.**
```bash
sudo tar -xvf parallelchain-cli_linux_v0.1.0.tar.xz --directory /usr/bin 
```

We need to rename the binary to `pchain` so that it is easier to follow this guide:
```bash
sudo mv /usr/bin/parallelchain-cli_linux_v0.1.0 /usr/bin/pchain
```

<details>
  <summary>Administrative Action in Linux</summary>
When you add a "sudo" keyword to the front of a command in linux, you need to enter your password like this:
```bash
[sudo] password for my_user:
```
and press enter to continue with the operation.
</details>

Type the command `pchain` to see if it launches.
```bash
pchain
```
<details>
  <summary>To verify that the Light Client works</summary>
    `pchain` is now an executable from anywhere on your system
    ```bash
    verylight 0.1.0
    <Parallel Chain Lab>
    Parallel Chain Mainnet verylight
    ```
</details>

Congratulations. You have successfully installed `pchain`.