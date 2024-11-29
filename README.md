# Simple YAML for an ESPHome based DSMR reader.

I had to patch the [Arduino DSMR library](https://github.com/wwebers/arduino-dsmr) to make it work with the Aidon 6534 reader. 
The library resides in another github repository and is a clone of the official Arduino library.

## Flashing the firmware

The easiest way to build and flash the firmware is to use docker and to follow the official ESPHome guide. The following steps
assume you're running Linux. Windows users will have to adjust the command lines accordingly. It is also assumed that the user
account has access rights for accessing the USB serial port.

### Clone the repository

Clone the repository...

    git clone https://github.com/wwebers/dsmr.git


...and adjust the YAML configuration according your needs. You can also add a `secrets.yaml` to store credentials if needed.

### Pull the docker image 

    docker pull ghcr.io/esphome/esphome

### Build and upload the image

Connect the board via USB to the computer and run the following commands:


    docker run --rm -v "${PWD}":/config -it --network host --device=/dev/ttyUSB0 ghcr.io/esphome/esphome clean dsmr.yaml
    docker run --rm -v "${PWD}":/config -it --network host --device=/dev/ttyUSB0 ghcr.io/esphome/esphome run dsmr.yaml

### Initial configuration

If the configuration does not include any information about the Wifi network (which it does not when just cloning this
 repository), ESPHome will start an access point (AP) after about a minute. You will have to connect to this AP with your 
 mobile device to configure the WLAN to which the DSMR reader should connect. The board will reboot and will connect to that WLAN
 and show up in HomeAssitant.

 ## Security recommendation

 It is recommended to adjust the YAML configuration to setup WLAN, OTA and API access with proper credentials. It is also 
 recommended to store those credentials inside an explicit `secrets.yaml` file.
