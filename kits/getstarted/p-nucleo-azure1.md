P-NUCLEO-AZURE1 kit connected to Azure IoT 
===
---

# Table of Contents

-   P-NUCLEO-AZURE1 kit
-   Connect the sensors
-   Build and Run the sample
-   Send Device Events to IoT Hub
-   Receive messages from IoT Hub
-   Next steps


# P-NUCLEO-AZURE1 kit

The P-NUCLEO-AZURE1 kit includes:

-   IoT Wi-Fi node based on STM32 NUCLEO-F476RG (STM32 [NUCLEO-F476RG][lnk-nucleo-l4] + [Wi-Fi expansion board][lnk-nucleo-wifi]) 
-   [X-NUCLEO-IKS01A2][lnk-nucleo-mems] (Motion MEMS and environmental sensor expansion board for STM32 Nucleo)
-   [X-NUCLEO-NFC01A1][lnk-nucleo-nfc] (Dynamic NFC tag expansion board based on M24SR for STM32 Nucleo)
-   Micro USB to USB cable 

![p-nucleo-azure](images/p-nucleo-azure1-intro.png)


# Connect the sensors 

Combine STM32 NUCLEO-F476RG, Wi-Fi, Sensors and NFC expansion boards using the ST morpho and Arduino UNO R2 connectors, 
following the order shown in the picture. 

![p-nucleo-azure-connect](images/p-nucleo-azure1-connect.png)

Then connect P-NUCLEO-AZURE1 to your laptop using the Micro USB to USB cable.

# Build and Run the sample

### Check pre-requisites

  1. One of the following IDE installed: [Keil MDK-ARM][lnk-ide-keil], [IAR Embedded Workbench][lnk-ide-iar], [AC6 System Workbench for STM32][lnk-ide-sw4stm32] ([SystemWorkbench for STM32][lnk-ide-sw4stm32] is used as reference in this tutorial; requires Java SW JRE 7 or newer)
  2. [STM32 ST-Link Utility][lnk-st-link] (Windows OS only)
  3. Serial line monitor (i.e. [TeraTerm][lnk-teraterm] for WindowsOS; [TeraTerm][lnk-teraterm] is used as reference in this tutorial)
  4. For NFC usage, any Android application capable to read/write NFC tag ([ST25 NFC App][lnk-nfc-app] is used as reference in this tutorial)
  5. [DeviceExplorer][lnk-dev-exp] (Windows OS only) or [iothub-explorer][lnk-iot-exp] ([iothub-explorer][lnk-iot-exp] is used as reference in this guide, requires Node.js version 4.x or newer)
  6. Set-up your IoT Hub following instructions provided [here][lnk-setup-iot-hub]

### Download firmware

Download [FP-CLD-AZURE1][lnk-fp-cld-azure] Function Pack (__**[temporary link][lnk-fp-cld-azure]**__). 
The Function Pack contains all the required drivers 
to use the [NUCLEO-F476RG][lnk-nucleo-l4] board with Wi-Fi, sensors and NFC expansion boards, 
together with pre-integrated Microsoft Azure IoT SDK. 

### Import solution file

Unzip the package and open one of the pre-configured project files available 
in ```Projects/STM32L476RG-Nucleo/Applications/Azure_Sns_DM```, according to the IDE installed 
(for [SystemWorkbench for STM32][lnk-ide-sw4stm32] project files can be found inside folder ```SW4STM32```). 
In [SystemWorkbench for STM32][lnk-ide-sw4stm32] select the project 
from menu ```File -> Import -> Existing Projects into Workspace```; browse 
folders and select as root 
directory ```Projects/STM32L476RG-Nucleo/Applications/Azure_Sns_DM/SW4STM32/STM32L476RG-Nucleo``` then 
click ```Finish```.

![p-nucleo-azure-swimport](images/p-nucleo-azure1-sw-import.png)

### Insert connection string

Open  file ```azure1_config.h``` and update ```AZUREDEVICECONNECTIONSTRING``` with the credentials 
retrieved once completed device 
registration in IoT Hub as described in [Step 1.1][lnk-setup-iot-hub]. You have also to set here SSID and 
Password for Wi-Fi access point 
by replacing ```AZURE_DEFAULT_SSID``` and ```AZURE_DEFAULT_SECKEY```.

