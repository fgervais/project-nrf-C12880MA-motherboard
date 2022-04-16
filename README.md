# Project Management

## Init

```bash
mkdir project-nrf-C12880MA-motherboard_west
cd project-nrf-C12880MA-motherboard_west
docker run --rm -it -u $(id -u):$(id -g) -v $(pwd):/workdir/project nrfassettracker/nrfconnect-sdk:v1.9-branch \
  bash -c "west init -m https://github.com/fgervais/project-nrf-C12880MA-motherboard.git . && west update"
```

### Add Thread network key

Add `secret.conf` in the `app` folder with content like this which matches your 
network key:

```
CONFIG_OPENTHREAD_NETWORKKEY="00:11:22:33:44:55:66:77:88:99:aa:bb:cc:dd:ee:ff"
```

## Docker environment

```bash
cd application
docker-compose run nrf bash
```

## Build

```bash
cd application
west build -b nrf52840dongle_nrf52840 -s app
```

## menuconfig

```bash
cd application
west build -b nrf52840dongle_nrf52840 -s app -t menuconfig
```

## Flash

```bash
nrfutil pkg generate --hw-version 52 --sd-req=0x00 \
        --application build/zephyr/zephyr.hex \
        --application-version 1 first.zip

nrfutil dfu usb-serial -pkg first.zip -p /dev/ttyACM0
```

# Hardware

https://github.com/fgervais/project-nrf-C12880MA-motherboard_hardware
