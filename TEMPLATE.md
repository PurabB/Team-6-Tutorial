---
title: Connecting Game Controller to any ESP32 dev module with Bluetooth tutorial.
date: 11-25-2024
authors:
  - name: Purab Balani
  - name: Syler Sylvester
  - name: Aleksandar Jeremic
---

![relevant graphic or workshop logo](image/path)

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

## Part 01: Name

### Introduction

Briefly introduce what  you are teaching in this section.

### Objective

- List the learning objectives of this section

### Background Information

Give a brief explanation of the technical skills learned/needed
in this challenge. There is no need to go into detail as a
separation document should be prepared to explain more in depth
about the technical skills

### Components

- List the components needed in this challenge

### Instructional

Teach the contents of this section

## Example

### Introduction

Introduce the example that you are showing here.

### Example

Present the example here. Include visuals to help better understanding

### Analysis

Explain how the example used your tutorial topic. Give in-depth analysis of each part and show your understanding of the tutorial topic

## Additional Resources

### Useful links

List any sources you used, documentation, helpful examples, similar projects etc.
