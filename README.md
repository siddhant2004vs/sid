📄 README_ButtonLogger.md
markdown
Copy
Edit
# Event-Driven Button Logger with FSM, EEPROM, Deep Sleep, and Watchdog

## 📘 Description

This embedded system runs on an Arduino Uno. It uses:
- Interrupts for handling two push buttons.
- Debouncing logic to avoid multiple triggers.
- A finite state machine (FSM) to toggle between IDLE and ACTIVE.
- EEPROM to store Button 2 press count across power cycles.
- Deep sleep mode in IDLE to save power.
- A watchdog timer to reset the system if it hangs.

---

## 🔁 FSM Diagram

csharp
Copy
Edit
   [B1 Press]
IDLE -------------> ACTIVE
<-------------
[B1 Press]

yaml
Copy
Edit

- **IDLE**: LED is OFF, Button 2 presses ignored.
- **ACTIVE**: LED blinks every 500ms, Button 2 counts logged.

---

## 🔌 Pin Mapping

| Component       | Arduino Pin | Description                     |
|----------------|-------------|---------------------------------|
| Button 1 (B1)   | D2 (INT0)   | Toggle FSM state                |
| Button 2 (B2)   | D3 (INT1)   | Count presses in ACTIVE state   |
| LED             | D13         | Blinks in ACTIVE state          |
| GND             | GND         | Connect to other leg of both buttons |

---

## ⚙️ Logic Summary

- **Button 1**: Toggles state between IDLE and ACTIVE.
- **Button 2**: Only active in ACTIVE state. Each press increments a count.
- **Debounce**: Ignores noise by waiting 200ms between valid presses.
- **EEPROM**: Press count is saved and restored even after power off.
- **Sleep Mode**: When IDLE, system enters deep sleep (SLEEP_FOREVER).
- **Watchdog Timer**: If system hangs for >2s, it's auto-reset by watchdog.

---

## 📝 Sample Serial Output

System Ready. State: IDLE
Restored Button 2 Count: 4
State: ACTIVE
Button 2 pressed. Count = 5
Button 2 pressed. Count = 6
State: IDLE
Entering Deep Sleep (IDLE mode)

yaml
Copy
Edit

---

## 📦 Required Libraries

Install this from the Arduino IDE Library Manager:

- **LowPower** by Rocket Scream

---

## ✅ Assumptions

- Buttons are active LOW and use internal pull-up resistors.
- Only INT0 (D2) can wake the device from sleep on Arduino Uno.
- EEPROM address 0 is used for storing count.
- Runs on 16 MHz Arduino Uno (ATmega328P).

---

## ❌ Limitations

- No debounce hardware (pure software debounce).
- Only one wake source from sleep (B1).
- Not tested on non-AVR boards (like ESP32).

---

**End of README**
