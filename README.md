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
