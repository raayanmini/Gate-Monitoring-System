# Gate Monitoring System – STM32F401 (Raayan Mini)

A **Gate Monitoring System** built on **STM32F401RBT6 (Raayan Mini)**.  
The system automatically detects **entry/exit at a gate**, plays **time-based greetings**, maintains a **people count**, and helps ensure the **gate is properly closed** so that street animals or unauthorized persons cannot easily enter when someone forgets to close the gate.

---

## 🧩 Problem Statement

At many institutes, hostels and apartments:

- People open the gate and **do not close it properly**  
- There is **no track** of how many people entered or exited  
- No **automatic greeting** or time-based behavior  
- A half-open gate can allow **street animals or intruders** to walk inside  

This project aims to provide a small embedded solution that:

- Detects **entry** and **exit** events using sensors  
- Keeps a **running people count** on an LCD  
- Plays **Good Morning / Afternoon / Evening / Thank You** messages via an external audio controller (UART)  
- Uses a **gate feedback sensor** to support proper gate closing

---

## 🛠 Hardware Overview

- **MCU Board:** Raayan Mini – STM32F401RBT6 (LQFP64)  
- **Display:** 16x2 HD44780-compatible monochrome LCD (4-bit mode)  
- **RTC:** DS1307 over I²C (device address `0x68`) for time & date  
- **EEPROM:** I²C (device address `0x50`) to store time slots and volume  
- **Sensors:** Magnetic / limit sensors for gate position and entry/exit detection  
- **Buttons:** 3 push buttons for configuration (increment, decrement, next)  
- **Status LEDs:** Red / Green for basic status indication  
- **UART:** USART2 to gate / audio controller for greetings and control frames  

---

## 🔌 Key Pin Connections (STM32F401)

| Function                 | STM32 Pin(s) | Notes                                   |
|--------------------------|--------------|-----------------------------------------|
| LCD Data (D4–D7)         | PB0–PB3      | 4-bit data bus                          |
| LCD RS                   | PB4          | Register select                         |
| LCD EN                   | PB8          | Enable                                  |
| Gate feedback sensor     | PA0          | Open/close status input (with pull-up)  |
| Additional sensor input  | PC0          | Entry/exit or auxiliary sensor          |
| UP_SW – INC              | PC8          | Increment value                         |
| DN_SW – DEC              | PC9          | Decrement value                         |
| ENT_SW – NEXT            | PC10         | Next/confirm field                      |
| Status LED (Red)         | PB13         | Error / gate not closed indication      |
| Status LED (Green)       | PB14         | Normal / OK indication                  |
| USART2 TX / RX           | PA2 / PA3    | To gate / audio controller              |

RTC (`0x68`) and EEPROM (`0x50`) share the same **I²C bus** of the STM32F4.

---

## ✨ Main Features

- Displays **current time and date** on a 16x2 LCD  
- Detects **entry and exit** using magnetic / limit sensors  
- Maintains and shows **total people count**  
- Sends different **greeting commands** over UART based on time slot:
  - Morning / Afternoon / Evening greetings  
  - “Thank You” for exit  
- Uses a **gate feedback sensor** to assist gate closing logic  
- Simple LCD menu with 3 buttons to:
  - Set **RTC time & date**  
  - Configure **morning / afternoon / evening** time ranges  
  - Set **volume level (0–30)** stored in EEPROM  

---

## 📂 Important Source Files

- `task.c` – Main gate logic, greetings, people count, UART frames  
- `config.c` – RTC time & date setting (LCD + buttons)  
- `system_config.c` – Time slot and volume configuration, helper functions  
- `LCD.c` – 16x2 LCD driver (4-bit mode)  
- `magnetic.c` – Sensor and LED GPIO/EXTI configuration  
- `SysTickTimer.c` – Delay and timing using SysTick  
- `itoa.c` – Small integer-to-string helper for LCD printing  

---

## 🛠 Build & Run (Keil µVision)

1. Download the repository as .zip
2. Extract the .zip file. 
3. Open the `.uvprojx` project file in **Keil µVision 5**.  
4. Select device **STM32F401RBTx** and make sure required Keil Packs are installed.  
5. Build the project to generate the firmware image.  
6. Flash the firmware to the **Raayan Mini** board using ST-Link / SWD.  

On reset, the system shows a **Gate Monitoring** welcome message, then displays **time and date**, and automatically handles **greetings, counting and gate closing support** when the sensors detect entry or exit.

## 📸 Demo

[![Watch the Demo Video](https://img.youtube.com/vi/IffCd3tEr48/maxresdefault.jpg)](https://www.youtube.com/watch?v=IffCd3tEr48)

> 🎥 Click the image to watch the Smart Weather Monitor System in action!
