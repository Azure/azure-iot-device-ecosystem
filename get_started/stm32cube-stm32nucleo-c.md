---
platform: STM32 Cube 

device: STM32 Nucleo F401RE

language: c

---

Run a simple C sample on STM32 Nucleo F401RE 
===
---

# Table of Contents
-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Next Steps](#NextSteps)


<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect STM32 [NUCLEO-F401RE][lnk-nucleo-f4] board together with [WiFi expansion board][lnk-nucleo-wifi] to the Microsoft Azure IoT Hub, by leveraging on Azure IoT Device SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering STM32 [NUCLEO-F401RE][lnk-nucleo-f4] to Azure IoT Hub
-   Build and deploy Azure IoT SDK on STM32 Nucleo
 

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process.

## 1.1 Development environment
- One among these three IDEs installed in your PC: [Keil MDK-ARM][lnk-ide-keil], [IAR Embedded Workbench][lnk-ide-iar], [AC6 System Workbench for STM32][lnk-ide-sw4stm32]
- For WindowsOS, the [STM32 ST-Link Utility][lnk-stlink] (requires registration to my.st.com)
- A serial terminal installed in your PC (e.g. [TeraTerm][lnk-teraterm] for Windows) 
- [Setup your IoT hub][lnk-setup-iot-hub]
- [Provision your device and get its credentials][lnk-manage-iot-hub]

#### NOTE
[SystemWorkbench for STM32][lnk-ide-sw4stm32] is the free integrated development environment for STM32, and it is used as reference in this guide.

## 1.2 Hardware components
 - STM32 Nucleo development board ([NUCLEO-F401RE][lnk-nucleo-f4])
 - Wi-Fi expansion board for STM32 Nucleo ([X-NUCLEO-IDW01M1][lnk-nucleo-wifi])

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
Combine [NUCLEO-F401RE][lnk-nucleo-f4] with [Wi-Fi expansion board][lnk-nucleo-wifi] as shown in the figure below. Then connect the [NUCLEO-F401RE][lnk-nucleo-f4] board to your PC using a mini USB cable.

![][1]
 

<a name="Build"></a>
# Step 3: Build and Run the sample 

<a name="Load"></a>
## 3.1 Build SDK and sample code

1. Download [FP-CLD-AZURE1][lnk-fp-cld-azure] Function Pack. The Function Pack contains all the required drivers to use the [NUCLEO-F401RE][lnk-nucleo-f4] board with Wi-Fi expansion boards, together with pre-integrated Microsoft Azure IoT SDK. 
2. Unzip the package and open one of the pre-configured project files available in ```Projects/STM32F401RE-Nucleo/Applications/Azure_Sns```, according to the IDE installed (for [SystemWorkbench for STM32][lnk-ide-sw4stm32] project files can be found inside folder ```SW4STM32```). 
3. In [SystemWorkbench for STM32][lnk-ide-sw4stm32] select the project from menu ```File -> Import -> Existing Projects into Workspace```; browse folders and select as root directory ```Projects/STM32F401RE-Nucleo/Applications/Azure_Sns/SW4STM32/STM32F401RE-Nucleo``` then click ```Finish```.
![][2]
4. Open  file ```azure1_config.h``` and update ```AZUREDEVICECONNECTIONSTRING``` with the credentials retrieved once completed device registration in IoT Hub as described in [Step 1.1][lnk-setup-iot-hub]. You have also to set here SSID and Password for Wi-Fi access point by replacing ```AZURE_DEFAULT_SSID``` and ```AZURE_DEFAULT_SECKEY```.
![][3]
5. Build the project according to the selected IDE. In [SystemWorkbench for STM32][lnk-ide-sw4stm32], click on ```Project -> Build All``` (or shortcut ```Ctrl+B```).
![][4]
6. Flash the binary to [NUCLEO-F401RE][lnk-nucleo-f4] board according to the selected IDE. In [SystemWorkbench for STM32][lnk-ide-sw4stm32] select ```Run -> Run Configurations```; click on ```Ac6 STM32 Debugging``` then ```STM32F4xx-Nucleo mbedTLS```  (see below); then in text box ```C/C++ Applications``` type in ```mbedTLS\STM32F4xx-Nucleo.elf```; then click on ```Apply``` and ```Run```.

![][5]



## 3.2 Connect STM32 Nucleo board to a WiFi access point 

To visualize log messages from [NUCLEO-F401RE][lnk-nucleo-f4] board, configure your serial terminal (e.g. [TeraTerm][lnk-teraterm] for Windows) with the following parameters 
- BaudRate : 115200
- Data : 8 bit
- Parity : none
- Stop : 1 bit 
- Flow Control : none

Press ```RESET``` button onboard [NUCLEO-F401RE][lnk-nucleo-f4] to restart the application; 
```LED2``` will blink once connection with Azure IoT Hub is established. Once connected to the IoT Hub, the application transmits periodically messages containing emulated sensors data.
Application can be stopped by pressing ```USER``` button. 
See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe in DeviceExplorer the messages IoT Hub receives from STM32 Nucleo.

Messages successfully transmitted to your Azure IoT Hub are also printed over your serial terminal interface. 




## 3.3 Receive messages from IoT Hub

See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages from IoT Hub.
Messages received by STM32 [NUCLEO-F401RE][lnk-nucleo-f4] are printed over serial terminal interface once received. 
Some cloud-to-device messages are also interpreted by the application: 
- Pause : pause the application (message to be entered in the form ```{"Name":"Pause", "Parameters":{}}``` )
- Play : restart the application after a pause (message to be entered in the form ```{"Name":"Play", "Parameters":{}}``` )
- LedOn/LedOff : turn on/off LED2 onboard Nucleo (message to be entered in the form ```{"Name":"LedOn", "Parameters":{}}``` )
- LedBlink : LED2 onboard Nucleo will blink for each message transmitted (message to be entered in the form ```{"Name":"LedBlink", "Parameters":{}}``` ).

A QuickStart guide to setup and run the application can also be downloaded from st.com at [this link][lnk-quickstart-st].

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
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[lnk-nucleo-f4]:http://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluation-tools/mcu-eval-tools/stm32-mcu-eval-tools/stm32-mcu-nucleo/nucleo-f401re.html
[lnk-nucleo-wifi]:http://www.st.com/content/st_com/en/products/ecosystems/stm32-open-development-environment/stm32-nucleo-expansion-boards/stm32-ode-connect-hw/x-nucleo-idw01m1.html
[lnk-fp-cld-azure]:http://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html
[lnk-ide-keil]:http://www.keil.com/
[lnk-ide-iar]:http://www.iar.com/
[lnk-ide-sw4stm32]:http://www.openstm32.org/System+Workbench+for+STM32
[lnk-teraterm]:https://ttssh2.osdn.jp
[lnk-quickstart-st]:http://www.st.com/content/ccc/resource/sales_and_marketing/presentation/product_presentation/group0/1f/8c/03/3b/a4/da/49/b4/FP-CLD-AZURE1%20quick%20start%20guide/files/fp-cld-azure1_quick_start_guide.pdf/jcr:content/translations/en.fp-cld-azure1_quick_start_guide.pdf

[1]: ./media/nucleol4.png
[2]: ./media/nucleol4-sw-import.png
[3]: ./media/nucleol4-sw-connstring.png
[4]: ./media/nucleol4-sw-build.png
[5]: ./media/nucleof4-run.png



