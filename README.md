Lesson 2.6 – Non-blocking LED Control

Introduced non-blocking programming using `HAL_GetTick()` to manage multiple tasks without stopping the main program loop.

The onboard LED connected to PC13 blinks automatically every 500 milliseconds without using `HAL_Delay()`.

The push button connected to PA0 uses an external interrupt (EXTI) to detect button presses. Each confirmed press increases the PWM brightness of an external LED connected to PA6 by 25%.

Non-blocking LED timing:


if ((HAL_GetTick() - lastLedToggleTime) >= LED_BLINK_INTERVAL)
{
    lastLedToggleTime = HAL_GetTick();

    HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
}


Button-controlled PWM brightness:**


if (HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0) == GPIO_PIN_SET)
{
    pwmBrightness += 25;

    if (pwmBrightness > 100)
    {
        pwmBrightness = 0;
    }

    setLedBrightness(pwmBrightness);
}


What I learned:

- Understanding blocking and non-blocking programming.
- Using `HAL_GetTick()` to manage elapsed time.
- Implementing periodic tasks without `HAL_Delay()`.
- Managing multiple tasks in the main loop.
- Combining EXTI interrupts with non-blocking timing.
- Controlling PWM brightness using button presses.
- Using separate GPIO pins for different functions.

Build status:** Successfully built in STM32CubeIDE with 0 errors and 0 warnings.

Physical testing:** Pending.

Note: PC13 handles automatic blinking, while PA6 handles PWM brightness control. These functions have separate responsibilities.








-
