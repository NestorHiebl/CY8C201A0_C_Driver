# CY8C201A0 STM32 HAL Driver

The code in this repo is a driver for the CY8C201A0 touch sensor, written in C and using the STM32 hardware abstraction layer to handle I²C communication. The driver does not encompass all possible functionality such as calibration, but is well documented and easy to build on.

A feature of the CY8C201A0 is that its I²C address is not fixed - one can change it by using a specific sequence of commands. For some reason, many of the sensors I have gotten my hands on had their address set to `0x00`, or the I²C general call address. This is reflected in the driver's init function, `CY8_init()`. It will attempt to locate the sensor at address `0x23` (defined in the `CY8C201A0_CUSTOM_I2C_ADDRESS` macro) and if unsuccessful, send the address change command sequence to `0x00`. If you wish to use a different address, change the macro in question.

Note that the sensor requires a short wait period after a command is sent. Due to this, the `CY8_send_command()` function contains a blocking `HAL_Delay()` call.

This behavior unfortunately makes it a bit more cumbersome to adapt the driver to other hardware abstraction libraries, as the I²C interfacing is not isolated to the generic read and write functions. If you wish to adapt it, it will be necessary to work through all of the code.
