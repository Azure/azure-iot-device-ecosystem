---
platform: yocto
device: fx30
language: javascript
---

Report data from your FX30 Octave to IoT Hub
===

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Setup your FX30 to report data to Octave cloud](#PrepareDevice)
-   [Step 3: Configure Octave to forward data to your IoT Hub](#Environment)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect Octave FX30 device running to IoT Hub. This multi-step process includes:
-   Configuring Azure IoT Hub and registering your IoT Hub device
-   Connect and configure your Octave FX30
-   Configure Octave to send your FX30 data to IoT Hub

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/setup_iothub.md)
-   [Provision your device and get its credentials](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md)
-   [FX30 Octave pre-requisites](https://docs.octave.dev/docs/getting-started-with-the-fx30#section-prerequisites)

**Important note:** Give your device the same name in IoT Hub and in Octave in order for the sample code below to work as such.

<a name="PrepareDevice"></a>
# Step 2: Setup your FX30 to report data to Octave cloud

![enter image description here](https://files.readme.io/b265052-sw-enabled_fx30.png)

Follow the FX30 Octave provided here: ["Getting started guide"](https://docs.octave.dev/docs/getting-started-with-the-fx30) up to Step 5 (included)

This will have you :

1. Set up and Verify Communication:

    ![enter image description here](https://files.readme.io/1f10427-octave-activate-device-1.png)

2. Commanding Resources (Optional)
3. Configuring sensor resources:

    ![enter image description here](https://files.readme.io/092ff2f-octave-fx30-adc-1.png)
4. Prepare an Observation for a Resource:

    ![enter image description here](https://files.readme.io/7dfbdd4-octave-fx30-observation-1.png)

    ![enter image description here](https://files.readme.io/e63d05f-octave-fx30-streams-3.png)

5. Sending data periodically to Octave cloud:

    ![enter image description here](https://files.readme.io/b91ea94-octave-fx30-store_and_forward.png)

After step 5 of this how-to, as we want to send data to IoT Hub, follow the steps indicated in the paragraphs below.

<a name="Build"></a>
# Step 3: Configure Octave to forward data to your IoT Hub

<a name="Load"></a>
## 3.1 Configure and retrieve your Shared access policy:

If this has not yet been done in the pre-requisites:
-   Create an IoT Hub instance in your Azure portal: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-through-portal

Create a device in IoTHub ("FX30_1" for instance):

-   Either from the IoT Hub Azure Portal UI
-   From the Cloud shell: 

        >az iot hub device-identity create --device-id FX30_1 --login "HostName=my-IoT-Hub.azure-devices.net;SharedAccessKeyName=Gateway;SharedAccessKey=eWfXkOxxxxxxxxxxWQ3g="

-   Or via IoT Hub API: (call with same Auth header than in the cloud action below : 

        "Authorization: 'SharedAccessSignature sr=my-Io........'" )
        API call to list devices: GET https://my-IoT-Hub.azure-devices.net/devices?api-version=2018-06-30
        API call to create device: PUT https://my-IoT-Hub.azure-devices.net/devices/FX30_1?api-version=2018-06-30



In order to send data from Octave to IoT Hub, we will use the method described here: <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-security#use-a-shared-access-policy>

From the Azure Portal UI, create a shared access policy for your IoT Hub with all rights enabled (it is called 'Gateway' in the example below), and launch the following command from the Cloud shell:

        > az iot hub generate-sas-token --du 36000000000 --login "HostName=my-IoT-Hub.azure-devices.net;SharedAccessKeyName=Gateway;SharedAccessKey=exxxxx"

This function call returns the SAS token that will be used to send device data to IoT Hub:
```
{"sas": "SharedAccessSignature sr=my-IoT-Hub.azure-devices.net&sig=Qei%2FbKJw%2Bnuhxxxxxxxxwo%3D&se=16088yyyyyyy19&skn=Gateway"}
```
To send data coming from any specific Octave device with that SAS token we will use and Octave Cloud Action, see the sample code in the following paragraph.

## 3.2 Configure Octave to forward data to IoT Hub


The principles of this Cloud Action is that:
-   "events" coming from your device Stream (/company_xxx/devices/device_yyy/sensor_2_cloud)  is pushed to IoT Hub through an HTTP.POST 
-   the POST used in the cloud action shall dynamically include the device name (FX30_1 in this example) in the 

        URI path : var result = Octave.Http.post('https://my-IoT-Hub.azure-devices.net/devices/FX30_1/messages/events?api-version=2018-06-30', postHeaders, postBody);

In order to configure your Cloud Action:
1.  Octave, Navigate to  **Cloud**  >  **Cloud Action**.
2.  Click  **Add Cloud Action**.
3.  In the form, choose the input stream containing Events from sensor (example: /company_xxx/devices/device_yyy/sensor_to_cloud)
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
- [Connecting your asset to an Octave device](https://docs.octave.dev/docs/overview-2) 
- [Control your device data flows](https://docs.octave.dev/docs/overview-1)
- [Orchestrate data in th cloud](https://docs.octave.dev/docs/overview-4)

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