![p-nucleo-azure-connectstring](images/p-nucleo-azure1-sw-connstring.png)

### Build the project 

Build the project according to the selected IDE. In [SystemWorkbench for STM32][lnk-ide-sw4stm32], click on ```Project -> Build All``` (or 
shortcut ```Ctrl+B```).

![p-nucleo-azure-build](images/p-nucleo-azure1-sw-build.png)

### Flash the binary 

Flash the binary to [NUCLEO-F476RG][lnk-nucleo-l4] board. 
The sample application you have compiled include a procedure to implement 
Over-The-Air (OTA) Firmware upadate, which can be combined with [Azure IoT Hub primitives for device management][lnk-dev-man] (see following sections). 
Firmware update procedure requires a bootloader to be installed together with the Firmware binary; in order to properly flash both, scripts are 
provided for each IDE used. 
In [SystemWorkbench for STM32][lnk-ide-sw4stm32] scripts for Windows/OSx/Linux can be found 
in ```Projects/STM32L476RG-Nucleo/Application/Azure_Sns_DM/SW4STM32/STM32L476RG-Nucleo```. 
In Windows simply click on ```CleanAzure1mbedTLS.bat```

![p-nucleo-azure-script-win](images/p-nucleo-azure1-script-win.png)

For OSx/Linux, scripts require configuration according to your setup; edit ```OpenOCD_DIR``` and ```OpenOCD_CFC``` variables as shown 
in the following picture,
following the installation path of openocd binaries (```tools/openocd/```) and scripts (```openocd/scripts```). 

![p-nucleo-azure-script-osx](images/p-nucleo-azure1-script-osx.png)

### Configure your serial terminal and start the application

Configure your serial terminal as shown in the following picture for TeraTerm (baudrate set to 115200):

![p-nucleo-azure-serial](images/p-nucleo-azure1-serial.png)

Reset the kit by pressing the RESET button on [NUCLEO-F476RG][lnk-nucleo-l4] board to start the application.



# Send Device Events to IoT Hub

After RESET, the application join the Wi-Fi access point and connect to your Azure IoT Hub. When connection 
is established, 
the application transmits periodically messages containing inertial and environmental data read 
from [X-NUCLEO-IKS01A2][lnk-nucleo-mems] sensors board. 

LED2 onboard [NUCLEO-F476RG][lnk-nucleo-l4] blinks once connection with Azure IoT Hub is established. 
Application can be stopped by pressing USER button. 


Messages successfully transmitted to your Azure IoT Hub are printed over your serial terminal interface. 

![p-nucleo-azure1-msg-sent-terminal](images/p-nucleo-azure1-msg-sent-terminal.png)

To visualize messages received in IoT Hub with iothub-explorer, open Node.js command prompt and insert the 
following commands:
```
iothub-explorer login <iot-hub-connection-string>
iothub-explorer monitor-events <device name> --login <iot-hub-connection-string>
```


