---
platform: windows 10 iot enterprise
device: cx5140 embedded pc
language: c
---

Run a simple C sample on CX5140 device running Windows 10 IoT Enterprise
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)


<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect a CX5140 device running Windows with Azure IoT Hub. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Install and configure TwinCAT IoT Data Agent

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

- [Setup your IoT hub][lnk-setup-iot-hub]
- [Provision your device and get its credentials][lnk-manage-iot-hub]
- Attach the CX5140 to your network by using the integrated Ethernet port
- Ensure that the CX5140 retrieved an IP address from a DHCP server

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

## Configure CX5140 for Azure IoT Hub by using TwinCAT IoT Data Agent

1.  Install TF6720 TwinCAT IoT Data Agent

2.  Open the Data Agent Configurator tool and add a new Azure IoT Hub gate

3.  Configure the gate with the device credentials from your Azure IoT Hub instance

## Documentation

The documentation can be found [here](http://infosys.beckhoff.com).

[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md