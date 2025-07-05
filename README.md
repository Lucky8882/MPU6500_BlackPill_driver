# STM32 Black Pill MPU6500 Driver Project

![Block Diagram](docs/block_diagram.png)

## Overview

This project provides a **clean, layered, production-quality C driver** for the MPU6500 6-axis IMU sensor, running on an STM32 Black Pill (STM32F4 series) board.  
It demonstrates robust I2C communication, register-level access, and high-level sensor data acquisition, with all data output over UART for easy debugging and integration.

---

## Features

- **No third-party MPU libraries:** All code is original and register-level.
- **Layered architecture:**  
  - Low-level: I2C register access (middleware)
  - High-level: Sensor configuration and data reading (driver)
- **Easy integration:** Just add your I2C/UART handles and call the API.
- **UART output:** Sensor data is printed for easy monitoring.
- **Extensible:** Add more features (temperature, interrupts, DMP, etc.) as needed.

---

## Block Diagram

![MPU6500 + STM32 Black Pill Block Diagram](docs/block_diagram.png)

*Replace the above image with your own block diagram or hardware photo for clarity.*

---

## Hardware Setup

- **MCU:** STM32F4 Black Pill (e.g., STM32F401CCU6)
- **Sensor:** MPU6500 (3.3V logic)
- **Connections:**
    - `SCL` → STM32 I2C SCL pin (e.g., PB6)
    - `SDA` → STM32 I2C SDA pin (e.g., PB7)
    - `VCC` → 3.3V
    - `GND` → GND
    - `UART TX` (STM32, e.g., PA2) → USB-UART adapter RX (for serial output)
    - `UART RX` (STM32, e.g., PA3) → USB-UART adapter TX (optional)

---

## Project Structure
Core/
├── Inc/
│ ├── mpu6500.h
│ ├── mpu6500_ll.h
│ └── mpu6500_reg.h
├── Src/
│ ├── mpu6500.c
│ ├── mpu6500_ll.c
│ └── main.c


- **mpu6500_reg.h:** Register addresses and bitfields for MPU6500.
- **mpu6500_ll.h/c:** Low-level I2C register access using STM32 HAL.
- **mpu6500.h/c:** High-level driver for sensor configuration and data reading.
- **main.c:** Example application: initializes everything, reads sensor data, prints over UART.

---

## Usage

1. **Clone the repository:**
    ```sh
    git clone https://github.com/yourusername/yourrepo.git
    ```

2. **Open in STM32CubeIDE.**

3. **Configure I2C and UART:**
    - Enable I2C (e.g., I2C1 on PB6/PB7).
    - Enable UART (e.g., USART2 on PA2/PA3).
    - Set UART baud rate to 115200.

4. **Build and flash the project** to your Black Pill board.

5. **Connect a USB-UART adapter** to your PC and open a serial terminal at 115200 baud.

6. **Observe output:**  
   The terminal will display real-time accelerometer and gyroscope data.

---

## Example Output

MPU6500 Example Start
WHO_AM_I: 0x70
MPU6500 detected!
ACCEL: X=123 Y=-456 Z=789 | GYRO: X=12 Y=34 Z=-56
ACCEL: X=124 Y=-455 Z=790 | GYRO: X=13 Y=33 Z=-55
...


---

## Code Highlights

- **Low-level I2C access:**
    ```c
    HAL_StatusTypeDef mpu6500_write_reg(I2C_HandleTypeDef *hi2c, uint8_t reg, uint8_t data);
    HAL_StatusTypeDef mpu6500_read_reg(I2C_HandleTypeDef *hi2c, uint8_t reg, uint8_t *data);
    ```

- **High-level API:**
    ```c
    HAL_StatusTypeDef mpu6500_init(I2C_HandleTypeDef *hi2c);
    HAL_StatusTypeDef mpu6500_read_accel(I2C_HandleTypeDef *hi2c, mpu6500_axis_t *accel);
    HAL_StatusTypeDef mpu6500_read_gyro(I2C_HandleTypeDef *hi2c, mpu6500_axis_t *gyro);
    ```

- **Simple main loop:**
    ```c
    while (1) {
        mpu6500_read_accel(&hi2c1, &accel);
        mpu6500_read_gyro(&hi2c1, &gyro);
        uart_printf("ACCEL: X=%d Y=%d Z=%d | GYRO: X=%d Y=%d Z=%d\r\n",
            accel.x, accel.y, accel.z, gyro.x, gyro.y, gyro.z);
        HAL_Delay(200);
    }
    ```

---
## Credits

- [MPU-6500 Datasheet](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6500-Datasheet1.pdf)
- STM32 HAL and STM32CubeIDE
---
