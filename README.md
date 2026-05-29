# ATX/SFX Power Supply Control Integration for AMD BC-250

[cite_start]This repository provides a hardware and software solution to adapt standard computer power supplies (ATX, SFX, Flex ATX, etc.) for use with the AMD BC-250 motherboard inside a conventional PC case[cite: 1].

## Objective

[cite_start]The goal is to replicate native desktop computer power behavior on the AMD BC-250[cite: 2]. [cite_start]This modification allows the power supply to turn off automatically and seamlessly using a standard momentary (non-latching) power button, removing the need to press the button twice, use a latching switch, or toggle a secondary hardware button[cite: 3].

## Required Components (BOM)

| Component | Image | Link |
| --- | --- | --- |
| ATX Breakout Board with NE555 circuit (1x) | | [AliExpress](https://a.aliexpress.com/_mOnLLjp) |
| Arduino Nano (1x) | | [AliExpress](https://a.aliexpress.com/_mL2nDMx) |
| PC817 Optocoupler (1x) | | [Mercado Livre](https://www.mercadolivre.com.br/50x-pc817-foto-acoplador-optoacoplador-pc817/p/MLB2089175070?pdp_filters=item_id%3AMLB2089230981&matt_tool=38524122#origin=share&sid=share&wid=MLB2089230981&action=copy) |
| 220 Ohm Resistor (1x) | | [Mercado Livre](https://www.mercadolivre.com.br/resistor-220-ohms-114w/up/MLBU3375587685?pdp_filters=item_id%3AMLB4172505715&matt_tool=38524122#origin=share&sid=share&wid=MLB4172505715&action=copy) |
| Jumper wires (40pcs) | | [Mercado Livre](https://www.mercadolivre.com.br/cabo-wire-jumper-fmea-x-fmea-20-cm-40pcs/p/MLB28119264?pdp_filters=item_id%3AMLB4818434646&matt_tool=38524122#origin=share&sid=share&wid=MLB4818434646&action=copy) |

> [cite_start]Note: The PC817 optocoupler, resistor, and jumper wires can also be easily sourced from local electronics stores[cite: 14].

## Wiring Diagram

### Power Supply (Arduino Nano)
* [cite_start]ATX Breakout Board **5VSB** -> Nano **VCC IN** (Powering the Arduino via 5VSB keeps it active and ready to listen for the power pulse) [cite: 24, 25]
* [cite_start]ATX Breakout Board **GND** -> Nano **GND** [cite: 26]

### AMD BC-250 Motherboard Connection
* [cite_start]BC-250 TPM **Pin 9 (3.3V)** -> Nano **D2** [cite: 28]
* [cite_start]BC-250 **GND** -> Nano **GND** [cite: 29]

### Optocoupler (PC817) and Resistor Connection
* [cite_start]Arduino Nano **D4** -> **220 Ohm Resistor** -> PC817 **Pin 1** [cite: 31]
* [cite_start]Arduino Nano **GND** -> PC817 **Pin 2** [cite: 32]
* [cite_start]Case Microswitch **Terminal 1** -> PC817 **Pin 4** [cite: 33]
* [cite_start]Case Microswitch **Terminal 2** -> PC817 **Pin 3** [cite: 34]

---

## Arduino Configuration

1. [cite_start]Download and install the Arduino IDE from the [Official Arduino Website](https://www.arduino.cc/en/software/)[cite: 36].
2. [cite_start]Open the IDE, navigate to `Tools > Board > Arduino AVR Boards` and select **Arduino Nano**[cite: 37].
3. [cite_start]Go to `Tools > Processor` and select **ATmega328P** (or **ATmega328P (Old Bootloader)** depending on your board clone)[cite: 83].
4. [cite_start]Clear the default text in the editor and paste the source code provided below[cite: 118].
5. [cite_start]Click the **Upload** button (the right arrow button in the top left corner)[cite: 168, 169].

### Troubleshooting
[cite_start]If an error occurs during compilation or transmission, toggle between **ATmega328P** and **ATmega328P (Old Bootloader)** under the processor menu and retry the upload[cite: 215].

---
