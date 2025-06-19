# Embedded Systems

An embedded system is a computer system designed to perform a specific task or set of tasks within a
larger mechanical or electrical system. Unlike general-purpose computers, embedded systems are
purpose-built and often operate in real-time. They are "embedded" because they are integrated into
the device they control and are not usually user-serviceable. They typically consist of:

- A microcontroller or microprocessor (CPU)
- Memory (RAM and ROM/Flash)
- Input/output interfaces
- Application-specific software

Examples:

- A microwave's control panel
- A smart thermostat

## Firmware

Firmware is the low-level software programmed into non-volatile memory (like flash ROM) that
directly controls hardware. It sits between the hardware and higher-level software. Firmware is
usually:

- Persistent (retained without power)
- Rarely updated (though some devices support firmware updates)
- Crucial for basic operation

In embedded systems, firmware is typically the main application software, written specifically for
the hardware and tightly coupled with it. Examples:

- The code that runs on your TV's remote control
- The BIOS/UEFI in a computer

# Device Drivers

A device driver abstracts the hardware details, providing a standardized interface to the operating
system.
