---
layout: post
title: 'Windows'
parent: '1.1 Installation'
grand_parent: 'Lesson 1: Zephyr Setup'
---

# Install

NOTE: Please follow either following instructions or refer to [official instructions](https://docs.zephyrproject.org/latest/develop/getting_started/index.html).

## 1) Install dependencies

- Install Chololatey
    Run `Get-ExecutionPolicy`. If it returns `Restricted`, then run `Set-ExecutionPolicy AllSigned` or `Set-ExecutionPolicy Bypass -Scope Process`.

    Now, run the following command:
    ```
    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
    ```

    If you don't see any errors, you are ready to use `Chocolatey`! Type `choco` or `choco -?` to verify you can use it.

- Open a command prompt (cmd.exe) as an Administrator (press the Windows key, type “cmd.exe” in the prompt, then right-click the result and choose “Run as Administrator”).

- Optionally disable global confirmation to avoid having to confirm installation of individual programs:
```
choco feature enable -n allowGlobalConfirmation
```

- Install CMake:
```
choco install cmake --installargs 'ADD_CMAKE_TO_PATH=System'
```

- Install the rest of the tools:
```
choco install git python ninja dtc-msys2 gperf
```

- Install `vs_BuildTools`
```
Download & install https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022 (this is current version as of 07/04/2022, use whatever is latest)
```

Select following options during installation:

![vs_BuildTool_setup_options](/images/zephyr-setup/vs_BuildTool_setup_options.png)

- Close the Administrator command prompt window.

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
    - Set `ZEPHYR_TOOLCHAIN_VARIANT` to `gnuarmemb`.
    - Set `GNUARMEMB_TOOLCHAIN_PATH` to the toolchain installation directory
    - E.g.:
        ```
        set ZEPHYR_TOOLCHAIN_VARIANT=gnuarmemb
        set GNUARMEMB_TOOLCHAIN_PATH=C:\gnu_arm_embedded
        ```

## 6) Build `Hello World!` sample
- Go to your <zephyr repo>
```
cd %homepath%\zephyrproject\zephyr
```

- Run following command to build with verbose (`-v`) and cleaning any previous build files (`--prestine`)
```
west -v build -b rpi_pico samples\hello_world --pristine
```
OR
- Run following command to build WITHOUT verbose and cleaning any previous build files (`--prestine`)
```
west build -b rpi_pico samples\hello_world --pristine
```
A succesful build looks like this:
![succes-build_win_blinky](/images/zephyr-setup/succes-build_win_blinky.png)

## 7) Build `Blinky` sample
- Go to your <zephyr repo>
```
cd %homepath%\zephyrproject\zephyr
```

- Run following command to build with verbose (`-v`) and cleaning any previous build files (`--prestine`)
```
west -v build -b rpi_pico samples\basic\blinky --pristine
```
OR
- Run following command to build WITHOUT verbose and cleaning any previous build files (`--prestine`)
```
west build -b rpi_pico samples\basic\blinky --pristine
```
A succesful build looks like this:
![succes-build_win_hello_world](/images/zephyr-setup/succes-build_win_hello_world.png)

## 7) Flash the Sample
```
cd %homepath%\zephyrproject\zephyr\samples\basic\blinky
west flash
```