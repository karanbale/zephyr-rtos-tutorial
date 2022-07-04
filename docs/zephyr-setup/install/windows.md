---
layout: post
title: 'Windows'
parent: '1.1 Installation'
grand_parent: 'Lesson 1: Zephyr Setup'
---

# Install

## 1) Install dependencies

- Install Chololatey
    Run Get-ExecutionPolicy. If it returns Restricted, then run Set-ExecutionPolicy AllSigned or Set-ExecutionPolicy Bypass -Scope Process.

    Now, run the following command:
    ```
    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
    ```

    If you don't see any errors, you are ready to use Chocolatey! Type choco or choco -?

    Open a command prompt (cmd.exe) as an Administrator (press the Windows key, type “cmd.exe” in the prompt, then right-click the result and choose “Run as Administrator”).

    Optionally disable global confirmation to avoid having to confirm installation of individual programs:
    ```
    choco feature enable -n allowGlobalConfirmation
    ```

    Install CMake:
    ```
    choco install cmake --installargs 'ADD_CMAKE_TO_PATH=System'
    ```

    Install the rest of the tools:
    ```
    choco install git python ninja dtc-msys2 gperf
    ```

    Close the Administrator command prompt window.

## 2) Bootstrap west

- West is zephyr's multi-purpose tool, that lets you easily get the zephyr project source code, instead of manually cloning the Zephyr repos along with west itself.

    Run following command.
    ```
    pip3 install west
    ```

    Open a new command line and run following command to check if West was correctly installed:
    ```
    pip3 check west
    ```

- If you have issues with West, please follow the [Troubleshooting West guide](https://docs.zephyrproject.org/latest/develop/west/troubleshooting.html).

## 3) Clone Zephyr into new folder `zephyrproject`

Following steps may run for serveral minutes, depending upon your network bandwidth, be patient.
You should see command prompt being udpated with status, it should not stay idle.

NOTE: You can rename `zephyrproject` to anything you wish/like.

```
west init zephyrproject
cd zephyrproject
west update
```

## 4) Install python dependencies

Install additioanl python packages required by Zephyr in `cmd.exe` prompt.

```
pip3 install -r zephyr/scripts/requirements.txt
```

## 5) Install GNU ARM Embedded toolchain
- Download [GNU ARM Embedded build](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads) 
    - For my raspberry pi pico board, I downloaded `AArch32 bare-metal target (arm-none-eabi)`

- Configure the environment variables needed to inform Zephyr build system to use this toolchain:
    - Set ZEPHYR_TOOLCHAIN_VARIANT to gnuarmemb.
    - Set GNUARMEMB_TOOLCHAIN_PATH to the toolchain installation directory
    - E.g.:
        ```
        set ZEPHYR_TOOLCHAIN_VARIANT=gnuarmemb
        set GNUARMEMB_TOOLCHAIN_PATH=C:\gnu_arm_embedded
        ```

## 6) Build Hello World! sample

## 7) Flash the Sample
