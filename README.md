# ATX/SFX Power Supply Control Integration for AMD BC-250

This repository provides a hardware and software solution to adapt standard computer power supplies (ATX, SFX, Flex ATX, etc.) for use with the AMD BC-250 motherboard inside a conventional PC case[cite: 218].

## Objective

The goal is to replicate native desktop computer power behavior on the AMD BC-250[cite: 219]. This modification allows the power supply to turn off automatically and seamlessly using a standard momentary (non-latching) power button, removing the need to press the button twice, use a latching switch, or toggle a secondary hardware button[cite: 220].

## Required Components (BOM)

| Component | Image | Link |
| --- | --- | --- |
| ATX Breakout Board with NE555 circuit (1x) | | [AliExpress](https://a.aliexpress.com/_mOnLLjp) |
| Arduino Nano (1x) | | [AliExpress](https://a.aliexpress.com/_mL2nDMx) |
| PC817 Optocoupler (1x) | | [Mercado Livre](https://www.mercadolivre.com.br/50x-pc817-foto-acoplador-optoacoplador-pc817/p/MLB2089175070?pdp_filters=item_id%3AMLB2089230981&matt_tool=38524122#origin=share&sid=share&wid=MLB2089230981&action=copy) |
| 220 Ohm Resistor (1x) | | [Mercado Livre](https://www.mercadolivre.com.br/resistor-220-ohms-114w/up/MLBU3375587685?pdp_filters=item_id%3AMLB4172505715&matt_tool=38524122#origin=share&sid=share&wid=MLB4172505715&action=copy) |
| Jumper wires (40pcs) | | [Mercado Livre](https://www.mercadolivre.com.br/cabo-wire-jumper-fmea-x-fmea-20-cm-40pcs/p/MLB28119264?pdp_filters=item_id%3AMLB4818434646&matt_tool=38524122#origin=share&sid=share&wid=MLB4818434646&action=copy) |

Note: The PC817 optocoupler, resistor, and jumper wires can also be easily sourced from local electronics stores[cite: 231].

## Wiring Diagram

### Power Supply (Arduino Nano)
* ATX Breakout Board 5VSB -> Nano VCC IN (Powering the Arduino via 5VSB keeps it active and ready to listen for the power pulse) [cite: 241, 242]
* ATX Breakout Board GND -> Nano GND [cite: 243]

### AMD BC-250 Motherboard Connection
* BC-250 TPM Pin 9 (3.3V) -> Nano D2 [cite: 245]
* BC-250 GND -> Nano GND [cite: 246]

### Optocoupler (PC817) and Resistor Connection
* Arduino Nano D4 -> 220 Ohm Resistor -> PC817 Pin 1 [cite: 248]
* Arduino Nano GND -> PC817 Pin 2 [cite: 249]
* Case Microswitch Terminal 1 -> PC817 Pin 4 [cite: 250]
* Case Microswitch Terminal 2 -> PC817 Pin 3 [cite: 251]

---

## Arduino Configuration

1. Download and install the Arduino IDE from the Official Arduino Website (https://www.arduino.cc/en/software/)[cite: 253].
2. Open the IDE, navigate to Tools > Board > Arduino AVR Boards and select Arduino Nano[cite: 254].
3. Go to Tools > Processor and select ATmega328P (or ATmega328P (Old Bootloader) depending on your board clone)[cite: 300].
4. Clear the default text in the editor and paste the source code provided below[cite: 335].
5. Click the Upload button (the right arrow button in the top left corner)[cite: 385, 386].

### Troubleshooting
If an error occurs during compilation or transmission, toggle between ATmega328P and ATmega328P (Old Bootloader) under the processor menu and retry the upload[cite: 432]. Once the "Upload complete" message appears, the hardware connection is ready to be finalized[cite: 433, 434].

---

## Source Code

The implementation code is detailed below. This code is also available as a standalone .ino sketch file within this repository.

```cpp
const int pinoSensorTPM = 2; 
const int pinoOptoacoplador = 4; 
bool sistemaInicializado = false; 
bool desligamentoExecutado = false; 

void setup() { 
  pinMode(pinoSensorTPM, INPUT); 
  pinMode(pinoOptoacoplador, OUTPUT); 
  digitalWrite(pinoOptoacoplador, LOW); 
  delay(10000); 
} 

void loop() { 
  int estadoPlaca = digitalRead(pinoSensorTPM); 
 
  if (estadoPlaca == HIGH) { 
    sistemaInicializado = true; 
  } 
 
  if (sistemaInicializado && !desligamentoExecutado) { 
    if (estadoPlaca == LOW) { 
      delay(200); 
 
      if (digitalRead(pinoSensorTPM) == LOW) { 
        digitalWrite(pinoOptoacoplador, HIGH); 
        delay(100); 
        digitalWrite(pinoOptoacoplador, LOW); 
 
        desligamentoExecutado = true; 
      } 
    } 
  } 
 
  if (desligamentoExecutado) { 
    delay(2000); 
    sistemaInicializado = false; 
    desligamentoExecutado = false; 
  } 
 
  delay(100); 
}
