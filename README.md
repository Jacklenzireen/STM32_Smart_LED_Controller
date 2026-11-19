 STM32 Smart LED Controller

This is a beginner-level embedded systems project using the **STM32F103C8T6 Blue Pill** development board.

The goal is to develop practical STM32 programming skills using the **STM32 Hardware Abstraction Layer (HAL)**, starting with basic LED control and gradually introducing more advanced GPIO features, external interrupts, timers, and PWM.

 Hardware

- STM32F103C8T6 Blue Pill development board
- Onboard LED connected to GPIO pin PC13
- Push button connected to PA0 (physical testing pending)
- External LED for PWM testing on PA6 (hardware testing pending)

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


What I learned:

- Configuring a GPIO pin as an output.
- Using `HAL_GPIO_TogglePin()` to change the LED state.
- Using `HAL_Delay()` to introduce a time delay.

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


What I learned:

- Configuring GPIO input and output pins.
- Using an internal pull-down resistor.
- Reading digital signals with `HAL_GPIO_ReadPin()`.
- Controlling an LED with `HAL_GPIO_WritePin()`.

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

Lesson 2.2 – Improved Button Debouncing and LED Toggle

Implemented an improved software debouncing technique using `HAL_GetTick()` and a 20-millisecond debounce interval.

The program detects changes in the raw button signal, waits until the signal remains stable for at least 20 milliseconds, and then accepts the new button state.

The LED toggles only once when a new button press is confirmed. Holding the button does not repeatedly toggle the LED.

Variables declared before the main while loop:


GPIO_PinState lastRawButtonState = GPIO_PIN_RESET;
GPIO_PinState stableButtonState = GPIO_PIN_RESET;

uint32_t lastDebounceTime = 0;
uint32_t debounceDelay = 20;

GPIO_PinState ledState = GPIO_PIN_SET;


Main program loop:


while (1)
{
    GPIO_PinState rawButtonState =
        HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0);

    if (rawButtonState != lastRawButtonState)
    {
        lastDebounceTime = HAL_GetTick();
        lastRawButtonState = rawButtonState;
    }

    if ((HAL_GetTick() - lastDebounceTime) >= debounceDelay)
    {
        if (rawButtonState != stableButtonState)
        {
            stableButtonState = rawButtonState;

            if (stableButtonState == GPIO_PIN_SET)
            {
                if (ledState == GPIO_PIN_SET)
                {
                    ledState = GPIO_PIN_RESET;
                }
                else
                {
                    ledState = GPIO_PIN_SET;
                }

                HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, ledState);
            }
        }
    }
}


What I learned:

- `HAL_GetTick()` returns the elapsed time in milliseconds.
- `uint32_t` stores an unsigned 32-bit integer.
- `!=` checks whether two values are different.
- `lastRawButtonState` remembers the previous raw button reading.
- `stableButtonState` stores the last confirmed button state.
- `lastDebounceTime` records when the raw button signal last changed.
- `debounceDelay` defines the 20-millisecond debounce interval.
- Rising-edge detection identifies a new button press (LOW to HIGH).
- The LED toggles once per confirmed press.

Build status: Successfully built in STM32CubeIDE with 0 errors and 0 warnings.

Physical hardware testing is still pending.

Note:The onboard PC13 LED is active-low. `GPIO_PIN_RESET` turns it ON, while `GPIO_PIN_SET` turns it OFF.

 Lesson 2.3 – Button Control Using EXTI Interrupts

Introduced external interrupts (EXTI) to detect button presses without continuously checking the PA0 input inside the main loop.

Configured PA0 as an external interrupt input using a rising-edge trigger and enabled the EXTI line 0 interrupt in the NVIC settings.

When the button is pressed, the interrupt callback records the event and sets a flag for the main program to process.

Variables:


volatile uint8_t buttonInterruptFlag = 0;
volatile uint32_t lastButtonInterruptTime = 0;

uint32_t buttonCheckTime = 0;

GPIO_PinState ledState = GPIO_PIN_SET;

#define BUTTON_DEBOUNCE_DELAY 20U


External interrupt callback:


void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
    if (GPIO_Pin == GPIO_PIN_0)
    {
        lastButtonInterruptTime = HAL_GetTick();
        buttonInterruptFlag = 1;
    }
}


What I learned:

- What external interrupts (EXTI) are and why they are useful.
- Configuring PA0 for an external interrupt.
- Enabling an interrupt through the NVIC.
- Understanding the role of `EXTI0_IRQHandler()`.
- Using `HAL_GPIO_EXTI_IRQHandler()` to handle the interrupt.
- Using `HAL_GPIO_EXTI_Callback()` to process the interrupt event.
- The purpose of the `volatile` keyword when sharing variables between interrupt and main code.
- Why interrupt callbacks should remain short and avoid blocking operations.

Build status: Successfully built in STM32CubeIDE with 0 errors and 0 warnings.

Physical hardware testing is still pending.

 Lesson 2.4 – STM32 Timer Interrupts

Introduced hardware timer interrupts using TIM2.

Configured TIM2 to generate an update interrupt every 500 milliseconds. The timer interrupt callback toggles the onboard LED connected to PC13.

Unlike `HAL_Delay()`, the timer interrupt allows the main program to continue executing while the timer operates independently.

TIM2 Configuration:

