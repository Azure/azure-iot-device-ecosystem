---
platform: freertos
device: esp-samples
language: c
---

# Samples for Espressif (ESP) Microcontrollers

### Introduction
In this repository, you will find:

1.  Azure samples for Espressif boards located in the `samples` folder.
2.  Azure IoT SDK component for the ESP stable version located in the `components` folder. These components can be used in any ESP project which desires to connect to Azure IoT.
 Two versions of the component are provided within this repository:
	-   Azure IoT SDK without PnP 
	-   Azure IoT SDK with PnP preview
>If you are a beginner with Azure IoT or Espressif boards, please look at the **[Key Concepts](#key-concepts)** section below.

In addition to the current ESP32 and ESP8266 hardware offerings, Espressif now offers the [ESP32 Azure IoT Board](https://www.espressif.com/en/products/hardware/esp32-azure-kit), a development board that includes key sensors, OLED screen, and support for Wi-Fi & Bluetooth protocols. More information about this Azure certified board is available in the [Azure IoT catalog](https://catalog.azureiotsolutions.com/details?title=AzureKit-ESP32&source=all-devices-page).

>For any questions or suggestions, please open an issue and tag @ericmitt, @drajput, @tawalke

### Table of Contents
-   [Prerequisites](#prerequisites) - Required items to run samples
-   [Installing Toolchain & Development Framework](#installing-toolchain-and-development-framework) - Dev environment setup
-   [Sample Setup](#setup-the-samples) - How to setup and run samples
-   [Key Concepts](#key-concepts) - Understand Azure IoT & ESP
-   [Contributing](#contributing) - How to contribute to this repository

## Samples available in this repository

Sample|Description
---|---|
[azure-esp-starter](./samples/azure-esp-starter)|Send telemetry data from the ESP32 Azure device to Azure IoT Hub
[azure-esp-starter-pnp](./samples/azure-esp-pnp-starter)|Send telemetry data from ESP32 Azure device to Azure IoT Hub with Azure IoT Plug and Play
*

The projects in the samples folder follow the folder structure specified by Espressif for ESP-IDF projects. See more information about ESP-IDF and the ESP-IDF build system in the [Key Concepts](#key-concepts) section.

## Prerequisites

1.  **Azure subscription**: You will need an active Azure subscription. If you do not have one, you can register via one of these two methods:
    -   Activate a [free 30-day trial Microsoft Azure account](https://azure.microsoft.com/free/).
    -   Claim your [Azure credit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) if you are MSDN or Visual Studio subscriber.
2.  **Azure IoT Hub**: An active Azure IoT Hub  
3.  **ESP32**: An Espressif development board which is registered in Azure IoT Hub as a device.

>If you're not familiar with Azure IoT or need help setting up an Azure IoT Hub, please see **[Key Concepts](#key-concepts)** section.

## Installing Toolchain and Development Framework

To build and deploy a project on your ESP Board, you need to complete the following steps:

-   Install the ESP toolchain
-   Install the ESP development framework
-   Setup PATH for ESP environment

Please follow the steps corresponding to your operating system below to ensure your development environment is ready for ESP32 development. 

**Windows 10**

Using Windows Subsystem Linux (WSL):

*ESP32:*
You can install the ESP32 toolchain and ESP-IDF on your windows machine using WSL. Start Windows Subsystem for Linux by typing `WSL` in your start menu and running [this setup script](./install-script/esp-setup.sh).

For instructions on running the setup scripts, please review the details provided [here](./install-script/readme.md).
*This is the recommended approach for novice ESP32  developers using these samples.*

>For more information on setting up WSL on Windows 10 [see how to setup WSL on Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

Using MSYS2 environment on Windows:

You can install the ESP32 toolchain on your windows machine using the MSYS2 environments aka [MinGW](http://mingw.org/) on Windows. To install, follow the steps outlined in Espressif documentation for setup on Windows found [here](https://docs.espressif.com/projects/esp-idf/en/stable/get-started/windows-setup.html). The quick setup option provides a pre-prepared environment with all-in-one toolchain and MSYS2. There is also information around steps to update an existing Windows environment.

After setup of your environment and toolchain, you would setup ESP-IDF by following steps outlined in the Espressif documentation. Links to get the toolchain and setup path are provided below: 

-   Get [ESP-IDF]( https://docs.espressif.com/projects/esp-idf/en/stable/get-started/index.html#get-esp-idf) *(stable version)*
-   Setup [ESP-IDF Path](https://docs.espressif.com/projects/esp-idf/en/stable/get-started/add-idf_path-to-profile.html#add-idf-path-to-profile-windows) for Windows

**Linux**

To install the stable version of the ESP32 toolchain and ESP-IDF development environment on Linux, please follow the setup steps outlined in the Espressif documentation. The links for these steps are provided below.

-   Get [ESP32 toolchain for Linux](https://docs.espressif.com/projects/esp-idf/en/stable/get-started/linux-setup.html).
-   Get [ESP-IDF](https://docs.espressif.com/projects/esp-idf/en/stable/get-started/index.html#get-esp-idf)
-   Setup [ESP-IDF Path](https://docs.espressif.com/projects/esp-idf/en/stable/get-started/add-idf_path-to-profile.html#add-idf-path-to-profile-linux-macos)

**macOS**

To install the stable version of the ESP32 toolchain and ESP-IDF development environment on macOS, please follow the setup steps outlined in the Espressif documentation. The links for these steps are provided below.

-   Get [ESP32 toolchain for macOS](https://docs.espressif.com/projects/esp-idf/en/stable/get-started/macos-setup.html).
-   Get [ESP-IDF](https://docs.espressif.com/projects/esp-idf/en/stable/get-started/index.html#get-esp-idf)
-   Setup [ESP-IDF Path](https://docs.espressif.com/projects/esp-idf/en/stable/get-started/add-idf_path-to-profile.html#add-idf-path-to-profile-linux-macos)

## Setup the samples

Before running any of the samples within this repository, please ensure that you have an active Azure IoT Hub and setup the ESP-IDF development environment. Please see **[Prerequisites](#prerequisites)** section for more information.

1. Clone this repository in your working directory.

    For example, if you use WSL:

   1. Launch WSL (type WSL in the start menu).
   2. Once started, navigate to the mounted partition (this way you can see all the files with the Windows file explorer): ```cd /mnt/c``` (or any other directory under C)
   3. Then clone this repository:
        ```git clone https://github.com/Azure-Samples/ESP-Samples.git```

   4. Copy the Azure component [esp-azure or esp-azure-pnp](https://github.com/Azure-Samples/ESP-Samples/tree/master/components) into the components folder of the sample you want to build.
   5. Navigate to the project folder *(within the repository's samples/ folder)* for the sample you want to use and follow the instructions in that project's readme file.

> Tip: For using Azure IoT SDK in your existing projects, just copy/clone the esp-azure or esp-azure-pnp folder in your components folder. That's it. You're ready to build!

## Key concepts

### Azure IoT

If you are new to [Azure IoT](https://azure.microsoft.com/en-us/overview/iot/), it is good to understand the services available for working with hardware like Espressif and Azure IoT. Some of the key services for Azure IoT are:

-   [Azure IoT Hub](https://azure.microsoft.com/en-us/services/iot-hub/)
-   [Azure IoT Central](https://azure.microsoft.com/en-us/services/iot-central/) 

The Azure IoT service used in for the samples in repository is **Azure IoT Hub**. To learn how to setup an Azure IoT Hub and add devices, please follow the steps outlined in the Azure IoT documentation for creating an Azure IoT Hub. Links are provided below: 

-   [Creating an IoT Hub with Azure Portal](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-through-portal)
-   [Creating an IoT Hub with Azure CLI](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-using-cli)

To add your device, go to "IoT devices" within your IoT Hub in the Azure portal. Click New and fill the form this way:

![ESP32 sample](https://github.com/Azure-Samples/ESP-Samples/blob/master/media/adddevice.JPG)

For more information on working with devices with Azure IoT, check out the information noted in the documentation [here](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-get-started-physical).

### Espressif IoT

There are various development frameworks available for working with ESP devices. The samples in this repository use the **Espressif IoT Development Framework**, otherwise known as **ESP-IDF**. 

In using the ESP-IDF framework, it is recommended to utilize the ESP-IDF build system folder structure which brings a notion of “components”. Components to be used in your project, like Azure IoT, should be placed within the appropriate project folder with Makefile and/or CMakelists files at the root of each component's folder. Project code is then typically placed in the main folder.  ESP-IDF projects have the components used and the project code built and statically linked at the same time. Learn more about the ESP-IDF build system and starting ESP projects from scratch in the Espressif documentation for the [ESP-IDF build system](https://docs.espressif.com/projects/esp-idf/en/v3.3/api-guides/build-system.html).

> To use Azure IoT in your own custom project, you can use the [empty project template](https://github.com/espressif/esp-iot-solution/tree/master/examples/empty_project) provided by Espressif. Once your folder structure is set up, copy the desired Azure IoT component into your components folder. The components provided in this repo are: [azure-esp-sdk](./components/esp-azure) and [esp-azure-pnp](./components/esp-azure-pnp) *(Plug and Play)*

## Contributing

This project welcomes contributions and suggestions. Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit <https://cla.opensource.microsoft.com>.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
