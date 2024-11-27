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

This tutorial is designed to guide you through connecting any ESP development module to a Bluetooth game controller. Specifically, we will demonstrate using a wireless Xbox controller with the ESP32-S3 dev module to control a robotic arm. The primary objective is to establish a connection between the controller and the ESP32 module, enabling you to read all inputs effectively.

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
software: Arduino IDE
description: The Arduino Integrated Development Environment (IDE) is a cross-platform application used to write and upload programs to Arduino-compatible boards.
installation process: 
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

![image](https://github.com/user-attachments/assets/51e16b75-9ca7-4fa5-a70d-b401a36cd52b)
![image](https://github.com/user-attachments/assets/374baaf6-805f-44bd-a40a-626ceee32bdb)



### Analysis

- Callbacks: onConnectedController() is called when a new controller connects, printing its model name.
- Controller Management: An array myControllers tracks up to 4 connected controllers.
- Initialization: BP32.setup() sets the connection callbacks, and forgetBluetoothKeys() clears previous Bluetooth pairings for clean connections.

## Additional Resources

### Useful links

List any sources you used, documentation, helpful examples, similar projects etc.