| Parameter | Value |
|---|---|
| Timer | TIM2 |
| Clock Source | Internal Clock |
| Timer Clock | 36 MHz |
| Prescaler (PSC) | 35999 |
| Auto-Reload Register (ARR) | 499 |
| Counter Mode | Up |
| Interrupt | Enabled |

Timer initialization and start:

c
MX_GPIO_Init();
MX_TIM2_Init();

HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, ledState);

HAL_TIM_Base_Start_IT(&htim2);


Timer interrupt callback:

```c
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
    if (htim->Instance == TIM2)
    {
        HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
    }
}


Timer calculation:

- Timer counter frequency = 36 MHz / (35999 + 1) = 1 kHz.
- Timer update interval = (499 + 1) / 1000 = 500 milliseconds.

What I learned:

- Configuring STM32 hardware timers.
- Understanding the prescaler (PSC) and auto-reload register (ARR).
- Calculating timer interrupt intervals.
- Starting a timer with `HAL_TIM_Base_Start_IT()`.
- Using `HAL_TIM_PeriodElapsedCallback()` to process timer events.
- Understanding the difference between blocking delays and timer interrupts.

Build status: Successfully built in STM32CubeIDE with 0 errors and 0 warnings.

Physical hardware testing is still pending.

Lesson 2.5 – PWM LED Brightness Control

Introduced Pulse Width Modulation (PWM) using TIM3 to control LED brightness.

Configured TIM3 Channel 1 as a PWM output on PA6.

The PWM duty cycle is adjusted using a reusable `setLedBrightness()` function. This allows the brightness to be controlled through percentage values from 0% to 100%.

TIM3 Configuration:

| Parameter | Value |
|---|---|
| Timer | TIM3 |
| PWM Channel | TIM3_CH1 |
| Output Pin | PA6 |
| Timer Clock | 36 MHz |
| Prescaler (PSC) | 35 |
| Auto-Reload Register (ARR) | 999 |
| Initial Pulse | 500 |
| PWM Mode | PWM Mode 1 |
| PWM Frequency | 1 kHz |

Starting PWM:


HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_1);

setLedBrightness(25);


Brightness control function:


void setLedBrightness(uint8_t percentage)
{
    if (percentage > 100)
    {
        percentage = 100;
    }

    uint32_t pulse = ((uint32_t)(__HAL_TIM_GET_AUTORELOAD(&htim3) + 1U)
                      * percentage) / 100U;

    __HAL_TIM_SET_COMPARE(&htim3, TIM_CHANNEL_1, pulse);
}


Examples:


setLedBrightness(0);    // 0% brightness
setLedBrightness(25);   // 25% brightness
setLedBrightness(50);   // 50% brightness
setLedBrightness(75);   // 75% brightness
setLedBrightness(100);  // 100% brightness
```

PWM calculation:

- Timer counter frequency = 36 MHz / (35 + 1) = 1 MHz.
- PWM frequency = 1 MHz / (999 + 1) = 1 kHz.
- Duty cycle = Compare Value / (ARR + 1) × 100.

For example, a compare value of 750 produces a 75% duty cycle.

What I learned:

- Understanding Pulse Width Modulation (PWM).
- Configuring TIM3 for PWM generation.
- Understanding PWM frequency and duty cycle.
- Using `HAL_TIM_PWM_Start()` to start PWM output.
- Using `__HAL_TIM_SET_COMPARE()` to change the PWM duty cycle.
- Creating a reusable function to control LED brightness.
- Understanding how timer configuration determines PWM frequency.

Build status: Successfully built in STM32CubeIDE with 0 errors and 0 warnings.

Physical hardware testing is still pending. An external LED and a current-limiting resistor will be required to test PWM output on PA6.

 Project Status

- [x] STM32CubeMX GPIO configuration
- [x] PC13 configured as GPIO output
- [x] Basic LED blinking using STM32 HAL
- [x] PA0 configured as GPIO input with pull-down
- [x] Push-button LED control implemented
- [x] Basic software button debouncing implemented
- [x] Improved software button debouncing implemented
- [x] LED toggle on confirmed button press
- [x] External interrupt (EXTI) configured and implemented
- [x] TIM2 timer interrupt configured and implemented
- [x] TIM3 PWM generation configured and implemented
- [x] Reusable PWM brightness control function implemented
- [x] Project successfully built in STM32CubeIDE
- [ ] Physical hardware testing
- [ ] Wokwi online simulation

 Learning Objectives

Through this project, I am developing practical skills in:

- Embedded C programming
- STM32 microcontrollers
- STM32 HAL libraries
- GPIO input and output configuration
- Digital signal reading
- Software button debouncing
- Button press and edge detection
- External interrupts (EXTI)
- NVIC interrupt configuration
- Hardware timers and timer interrupts
- Pulse Width Modulation (PWM)
- Timer prescaler and auto-reload configuration
- PWM frequency and duty-cycle calculations
- Reusable embedded C functions
- Debugging and version control using GitHub

 Future Improvements

- Test the button and LED functionality on physical STM32 hardware.
- Test PWM brightness control using an external LED.
- Explore additional STM32 timer features.
- Introduce ADC and sensor reading.
- Implement UART communication.
- Develop more advanced embedded systems projects.

 Simulation

Wokwi simulation link: Coming soon.

 Author

Jack Mbembele Katende

Beginner Embedded Systems Developer
