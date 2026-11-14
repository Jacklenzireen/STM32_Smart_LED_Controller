 STM32 Smart LED Controller

This is a beginner-level embedded systems project using the **STM32F103C8T6 Blue Pill** development board.

The goal is to develop practical STM32 programming skills using the **STM32 Hardware Abstraction Layer (HAL)** and real embedded systems techniques.

Hardware

- STM32F103C8T6 Blue Pill
- Onboard LED (PC13)
- Push button (PA0)

 Software and Tools

- STM32CubeIDE
- STM32CubeMX
- STM32 HAL Library
- GitHub

 Completed Lessons

 Lesson 1 – Basic LED Blinking
- Configured GPIO output.
- Controlled the onboard LED using HAL.
- Used `HAL_GPIO_TogglePin()`.
- Introduced `HAL_Delay()`.

 Lesson 2 – Push Button Controls LED
- Configured PA0 as GPIO input.
- Used `HAL_GPIO_ReadPin()`.
- Controlled the LED using a physical button.

 Lesson 2.1 – Button Debouncing
- Understood mechanical button bouncing.
- Implemented software debouncing.
- Used `HAL_GetTick()` to measure elapsed time.
- Prevented multiple detections from a single button press.

 Lesson 2.2 – Improved Button Debouncing
- Implemented button state detection.
- Used edge detection to identify button presses.
- Implemented LED toggling with stable button readings.

 Lesson 2.3 – Button Control Using External Interrupts (EXTI)
- Configured PA0 as an external interrupt.
- Enabled EXTI line 0 in NVIC.
- Implemented `HAL_GPIO_EXTI_Callback()`.
- Learned about interrupt handling and the `volatile` keyword.
- Used a flag to process button events in the main loop.

 Lesson 2.4 – STM32 Timer Interrupts
- Configured TIM2 using STM32CubeMX.
- Learned how Prescaler (PSC) and Auto-Reload Register (ARR) control timer intervals.
- Enabled TIM2 global interrupt through NVIC.
- Started the timer using `HAL_TIM_Base_Start_IT()`.
- Implemented `HAL_TIM_PeriodElapsedCallback()`.
- Controlled LED blinking using hardware timer interrupts instead of `HAL_Delay()`.

Timer Configuration:**

| Parameter | Value |
|---|---|
| Timer | TIM2 |
| Timer Clock | 36 MHz |
| Prescaler | 35999 |
| Counter Period | 499 |
| Interrupt Interval | 500 ms |

 Project Status

- [x] Basic LED blinking
- [x] Push button input
- [x] Software debouncing
- [x] Improved button state detection
- [x] External interrupts (EXTI)
- [x] STM32 timer interrupts
- [ ] PWM LED brightness control
- [ ] ADC
- [ ] UART communication
- [ ] I2C and SPI communication
- [ ] Advanced embedded systems projects

 Learning Objectives

- Develop practical STM32 programming skills.
- Understand GPIO, interrupts, timers and PWM.
- Learn how to use STM32 HAL functions.
- Write clean and reusable embedded C code.
- Build practical embedded systems projects.

 Future Improvements

- PWM-based LED brightness control.
- Sensor integration using ADC.
- UART communication.
- I2C and SPI peripheral communication.
- More advanced embedded firmware projects.

 Simulation

Hardware testing is pending. Simulation support will be explored in future development.

Author

Jack Mbembele Katende

Beginner Embedded Systems Developer
