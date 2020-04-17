---
platform: other os
device: nx102-9020
language: java
---

Connect NX102-9020 device to Azure IoT Hub
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Modify configurations and Run the sample](#Modify)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect NX102-9020 device to Azure IoT Hub. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Modifying configurations of the IoT device to connect to Azure IoT Hub

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   NX102-9020 device

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Shutdown NX102-9020 device
-   Make sure an SD Memory card is inserted to NX102-9020 device

<a name="Modify"></a>
# Step 3: Modify configurations and Run the sample

-   Remove the SD Memory Card from NX102-9020 device and insert it into PC
-   Open /SAP/iot\_gateway\_app/iotgateway.config in the SD Memory Card with a text editor
-   Modify following two parameters of the `OutputAzure` Node
    -   `ConnectionString`:
        -   Set connection strings you got in [provisioning device][lnk-manage-iot-hub_getting-connection-strings]
    -   `Protocol`:
        -   Set communication protocol to be used. Specify one of `HTTPS` / `AMQPS` / `MQTT` / `AMQPS_WS` / `MQTT_WS`. If omitted, `HTTPS`
-   Insert the SD Memory Card to NX102-9020 device
-   Restart NX102-9020 device

[lnk-setup-iot-hub]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md
[lnk-manage-iot-hub]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md
[lnk-manage-iot-hub_getting-connection-strings]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md#use-azure-cli-and-azure-cli-extensions-to-provision-a-device

