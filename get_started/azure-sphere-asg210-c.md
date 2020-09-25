---
platform: azure sphere
device: asg210
language: c
---

Get Started with Microsoft Azure IoT using WIZnet Azure Sphere Guardian ASG210
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Connect to Azure Service](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to configurate Azure Sphere development environment and to connect ASG210 with Microsoft Azure Sphere Services. This multi-step process includes:

-   Configuring Azure Sphere development environment
-   Building ASG210 application and preparing your device
-   Connect with Azure IoT and transmit data

**About ASG210**

[ASG210](https://doc.wiznet.io/docs/Product/Azure-Sphere/asg210/) is designed for the existing system, Brown-field, which is reqired to data communication with Cloud service or remote access, on Azure Sphere Security system which ganuntees high-level security in hardware & network side. ASG210 supports a variety of interfaces, GPIOs/I2C/UART(RS232/485/422) to help reducing additional cost for Brown-field system introducing high-level security system and Cloud service.

-   ASG210 picture

 ![](./media/asg210/ASG210.png)

<a name="Prerequisites"></a>
# Step 1: Prerequisites

If you have an Azure Sphere development environment that has not yet been configured, complete the below process:

-   [Set development environment](https://docs.microsoft.com/en-us/azure-sphere/install/overview)
-   Install Azure Sphere SDK
  -   [Install Azure Sphere SDK for Window OS](https://docs.microsoft.com/en-us/azure-sphere/install/install-sdk?pivots=visual-studio)
  -   [Install Azure Sphere SDK for Linux OS](https://docs.microsoft.com/en-us/azure-sphere/install/install-sdk-linux?pivots=vs-code-linux)

You should have the following items ready before beginning the process:

-   Desktop of laptop computer
-   MicroUSB cable
-   MS Azure Account
-   ASG210 and ASG210 Dubugger board

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

## ASG210 Debugger

First of all, ASG210 has 18 header pins, supporting UARTs and SWD interface, for ASG210 Debugger board. To program, setup and debug ASG210, connect between PC and ASG210 via USB-to-UART from Debugger board. To use ASG210 Debugger board, it should be ready to program Debugger board FTDI.

Please follow these steps described in this link:

-   [FTDI FT_PROG programming tool](https://docs.microsoft.com/en-us/azure-sphere/hardware/mt3620-mcu-program-debug-interface#ftdi-ft_prog-programming-tool)

## ASG210 Development Environment

### Azure Sphere CLI

The azsphere.exe command-line utility supports commands that manage Azure Sphere elements. For the more details, enter the below link:

-   [azsphere command-line utility](https://docs.microsoft.com/en-us/azure-sphere/reference/overview)

 ![](./media/asg210/azure_sphere_cli.png)

On Azure Sphere Developer Command Prompt Preview, the option, -?, helps to show the command information.

### Register User Account

To manage Azure Sphere elements for development, log in Azure Sphere Developer Command Prompt Preview with Microsoft account. To use Azure Sphere Security Service, Microsoft Account is required.

-   Log in on azsphere login command (Needed the option, –-newuser, with azsphere login command to register the account only have to sign in once.)

        azsphere login --newuser <MS account>

### Azure Sphere Tenant

An Azure Sphere tenant provides a secure way for your organization to remotely manage its Azure Sphere devices in isolation from other customer’s devices. And it is accessed based on RBAC (Role Based Access Control). Only people with an account in that directory will be able to manage devices within your Azure Sphere tenant.

**Role assigned an account in the Azure Sphere tenant**

Follow these steps to select the role assigned Azure Sphere tenant:

-   Search the tenant list.

        azsphere tenant list

-   Select the tenant from the list with tenant id.

        azsphere tenant select -i <tenant id>

-   Check the selected tenant.

        azsphere tenant show-selected

**Create new tenant**

There is no existed Azure Sphere tenant or assigned role in it. User can create new Azure Sphere tenant.

-   Create new tenant

        azsphere tenant create -n <tenant name>

### ASG210 Claim

Check the selected tenant for development environment. Once ASG210 claimed to the Azure Sphere tenant, claiming to other tenant is prohibited followed Azure Sphere Security policy.

-   Claim ASG210 to the selected tenant

        azsphere device claim

### ASG210 Configuration

**Recovery interface**

Once ASG210 is connected to the internet, Azure Sphere OS updates are initiated automatically via OTA (Over The Air) Wi-Fi interface. Also, user can manually update Azure Sphere OS with recovery. Recovery is the process of replacing the latest system software on the device using a special recovery bootloader instead of cloud update.

Follow these steps to update the latest Azure Sphere OS:

-   Set Wi-Fi interface

        azsphere device wifi add –ssid <SSID> --psk <Password>

-   Check Wi-Fi Status

        azsphere device wifi show-status

-   Recovery for Azure Sphere OS update

        azsphere device recover

-   Check Azure Sphere OS version

        azsphere device show-os-version

**Development Mode**

Connect Debugger board which is attached to ASG210 debug interface to PC and set development mode for debugging on In Azure Sphere Developer Command Prompt Preview.

On development mode, OTA is inactivated.

-   Development mode for debugging (Add option for RT App debugging)

   -   `--enablertcoredebugging` option requires administrator privilege because it installs USB drivers for the debugger.

   -   Right-click the Azure Sphere Developer Command Prompt shortcut and select More>Run as administrator.

            azsphere device enable-development --enablertcoredebugging

<a name="Build"></a>
# Step 3: Connect to Azure services

## Run Azure IoT sample

For the ASG210 application, chapter 5, Development Environment, is preceded.

ASG210 application has two types of applications, High-level application and Real-time capable application.
To Clone this repository:

    $ git clone https://github.com/WIZnet-Azure-Sphere/ASG210_App

Refer each application page for details on how to use.

### Real-time capable Application: W5500 SPI BareMetal

-   [Run Real-time capable Application: W5500 SPI BareMetal](https://github.com/WIZnet-Azure-Sphere/ASG210_App/tree/master/Software/ASG210_RTApp_W5500_SPI_BareMetal)

### High-level Application: AzureIoT

-   [Run High-level Application: AzureIoT](https://github.com/WIZnet-Azure-Sphere/ASG210_App/tree/master/Software/ASG210_HLApp_AzureIoT)

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

- [Manage cloud device messaging with iothub-explorer]
- [Save IoT Hub messages to Azure data storage]
- [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
- [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
- [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
- [Remote monitoring and notifications with Logic Apps]

[manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[save iot hub messages to azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[use power bi to visualize real-time sensor data from azure iot hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[use azure web apps to visualize real-time sensor data from azure iot hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[weather forecast using the sensor data from your iot hub in azure machine learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[remote monitoring and notifications with logic apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[setup-devbox-windows]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[wizfi360]: https://wizwiki.net/wiki/doku.php/products:wizfi360:start
[wizfi360-evb-shield]: https://wizwiki.net/wiki/doku.php/products:wizfi360:board:wizfi360-evb:start
[quickstart: create a stream analytics job by using the azure portal]: https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-quick-create-portal#next-steps
[communicate with your iot hub using the mqtt protocol: using the mqtt protocol directly (as a device)]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-mqtt-support#using-the-mqtt-protocol-directly-as-a-device

