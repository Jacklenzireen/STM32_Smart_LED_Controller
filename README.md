Lesson 3.2 – I²C Communication and Device Scanner

Objective: Configure the STM32F103C8T6 to communicate with external I²C devices and scan the I²C bus for connected devices.

Configuration:
- Peripheral: I2C1
- SCL: PB6
- SDA: PB7
- Communication speed: 100 kHz
- Addressing mode: 7-bit
- UART output: USART1 (115200 baud)

Implementation:
- Configured I²C1 using STM32CubeMX.
- Implemented an I²C device scanner using `HAL_I2C_IsDeviceReady()`.
- Scanned device addresses from `0x08` to `0x77`.
- Displayed detected device addresses through UART.
- Used `snprintf()` and `HAL_UART_Transmit()` for serial reporting.

Build Status: Successful – 0 errors, 0 warnings.


Key Learning: Understanding I²C master communication, 7-bit device addressing, device detection, and UART-based debugging.
