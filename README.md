 Lesson 3.2 – I²C Communication and Device Scanner

In this lesson, I implemented I²C communication using the STM32 HAL library on the STM32F103C8T6 Blue Pill development board.

Objectives:
- Understand the basic principles of I²C communication.
- Configure I²C1 using STM32CubeMX.
- Configure PB6 as I2C1_SCL and PB7 as I2C1_SDA.
- Set the I²C communication speed to 100 kHz (Standard Mode).
- Configure 7-bit device addressing.
- Implement an I²C device scanner using `HAL_I2C_IsDeviceReady()`.
- Display detected I²C device addresses through USART1.

I²C Configuration:

| Parameter | Configuration |
|---|---|
| Peripheral | I2C1 |
| SCL Pin | PB6 |
| SDA Pin | PB7 |
| Clock Speed | 100 kHz |
| Addressing Mode | 7-bit |
| Duty Cycle | Standard Mode |

Implementation:

The `I2C_Scan()` function checks I²C addresses from `0x08` to `0x77` to identify connected devices.

When a device responds, its address is displayed through UART using `HAL_UART_Transmit()`.

The implementation uses:
- `HAL_I2C_IsDeviceReady()` – checks whether an I²C device responds.
- `HAL_UART_Transmit()` – sends scan results through USART1.
- `snprintf()` – formats device addresses for UART output.

Build and Testing:

- Build Status: Successful
- Errors: 0
- Warnings: 0
- Physical I²C communication testing: Pending (hardware not yet available).

Result:

Successfully implemented and compiled an I²C device scanner using STM32 HAL, integrating I²C communication with the existing UART monitoring functionality.
