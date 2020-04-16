# Prepare your development environment

This document describes how to prepare your development environment to use the *Microsoft Azure IoT SDK for Node.js*.

- [Setup your development environment](#devenv)
- [Sample applications](#readme)

<a name="devenv"/>
## Setup your development environment

Complete the following steps to set up your development environment:
- Ensure that Node.js version 0.12.x or later is installed. Run `node --version` at the command line to check the version. For information about using a package manager to install Node.js on Linux, see [Installing Node.js via package manager][node-linux].

- When you have installed Node.js, clone the latest version of this repository ([azure-iot-sdk-node](https://github.com/Azure/azure-iot-sdk-node)) to your development machine or device. You should always use the **master** branch for the latest version of the libraries and samples.

- If you are using Windows, open the **Node.js command prompt** and navigate to the **node** folder in your local copy of this repository ([azure-iot-sdk-node](https://github.com/Azure/azure-iot-sdk-node)). Run the `build\dev-setup.cmd` script to prepare your development environment. Then run the `build\build.cmd` script to verify your installation.

- If you are using Linux, open a shell and navigate to the **node** folder in your local copy of this repository ([azure-iot-sdk-node](https://github.com/Azure/azure-iot-sdk-node)). Run the `build/dev-setup.sh` script to prepare your development environment. Then run the `build/build.sh` script to verify your installation.

<a name="samplecode"/>
## Sample applications

This repository contains various Node.js sample applications that illustrate how to use the Microsoft Azure IoT SDK for Node.js. To learn how to run a sample application that sends messages to an IoT hub, see [Getting started - running a Node.js sample application][getstarted].

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
-   [Remote monitoring and notifications with Logic Apps]   

[Manage cloud device messaging with iothub-explorer]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging
[Save IoT Hub messages to Azure data storage]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage
[Use Power BI to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi
[Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps
[Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning
[Remote monitoring and notifications with Logic Apps]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[getstarted]: node-run-sample.md
