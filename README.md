STM32 Smart LED Controller

This is a beginner-level embedded systems project using the **STM32F103C8T6 Blue Pill** development board.

The goal is to develop practical STM32 programming skills using the STM32 Hardware Abstraction Layer (HAL), starting with basic LED control and gradually introducing more advanced GPIO features, software debouncing, and external interrupts.

 Hardware

- STM32F103C8T6 Blue Pill development board
- Onboard LED connected to GPIO pin PC13
- External push button connected to PA0 (physical testing pending)

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

The program continuously reads the button state and controls the onboard LED accordingly.


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


Initialize the LED as OFF after GPIO initialization:


MX_GPIO_Init();

HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET);


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

Lesson 2.3 – Button Control Using External Interrupts (EXTI)

Introduced STM32 external interrupts (EXTI) to detect button presses without continuously polling the PA0 input pin.

Configured PA0 as an external interrupt input using rising-edge detection and an internal pull-down resistor.

When the button is pressed, the input changes from LOW to HIGH, triggering the EXTI0 interrupt.

The STM32 HAL interrupt callback detects the event and records the interrupt timestamp. The main program then checks the 20-millisecond debounce interval and verifies that the button is still pressed before toggling the LED.

Global variables declared before the main function:


volatile uint8_t buttonInterruptFlag = 0;
volatile uint32_t lastButtonInterruptTime = 0;

uint32_t buttonCheckTime = 0;

GPIO_PinState ledState = GPIO_PIN_SET;

#define BUTTON_DEBOUNCE_DELAY 20U


Initialize the LED as OFF after GPIO initialization:


MX_GPIO_Init();

HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, ledState);


Main program loop:


while (1)
{
    if (buttonInterruptFlag == 1)
    {
        buttonCheckTime = HAL_GetTick();

        if ((buttonCheckTime - lastButtonInterruptTime) >= BUTTON_DEBOUNCE_DELAY)
        {
            buttonInterruptFlag = 0;

            if (HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0) == GPIO_PIN_SET)
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

- EXTI stands for External Interrupt/Event Controller.
- PA0 can generate an external interrupt when its signal changes.
- Rising-edge detection identifies a LOW-to-HIGH transition.
- `EXTI0_IRQHandler()` handles the interrupt for PA0.
- `HAL_GPIO_EXTI_IRQHandler()` processes the interrupt through the STM32 HAL.
- `HAL_GPIO_EXTI_Callback()` allows the application to respond to the interrupt.
- NVIC (Nested Vectored Interrupt Controller) manages interrupt handling.
- `volatile` tells the compiler that a variable may change unexpectedly, including inside an interrupt callback.
- Interrupt flags allow the main program to process events recorded by an interrupt.
- `BUTTON_DEBOUNCE_DELAY` defines the 20-millisecond debounce interval.
- Long delays should be avoided inside interrupt callbacks.

GPIO configuration:

- PA0 configured as GPIO external interrupt with rising-edge detection.
- Internal pull-down resistor enabled.
- EXTI line 0 interrupt enabled in NVIC.
- PC13 configured as GPIO output.

Build status: Successfully built in STM32CubeIDE with 0 errors and 0 warnings.

Physical hardware testing is still pending.

 Project Status

- [x] STM32CubeMX GPIO configuration
- [x] PC13 configured as GPIO output
- [x] Basic LED blinking using STM32 HAL
- [x] PA0 configured as GPIO input with pull-down
- [x] Push-button LED control implemented
- [x] Basic software button debouncing implemented
- [x] Improved software button debouncing implemented
- [x] LED toggle on confirmed button press
- [x] PA0 configured for EXTI0 rising-edge detection
- [x] NVIC EXTI0 interrupt enabled
- [x] HAL external interrupt callback implemented
- [x] Interrupt-based button handling implemented
- [x] Project successfully built in STM32CubeIDE
- [ ] Physical hardware testing
- [ ] Wokwi online simulation
- [ ] Advanced interrupt debouncing and validation

 Learning Objectives

Through this project, I am developing practical skills in:

- Embedded C programming
- STM32 microcontrollers
- GPIO input and output configuration
- STM32 HAL libraries
- Digital signal reading
- Software debouncing
- Button press detection
- External interrupts (EXTI)
- NVIC interrupt management
- Interrupt callbacks
- Debugging and version control using GitHub

Future Improvements

- Test the button and LED functionality on physical STM32 hardware.
- Explore more advanced interrupt debouncing techniques.
- Add different LED blinking patterns.
- Explore STM32 timers and timer interrupts.
- Introduce more GPIO peripherals.
- Build additional STM32 embedded systems projects.

 Simulation

Wokwi simulation link: Coming soon.

 Author

Jack Mbembele Katende

Beginner Embedded Systems Developer


Note: The onboard PC13 LED is active-low. `GPIO_PIN_RESET` turns it ON, while `GPIO_PIN_SET` turns it OFF.
