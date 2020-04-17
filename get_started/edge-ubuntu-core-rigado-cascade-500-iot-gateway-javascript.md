---
platform: edge-ubuntu core 16.04
device: rigado cascade-500 iot gateway
language: javascript
---

Run a simple JavaScript sample on Rigado Cascade-500 IoT Gateway device running Ubuntu Core 16.04
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#ExecuteTests)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect Rigado Cascade-500 IoT Edge device running Ubuntu Core 16.04 with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT Edge device
-   Execute Device Management manual test

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your edge device and get its credentials][lnk-manage-iot-hub]
-   Windows PC with [Git client](https://git-scm.com/download/gui/windows), [WinSCP](https://winscp.net/eng/download.php) and [PuTTY](https://www.putty.org/) installed
-   [Cascade-500 Eval Kit](https://www.rigado.com/cascade-evaluation-kit/)
-   [Edge Direct](https://www.rigado.com/products/iot-edge-as-a-service/) account access granted

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Follow the vendor instructuions [Set up your Gateway](https://docs.rigado.com/en/latest/eval-kit/setting-up-gateway.html). Plug power supply and Ethernet cable

-   [Submit a request](https://rigado.zendesk.com/hc/en-us/requests/new) to Rigado. Ask to provide [developer access (ssh)](https://en.wikipedia.org/wiki/Secure_Shell) to your Gateway

-   Wait untill the LED is solid green on the Gateway

-   Login to the [Edge Direct](https://app.rigado.com) account. Go to the `GATEWAYS` tab. Click on your gateway (You can find S/N of the gateway on the sticker). Go to `NETWORK` Tab. Discover IP-adress of `eth0` and use it for connection via ssh

<a name="ExecuteManualTests"></a>
# Step 3: Execute manual tests

<a name="Load"></a>
## 3.1 Load the Azure IoT bits and prerequisites on device

-   Open a WinSCP session and connect to the device.

-   Copy file [azure-iot-edge-device_1.0.0_armhf.snap](./azure-iot-edge-device_1.0.0_armhf.snap) to the device home directory.

-   Open a PuTTY session and connect to the device.

-   Install docker snap

-   Install certification toolkit on the gateway. Run the following command in bash:

        snap install ~/azure-iot-edge-device_1.0.0_armhf.snap --devmode --dangerous

-   Connect snap interfaces via executing bash:

        sudo /snap/azure-iot-edge-device/current/bin/connect-interfaces

## 3.1.1 Set Edge device connection string

In Azure, create and IoT Edge device and an IoT device for the gateway.  Get the connection strings for both.

-   Open a PuTTY session and connect to the device.

    bash:

        sudo snap set azure-iot-edge-device edge-device-connection-string='<Connection string for IoT Edge device'

        sudo snap set azure-iot-edge-device device-connection-string='<Connection string for IoT device'

-   To see Azure IoT Edge Security Daemon logs use bash:

        sudo snap logs -f azure-iot-edge-device.iotedged

-   To list running modules use

    bash:

        sudo azure-iot-edge-device.iotedge list


<a name="ExecuteTests"></a>
## 3.2 Execute Device Management manual test

### 3.2.1 Use of device twins to operating system update

-   On the gateway run the sample by issuing the following command with connection string as an argument and verify that gateway successfully updates installed snap.

        sudo azure-iot-edge-device.dmpatterns-fwupdate-device

-   In the Azure portal, open `IoT Hub` move to `Device details` `Direct method` set Method Namee `firmwareUpdate` and click Invoke Method.

See [Firmware update process][Implement a device firmware update process].

### 3.2.2 Use direct methods to trigger a reboot

-   On the gateway run the sample by issuing the following command with connection string as an argument and verify that gateway successfully rebooted installed snap.

        sudo azure-iot-edge-device.dmpatterns-reboot-device

-   In the Azure portal, open `IoT Hub` move to `Device details` `Direct method` set Method Namee `reboot` and click Invoke Method.

<a name="NextSteps"></a>
# Next Steps

You have now learned how register edge device and how to execute manual tests on the device, please click on the following lessons:

-   [Azure IoT Edge certification]
-   [Azure IoT Edge runtime and its architecture]
-   [Overview of device management with IoT Hub]
-   [Understand and invoke direct methods from IoT Hub]
-   [Implement a device firmware update process]
-   [Integrate your device HSM with Azure services and software]

[Azure IoT Edge certification]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/iotcertification/iotedge/iotedge_certification_linux/iotedge_certification_linux.md
[Azure IoT Edge runtime and its architecture]: https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/iot-edge/iot-edge-runtime.md
[Overview of device management with IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-overview
[Understand and invoke direct methods from IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-direct-methods
[Implement a device firmware update process]: https://docs.microsoft.com/en-us/azure/iot-hub/tutorial-firmware-update
[Integrate your device HSM with Azure services and software]: https://github.com/Azure/iotedge/blob/master/edgelet/hsm-sys/azure-iot-hsm-c/README.md

[lnk-setup-iot-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/quickstart-send-telemetry-dotnet#create-an-iot-hub
[lnk-manage-iot-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-overview