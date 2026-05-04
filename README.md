# Arduino 32-Band Audio Spectrum Visualizer with Clock Mode

## Overview  
This project implements a **32-band audio spectrum visualizer** using an Arduino and a **32×8 LED matrix display**, with an additional **real-time clock (RTC) mode**.

It is designed for beginners and hobbyists who have basic knowledge of Arduino, electronics, and C/C++ programming. The components are affordable and easy to assemble.

---

## Main Features  
- Uses **arduinoFFT**, **MD_MAX72xx**, and **MD_Parola** libraries  
- Real-time **audio spectrum visualization (32 bands)**  
- **3 different visualizer display modes**, switchable via button  
- **Clock mode (HH:MM:SS)** using RTC module  
- Toggle between **Visualizer ↔ Clock display** with a second button  
- Peak hold effect for smoother visuals  
- Works with **32×8 LED matrix (MAX7219-based)**  
- Audio input via analog pin (e.g., from headphone/line-out)

---

## Components Required  
- Arduino Nano / Uno (ATmega328P-based recommended)  
- 32×8 LED Matrix (MAX7219, FC-16 type) – 4 modules  
- RTC Module (DS1307)  
- 2× Push buttons  
- Audio input circuit (resistors + capacitors for biasing)  
- Jumper wires  
- 5V power supply (USB is sufficient)

> Note: Exact resistor values are not critical, but proper DC biasing is required for clean audio sampling.

---

## Pin Configuration  

### LED Matrix (SPI)
- DIN → Pin 11  
- CLK → Pin 13  
- CS  → Pin 10  

### Buttons
- Mode Button (change visual style) → Pin 5  
- Switch Button (Visualizer ↔ Clock) → Pin 6  

### RTC (I2C)
- SDA → A4  
- SCL → A5  

### Audio Input
- Analog input → A0  

---

## How It Works  

### Audio Sampling & FFT  
- The Arduino samples the analog audio signal using its internal ADC  
- Sampling size: **64 samples**  
- A **Hamming window** is applied  
- FFT converts the signal into frequency components  
- Frequency bins are grouped into **32 columns**  

### Display Mapping  
- Each column represents a frequency band  
- Amplitude is mapped to **8 vertical levels (rows)**  
- A **peak hold effect** is applied for smoother visualization  

### Display Modes  
There are **3 visual styles**, achieved by changing LED bit patterns:
1. Standard bar graph  
2. Symmetric / mirrored pattern  
3. Rising pattern  

You can cycle through them using the **Mode Button (Pin 5)**.

---

## Clock Mode  

- Uses **RTC DS1307** module  
- Displays time in format: `HH:MM:SS`  
- Uses scrolling animation via **MD_Parola**  
- Automatically initializes RTC if not already running  

---

## Controls  

| Button | Function |
|--------|----------|
| Button 1 (Pin 5) | Change visualizer display mode |
| Button 2 (Pin 6) | Toggle between Visualizer and Clock |

---

## Operating Modes  

### 1. Visualizer Mode  
- Real-time audio spectrum display  
- Peak detection enabled  
- Multiple display styles  

### 2. Clock Mode  
- Scrolls current time across LED matrix  
- Clean transition when switching modes  

---

## Mode Switching Logic  
- Press **Switch Button** → toggles mode  
- When switching:
  - Display is cleared  
  - Peaks reset (when returning to visualizer)  

---

## Technical Details  

- ADC offset (~512) is removed to center signal  
- Input signal is scaled for better FFT resolution  
- FFT output is averaged into 32 bands  
- Values are constrained and mapped to display height  

---

## Customization Ideas  

- Add more display patterns  
- Use a **DS3231 RTC** for higher accuracy  
- Add brightness control (potentiometer or auto-adjust)  
- Stereo visualization (dual-channel processing)  
- Bluetooth audio input  

---

## Notes  

- Ensure proper **audio signal conditioning** (DC bias + attenuation)  
- Avoid feeding high-voltage audio signals directly  
- RTC battery backup recommended for time retention  
