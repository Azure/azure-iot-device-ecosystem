---
platform: arduino
device: xinabox xchip cw02 wifi
language: c
---

Connect XinaBox xChip CW02 WiFi to Azure IoT Hub in the cloud
===
---

![ardino-cw02-microsoft-azure.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/arduino-cw02-microsoft-azure.png)

In this tutorial, you begin by learning the basics of working with your
XinaBox CW02 WiFi and SL06 xChips using [Arduino IDE](https://www.arduino.cc).

You will also learn how to connect your [CW02](http://wiki.xinabox.cc/CW02_-_Wi-Fi_%26_Bluetooth_Core) xChip device to the Azure
cloud by using [Azure IoT
Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-what-is-iot-hub).

## What you need to do

Connect the XinaBox [CW02](http://wiki.xinabox.cc/CW02_-_Wi-Fi_%26_Bluetooth_Core) WiFi xChip to an IoT hub that you create.

Run a sample application on xChip [CW02](http://wiki.xinabox.cc/CW02_-_Wi-Fi_%26_Bluetooth_Core) to fetch time from the internet,
and ambient light data from the xChip [SL06(APDS-9960)](http://wiki.xinabox.cc/SL06_-_Gesture).

This sample application will also send the collected data to your IoT
hub.

## What you will learn

-   How to create an IoT hub and register the XinaBox [CW02](http://wiki.xinabox.cc/CW02_-_Wi-Fi_%26_Bluetooth_Core) WiFi xChip.

-   How to connect the XinaBox [CW02](http://wiki.xinabox.cc/CW02_-_Wi-Fi_%26_Bluetooth_Core) WiFi xChip to your computer, and to the xChip [SL06(APDS-9960)](http://wiki.xinabox.cc/SL06_-_Gesture).

-   How to collect data from the xChip [SL06(APDS-9960)](http://wiki.xinabox.cc/SL06_-_Gesture) using the provided sample application which executes in the XinaBox [CW02](http://wiki.xinabox.cc/CW02_-_Wi-Fi_%26_Bluetooth_Core) WiFi xChip.

-   How to send the data collected from the internet and the [SL06(APDS-9960)](http://wiki.xinabox.cc/SL06_-_Gesture) xChip to your IoT hub.

## What you need for this tutorial

![xinabox_xchips_and_computer.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/xinabox_xchips_and_computer.png)

You will require the following before proceeding:

-   XinaBox IP01(FTDI) xChip, 

-   XinaBox CW02(ESP32) xChip, 

-   XinaBox [SL06(APDS-9960)](http://wiki.xinabox.cc/SL06_-_Gesture) xChip.

-   A Computer with the latest [Arduino IDE](https://www.arduino.cc).

-   A free USB socket on your computer.

-   Access to the internet via a wireless router.

## Create an IoT hub

1.  In the [Azure portal](https://portal.azure.com/),
    click **New** \> **Internet of Things** \> **IoT Hub**.

    ![azure_new_iothub.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/azure_new_iothub.png)

2.  In the **IoT hub** pane, enter the following information for your
    IoT hub:

    **Name**: The name you have chsen for your new IoT hub. If the name you enter is valid, a green check mark appears.
 
    **Pricing and scale tier**: Choose the free F1 tier, which is sufficient for your sample application. See [pricing and scale tier](https://azure.microsoft.com/pricing/details/iot-hub/).

    **Resource group**: Create a resource group to host the IoT hub or use an existing one. See [Using resource groups to manage your Azure resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-portal).

    **Location**: Choose a location closest to your location.

    **Pin the dashboard**: Check this option for easy access to your IoT hub from the dashboard.

    ![azure_iothub_create.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/azure_iothub_create.png)

3.  Click **Create**. It could take a few minutes for your IoT hub to be
    created. You can see progress in the **Notifications** pane.

    ![azure_notifications.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/azure_notifications.png)

4.  Once your IoT hub is created, click it from the dashboard. Make a
    note of the **Hostname**, and then click **Shared access policies**.

    ![azure_iothub.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/azure_iothub.png)

5.  In the **Shared access policies** pane, click the **iothubowner** policy, and then copy and make a note of the **Connection string** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-security).

    ![azure_iothub_shared_access.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/azure_iothub_shared_access.png)

## Register a device in the IoT hub for the your device

1.  In the [Azure portal](https://portal.azure.com/), open
    your IoT hub.

2.  Click **IoT devices under Explorers**.

3.  In the Device Explorer pane, click **Add** to add a device to your
    IoT hub.

    ![azure_iotdevices_explorer.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/azure_iotdevices_explorer.png)

    ![azure_iotdevices_createdevice.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/azure_iotdevices_createdevice.png)

4.  Add the following details.

     **Device ID:** The ID of the new device.

     **Authentication Type:** Select Symmetric Key.
  
     **Auto Generate Keys:** Check this field.

     **Connect device to IoT Hub:** Click Enable.

5.  Click **Save**.

6.  After the device is created, open the device in the **IoT devices Explorer** pane.

    ![azure_iotdevices_devicedetails.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/azure_iotdevices_devicedetails.png)

7.  Make a note of the **Connection string(secondary key)** and copy it for later use.

## Connect the XinaBox Wifi xChip [CW02](http://wiki.xinabox.cc/CW02_-_Wi-Fi_%26_Bluetooth_Core) and xChip [SL06(APDS-9960)](http://wiki.xinabox.cc/SL06_-_Gesture) to your computer

Make sure your XinaBox xChips are arranged as follows.

 ![cw02_sl06_ip01.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/cw02_sl06_ip01.png)

Plug the XinaBox [IP01](http://wiki.xinabox.cc/IP01_-_USB_Programming_Interface) xChip, with [CW02](http://wiki.xinabox.cc/CW02_-_Wi-Fi_%26_Bluetooth_Core) xChip and [SL06(APDS-9960)](http://wiki.xinabox.cc/SL06_-_Gesture) xChip connected as
above, into a USB port on your computer.

## Prepare the Arduino IDE for the XinaBox xChips

1.  Install [Arduino IDE](https://www.arduino.cc/en/Main/Software)

2.  Make sure to install the latest release.

## Install ArduinoCore for ESP32

Follow the instructions provided [here](https://github.com/espressif/arduino-esp32/blob/master/docs/arduino-ide/boards_manager.md) to get support for your CW02(ESP32) in Arduino IDE.

Once installed if you navigate to \"**Tools-\>Board**\" you will see that new boards have been added to the Arduino IDE as seen below

 ![arduino_esp32_boardfile_cw02.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/arduino_esp32_boardfile_cw02.png)

## Install the SL06 Sensor Library

1.  Go to this Github Page [XinaBox SL06](https://github.com/xinabox/arduino-SL06)
2.  Follow the installation guide provided on the page.
3.  Go to this Github Page [XinaBox CORE](https://github.com/xinabox/arduino-CORE)
4.  Follow the installation guide provided on the page.

**The next steps are:**

-   Select the XinaBox CW02 from the Board options
-   Select your Port for programming.

**After the above setup, the XinaBox CW02 is ready to be programmed.**

## Collect sensor data and send it to your IoT hub

In this section, you use the Arduino IDE to open, verify, and load a
sample application onto the XinaBox CW02 WiFi xChip.

The application collects time data off an internet time server, and data
from the XinaBox sensor xChip SL06, once every 30 seconds, and then
sends the data to your IoT hub.

### Download the sample application from GitHub and load it into the Arduino IDE

The XinaBox sample application is hosted on GitHub.

Clone the sample repository from GitHub that contains the sample
application.

To clone the sample repository, follow these steps:

1.  Open a command prompt or a terminal window.

2.  Go to a folder where you want the sample application to be saved.

3.  Run the following command:

        git clone
        https://github.com/XinaBoxSamples/iot-hub-xinabox-cw02-client-app.git

4.  In Arduino IDE, open \"**File-\>Open**\" and navigate to the folder which contains the sample application.

5.  Open the `app.ino` file of the sample application.

 ![arduino_app.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/arduino_app.png)

## Modify the sample application

1. Scroll down `app.in` to line 18, as seen below:

    ![credentials_input.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/credentials_input.png)

2. Modify the required fields to connect your Access Point,

3. Save the file,

**Your Arduino Sketch is now ready to be uploaded.**

## Don't have a real [SL06(APDS-9960)](http://wiki.xinabox.cc/SL06_-_Gesture) sensor?

The sample application can simulate ambient light in
case you don't have a real [SL06(APDS-9960)](http://wiki.xinabox.cc/SL06_-_Gesture) sensor. To set up the sample
application to use simulated data, follow these steps:

1.  Open the `app.ino` file in the app folder.

2.  Locate the below line of code and change the value from
    `False` to `True`:

    `SIMULATED_DATA = False`

    ![simulate_sl06.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/simulate_sl06.png)

3.  Save the file with Control-S.

## Deploy the sample application to the XinaBox CW02 WiFi xChip

1.  In the Arduino IDE, click the dedicated **upload** button.

    ![upload_button.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/upload_button.png)

**Verify the sample application is running successfully**

If you see the following output from the serial console window, the
sample application is running successfully.

Note that the message log below includes messages sent from the xChip
CW02 device to Azure, and also 1 message sent from Azure to the xChip CW02 device.

The following image shows how the message was sent from the
**Device-\>Message to device\** menu in Azure.

 ![azure_iotdevicedetails_messagetodevice.png](media/iot-hub-xinabox-cw02-wifi-xchip-arduino-get-started/azure_iotdevicedetails_messagetodevice.png)

## Next steps

You have successfully connected an XinaBox CW02 xChip to the Azure IoT
hub you created, and sent the captured sensor data to your IoT hub.

You have now learned how to run a sample application that collects
sensor data and sends it to an Azure IoT hub. To explore how to store,
analyze and visualize the data from this application in Azure using a
variety of different services, please click on the following lessons:

-   [[Manage cloud device messaging with
    iothub-explorer]](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)

-   [[Save IoT Hub messages to Azure data
    storage]](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage)

-   [[Use Power BI to visualize real-time sensor data from Azure IoT
    Hub]](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)

-   [[Use Azure Web Apps to visualize real-time sensor data from Azure
    IoT
    Hub]](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)

-   [[Weather forecast using the sensor data from your IoT hub in Azure
    Machine
    Learning]](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)

-   [[Remote monitoring and notifications with Logic
    Apps]](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
