 Lesson 3.1 – ADC and UART Sensor Monitoring

 Project Overview

In this lesson, the STM32 Smart LED Controller was extended with ADC (Analog-to-Digital Converter) and UART (Universal Asynchronous Receiver/Transmitter) functionality.

The goal was to learn how to read an analog input, convert the ADC reading into a voltage value, and transmit the information to a computer through serial communication.

 Hardware Configuration

| Peripheral | Pin | Configuration |
|---|---|---|
| Onboard LED | PC13 | GPIO Output |
| Push Button | PA0 | GPIO External Interrupt, Rising Edge |
| PWM Output | PA6 | TIM3 Channel 1 |
| Analog Input | PA1 | ADC1 Channel 1 |
| UART TX | PA9 | USART1 Transmit |
| UART RX | PA10 | USART1 Receive |

ADC Configuration

- ADC Peripheral: ADC1
- Channel: ADC Channel 1
- Input Pin: PA1
- Resolution: 12 bits
- ADC Reading Range: 0–4095
- Conversion Mode: Single Conversion
- Sampling Time: 55.5 ADC cycles
- ADC Clock: 9 MHz
- Reference Voltage Assumption: 3.3 V

The ADC converts the analog input voltage into a digital value.

The voltage is calculated using:

`Voltage = (ADC Value × 3300) / 4095`

The result is calculated in millivolts to avoid relying on floating-point formatting.

UART Configuration

- Peripheral: USART1
- Baud Rate: 115200
- Word Length: 8 bits
- Parity: None
- Stop Bits: 1
- Mode: TX and RX
- Flow Control: None

The STM32 sends the ADC reading and calculated voltage through UART every 250 milliseconds.

Example of expected serial output:


ADC Value: 2048 | Voltage: 1.650 V
ADC Value: 2048 | Voltage: 1.650 V
ADC Value: 2048 | Voltage: 1.650 V


These are illustrative values, not measurements from physical hardware.

 System Clock Configuration

The system clock was configured using the internal High-Speed Internal (HSI) oscillator and PLL.

- HSI: 8 MHz
- PLL Input: HSI / 2 = 4 MHz
- PLL Multiplier: ×9
- System Clock: 36 MHz
- AHB Clock: 36 MHz
- APB1 Clock: 36 MHz
- APB2 Clock: 36 MHz
- ADC Clock: 9 MHz

 Combined Project Functionality

The project now combines several STM32 peripherals:

1. GPIO: Controls the onboard LED and reads the push button.
2. EXTI Interrupts: Detects button presses through an external interrupt.
3. TIM2: Generates an interrupt every 500 milliseconds to blink the onboard LED.
4. TIM3 PWM: Controls LED brightness through PA6.
5. ADC1: Reads analog input voltage through PA1.
6. USART1: Transmits ADC readings and calculated voltage to a computer.

The push button increases PWM brightness in 25% increments, cycling through 25%, 50%, 75%, 100%, and 0%.

 Build Status

- STM32CubeIDE build: Successful
- Compiler Errors: 0
- Compiler Warnings: 0

 Hardware Testing Status

The firmware has been successfully compiled in STM32CubeIDE.

Physical testing is still pending because the STM32 development board and external components are not currently available.

 Skills Practiced

- STM32 ADC configuration and conversion
- Analog voltage calculation
- UART configuration and serial communication
- STM32 HAL functions
- Timer interrupts
- PWM signal generation
- External interrupts
- Combining multiple peripherals in one embedded application
- System clock configuration
