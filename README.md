# Raspberry-pi--pico

# Smart Workstation Ergonomics & Environmental Monitor

An IoT project built on the Raspberry Pi Pico W.

**Tech stack:** MicroPython | Wi-Fi | DHT11 | Relay

**Key features:** Work-rest timer, automatic climate control, and a live web dashboard.

---

## Why Desk Workers Need This

| Problem | Details |
|---|---|
| **Long hours without a break** | Nobody reminds you to take a break. Deep focus makes us forget to rest, which leads to back pain, neck pain, and eye strain. |
| **Warm rooms reduce focus** | Rising temperature quietly lowers concentration and productivity. |
| **No feedback** | Most people cannot see how much they worked or how the room feels. |

The device addresses all three problems.

---

## What the Device Does

1. **Work-rest timer**: 45 minutes of focus with a green LED, then a 15-minute break with a buzzer and a red LED.
2. **Automatic climate control**: A DHT11 sensor reads temperature and humidity. A relay switches the fan on above 26 °C.
3. **Live web dashboard**: Open the board's IP address on any phone or laptop to see the timer, sessions, and room conditions.

Health reminders, environmental automation, and remote monitoring on one low-cost board.

---

## Hardware

Every part is already on the **Joy-IT RB-P-XPLR** training board. Slide switches connect each module to its pin. The only extra part is a small 5 V DC fan.

| Component | Role | Pin |
|---|---|---|
| Raspberry Pi Pico W (RP2040) | Wi-Fi microcontroller | n/a |
| DHT11 sensor | Temperature and humidity | GP0 |
| Relay | Switches the fan | GP28 |
| Buzzer | Break alert | GP27 |
| LEDs | Work / break status | GP1 |
| Push button | Reset or pause | GP15 |

---

## How It Works

**Inputs:** DHT11 sensor, push button

**Outputs:** LEDs, buzzer, relay and fan

**Wi-Fi:** Web dashboard on phone or laptop

The Pico W runs one non-blocking main loop that repeats three steps:

1. Update the timer
2. Control the climate
3. Serve the web page

---

## Timer: A Two-State Machine

| State | Duration | Indicator |
|---|---|---|
| **WORK** | 45 minutes | Green LED |
| **BREAK** | 15 minutes | Red LED |

- When the 45 minutes end, the break starts: the buzzer sounds and the red LED turns on.
- When the 15 minutes end, the timer returns to WORK.
- Each finished work phase adds 1 to the session counter.
- A button press resets or pauses the timer (debounced).
- Non-blocking timing with `ticks_ms` keeps the rest of the program running.

---

## Climate Control with Hysteresis

| Temperature | Fan |
|---|---|
| 26 °C or higher | **ON** |
| Between 25 °C and 26 °C | Keeps its current state |
| 25 °C or lower | **OFF** |

**Why two thresholds?**

- Room temperature often hovers around the limit.
- With a single threshold, the fan would switch on and off rapidly.
- Rapid switching is noisy and wears out the relay.
- In the middle zone, the fan simply keeps its current state.

---

## Web Dashboard

Shown live on any phone or laptop. Type the board's IP address into a browser (same Wi-Fi network). The page refreshes every few seconds.

The dashboard shows:

- Timer state: work or break
- Time remaining
- Completed work sessions
- Temperature and humidity
- Fan (relay) status

**Example values:**

| Field | Value |
|---|---|
| State | WORK |
| Remaining | 32:14 |
| Sessions | 3 |
| Temperature | 24.5 °C |
| Humidity | 48 % |
| Fan | OFF |

---

## 10-Step Roadmap

**Prepare and test hardware**

1. Define requirements
2. Collect components
3. Set up software
4. Assemble circuit
5. Test each part

**Write the software**

6. Build the timer
7. Temperature control
8. Web dashboard

**Integrate and deliver**

9. Integrate and test
10. Document and present

The project was built incrementally, and each step has a clear "done when" condition.

---

## Challenges and Next Steps

**Key challenges**

- Running the timer, sensor, and web server together without blocking
- The DHT11 is slow and accurate to only about 2 °C
- Button bounce needs debouncing to avoid false presses

**Future improvements**

- Show status on the built-in TFT display
- Save the session count in flash memory
- Change times and limits from the dashboard
- Use a more accurate sensor (DHT22 or BME280)
- Protect the dashboard with a password

---

## Summary

A low-cost IoT device that combines **health** reminders, environmental **automation**, and remote monitoring (**IoT**) on one board.
