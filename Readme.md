Basic Hello World for esp-idf blink on Esp32-C6 (I have Esp32-C6-DevkitC-1) and PlatformIO

This repo contains all the neccesarry code to get the Esp32-C6 run the (blink example provided by expressif in esp-idf)[https://github.com/espressif/esp-idf/blob/master/examples/get-started/blink/main/blink_example_main.c]. The main.c file is just that code without any changes. 

The contribution of the repo is an esasy to use configuaration with the build envorionment already provided and all the other build flags already set. See platformio.ini for the neccessary build flags. 

In some ways espressifs build really is overly complicated. If you wanted a basic blink sketch that was as simple as possible this is not it. Since it supports multiple scenarious and hardware, and changes behavior based on the build flags set in platformio.ini. For most real world scenarious it's prerry meaningless. However its a good example of the whole build system including the environment set in platformio.ini and sdkconfig.

# Rewuirements
Before getting started with the project make sure you have platform io core and esptool.py installed. 

## PlatformIO Core
If you are on a mac like me just install it with homebrew by running `brew install pio`

Otherwise PlatformIO has a guide on their website on (how to install PlatformIO core)[https://docs.platformio.org/en/latest/core/installation/index.html] whoch is a CLI tool. 

## esptool.py
A command line tool for managing ESP. Can be installed with pip. 
````
▶ pip install esptool # install ESP tool if you do nbot have already
````

Espressif also has (a handy guide)[https://docs.espressif.com/projects/esptool/en/latest/esp32/installation.html] showing how to install it on different systems.

# Setup

Before runnging make sure to make sure your boards flash size is set in sdkconfig.defaults. If you are unsure which specific flash size your board has run the below command to check. 

````
▶ esptool.py --port /dev/cu.usbmodem21401 flash_id

````

Expected output:
````
esptool.py v4.8.1
Serial port /dev/cu.usbmodem21401
Connecting...
Detecting chip type... ESP32-C6
Chip is ESP32-C6 (QFN40) (revision v0.1)
Features: WiFi 6, BT 5, IEEE802.15.4
Crystal is 40MHz
MAC: 40:4c:ca:ff:fe:5f:7f:14
BASE MAC: 40:4c:ca:5f:7f:14
MAC_EXT: ff:fe
Uploading stub...
Running stub...
Stub running...
Manufacturer: c8
Device: 4017
Detected flash size: 8MB
Hard resetting via RTS pin...
````

As you can see mine has 8MB of flash. Therefore the content of sdkconfig.defaults needs to be `CONFIG_ESPTOOLPY_FLASHSIZE_8MB=y`

If you have another flash size on your board make sure the line in sdkconfig.defaults matches that flash size. Below is some examples. 

````
CONFIG_ESPTOOLPY_FLASHSIZE_1MB=y
CONFIG_ESPTOOLPY_FLASHSIZE_2MB=y
CONFIG_ESPTOOLPY_FLASHSIZE_4MB=y
CONFIG_ESPTOOLPY_FLASHSIZE_8MB=y
CONFIG_ESPTOOLPY_FLASHSIZE_16MB=y
CONFIG_ESPTOOLPY_FLASHSIZE_32MB=y
CONFIG_ESPTOOLPY_FLASHSIZE_64MB=y
````

Choose only one of the lines above that matches your flash size and put in sdkconfig.defaults. Be aeare that a board specific file will be generated by PlatformIO at built. It will be called `sdkconfig.*` followed by the soecific board you have. If you already has tried to built before making this change this file might already be present. And it might have the wrong flash size. In that case. Sinxe the file is auto generated, you can just delete it. 
