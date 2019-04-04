# Introduction

**About this document**

Thank you for your interest in certifying your Microsoft Azure IoT Edge device. This document provides an overview of **Microsoft Azure IoT Edge device** certification, which is part of the Azure Certified for IoT program.

Azure Certified for IoT program continues to support the existing certification requirements for [IoT devices](https://github.com/Azure/azure-iot-device-ecosystem/tree/master/iotcertification) and [Starter kits](https://github.com/Azure/azure-iot-device-ecosystem/blob/master/kits/iotcertification/iot_certification_kit.md). The program is extending to support a new device type named **Azure IoT Edge** device which provides higher quality standards than the other IoT devices and Starter kits.

# Device Prerequisites

Your IoT Edge device is required to pre-install [Azure IoT Edge](https://github.com/Azure/iot-edge/blob/master/README.md) runtime to be certified as Azure IoT Edge device.  Pre-installing IoT Edge runtime in your device can occur at multiple stages in value chain.

-   Pre-install IoT Edge runtime at the OEM or ODM manufacturing facility.
-   Pre-install IoT Edge runtime that is supplied by OEM at the point of distribution. This is the scenario where any channels such as distributor(s), value-added reseller(s) etc. installs OEM supplied IoT Edge runtime.
-   If the channel takes OEM Edge device, and installs the channel specific IoT Edge runtime, the program accepts the submission as a different submission entity. In this case, the channel and OEM needs to agree on specifics on branding, device names etc that are shown in [device catalog](https://catalog.azureiotsolutions.com/).

IoT devices like Raspberry Pi3 etc can continue to run IoT Edge runtime. Azure Certified for IoT program is certifying against the pre-installed Edge runtime in the device controlled by either OEMs or channels to provide the best out-of-the-box experience on IoT Edge devices.

# IoT Edge Device certification program overview

IoT Edge certification program has the capability-based certification concept. Each capability has its own level to provide granularity of the IoT Edge device that device seekers are looking for, and allows the Azure Certified for IoT program to evolve in the future.

Each capability contains its own leveling with **Level 1** being the lowest. 

For the device to be certified as IoT Edge device, the device needs to pass all mandatory requirements:

-   [Mandatory] Edge runtime (Level 1 only)
-   [Mandatory] Device management (Level 1 only)
-   [Optional] Security (4 levels: Level 1 – 4)

# Certification Criteria: Description of capabilities and levels

Below describes the IoT Edge device certification criteria and associated capabilities for each level:

**Note:** Currently we only certify against [T1 OS](https://docs.microsoft.com/en-us/azure/iot-edge/support)

-   Device management: Basic device management operations (reboot, FW/OS upgrades) triggered by messages from IoT Hub.

Please click [here](https://github.com/Azure/azure-iotedge/blob/master/LICENSE) for MICROSOFT SOFTWARE LICENSE TERMS for IoT Edge runtime

# Next steps

You have now learned about the overview of Azure Certified for IoT program extending to support Azure IoT Edge devices.

Read more details about specific requirements and validation process to be certified as Azure IoT Edge devices. 

If you have any questions, please contact Azure Certified for IoT [iotcert@microsoft.com](mailto:iotcert@microsoft.com).