![[p-nucleo-azure-monitor](images/p-nucleo-azure1-monitor.png)


# Receive messages from IoT Hub
To send a message from IoT Hub to [NUCLEO-F476RG][lnk-nucleo-l4] board with iothub-explorer, open Node.js 
command prompt and insert the following commands:
```
iothub-explorer send <device name> <message> --ack=full
```


Messages received by STM32 [NUCLEO-F476RG][lnk-nucleo-l4] are printed over serial terminal interface once received. 
Some cloud-to-device messages are also interpreted by the application: 
-		Pause : pause the application 
-		Play : restart the application after a pause 
-		LedOn/LedOff : turn on/off LED2 onboard Nucleo
-		LedBlink : LED2 onboard Nucleo will blink for each message transmitted.

See [Manage IoT Hub][lnk-manage-iot-hub] to learn more on how to send cloud-to-device messages from IoT Hub.


# Trigger remote firmware update using Direct Methods and monitor device status with reported properties 
The application support firmware update procedure; when triggered, it stops its normal exection, download and install a new firmware version. 
By using [direct methods][lnk-direct-methods], it is possible to trigger firmware update procedure using iothub-explorer; 
open Node.js command prompt and insert the following commands:
```
iothub-explorer device-method <device name> <method name> <method properties> <timeout>
```

where
  -		```<method name>``` : ```FirmwareUpdate```
  -		```<method properties>``` : URI where to download the new firmware version, in 
  the format ```"{\"FwPackageUri\":\"https:....\"}"```
  -		```<timeout>``` : timeout to receive feedback from device 

It possible to monitor the execution of the firmware update procedure in the serial terminal:

![[p-nucleo-azure-fwupdate](images/p-nucleo-azure1-fwupdate.png)

The application also reports its current status to the IoT Hub; device status can be monitored in iothub-explorer with the following command:

```
iothub-explorer get-twin <device name>
```

![[p-nucleo-azure-fwupdate2](images/p-nucleo-azure1-fwupdate2.png)


# Use NFC to configure Wi-Fi Access Point parameters  

[NFC expansion board][lnk-nucleo-nfc] can be used to configure embedded application parameters. 
In FP-CLD-AZURE1 NFC board is used for example to modify SSID and PWD for Wi-Fi AP.
-		Install in an Android phone [ST25 NFC App][lnk-nfc-app] mobile app
-		Launch the application; click on Compose NDEF then select in menu the Wi-Fi option. Insert SSID and Password, then approach the 
mobile phone to the NFC expansion board and click on ```Write to tag``` 


![[p-nucleo-azure-android](images/p-nucleo-azure1-android.png)
-		Press Reset Button to restart the application
-		Press twice the User Button (Blue Button) when requested (after 3 seconds timeout default values will be used). The application 
will read NDEF parameters from NFC and will connect to the Access Point.


![[p-nucleo-azure-android](images/p-nucleo-azure1-nfc-serial.png)


# Next steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
-   [Save IoT Hub messages to Azure data storage](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
-   [Remote monitoring and notifications with ​​Logic ​​Apps](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
-   [Device management with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-iothub-explorer)
    -   Note: For your application to support Device Management (DM), you will need to make additional changes that are not covered in this document. We suggest that you look at the DM tutorial, to understand how you can add DM support to your application. However, if you do not modify your application accordingly, then you should not provide links to this tutorial.
	

[lnk-setup-iot-hub]: ../../setup_iothub.md	
[lnk-manage-iot-hub]: ../../manage_iot_hub.md
[lnk-nucleo-l4]:http://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluation-tools/mcu-eval-tools/stm32-mcu-eval-tools/stm32-mcu-nucleo/nucleo-l476rg.html
[lnk-nucleo-wifi]:http://www.st.com/content/st_com/en/products/ecosystems/stm32-open-development-environment/stm32-nucleo-expansion-boards/stm32-ode-connect-hw/x-nucleo-idw01m1.html
[lnk-nucleo-mems]:http://www.st.com/content/st_com/en/products/ecosystems/stm32-open-development-environment/stm32-nucleo-expansion-boards/stm32-ode-sense-hw/x-nucleo-iks01a2.html 
[lnk-nucleo-nfc]:http://www.st.com/content/st_com/en/products/ecosystems/stm32-open-development-environment/stm32-nucleo-expansion-boards/stm32-ode-connect-hw/x-nucleo-nfc01a1.html
[lnk-ide-sw4stm32]:http://www.openstm32.org/System+Workbench+for+STM32
[lnk-ide-keil]:http://www.keil.com/
[lnk-ide-iar]:http://www.iar.com/
[lnk-teraterm]:https://ttssh2.osdn.jp
[lnk-st-link]:http://www.st.com/content/st_com/en/products/embedded-software/development-tool-software/stsw-link004.html   
[lnk-minicom]:https://help.ubuntu.com/community/Minicom 
[lnk-iothub-explorer]:https://github.com/Azure/iothub-explorer
[lnk-direct-methods]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-direct-methods
[lnk-desired-prop]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins
[lnk-dev-man]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-overview
[lnk-fp-cld-azure]:https://onedrive.live.com/?authkey=%21AA3Nn3C4VxGpjqQ&id=D0937BB59FC4DE68#authkey=%21AA3Nn3C4VxGpjqQ&id=D0937BB59FC4DE68%21100702&cid=D0937BB59FC4DE68
[lnk-nfc-app]:https://play.google.com/store/apps/details?id=com.st.demo
[lnk-dev-exp]:https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iot-exp]:https://github.com/Azure/iothub-explorer 




