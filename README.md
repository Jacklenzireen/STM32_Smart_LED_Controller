# STM32 Smart LED Controller

## Project Overview

This project is a beginner-level embedded systems project using the STM32F103C8T6 Blue Pill development board.

The goal is to control the onboard LED connected to GPIO pin PC13 using the STM32 Hardware Abstraction Layer (HAL).

The LED switches ON and OFF every 500 milliseconds.

## Hardware

- STM32F103C8T6 Blue Pill development board
- Onboard LED connected to PC13

## Software and Tools

- STM32CubeIDE
- STM32CubeMX
- Embedded C
- STM32 HAL Library
- GitHub

## How It Works

The project uses the STM32 HAL functions to control the LED.

```c
MX_GPIO_Init();

while (1)
{
    HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
    HAL_Delay(500);
}
```

### Code Explanation

- `MX_GPIO_Init()` initializes the GPIO configuration.
- `while (1)` continuously repeats the instructions.
- `HAL_GPIO_TogglePin()` switches the LED between ON and OFF.
- `HAL_Delay(500)` creates a 500-millisecond delay.

## Project Status

- [x] STM32CubeMX GPIO configuration
- [x] PC13 configured as GPIO output
- [x] LED blinking logic implemented using STM32 HAL
- [x] Project successfully built in STM32CubeIDE
- [ ] Wokwi online simulation
- [ ] Additional embedded systems projects

## Learning Objectives

Through this project, I am developing practical skills in:

- Embedded C programming
- STM32 microcontrollers
- GPIO configuration
- STM32 HAL libraries
- Hardware and software integration

## Future Improvements

- Add a push button to control the LED.
- Implement different LED blinking patterns.
- Explore timers and interrupts.
- Build more STM32 embedded systems projects.

## Simulation

Wokwi simulation link: *Coming soon.*

## Author

Jack Mbembele Katende

Beginner Embedded Systems Developer
