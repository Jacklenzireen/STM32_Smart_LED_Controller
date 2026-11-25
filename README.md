 Lesson 3.3 – SPI Communication

Implemented SPI communication using the STM32F103C8T6 Blue Pill and STM32 HAL.

SPI Configuration:
- SPI1 configured as Full-Duplex Master.
- PA5 – SPI1_SCK (Clock).
- PA6 – SPI1_MISO (Master In Slave Out).
- PA7 – SPI1_MOSI (Master Out Slave In).
- PA4 – Manual Chip Select (CS).
- 8-bit data size, MSB first.
- Clock polarity: Low.
- Clock phase: First Edge.
- SPI clock frequency: 4.5 MHz.

Implementation:
- Initialized SPI1 using `MX_SPI1_Init()`.
- Configured PA4 as a manual Chip Select output.
- Set CS HIGH when the SPI device is not selected.
- Set CS LOW before transmission and HIGH after transmission.
- Used `HAL_SPI_Transmit()` to transmit four test bytes: `0xA5`, `0x5A`, `0x01`, and `0x02`.
- Scheduled transmissions every second using `HAL_GetTick()` without using `HAL_Delay()`.

Validation:
- Project builds successfully with 0 errors and 0 warnings.
