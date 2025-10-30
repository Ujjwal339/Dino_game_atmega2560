# 🦖 LCD Dino Game using AVR Microcontroller

An interactive **Dino Game** built using an **ATmega2560** microcontroller and a **16x2 LCD display**, inspired by the classic Chrome Dino game. The game uses custom CGRAM characters for animation and real-time obstacle detection to make gameplay smooth and fun.

---

## 🎮 Project Overview

This project implements a simple yet engaging game on an embedded system platform.  
The player controls a dinosaur that must **jump over obstacles** to survive as long as possible.  
The score increases with time, and collisions reset the game.

### 🧩 Key Features
- **Custom LCD Graphics:** Designed unique characters for the Dino and obstacles using LCD CGRAM.
- **Dynamic Gameplay:** Real-time obstacle spawning and collision detection.
- **Smooth Animation:** Optimized LCD refresh logic for flicker-free display.
- **Score System:** Continuously updating score displayed on-screen.
- **Low Power Consumption:** Efficient code and timing control using AVR timers.

---

## ⚙️ Hardware Requirements
| Component | Description |
|------------|--------------|
| **Microcontroller** | ATmega2560 (tested on Fire Bird V platform) |
| **Display** | 16x2 Character LCD |
| **Buttons/Switches** | For jump and reset controls |
| **Power Source** | 5V regulated power supply |
| **Miscellaneous** | Connecting wires, Breadboard/PCB setup |

---

## 🧠 Software & Tools Used
- **Programming Language:** Embedded C  
- **IDE:** Atmel Studio / Microchip Studio  
- **Compiler:** AVR-GCC  
- **Simulation (optional):** Proteus  
- **Flashing Tool:** AVRDUDE  
- **Libraries Used:** Standard AVR I/O and delay libraries  

---

## 🕹️ Game Controls
- **Jump Button:** Makes the Dino jump over obstacles.  
- **Reset Button:** Restarts the game after collision.  

---

