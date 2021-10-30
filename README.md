# WEEDO X40 Community Firmware
![image](http://www.weedo.ltd/wp-content/uploads/2021/04/970x300-ABanner1.jpg)

## Summary
This is the repository that contains the community version firmware for the WEEDO X40 3D Printer.  The framework of the firmware is based on the Marlin 2.0.x version.  We fixed some bugs in the dual x carriage modules. The user interaction and network communication modules are developed by WEEDO3D.

- Community development discussions take place here: https://discord.gg/zXbhdnQyYH

- This firmware is incompatible with WiiBuilder/pre-sliced gcode on the SD card.  (See https://github.com/mah115/WEEDOX40firmware/issues/1 for more info).  You will need to delete the line ```G1 X-47 F3000``` from the start gcode in WiiBuilder generated gcode files.

## Instructions:
- Upload firmware using Weedo's firmware flasher (see below)
- You will also need update the PID gains in EEPROM after flashing, run gcode:
```
M301 E0 P9 I0.34 D30
M301 E1 P9 I0.34 D30
M500
```
- Precompiled binary file is in **WEEDOX40_firmware\.pio\build\stm32_vet6\firmware.bin**
- To go back to the official firmware, load **Release/X40_V1.2.0_Community_to_Official.bin**
- To use USB, use buad rate 115200
- To use USB with Linux/Raspberry Pi, you need to replace the CH340 driver: https://github.com/mah115/WEEDO_X40_files/blob/main/CH34x_raspi_driver_installation.md

## Major differences from the official firmware:
- This firmware does not have the SD card firmware update capability, use the USB to load (see below)
- Improved jog mode responsiveness and made left/right arrows consistent with coordinate increase/decrease.
- Fixed excessive hotend temp overshoot by modifying PID controller.
- G2/G3 implementation is updated from a newer version of Marlin which fixes a bug that results in incorrect move speeds.  This is needed to make it work well with Arc Welder.
- Z-axis offset adjustment resolution increased from 0.1mm to 0.02mm.
- Extruder will brush both sides of the nozzle before printing.
- Extruder nozzle will stay at home position while heating up instead of hovering over the part.
- Relaxed tolerances for temperature hysteresis.
- Fixed issue where right extruder would crash into left when going to X=0.  This fix may cause the sample Gcode on the SD card to not print properly due to a bug in their start gcode.  To fix, add a line **G28 X** after **G1 X-47** in the start gcode. 

## Compile requirements

- Download and install [VSCode](https://code.visualstudio.com/)
- Search and install PlatformIO IDE from store marketplace
- Search and install ST STM32 embedded platform from PIO Home
- Copy framework-arduinoststm32-maple@99.99.99 from /buildroot/lib to PlatformIO library location (user)/.platformio/packages/
- Install the USB driver from /buildroot/driver/CH341SER.ZIP

## Upload firmware

X40 uses a customized bootloader, which requires a customized download program for firmware update.  

The Windows version of the download program WEEDOIAP.exe is located in the /buildroot/upload/ directory. The macos version is still under development.

Lanuch the WEEDOIAP.exe, open the firmware.bin file from build directory, choice the com port, then click the update.

![image](http://www.weedo.ltd/wp-content/uploads/2021/04/weedoiap.png)

X40 will restart to enter the firmware update mode. Wait for about 1 minute, the printer will automatically restart after the firmware update is completed.

![image](http://www.weedo.ltd/wp-content/uploads/2021/04/iap.jpg)



## The difference between the community version firmware and the official version firmware

The official firmware has the function of upgrading the firmware via TF card. Use the IAP function in the control menu to read the flash.wfm file on the TF card to upgrade the firmware.

The community version firmware does not support the wfm format, so it does not support firmware upgrade via TF card.


## Return to the official vesrion firmware

Use WEEDOIAP.exe to flash X40_Vxxx_Community_to_Official.bin in the /Release directory.
