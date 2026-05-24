# LCD Dino Game — ATmega2560 Bare-Metal Embedded C

> Chrome's offline Dino game, rebuilt from scratch on a 16×2 LCD using bare-metal AVR programming — no Arduino libraries, no OS, just registers.  
> **ATmega2560 | Embedded C | AVR-GCC | Custom CGRAM Graphics | Hardware Timers | GPIO Interrupts**

---

## Overview

This project implements a fully playable Dino Jump game on an ATmega2560 microcontroller driving a 16×2 character LCD — built entirely in bare-metal Embedded C without any Arduino abstraction layer.

Every component — LCD communication, character animation, obstacle movement, collision detection, score tracking, and display refresh — is handled at the hardware register level using AVR timers and GPIO interrupts.

---

## Demo
<img width="1263" height="1565" alt="WhatsApp Image 2026-05-23 at 21 13 32" src="https://github.com/user-attachments/assets/e5b522b9-83be-442b-95ec-5f1ec5ff2316" />

<img width="1263" height="1563" alt="WhatsApp Image 2026-05-23 at 21 13 19" src="https://github.com/user-attachments/assets/938c0eff-c515-4645-9b3e-2e4105c3f920" />

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
Open `Dino.yaml` at [wokwi.com](https://wokwi.com) to run the simulation in browser without hardware.

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
