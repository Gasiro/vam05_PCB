# vam05_PCB
This is an BLE PCB designed for the v4n layout and is compatible with ZMK firmware.

The vam05 is based on trashman’s original v4n layout design, v4n4g0n, and integrates features from several later open-source variants of the v4n PCB — including underglow lighting using WS2812 LEDs.
![Vam05PCBFront](https://github.com/user-attachments/assets/7c10bdcd-64a6-40ac-a1de-a828d1bf4bd3)
![Vam05PCBBack](https://github.com/user-attachments/assets/776ec8a7-192a-40b2-8d78-594aae91401b)

## Install / Uninstall the HAC-006 battery
⬇ Watch the video (installing the battery)

[![Install the battery](https://img.youtube.com/vi/nf2Lfo7mWOo/0.jpg)](https://www.youtube.com/watch?v=nf2Lfo7mWOo)

⚠️ **When removing the battery, do not pull it off directly, as this may damage the EZM connector. Instead, use a thin plastic pry tool to gently lift the battery from the EZM end.** (Such a tool is usually included with the HAC-006 battery.)

⬇ Watch the video (uninstalling the battery)

[![Uninstall the battery](https://img.youtube.com/vi/oJaKfc10HQI/0.jpg)](https://www.youtube.com/watch?v=oJaKfc10HQI)


# Design Purpose
The vam05 aims to make the v4n layout portable and wireless. Frequent plugging/unplugging of USB-C cables on wired versions risks static discharge damage to the PCB. To mitigate this, the vam05 was designed with BLE support.

⚠️ Regardless of version, always use properly shielded USB-C cables. Avoid handmade/custom USB cables that may lack grounding — these can easily damage your PCB.

# Features
- **12 WS2812 RGB LEDs** form a circular underglow on the back of the board. The lighting is reasonably bright — especially with transparent cases like the _paw / vam05 PC version.

  - When turned off via keymap control, the LEDs do not significantly drain battery life.

- **Battery Power Switch**: Instead of completely cutting off power, it uses a clever circuit design that allows charging even when the switch is off.

- **Simplified v4n Layout**:

  - No split spacebar or encoder support (due to BLE MCU size constraints).

  - However, split-spacebar functionality can be simulated via ZMK: e.g., short press = space, long press = another key.

- **Three Pre-soldered Indicator LEDs** (same as other v4n variants):

- **Indicator1**:

  - Red = Charging

    - Off = Fully charged

  - On boot, it also shows battery status via green blinks:

    - 80%: 2 blinks

    - <20%: 5 blinks

    - <5%: 10 blinks

- **Indicator2**:
  - Layer indicator

    - Lime = Layer 1

    - Magenta = Layer 2

    - Cyan = Layer 3
      
  - Bluetooth connection indicator

    - Red = not connected to any profile

    - Blue = bluetooth connected

- **Indicator3**: Caps Lock indicator

### Note on Caps Lock LED
💡 Unlike layer indicators, Caps Lock feedback depends on how the operating system handles the key press.
Some systems (**like macOS**) may ignore very quick taps and require the key to be held for around **200 ms** to register properly.
If the indicator doesn't light up, try pressing and holding the key slightly longer.

# Keymap Editing
Use https://zmk.studio/ to modify keymaps in real time via USB connection.
![keymap](https://github.com/user-attachments/assets/7d019430-5735-4961-abc1-3cefaf85caa3)


# Firmware
If you purchased the PCB directly from me, it comes pre-flashed. 
[firmware.zip](https://github.com/user-attachments/files/20825480/firmware.zip)


There is a reset button next to the SWD footprint on the back of the PCB. Pressing it once will reset the board. Double-clicking it quickly within 500ms will enter .uf2 bootloader mode. In this mode, a disk named nrf52boot will appear on your computer. Simply drag and drop the .uf2 file (the .uf2 file in firmware.zip) into the disk to flash the firmware.

During flashing, the green indicator1 LED will stay on. (If the battery is charging at the same time, the green LED will combine with the red charging LED and appear yellow.)

However, you are free to flash your own firmware if needed. In most cases, the online configurator is sufficient.

If you're building the PCB from open-source files and using the nRF52840, note that this MCU needs a bootloader before it can flash .uf2 files. 

Here is a modified bootloader suited for vam05:

👉 [Bootloader Download](https://github.com/Gasiro/Adafruit_nRF52_Bootloader/actions/runs/14868433935/artifacts/3073119494)


# Flashing for the First Time

You’ll need a J-Link programmer (must support nRF52 — avoid some clones that cannot flash nRF chips and might damage your board).

- The SWD interface on vam05 uses the TC2030-CTX-NL connector:

  - https://www.tag-connect.com/wp-content/uploads/bsk-pdf-manager/TC2030-CTX_1.pdf

# Steps:
1. Open J-Flash

2. Set target device to nrf52840_XXAA

3. Set interface to SWD

4. Load the pca10100_bootloader-08ff459_s140_7.3.0.hex file (linked above)

5. Click “Erase Chip”

6. Click “Program Device”

7. Once flashed, disconnect the board and plug it back in. A USB drive will appear — drag and drop your .uf2 firmware into it, and you're done!


# Bluetooth Connection Guide

Layer 3 (MO3) keys 1–4 map to BT Profiles 0–3 respectively, allowing connection to up to four devices (ZMK supports five, but most users will find four sufficient).

### First-Time Pairing

Press the key for BT Profile 0 — LED2 turns lime to show it’s ready but unpaired.

- Red = Profile paired, device not currently connected

- Blue = Connection successful

Now, pair with your first device (e.g. PC). Once connected, LED2 turns blue.
Next, try pairing BT Profile 1 (key 2 on Layer 3) with your second device (e.g. phone) the same way — don’t press BT_CLR between pairings.

Now you can switch freely between the two devices using their profile keys.

Think of the 4 BT profiles as parking spaces:

- You just parked your first and second cars.

- Pressing BT_CLR while in a profile clears that space.

- Pressing BT_CLR_ALL clears all paired devices.



