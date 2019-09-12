---
platform: freertos
device: azurekit esp32language: c---
Connect AzureKit_ESP32 device to your Azure IoT Central Application===---
# Table of Contents
-   [Introduction](#Introduction)-   [Step 1: Prerequisites](#Prerequisites)-   [Step 2: Device Connection Details](#Deviceconnectiondetails)-   [Step 3: Prepare the Device](#Preparethedevice)-   [Step 4: Integration with IoT Central](#IntegrationwithIoTCentral)-   [Step 5: Additional Links](#AdditionalLinks)
<a name="Introduction"></a># Introduction 
**About this document**
This document describes how to connect AzureKit_ESP32 to Azure IoT Central application using the IoT plug and Play model. Plug and Play simplifies IoT by allowing solution developers to integrate devices without writing any device code. Using Plug and Play, device manufacturers will provide a model of their device to cloud developers to be integrated quickly into IoT Central or any solution built on the Azure IoT platform. IoT Plug and Play will be open to the community by way of a definition language and SDKs.
<a name="Prerequisites"></a># Step 1: PrerequisitesYou should have the following items ready before beginning the process: -   [Azure Account](https://portal.azure.com)-   [Azure IoT Hub Instance](https://docs.microsoft.com/en-us/azure/iot-hub/about-iot-hub)-   [Azure IoT Hub Device Provisioning Service](https://docs.microsoft.com/en-us/azure/iot-dps/about-iot-dps)-   Provide Network connectivity (Wifi, LAN) supported by the device-   Its mandatory that the device code/software image is preinstalled in device to enable Plug and Play
**Note:** If the device code is not preinstalled following are the [options](#preparethedevice) to choose to enable the plug and play device
# Step 2: Device Connection Details.
Please describe exactly how the devices use the DPS configuration (ID Scope, SAS Key, Device ID, Registration ID) to provision to IoT Central.

-   Create an IoT Central application using the Preview application template: [readme](https://docs.microsoft.com/en-us/azure/iot-central/quick-deploy-iot-central-pnp)
-   From the device templates, Click +New and choose the pre-certified device(AzureKit_ESP32) to create Device templates
-   Create a leaf certificate. (for creating a test certificate, you can use the [tool](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) in IoT hub C SDK.)
-   Make sure that add the new certificate to Azure IoT Central application (**Administration -> Device Connection -> Certificates(X.509)**)

# Step 3: Prepare the Device.
-   Setup the ESP-IDF development environment by following this guide [get-started](https://docs.espressif.com/projects/esp-idf/en/latest/get-started)

## Option 1
-   For the partners using the Microsoft PnP SDK samples
-   Copy the generated certname-all.pem and certname-private.pem to the .\examples\azure\_iot\_pnp\_sample\main\certs folder, and rename to leaf\_certificate.pem and leaf\_private_key.pem
-   Run make menuconfig, and update the values under Example Configuration section.  
      -   **Device Leaf Certificate Common Name:** the certname generated in step 2; 
      -   **Unique DPS ID Scope of Device provisioning service:** DPS scope ID, for AICS, you can find the scope id in the AICS setup page.
-   Run make flash to flash the app to AzureKit_ESP32.-   Compile the code by following this guide [get-started](https://docs.espressif.com/projects/esp-idf/en/latest/get-started).
# Step 4: Integration with IoT Central
-   AzureKit_ESP32 provide fan speed and temperature from IoT Central shows/visualize telemetry as follows:

![p1](./media/azurekit-esp32/1.png)
# Step 5: Additional Links
Please refer to the below link for additional information for Plug and Play -   [Blog](https://azure.microsoft.com/en-us/blog/iot-plug-and-play-is-now-available-in-preview/)-   [FAQ](TBD) -   [Plug and Play C SDK](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) -   [Plug and Play Node SDK](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview)-   [Plug and Play Definitions](https://github.com/Azure/IoTPlugandPlay)