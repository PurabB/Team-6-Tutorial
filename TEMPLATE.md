---
title: Connecting Game Controller to any ESP32 dev module with Bluetooth tutorial.
date: 11-25-2024
authors:
  - name: Purab Balani
  - name: Syler Sylvester
  - name: Aleksandar Jeremic
---

![th](https://github.com/user-attachments/assets/b54308e5-dd03-415c-a266-4db8675e2418)


## Introduction

This tutorial is designed to guide you through connecting any ESP development module to a Bluetooth game controller. Specifically, we will demonstrate using a wireless Xbox controller with the ESP32-S3 dev module to control a robotic arm. The primary objective is to establish a connection between the controller and the ESP32 module, enabling you to read all inputs effectively and then control the intensity of an LED through the joystick.

The motivation behind this tutorial is to provide a practical example of integrating Bluetooth game controllers with ESP modules, showcasing their potential in robotics and other applications. By following this tutorial, readers will gain hands-on experience in setting up and configuring the hardware and software, ultimately enhancing their understanding of Bluetooth communication and control systems.

### Learning Objectives

- Bluetooth Communication: Understanding how to establish a Bluetooth connection between the ESP32-S3 and a game controller.
- ESP32-S3 Configuration: Setting up the ESP32-S3 development module for Bluetooth communication.
- Controller Input Reading: Learning how to read inputs from a wireless Xbox controller.
- Firmware Development: Writing and configuring firmware to handle Bluetooth communication and input processing.
- Troubleshooting: Identifying and resolving common issues in Bluetooth communication and hardware setup.

Any additional notes from the developers can be included here.

### Background Information

This tutorial demonstrates how to connect a Bluetooth game controller to an ESP32 development module, specifically using a wireless Xbox controller with the ESP32-S3 to control a robotic arm. The primary objective is to establish a connection between the controller and the ESP module, enabling the reading of all inputs effectively. The motivation behind this tutorial is to provide a practical example of integrating Bluetooth game controllers with ESP modules, showcasing their potential in robotics and other applications. By following this tutorial, readers will gain hands-on experience in setting up and configuring the hardware and software, ultimately enhancing their understanding of Bluetooth communication and control systems.

## Getting Started
### Required Downloads and Installations
---
Software: Arduino IDE
Description: The Arduino Integrated Development Environment (IDE) is a cross-platform application used to write and upload programs to Arduino-compatible boards.
Installation Process: 
  - Download the Arduino IDE from the [official website](https://www.arduino.cc/en/software).
  - Follow the installation instructions for your operating system (Windows, macOS, or Linux).
  - Open the Arduino IDE and go to File > Preferences.
  - In the "Additional Boards Manager URLs" field, add the following URLs:
    `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
    `https://raw.githubusercontent.com/ricardoquesada/esp32-arduino-lib builder/master/bluepad32_files/package_esp32_bluepad32_index.json`
  - Go to Tools > Board > Boards Manager, search for "ESP32", and install the ESP32 by Espressif as well as esp32_bluepad32 by Ricardo Quesada board package.
  - Go to Tools > Board > esp32_bluepad32, search for your esp board module in my case it is ESP32S3 Dev Module, and choose your COM port.
---


### Required Components

| Component Name           | Quantity |
|--------------------------|----------|
| ESP32-S3 Dev Module      | 1        |
| Wireless Xbox Controller | 1        |
| USB Cable                | 1        |
| Power Supply             | 1        |

### Required Tools and Equipment

All you will need is a functional computer with Arduino installed and running with an USB port.

---
## Part 01: Connecting controller with ESP32

### Introduction

In this section, you will learn how to connect a game controller to the ESP32 using the Bluepad32 library. This forms the foundation for reading and processing input values from the controller.

### Objective

- Understand the process of connecting a game controller to the ESP32.
- Learn to set up Bluepad32 callbacks for controller connections.
- Identify and handle multiple connected controllers.

### Background Information

- Programming in C/C++ for ESP32.
- Bluetooth communication principles.
- Setting up the Arduino environment for ESP32.

### Components

- ESP32 development board
- Game controller (Bluetooth-compatible)
- USB cable for programming ESP32
- Arduino IDE or PlatformIO

### Instructional

- Install Bluepad32 Library: Ensure you have the Bluepad32 library installed in your Arduino environment.
- Set Up Callbacks: Implement onConnectedController() and onDisconnectedController() functions to handle controller connections.
- Connect Controller: Power on your controller and put it into pairing mode.
- Upload Code to ESP32: Compile and upload the code provided in the example section to your ESP32.
- pair the controller

## Example

### Introduction

This example demonstrates how to detect when a game controller connects to the ESP32 and outputs basic information about the connected controller.

### Example

```cpp
#include <Bluepad32.h>
const unsigned int LED{17}; // define a constant for the LED pin

ControllerPtr myControllers[BP32_MAX_GAMEPADS];

// This callback gets called any time a new gamepad is connected.
// Up to 4 gamepads can be connected at the same time.
void onConnectedController(ControllerPtr ctl) {
    bool foundEmptySlot = false;
    for (int i = 0; i < BP32_MAX_GAMEPADS; i++) {
        if (myControllers[i] == nullptr) {
            USBSerial.printf("CALLBACK: Controller is connected, index=%d\n", i);
            // Additionally, you can get certain gamepad properties like:
            // Model, VID, PID, BTAddr, flags, etc.
            ControllerProperties properties = ctl->getProperties();
            USBSerial.printf("Controller model: %s, VID=0x%04x, PID=0x%04x\n", ctl->getModelName().c_str(), properties.vendor_id,
                           properties.product_id);
            myControllers[i] = ctl;
            foundEmptySlot = true;
            break;
        }
    }
    if (!foundEmptySlot) {
        USBSerial.println("CALLBACK: Controller connected, but could not found empty slot");
    }
}

void onDisconnectedController(ControllerPtr ctl) {
    bool foundController = false;

    for (int i = 0; i < BP32_MAX_GAMEPADS; i++) {
        if (myControllers[i] == ctl) {
            USBSerial.printf("CALLBACK: Controller disconnected from index=%d\n", i);
            myControllers[i] = nullptr;
            foundController = true;
            break;
        }
    }

    if (!foundController) {
        USBSerial.println("CALLBACK: Controller disconnected, but not found in myControllers");
    }
}

void processControllers() {
    for (auto myController : myControllers) {
        if (myController && myController->isConnected() && myController->hasData()) {
            if (myController->isGamepad()) {
                processGamepad(myController);
            }
            else {
                USBSerial.println("Unsupported controller");
            }
        }
    }
}

```
### Analysis

The first part of this tutorial focuses on establishing a connection between the game controller and the ESP32 module using the Bluepad32 library. The example code includes essential steps such as initializing the library, defining callback functions for handling controller connections, and implementing basic diagnostics to confirm successful pairing.

#### Code Walkthrough

- **Callback Functions**: `onConnectedController` and `onDisconnectedController` handle events when a controller connects or disconnects. They assign or clear the controller pointer from the `myControllers` array.
- **Controller Properties**: The code retrieves and prints controller properties like vendor ID (VID) and product ID (PID) for diagnostic purposes.
- **Processing Controllers**: The `processControllers()` function iterates through the array of connected controllers and processes inputs only if the controller is connected and has new data.

This section ensures a robust connection between the game controller and ESP32, enabling further interaction.

---

## Part 02: retrieving Gamepad Data

### Introduction

In this section, you will learn how to retrieve and display real-time data from the game controller. This involves accessing inputs such as joystick positions, button states, and motion data, enabling you to monitor the controller's state in detail.

### Objective

- Understand how to extract gamepad data using Bluepad32.
- Display real-time gamepad data via serial output.
- Familiarize yourself with controller properties like axis, buttons, and motion sensors.

### Background Information

- Reading and interpreting joystick and button inputs.
- Serial communication with the ESP32 for data monitoring.
- Controller properties and their relevance to applications.

### Components

- ESP32 development board
- Bluetooth-compatible game controller
- USB cable for programming and monitoring ESP32
- Arduino IDE or PlatformIO

### Instructional

- **Gamepad Data Function**: Implement the `dumpGamepad()` function to retrieve all relevant data from the connected game controller.
- **Access Controller Properties**: Use the gamepad API to fetch joystick positions, button states, and sensor data.
- **Monitor Serial Output**: View the retrieved data in the serial monitor for analysis.
- **Iterate Over Controllers**: Ensure data is dumped for all connected controllers using a loop.

## Example

### Introduction

The example below retrieves and displays gamepad data such as joystick axis values, button states, and accelerometer/gyroscope readings.

### Example Code

```cpp
void dumpGamepad(ControllerPtr ctl) {
    USBSerial.printf(
        "idx=%d, dpad: 0x%02x, buttons: 0x%04x, axis L: %4d, %4d, axis R: %4d, %4d, brake: %4d, throttle: %4d, "
        "misc: 0x%02x, gyro x:%6d y:%6d z:%6d, accel x:%6d y:%6d z:%6d\n",
        ctl->index(),        // Controller Index
        ctl->dpad(),         // D-pad
        ctl->buttons(),      // Bitmask of pressed buttons
        ctl->axisX(),        // (-511 - 512) left X Axis
        ctl->axisY(),        // (-511 - 512) left Y Axis
        ctl->axisRX(),       // (-511 - 512) right X Axis
        ctl->axisRY(),       // (-511 - 512) right Y Axis
        ctl->brake(),        // (0 - 1023): Brake button
        ctl->throttle(),     // (0 - 1023): Throttle button
        ctl->miscButtons(),  // Bitmask of "misc" buttons
        ctl->gyroX(),        // Gyroscope X
        ctl->gyroY(),        // Gyroscope Y
        ctl->gyroZ(),        // Gyroscope Z
        ctl->accelX(),       // Accelerometer X
        ctl->accelY(),       // Accelerometer Y
        ctl->accelZ()        // Accelerometer Z
    );
}
```
### Analysis

This part introduces retrieving real-time input data from the controller and dumping it to the serial monitor for analysis. It helps developers understand how the controller's inputs can be accessed and interpreted.

#### Code Walkthrough

- **Dump Function**: The `dumpGamepad()` function retrieves key data points from the controller, such as:
  - **Joystick Axes (`axisX`, `axisY`, `axisRX`, `axisRY`)**: These represent the X and Y positions of the left and right joysticks.
  - **D-pad and Buttons**: Represented as bitmasks for easily determining the state of each input.
  - **Accelerometer and Gyroscope Data**: Provides motion-sensing capabilities for advanced control.
- **Serial Output**: Outputs the retrieved data in a structured format for debugging or analysis.

This section provides a clear understanding of game controller input structures, preparing developers for integration with hardware.

---

## Part 03: Mapping Controller Inputs to LED Brightness

### Introduction

In this section, you will learn how to map the input from the game controller’s joystick to control the brightness of an LED. This exercise demonstrates how controller inputs can be used for real-time hardware interaction.

### Objective

- Understand how to retrieve joystick input values.
- Learn to map input values to a hardware output range.
- Control LED brightness using joystick movements.
- Explore the use of PWM for LED dimming.

### Background Information

- **PWM (Pulse Width Modulation)**: Used to simulate analog output for controlling devices like LEDs.
- **Mapping Values**: The `map()` function scales an input range to an output range.
- **Joystick Axis Input**: Game controllers output analog values for joystick movements, which can be utilized for dynamic hardware control.

### Components

- ESP32 development board
- Bluetooth-compatible game controller
- LED
- Resistor (appropriate for the LED)
- Breadboard and jumper wires
- USB cable for programming and power

### Instructional

1. **Setup LED Output**: Connect an LED to the ESP32 with an appropriate resistor.
2. **Access Joystick Data**: Use the `axisRY()` method to retrieve the joystick's Y-axis value.
3. **Map Axis Value**: Convert the joystick’s range (-511 to 512) to a brightness range (0 to 255) using the `map()` function.
4. **Constrain Brightness**: Use `constrain()` to ensure the brightness value stays within the valid range.
5. **Apply PWM to LED**: Use the `analogWrite()` function to set the LED brightness based on the mapped value.
6. **Monitor LED Behavior**: Observe how the LED’s brightness changes as the joystick is moved.

## Example

### Introduction

The following example maps the right joystick’s Y-axis value to control an LED’s brightness. The joystick input is processed in real-time, and the LED responds by dimming or brightening accordingly.

### Example Code

```cpp
void processGamepad(ControllerPtr ctl) {
    int axisValue = ctl->axisRY();  // Get the right Y-axis value (-511 to 512)

    // Map axis value to PWM range (0 to 255)
    int brightness = map(axisValue, -511, 512, 255, 0);  // -511 is max bright, 512 is dim
    brightness = constrain(brightness, 0, 255);  // Ensure valid range

    analogWrite(LED, brightness);  // Apply PWM to LED pin

    // Optional: Dump gamepad data for debugging
    dumpGamepad(ctl);
}
```
### Analysis

This part builds on the previous sections by using the joystick’s Y-axis input to control an LED’s brightness, demonstrating the practical application of game controller inputs.

#### Code Walkthrough

- **Retrieve Input**: The `axisRY()` method fetches the joystick’s vertical position, which ranges from -511 (up) to 512 (down).
- **Mapping and Constraining Values**:
  - `map(axisValue, -511, 512, 255, 0)` converts joystick input to PWM-compatible brightness levels (0–255).
  - `constrain(brightness, 0, 255)` ensures the value stays within the valid range.
- **Analog Output**: The `analogWrite(LED, brightness)` function adjusts the LED’s brightness using PWM.

#### Implementation

This simple yet effective demonstration bridges input data processing and real-world hardware control. It highlights the ESP32’s capability for real-time interactive applications.

---

## Additional Resources

### Useful links

- (https://racheldebarros.com/esp32-projects/connect-your-game-controller-to-an-esp32/)
- https://github.com/ricardoquesada/bluepad32-arduino
