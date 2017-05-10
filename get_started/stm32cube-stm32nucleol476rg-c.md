---
platform: STM32 Cube 

device: STM32 NUCLEO-L476RG

language: c

---

Run a simple C sample on STM32 NUCLEO-L476RG 
===
---

# Table of Contents
-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)


<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect STM32 [NUCLEO-F476RG][lnk-nucleo-l4] board together with [WiFi expansion board][lnk-nucleo-wifi] to the Microsoft Azure IoT Hub, by leveraging on Azure IoT Device SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering STM32 [NUCLEO-F476RG][lnk-nucleo-l4] to Azure IoT Hub
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
 - STM32 [NUCLEO-F476RG][lnk-nucleo-l4] development board 
 - Wi-Fi expansion board for STM32 Nucleo ([X-NUCLEO-IDW01M1][lnk-nucleo-wifi])
 

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
Combine [NUCLEO-F476RG][lnk-nucleo-l4] with [Wi-Fi expansion board][lnk-nucleo-wifi] as shown in the figure below. Then connect the [NUCLEO-F476RG][lnk-nucleo-l4] board to your PC using a mini USB cable.

![][1]
 

<a name="Build"></a>
# Step 3: Build and Run the sample 

<a name="Load"></a>
## 3.1 Build SDK and sample code

1. Download [FP-CLD-AZURE1][lnk-fp-cld-azure] Function Pack (__**plese note: the version of FP-CLD-AZURE1 is being updated; web link may still be pointing at the older version of the firmware**__). The Function Pack contains all the required drivers to use the [NUCLEO-F476RG][lnk-nucleo-l4] board with Wi-Fi expansion boards, together with pre-integrated Microsoft Azure IoT SDK. 
2. Unzip the package and open one of the pre-configured project files available in ```Projects/STM32L476RG-Nucleo/Applications/Azure_Sns_DM```, according to the IDE installed (for [SystemWorkbench for STM32][lnk-ide-sw4stm32] project files can be found inside folder ```SW4STM32```). 
3. In [SystemWorkbench for STM32][lnk-ide-sw4stm32] select the project from menu ```File -> Import -> Existing Projects into Workspace```; browse folders and select as root directory ```Projects/STM32L476RG-Nucleo/Applications/Azure_Sns_DM/SW4STM32/STM32L476RG-Nucleo``` then click ```Finish```.
![][2]
4. Open  file ```azure1_config.h``` and update ```AZUREDEVICECONNECTIONSTRING``` with the credentials retrieved once completed device registration in IoT Hub as described in [Step 1.1][lnk-setup-iot-hub]. You have also to set here SSID and Password for Wi-Fi access point by replacing ```AZURE_DEFAULT_SSID``` and ```AZURE_DEFAULT_SECKEY```.
![][3]
5. Build the project according to the selected IDE. In [SystemWorkbench for STM32][lnk-ide-sw4stm32], click on ```Project -> Build All``` (or shortcut ```Ctrl+B```).
![][4]
6. Flash the binary to [NUCLEO-F476RG][lnk-nucleo-l4] board. The sample application you have compiled include a procedure to implement Over-The-Air (OTA) Firmware upadate, which can be combined with [Azure IoT Hub primitives for device management][lnk-dev-man]. 
Firmware update procedure requires a bootloader to be installed together with the Firmware binary; in order to properly flash both, scripts are provided for each IDE used. 
In [SystemWorkbench for STM32][lnk-ide-sw4stm32] scripts for Windows/OSx/Ubuntu can be found in ```Projects/STM32L476RG-Nucleo/Application/Azure_Sns_DM/SW4STM32/STM32L476RG-Nucleo```. 
In Windows simply click on ```CleanAzure1mbedTLS.bat```; for OSx and Ubuntu, scripts require configuration according to your setup, please refer to ```readme.txt``` in 
folder ```Projects/STM32L476RG-Nucleo/Application/Azure_Sns_DM``` to learn how to properly edit them. 





## 3.2 Connect and send messages to Azure IoT Hub 

To visualize log messages from [NUCLEO-F476RG][lnk-nucleo-l4] board, configure your serial terminal (e.g. [TeraTerm][lnk-teraterm] for Windows) with the following parameters 
- BaudRate : 115200
- Data : 8 bit
- Parity : none
- Stop : 1 bit 
- Flow Control : none

Press ```RESET``` button onboard [NUCLEO-F476RG][lnk-nucleo-l4] to restart the application; 
```LED2``` will blink once connection with Azure IoT Hub is established. Once connected to the IoT Hub, the application transmits periodically messages containing emulated sensors data.
Application can be stopped by pressing ```USER``` button. 
Messages successfully transmitted to your Azure IoT Hub are also printed over your serial terminal interface. 
![][6]


## 3.3 Receive messages from IoT Hub

See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages from IoT Hub.
Messages received by STM32 [NUCLEO-F476RG][lnk-nucleo-l4] are printed over serial terminal interface once received. 
Some cloud-to-device messages are also interpreted by the application: 
- Pause : pause the application 
- Play : restart the application after a pause 
- LedOn/LedOff : turn on/off LED2 onboard Nucleo
- LedBlink : LED2 onboard Nucleo will blink for each message transmitted.

The application also support [direct methods][lnk-direct-methods] and [desired properties][lnk-desired-prop] as alternative methods to remotely control the device.
 

[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[lnk-nucleo-l4]:http://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluation-tools/mcu-eval-tools/stm32-mcu-eval-tools/stm32-mcu-nucleo/nucleo-l476rg.html
[lnk-nucleo-wifi]:http://www.st.com/content/st_com/en/products/ecosystems/stm32-open-development-environment/stm32-nucleo-expansion-boards/stm32-ode-connect-hw/x-nucleo-idw01m1.html
[lnk-nucleo-fp]:http://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html
[lnk-ide-keil]:http://www.keil.com/
[lnk-ide-iar]:http://www.iar.com/
[lnk-ide-sw4stm32]:http://www.openstm32.org/System+Workbench+for+STM32
[lnk-teraterm]:https://ttssh2.osdn.jp
[lnk-stlink]:http://www.st.com/content/st_com/en/products/embedded-software/development-tool-software/stsw-link004.html   
[lnk-minicom]:https://help.ubuntu.com/community/Minicom 
[lnk-iothub-explorer]:https://github.com/Azure/iothub-explorer
[lnk-direct-methods]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-direct-methods
[lnk-desired-prop]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins
[lnk-dev-man]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-overview
[lnk-fp-cld-azure]:http://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html



[1]: ./media/nucleol4.png
[2]: ./media/nucleol4-sw-import.png
[3]: ./media/nucleol4-sw-connstring.png
[4]: ./media/nucleol4-sw-build.png
[5]: ./media/nucleol4-button.png
[6]: ./media/nucleol4-msg-sent-terminal.PNG

