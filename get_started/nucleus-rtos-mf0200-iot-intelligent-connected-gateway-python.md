---
platform: nucleus rtos
device: mf0200 iot intelligent connected gateway
language: c
---

Run a simple C sample on MF0200 IoT Intelligent Connected Gateway device running Nucleus RTOS
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect MF0200 IoT Intelligent Connected Gateway device running Nucleus RTOS with Azure IoT SDK. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-python]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   MF0200 IoT Intelligent Connected Gateway device.
-   A computer with Nucleus development environment for MF0200 IoT Intelligent Connected Gateway installed. 
-   Terminal Emulator software.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

Building and running the sample involves the following steps:

-   Setting up and building a Nucleus System Project
-   Setting up and building an Azure IoT Device SDK Library Project
-   Setting up and building an Application Project

<a name="Build"></a>
# Step 3: Build and Run the sample

## Send Device Events to IoT Hub:

-   To send device events to the IoT Hub, you need to run one of the sample applications on the target. Once you have built the demo binary, follow steps below to execute on MF0200:

-   Copy the **&lt;application project name&gt;.bin** to an sd card.

-   Insert the sd card into MF0200 and power the board.

-   You will see some serial output from u-boot.

-   Once you get to a command prompt, use following two commands to run your application:

        fatload mmc 1 0x10000000 demo_test.bin
        go 0x10000000

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.


[setup-devbox-python]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/get_started/python-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
