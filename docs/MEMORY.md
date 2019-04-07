# Memory structures on ZLadder controller

This document intends to specify the memory structures and formats used on ZLadder controller for multiple purposes.

## EEPROM

Being the only non-volatile structure on the arduino controller, the EEPROM memory is the base for all information on controller that needs to be persisted across device reboots. This information includes:
- Device UUID: Unique identifier for the ZLadder controller. It intends to be setted only once on first configuration of Zladder controller and remains forever.
- Version UUID: Unique identifier for the program version and configuration currently active on Zladder controller. Each time any of these informations change, a new UUID has to be used.
- Variables: Variables that have to be persisted across device reboots. Most variables will be stored on volatile memory, but some states need to be preserved for a good functioning of the controller.
- Configuration: General configuration needed for initial setup of the controller. Actually only includes port configurations
- Program: Defines the logics to be followed by the ZLadder engine. To increase the program size supported by the controller it is bit optimized as in the program bit optimization definition.

For the ZLadder controller, the entire EEPROM is reserved for these previously defined informations and is stored on memory in the following sequence starting from byte 0:
```
|DEVICE-UUID|VERSION-UUID|VARIABLES|CONFIGURATION|PROGRAM|
```

The size of each area is defined as follows:
- DEVICE-UUID and VERSION-UUID: 16 bytes as the standard definition of UUIDs
- VARIABLES: Varies according to available memory of each board type
- CONFIGURATION: Varies according to the number of available ports on each board type
- PROGRAM: Varies according to available memory of each board type, consuming the remaining memory

### Configuration

The configuration is a list of available ports and their states at the device initialization. 1 byte is used for each port and actually allows the following values:
- Output: 0x00
- Input: 0x01
- Output (PWM): 0x02

The remaining values on this configuration of ports can be used for future features.

## RAM


