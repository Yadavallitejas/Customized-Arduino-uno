### **Custom Arduino Uno Documentation Using ATmega328P**

This documentation provides a detailed description of designing a custom Arduino Uno using the ATmega328P microcontroller. The project also incorporates the following components:
- **7-segment display (SSD)**
- **CD4511BE** as the SSD driver IC
- **L293D** for motor driving
- **Programmable debugging capability**
- **Potentiometer**
- **RGB LED with switch**
- **Power supply using 7805 voltage regulator**
- **16 MHz external oscillator** for the microcontroller.

---

## **1. Block Diagram**

The main components of the custom Arduino are interconnected as follows:

```
+---------------------------+
|                           |
|    ATmega328P             |
|    (Microcontroller)      |
|                           |
|  +---------------------+  |
|  | 7-Segment Display    |  |
|  |   CD4511BE Driver    |  |
|  +---------------------+  |
|                           |
|  +---------------------+  |
|  | Motor Driver         |  |
|  |   L293D              |  |
|  +---------------------+  |
|                           |
|  +---------------------+  |
|  | RGB LED and Switch   |  |
|  +---------------------+  |
|                           |
|  +---------------------+  |
|  | Potentiometer        |  |
|  +---------------------+  |
|                           |
|  +---------------------+  |
|  | Power Supply (7805)  |  |
|  +---------------------+  |
|                           |
+---------------------------+
```



## **2. Hardware Components**

### **2.1 ATmega328P Microcontroller**

- **Core component**: ATmega328P is the microcontroller used in the original Arduino Uno.
- **Pinout**: 28 pins in total.
- **Clock Speed**: 16 MHz external crystal oscillator (necessary for stable and precise timing).
- **Programming**: The microcontroller can be programmed via a serial interface (FTDI or USB to UART converter).
- **Reset circuit**: A 10k pull-up resistor on the reset pin with a capacitor connected to ground ensures proper reset behavior.

### **2.2 7-Segment Display with CD4511BE IC**

- **Display Type**: Common Cathode 7-segment display.
- **Driver IC (CD4511BE)**: The CD4511BE is a BCD to 7-segment driver IC. It converts the binary coded decimal input from the ATmega328P to a corresponding output to control the SSD.
  - **Connections**:
    - Input pins: The ATmega328P sends a 4-bit BCD code to CD4511BE on four input pins.
    - Output pins: These connect to the SSD to control the segments (a-g).

### **2.3 Motor Driver with L293D**

- **Motor Control**: L293D is a dual H-bridge motor driver IC used to control DC motors or stepper motors.
- **Pinout**: L293D has 16 pins.
  - **Enable pins**: Control the motor's operation by enabling or disabling the H-bridges.
  - **Input pins**: Controlled by the ATmega328P to set the motor direction and speed.
  - **Output pins**: Connect to the motor to provide power for motor rotation.

### **2.4 Potentiometer**

- **Function**: Used to adjust input values, typically for adjusting motor speed or LED brightness.
- **Connection**: Connect the potentiometer to one of the analog input pins (A0-A5) of the ATmega328P.
- **Operation**: The varying resistance changes the voltage, which is read by the ATmega328P as an analog input signal.

### **2.5 RGB LED with Switch**

- **RGB LED**: A tri-color LED that can be controlled to display a range of colors.
- **Connections**: The Red, Green, and Blue pins of the LED connect to three digital pins on the ATmega328P, each with a current-limiting resistor (typically 220Ω).
- **Switch**: A push-button switch can be connected to a digital pin to toggle the LED on or off.

### **2.6 Power Supply with 7805 Voltage Regulator**

- **7805 IC**: A voltage regulator that outputs a stable 5V from a higher input voltage (e.g., 9V or 12V).
- **Capacitors**: Typically, a 0.1 µF capacitor is placed between the input pin and ground, and a 0.1 µF capacitor between the output pin and ground for filtering.
- **Power supply**: Provides 5V to the ATmega328P and other components.

### **2.7 16 MHz External Oscillator**

