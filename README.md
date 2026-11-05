STM32 Smart LED Controller

This is a beginner-level embedded systems project using the **STM32F103C8T6 Blue Pill** development board.

The goal is to develop practical STM32 programming skills using the STM32 Hardware Abstraction Layer (HAL), starting with basic LED control and gradually introducing more advanced GPIO features.

Hardware

- STM32F103C8T6 Blue Pill development board
- Onboard LED connected to GPIO pin PC13
- External push button (physical testing pending)

 Software and Tools

- STM32CubeIDE
- STM32CubeMX
- Embedded C
- STM32 HAL Library
- GitHub

 Completed Lessons

Lesson 1 – Basic LED Blinking

The onboard LED connected to PC13 switches ON and OFF every 500 milliseconds.


MX_GPIO_Init();

while (1)
{
    HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
    HAL_Delay(500);
}


 Lesson 2 – Push Button Controls LED

Configured PA0 as a digital input with an internal pull-down resistor and PC13 as a digital output.

The program reads the button state and controls the onboard LED.


while (1)
{
    if (HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0) == GPIO_PIN_SET)
    {
        HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET);
    }
    else
    {
        HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET);
    }

    HAL_Delay(50);
}


 Lesson 2.1 – Software Button Debouncing

Implemented a basic software debouncing technique to reduce the effect of unwanted electrical signals produced by mechanical button contacts.

The program reads the button, waits 20 milliseconds, and reads it again. It accepts the reading only if both readings match.


while (1)
{
    GPIO_PinState buttonState =
        HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0);

    HAL_Delay(20);

    if (buttonState == HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0))
    {
        if (buttonState == GPIO_PIN_SET)
        {
            HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET);
        }
        else
        {
            HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET);
        }
    }

    HAL_Delay(50);
}


What I learned:

- `GPIO_PinState` stores a GPIO pin reading.
- `HAL_GPIO_ReadPin()` reads the button state.
- `HAL_Delay(20)` waits 20 milliseconds.
- `==` compares the first and second readings.
- `HAL_GPIO_WritePin()` controls the onboard LED.

This is a basic two-reading debounce implementation. Physical hardware testing is still pending.

Note: The onboard PC13 LED is active-low. `GPIO_PIN_RESET` turns it ON, while `GPIO_PIN_SET` turns it OFF.

Project Status

- [x] STM32CubeMX GPIO configuration
- [x] PC13 configured as GPIO output
- [x] Basic LED blinking using STM32 HAL
- [x] PA0 configured as GPIO input with pull-down
- [x] Push-button LED control implemented
- [x] Basic software button debouncing implemented
- [x] Project successfully built in STM32CubeIDE
- [ ] Physical hardware testing
- [ ] Wokwi online simulation
- [ ] STM32 timers and interrupts

 Learning Objectives

Through this project, I am developing practical skills in:

- Embedded C programming
- STM32 microcontrollers
- GPIO input and output configuration
- STM32 HAL libraries
- Digital signal reading
- Software debouncing
- Debugging and version control using GitHub

 Future Improvements

- Improve the button debouncing implementation.
- Add different LED blinking patterns.
- Explore STM32 timers and interrupts.
- Introduce more GPIO peripherals.
- Build additional STM32 embedded systems projects.

 Simulation

Wokwi simulation link: Coming soon.

 Author

Jack Mbembele Katende

Beginner Embedded Systems Developer
