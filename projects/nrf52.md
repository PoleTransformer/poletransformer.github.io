---
layout: post
title: My journey with the NRF52
---

We are all familiar with the ESP32, STM32, and Arduino boards. They are very good hobby grade boards, but these are rarely used in production setups. I want to try to design a custom PCB that can fit in my wallet that has BLE, NFC, and RFID capabilities. Basically a DIY flipper zero. Lets get started:

Before I buy anything from Digikey or Mouser, I want to do as much prep as possible to reduce the workload. First, installing the toolchain and SDK. Nordic Semiconductor provides decent docs for this. At first, it can be confusing, but after reading it a few dozen times, I somewhat understand it. I am using Vscode as my IDE. There is also an extension called `nRF Connect`, but honestly I prefer the command line as its more familiar for me. Here are the refined steps I followed for a Unix based installation:

1. Download `nrfutil` [here](https://www.nordicsemi.com/Products/Development-tools/nRF-Util/Download#infotabs), `chmod +x nrfutil`, and move it into `/usr/local/bin`.
2. Install the toolchain manager: `nrfutil install toolchain-manager`
3. Install the toolchain. As of 01/10/25, v2.9.0 is the latest, specify the version you want: `nrfutil toolchain-manager install --ncs-version v2.9.0`
4. Start toolchain environment: `nrfutil toolchain-manager launch --shell`
5. cd into the default toolchain directory: `cd ~/ncs` If you skip this, `west` will install the SDK in the current directory.
6. Initialize SDK: `west init -m https://github.com/nrfconnect/sdk-nrf --mr main main` You can specify a specific version by replacing first main with a version tag, 2nd main is the name of the directory created.
7. cd into the SDK directory: `cd main` Don't skip this!!
8. Update: `west update`. This takes a while. It pulls dependancies from GitHub and puts them into the current SDK directory.
9. Create cmake bindings: `west zephyr-export`. Exports info for cmake into `~/.cmake`.

>**Note:**
> If you get the error that `libunistring.so.2` is missing, create a symbolic link: `ln -s /lib/libunistring.so /lib/libunistring.so.2`. Some Linux distros don't include `libunistring.so.2` by default, `libunistring.so` is the same(I think) as `libunistring.so.2`, so we just symlink it.

If there are zero errors, congrats! You have installed the toolchain and SDK successfully. In VScode open the `~/ncs/main/nrf` directory. If needed, replace main with the version of the SDK you specified. Now everytime we need to compile, we must start the toolchain environment in the terminal by running: `nrfutil toolchain-manager launch --shell`. This is really annoying every single time. In Vscode, you are able to define tasks by creating a json: `.vscode/tasks.json`. We run this task everytime the directory is opened, so we are automatically in the toolchain.

```
//tasks.json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Activate dev environment",
            "type": "shell",
            "command": "nrfutil toolchain-manager launch --shell",
            "runOptions": {
                "runOn": "folderOpen"
              }
        }
    ]
}
```

Now lets try building a sample project for the NRF52840dk. This is the development kit I will purchase after I finish designing the PCB.

1. cd into sample projects directory: `cd samples/bluetooth/central_and_peripheral_hr`
2. Build project: `west build -b nrf52840dk/nrf52840` Specify the board using the `-b` flag.

If everything ran with no errors, the .hex file should be in the build directory named `merged.hex`. This is what will be uploaded to the board. Obviously can't do that just yet, but don't worry, I will document my experience when I receive the board. If there are any errors, make sure you are in the toolchain environment. Click the garbage can icon in VScode and execute `nrfutil toolchain-manager launch --shell` in a new terminal window and try again. Moving on, I want to write a program to blink an LED just to get familiar with the SDK. The documentation has good information on how this is done. We want to make a standlone application.

1. Create a new project directory in the SDK directory: `mkdir -p projects/ledBlink`, `cd projects/ledBlink`
2. Create src directory: `mkdir src`, `touch src/main.c`
3. Create required dependancy files: `touch CMakeLists.txt`, `prj.conf`, `app.overlay`

```
//CMakeLists.txt
cmake_minimum_required(VERSION 3.20.0)

find_package(Zephyr)
project(blink)

target_sources(app PRIVATE src/main.c)
```
```
//prj.conf
CONFIG_DK_LIBRARY=y
```
```
//main.c
#include <dk_buttons_and_leds.h>
#include <zephyr/kernel.h>

int main() {
    dk_leds_init();
    while (true) {
        dk_set_led_on(DK_LED1);
        k_sleep(K_MSEC(500));
        dk_set_led_off(DK_LED1);
        k_sleep(K_MSEC(500));
    }
}
```
Run the build commands specified earlier and to my surprise, no errors! A .hex file was generated in the build folder. Alright great, I am now somewhat familiar with the development. Lets go to the PCB design:

I am using KiCAD to design the PCB. Its open source, lightweight, and no bloody license key. I hate license keys, so annoying and I have to connect to the internet every single time to use the software. KiCAD also includes the majority of the features I need, so I don't need to use software like Altium Designer. My plan is to use a 4 layer PCB, which allows me to get the correct impedances for the antenna traces. This board will have a 2.4GHz BLE antenna and a 13.56 MHz NFC antenna. These must be designed very carefully or the performance will suck. Here is the PCB I have so far:

<div style="text-align: center;">
    <figure style="display: inline-block;">
        <img src="/images/nrf_pcb.png" width="800"/>
        <figcaption>Front layer</figcaption>
    </figure>
</div>