- **Crystal Oscillator**: The ATmega328P requires a 16 MHz external crystal oscillator to operate at the standard Arduino clock frequency.
- **Capacitors**: Two 22 pF capacitors are connected to the crystal to stabilize the oscillations.

### **2.8 Programmable Debugging (Prog Debug)**

- **ISP Header**: Include a 6-pin in-system programming (ISP) header to program and debug the ATmega328P using an external programmer.
  - **Pins**: MISO, MOSI, SCK, RESET, VCC, GND.
- **UART Interface**: Connect pins (TX, RX) to a USB-to-serial converter for debugging and loading code via the Arduino IDE.

---

## **3. Circuit Schematic and Wiring**

Here’s how each component is connected to the ATmega328P.

### **3.1 ATmega328P Connections**
- **Power Supply**: The 7805 IC regulates the input voltage to 5V, which is supplied to the VCC and AVCC pins.
- **Ground Pins**: GND of the power supply is connected to the GND pins of the ATmega328P.
- **Reset Pin**: Connect a 10kΩ resistor between the reset pin and VCC. Also, add a 0.1µF capacitor between the reset pin and ground.
- **16 MHz Oscillator**: Connect the crystal between XTAL1 and XTAL2 pins. Place two 22 pF capacitors between these pins and ground.
- **LED**: Connect a 220Ω resistor in series with the LED to digital pin 13.

### **3.2 7-Segment Display with CD4511BE**
- **BCD Inputs** (A-D): Connect four digital pins from the ATmega328P to the CD4511BE’s A, B, C, and D inputs.
- **7-segment Outputs**: Connect the output pins of the CD4511BE (a, b, c, d, e, f, g) to the respective segments of the SSD.
- **Common Cathode**: Connect the common cathode of the SSD to ground.

### **3.3 Motor Driver with L293D**
- **Motor Inputs**: Connect the motor control pins (IN1, IN2, IN3, IN4) of L293D to four digital output pins of the ATmega328P.
- **Motor Power**: Connect the motor’s power supply to the L293D's VCC and ground.
- **Enable Pins**: Connect the enable pins of the L293D to 5V to activate the H-bridge.

### **3.4 Potentiometer**
- **Wiper Pin**: Connect the wiper pin of the potentiometer to one of the analog pins (e.g., A0).
- **Other Pins**: Connect one pin to 5V and the other to GND.

### **3.5 RGB LED and Switch**
- **RGB LED**: Connect the RGB LED's cathode to ground, and the red, green, and blue pins to three PWM-enabled digital pins (e.g., D9, D10, D11) via 220Ω resistors.
- **Switch**: Connect the switch between a digital pin (e.g., D2) and ground, with a 10kΩ pull-up resistor between the pin and VCC.

### **3.6 Power Supply (7805)**
- **Input Voltage**: Connect a 9V or 12V power source to the input of the 7805.
- **Output**: Connect the output of the 7805 to the VCC of the ATmega328P.

---

## **4. Software Implementation**
- by using avrdude software install microcontroller code which is present in arduino ide software which is specified to atmega328p
1. **Arduino IDE Setup**:
   - Install the **ATmega328P** board in the Arduino IDE.
   - Select the correct **board** and **port** in the IDE.

2. **7-Segment Display Control**:
   - Use the digitalWrite() function to send a BCD code to the CD4511BE to display the corresponding number.

3. **Motor Control**:
   - Use digitalWrite() and analogWrite() to control the L293D and regulate motor speed and direction.

4. **RGB LED**:
   - Use analogWrite() on the RGB pins to mix colors by varying PWM duty cycles.

5. **Potentiometer Input**:
   - Read the analog input from the potentiometer using analogRead() and map the value to motor speed or LED brightness.

6. **Switch Control**:
   - Use digitalRead() to detect the switch state and toggle the RGB LED.

---

## **5. Conclusion**

This custom Arduino Uno design with the ATmega328P is tailored for various functionalities like motor control, display, sensor input, and debugging.
