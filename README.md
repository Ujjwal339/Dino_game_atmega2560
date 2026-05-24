# LCD Dino Game — ATmega2560 Bare-Metal Embedded C

> Chrome's offline Dino game, rebuilt from scratch on a 16×2 LCD using bare-metal AVR programming — no Arduino libraries, no OS, just registers.  
> **ATmega2560 | Embedded C | AVR-GCC | Custom CGRAM Graphics | Hardware Timers | GPIO Interrupts**

---

## Overview

This project implements a fully playable Dino Jump game on an ATmega2560 microcontroller driving a 16×2 character LCD — built entirely in bare-metal Embedded C without any Arduino abstraction layer.

Every component — LCD communication, character animation, obstacle movement, collision detection, score tracking, and display refresh — is handled at the hardware register level using AVR timers and GPIO interrupts.

---

## Demo
<img width="1263" height="1565" alt="WhatsApp Image 2026-05-23 at 21 13 32" src="https://github.com/user-attachments/assets/c07381e9-9f2e-4a88-b361-15ad9e96acc9" />
<img width="1263" height="1563" alt="WhatsApp Image 2026-05-23 at 21 13 19" src="https://github.com/user-attachments/assets/96f4f6d5-73d5-4004-80f5-842a34fcce52" />


---

## Features

- **Custom CGRAM Graphics** — Dino and obstacle sprites designed as custom 5×8 pixel characters written directly to LCD CGRAM
- **Hardware Timer-Driven Logic** — Obstacle movement and game speed controlled via AVR hardware timer (no `delay()` blocking)
- **GPIO Interrupt Jump** — Button press handled through hardware interrupt for zero-latency response
- **Collision Detection** — Real-time position comparison between Dino and obstacle at every timer tick
- **Live Score Tracking** — Score increments continuously while alive, displayed on LCD row 2
- **Flicker-Free Refresh** — Optimized LCD write sequence avoids full-screen redraws
- **Game Over & Reset** — Collision triggers game-over message; reset button restarts cleanly

---

## Hardware

| Component | Details |
|---|---|
| Microcontroller | ATmega2560 (Fire Bird V platform) |
| Display | 16×2 Character LCD |
| Jump Control | Push button (GPIO interrupt) |
| Reset Control | Push button |
| Power | 5V regulated |

### Pin Configuration

| Signal | ATmega2560 Port | PORTC Bit |
|---|---|---|
| RS (Register Select) | PC0 | Bit 0 |
| RW (Read/Write) | PC1 | Bit 1 |
| EN (Enable) | PC2 | Bit 2 |
| D4 (Data) | PC4 | Bit 4 |
| D5 (Data) | PC5 | Bit 5 |
| D6 (Data) | PC6 | Bit 6 |
| D7 (Data) | PC7 | Bit 7 |
| Jump Button | PE7 | Active LOW |

> Full PORTC (`DDRC |= 0xFF`) used as LCD port. 4-bit mode — D0–D3 unused.  
> Jump button is active LOW — press = LOW on PE7.  
> On Fire Bird V, board pull-ups handle PE7. For standalone builds, add 10kΩ pull-up to VCC.

---

## How It Works

```
Power On
   │
   ▼
Initialize LCD + CGRAM (custom Dino & obstacle sprites)
   │
   ▼
Start Hardware Timer (controls game tick rate)
   │
   ┌──────────────────────────────────────┐
   │           GAME LOOP                  │
   │  Timer ISR fires every N ms          │
   │  ├── Move obstacle left 1 position   │
   │  ├── Check collision with Dino       │
   │  │     ├── HIT  → Game Over screen   │
   │  │     └── MISS → Increment score    │
   │  ├── If obstacle off-screen → reset  │
   │  └── Refresh LCD (partial update)    │
   │                                      │
   │  Button ISR (GPIO interrupt)         │
   │  └── Trigger Dino jump animation     │
   └──────────────────────────────────────┘
```

---

## Project Structure

```
Dino_game_atmega2560/
├── Dino_game_c_main     ← Main source file (bare-metal Embedded C)
├── Dino.yaml            ← Wokwi simulation file
├── results/
│   ├── gameplay_1.jpeg
│   └── hardware_setup.jpeg
└── README.md
```

---

## Build & Flash

### Requirements
- AVR-GCC compiler
- AVRDUDE (for flashing)
- Atmel Studio / Microchip Studio (optional IDE)

### Compile
```bash
avr-gcc -mmcu=atmega2560 -O2 -o dino.elf Dino_game_c_main
avr-objcopy -O ihex dino.elf dino.hex
```

### Flash
```bash
avrdude -c wiring -p m2560 -P /dev/ttyUSB0 -b 115200 -U flash:w:dino.hex
```

### Simulate (Wokwi)
1. Go to [wokwi.com](https://wokwi.com) → New Project → ATmega2560
2. Replace `diagram.json` with the file in this repo
3. Paste `Dino_game_c_main` as `sketch.c`
4. Click Run — no hardware needed

> Crystal frequency set to 14.7456 MHz to match `F_CPU` in the code.

---

## Technical Highlights

- **Zero HAL dependency** — Direct register manipulation (DDRB, PORTB, TCCR1B, OCR1A, EIMSK)
- **ISR-based architecture** — Game logic driven by `TIMER1_COMPA_vect`, jump by `INT0_vect`
- **CGRAM sprite encoding** — Custom 5×8 bitmaps written to LCD addresses 0x00–0x07
- **Partial LCD update** — Only changed positions rewritten per frame, eliminating flicker

---

## What I Learned

Building this without Arduino libraries forced me to understand:
- How 4-bit LCD communication works at the signal level
- How AVR hardware timers generate precise periodic interrupts
- How to design game state machines in constrained memory (2KB RAM)
- How CGRAM addressing works for custom character creation

---

## Platform
Tested on **Fire Bird V** (ATmega2560-based robotics platform), IIIT Manipur Embedded Systems Lab
