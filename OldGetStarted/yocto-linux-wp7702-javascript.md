---
platform: yocto linux
device: wp7702
language: javascript
---

Report data from your WP7702 powered device to IoT Hub
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Prerequisites](#Prerequisites)
-   [Step 1: Register your WP7702 on Octave](#Register)
-   [Step 2: Setup your device to report data to Octave cloud](#PrepareDevice)
-   [Step 3: Configure Octave to forward data to your IoT Hub](#Environment)
-   [Explore further](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect your Octave WP7702 powered device powered to IoT Hub. This multi-step process includes:
-   Configuring Azure IoT Hub and registering your IoT Hub device
-   Register your WP7702 on Octave
-   Configure your device (example given here with a  MangOH Yellow)
-   Configure Octave to send your device  data to IoT Hub

<a name="Prerequisites"></a>
# Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md)
-   [Provision your device and get its credentials](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md)
-   [MangOH Yellow pre-requisites (optional if you use your own hardware)](https://docs.octave.dev/docs/getting-started-with-the-mangoh-yellow#section-prerequisities)

Important note: Give your device the same name in IoT Hub and in Octave in order for the sample code below to work as such.

<a name="Register"></a>
# Step 1: Register your WP7702 on Octave

1.  Open the Octave  [user interface](https://octave.sierrawireless.io/)  in a browser on your development PC.

    ![enter image description here](https://files.readme.io/1f10427-octave-activate-device-1.png)

2.  Select WP7702 and activate your device entering the Serial Number (S/N) and IMEI printed on the side of the FX30, and enter a name you will use later to identify it on Octave.

    ![enter image description here](https://files.readme.io/ef088cd-activate_yellow_1.png)

<a name="PrepareDevice"></a>
# Step 2: Setup your device to report data to Octave cloud

![enter image description here](https://www.sierrawireless.com/-/media/iot/products/product%20selector/product%20images/wp%20series%20150x150.png)

In case you are using the WP7702 on your own custom hardware, you can follow the same step by step approach as the one given below. You only need customizing the resource you observe with your own device properties.

In this tutorial, we show how to report data with a MangOH Yellow open source hardware. To use a MangOH Yellow with a WP7702, source the MangOH Yellow "Basic" SKU: [https://mangoh.io/buy-boards](https://mangoh.io/buy-boards).

Follow the MangOH Yellow how-to provided here: ["Getting started guide"](https://docs.octave.dev/docs/getting-started-with-the-mangoh-yellow) up to Step 6 (included)

This will have you :

1.  Setup and load the latest Octave firmware
2.  Activate the MangOH in Octave: skip this step as you already did it for your module
3.  Commanding Resources (Optional)
4.  Configuring sensor resources:

    ![enter image description here](https://files.readme.io/092ff2f-octave-fx30-adc-1.png)
5. Prepare an Observation for a Resource:

    ![enter image description here](https://files.readme.io/7dfbdd4-octave-fx30-observation-1.png)

    ![enter image description here](https://files.readme.io/d786145-yellow_streams_4.png)

6. Sending data periodically to Octave cloud:

    ![enter image description here](https://files.readme.io/b91ea94-octave-fx30-store_and_forward.png)

After step 6 of this how-to, as we want to send data to IoT Hub, follow the steps indicated in the paragraphs below.

<a name="Build"></a>
# Step 3: Configure Octave to forward data to your IoT Hub
<a name="Load"></a>
## 3.1 Configure and retrieve your Shared access policy:

If this has not yet been done in the pre-requisites:

-   Create an IoT Hub instance in your Azure portal: <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-through-portal>

Create a device in IoTHub ("mangoh_1" for instance):

-   Either from the IoT Hub Azure Portal UI
-   From the Cloud shell:

        >az iot hub device-identity create --device-id mangoh_1 --login "HostName=my-IoT-Hub.azure-devices.net;SharedAccessKeyName=Gateway;SharedAccessKey=eWfXkOxxxxxxxxxxWQ3g="

-   Or via IoT Hub API: (call with same Auth header than in the cloud action below :

        "Authorization: 'SharedAccessSignature sr=my-Io........'" )
        API call to list devices: GET https://my-IoT-Hub.azure-devices.net/devices?api-version=2018-06-30
        API call to create device: PUT https://my-IoT-Hub.azure-devices.net/devices/mangoh_1?api-version=2018-06-30

In order to send data from Octave to IoT Hub, we will use the method described here: <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-security#use-a-shared-access-policy>

From the Azure Portal UI, create a shared access policy for your IoT Hub with all rights enabled (it is called 'Gateway' in the example below), and launch the following command from the Cloud shell:

    > az iot hub generate-sas-token --du 36000000000 --login "HostName=my-IoT-Hub.azure-devices.net;SharedAccessKeyName=Gateway;SharedAccessKey=exxxxx"

This function call returns the SAS token that will be used to send device data to IoT Hub:

    {"sas": "SharedAccessSignature sr=my-IoT-Hub.azure-devices.net&sig=Qei%2FbKJw%2Bnuhxxxxxxxxwo%3D&se=16088yyyyyyy19&skn=Gateway"}

To send data coming from any specific Octave device with that SAS token we will use and Octave Cloud Action, see the sample code in the following paragraph.

## 3.2 Configure Octave to forward data to IoT Hub

The principles of this Cloud Action is that:

-   "events" coming from your device Stream (/company\_xxx/devices/device\_yyy/light\_2\_cloud)  is pushed to IoT Hub through an HTTP.POST 
-   the POST used in the cloud action shall dynamically include the device name (mangoh\_1 in this example) in the URI path: 

        var result = Octave.Http.post('https://my-IoT-Hub.azure-devices.net/devices/mangoh_1/messages/events?api-version=2018-06-30', postHeaders, postBody);

In order to configure your Cloud Action:

1.  Octave, Navigate to  **Cloud**  >  **Cloud Action**.
2.  Click  **Add Cloud Action**.
3.  In the form, choose the input stream containing Events from sensor (example: /company\_xxx/devices/device\_yyy/light\_2\_cloud)
4.  Select the POST HTTP option: You cannot type in Javascript yet! but click to continue.
5.  Paste the code snippet given below,

Sample Cloud Action to push data to IoT Hub:

    function(event) {
    
    var deviceId = event.path.split("/")[3];
    
    /// Gateways SAS
    // Use your SAS token here below
    var sas = 'SharedAccessSignature sr=my-IoT-Hub.azure-devices.net&sig=Qei%2FbKJw%2Bnuh%2FCBxxxxxxxxxxxxx7jS5qcwo%3D&se=1608885019&skn=Gateway'; 
    
    // define the values specific to your use case, they should be defined as properties.desired of the Post body
    var payload = {
    "deviceId": deviceId,
    "properties": {
    "desired": {
    "sensor": event.elems.io.sensor}}};
    
    var postBody = JSON.stringify(payload);
    var postHeaders = {
    'Content-Type': 'application/json',
    'Authorization': sas
    };
    
    // When Using the generic SAS: 
    var result = BK.Http.post('https://my-IoT-Hub.azure-devices.net/devices/'+deviceId+'/messages/events?api-version=2018-06-30', postHeaders, postBody);
    
    // return is optional, it allows you to keep track of IoT Hub responses (in order to use this part, create the stream from Octave UI / Cloud / Streams / Add Stream
    
    return {
    // "/my_company/users/iotub_post_results/:reports": [new_event]
    };
    }

## 3.3 Monitor messages in IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

<a name="NextSteps"></a>
# Explore further

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. 

To learn further how to collect and orchestrate Edge data with your Octave device: 
-   [Connecting your asset to an Octave device](https://docs.octave.dev/docs/overview-2) 
-   [Control your device data flows](https://docs.octave.dev/docs/overview-1)
-   [Orchestrate data in th cloud](https://docs.octave.dev/docs/overview-4)

To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

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
[setup-devbox-python]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/get_started/python-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

