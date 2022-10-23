# Install Klipper on Ender 3 Pro w/ OctoPrint and CRTouch

Setup for OctoPrint and Klipper on a RPi 3 controlling an Ender 3 Pro w/ CRTouch

## Initial Setup

I have a "basic" Ender 3 Pro that was purchased from MicroCenter. By "basic" I mean I have no additional upgrades other than the CR Touch installed. The controller board is a Creality v4.2.2 board verified by inspecting the stamp on the board. After installing the CR Touch kit from Creality, I was unable to get Marlin v1 to work correctly, so I used the Marlin v2.0.9.3 available from ShinyUpgrades.com.

[Ender 3 Pro Firmware Install Guide - Update Firmware with MicroSD Card](https://shinyupgrades.com/pages/ender-3-pro-firmware-install-guide-update-firmware-with-microsd-card)

[Motherboard Version: 4.2.2 with STM32F103 RET6, Bed Size 220x220](https://cdn.shopify.com/s/files/1/0613/3560/1376/files/ShinyUpgrades-2.0.9.3-With-CR-Touch-7x7-422-Board-Host-Commands-20211228-162959.bin?v=1641318680)

## Document Settings Before Upgrade

Before upgrading from Marlin to Klipper, I think it's best to record any settings you can in case you need them to revert back if things go wrong.

### OctoPrint Settings

- Connection
  - Serial Port: /dev/ttyUSB0
  - Baud: 115200
- General
  - Printer Model: Creality Ender 3 Pro
- Print bed & build volume
  - Form Factor: Rectangular
  - Origin: Lower Left
  - [x] Heated Bed
  - [ ] Heated Chamber
  - Width  (X): 220mm
  - Depth  (Y): 220mm
  - Heigth (Z): 250 mm
  - [ ] Custom bounding box
- Axes
  - X: 6000mm/min
  - Y: 6000mm/min
  - Z:  200mm/min
  - E:  300mm/min
- Hotend & extruder
  - Nozzle Diameter: 0.4mm
  - # of extruders: 1
  - Default extrusion length: 5mm
  
### Printer Settings in Firmware  

> These settings can be accessed from the printer display

- Marlin: 2.0.9.3 (ShinyUpgrades E3P w/ CRTouch)
- Velocity
  - Max X: 500
  - Max Y: 500
  - Max Z:   5
  - Max E:  25
- Z Offset: -01.350
- Probe Offset
  - X: -47.0
  - Y: -05.0
  - Z: -1.35
- Steps:
  - X:  80.0
  - Y:  80.0
  - Z: 400.0
  - E: 139.0
- PIDs
  - PID -P E1:   +028.7
  - PID -I E1:   +002.62
  - PID -D E1:   +078.8
  - Autotune E1:  185
  - PID -P Bed:  +462.1
  - PID -I Bed:  +085.47
  - PID -D Bed:  +624.6
  - Autotune Bed:  60
- Thermistors
  - E1:  EPCOS 100K
  - Bed: EPCOS 100K

## Install Klipper

I'm following the instructions available from this site, [https://3dprintbeginner.com/install-klipper-on-ender-3/](https://3dprintbeginner.com/install-klipper-on-ender-3/) as I'm already using OctoPrint.

Start by running `sudo apt update && sudo apt upgrade` to get your Rpi updated and reboot before starting the install. After that, follow the instructions from the site linked above. Below are the options I selected as the screenshots from the site are no legible.

- Step 5: Klipper Firmware Configuration
  - Check the config file for the Ender 3 Pro from Klipper for the settings to use, [https://github.com/Klipper3d/klipper/blob/master/config/printer-creality-ender3pro-2020.cfg](https://github.com/Klipper3d/klipper/blob/master/config/printer-creality-ender3pro-2020.cfg)
    
    ```text
    # This file contains pin mappings for the stock 2020 Creality Ender 3
    # Pro with the 32-bit Creality 4.2.2 board. To use this config, during
    # "make menuconfig" select the STM32F103 with a "28KiB bootloader" and
    # serial (on USART1 PA10/PA9) communication.

    # It should be noted that newer variations of this printer shipping in
    # 2022 may have GD32F103 chips installed and not STM32F103. You may
    # have to inspect the mainboard to ascertain which one you have. If it
    # is the GD32F103 then please select Disable SWD at startup in the
    # "make menuconfig" along with the same settings for STM32F103.
    ```
    
  - Use these settings:

    ```text
           Klipper Firmware Configuration
    [*] Enable extra low-level configuration options
        Micro-controller Architecture (STMicroelectronics STM32)  --->
        Processor model (STM32F103)  --->
    [ ] Only 10KiB of RAM (for rare stm32f103x6 variant) (NEW)
    [*] Disable SWD at startup (for GigaDevice stm32f103 clones)
        Bootloader offset (28KiB bootloader)  --->
        Clock Reference (8 MHz crystal)  --->
        Communication interface (Serial (on USART1 PA10/PA9))  --->
    (250000) Baud rate for serial port
    ()  GPIO pins to set at micro-controller startup (NEW)
    ```

- Press `esc` and save config

    ```bash
    Using default symbol values (no '/home/gairy/klipper/.config')
    Configuration saved to '/home/gairy/klipper/.config'
    Creating symbolic link out/board
    Loaded configuration '/home/gairy/klipper/.config'
    Configuration saved to '/home/gairy/klipper/.config'
    ```

- Step 9

    ```bash
    $ ls /dev/serial/by-id/*
    /dev/serial/by-id/usb-1a86_USB_Serial-if00-port0
    
    $ make flash FLASH_DEVICE=/dev/serial/by-id/usb-1a86_USB_Serial-if00-port0
    ```

One issue I found was that the sample config in step 17 was no longer available. 
