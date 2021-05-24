
# Tutorial: Installing OS Config private Preview on a Linux host

In this tutorial, you will learn how to:
> * Install Identity service via IoT Edge 1.2 and OS Config
> * Validate OS Config functionality

## Prerequisites

- Azure Account with an IoT Hub and Resource group already created.
- Desktop or laptop with [Azure IoT Exploer (Preview)](https://github.com/Azure/azure-iot-explorer/releases) installed.
- Desktop or laptop with Azure CLI installed.

## Signing into the Azure portal

To get started, you must sign in to the portal, where you'll be providing your device information, completing certification testing, and managing your device publications to the Azure Certified Device catalog.

1. Log into the [Azure portal](https://portal.azure.com).
1. Select IoT Hub
1. Click on "IoT Devices"
1. Create a device with a name

## Install the IoT Edge 1.2 with the Identity Service
[Install IoT Edge 1.2 for Linux](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge?view=iotedge-2020-11)

## Install OSConfig
1. Download the package from Collaborate
1. Upload the package to your device
    1. sftp [accountname]@[IP Address]
    1. put [path to osconfig_0.3.0_amd65.deb] 
1. Install the OSConfig package
    1. sudo apt install ./osconfig_0.3.0_amd64.deb

## Associate OSConfig with IoT Hub
1. Go into portal
1. Create a device identity
1. Give device credential to identity service so that it can connect to the IoT Hub Resource Group
 

## Validate OSConfig works
1. Open up IoT Explorer Preview
1. Click on IoT hubs on the left navigation
1. Click into your IoT Hub
1. Click on your device
1. Click on Module identities from the left navigation
1. Click on osconfig
1. Click on IoT Plug and Play components from the left navigation
1. Click on CommandRunner
1. Click on Properties (writeable) on teh upper tabs
1. Provide a CommandId (example "01-whoami")
1. Provide an "Arugments" (example "whoami")
1. Provide an action (example "RunCommand")
1. Click "Update desired value"
1. Wait a few minutes
1. Click on "Properties (read-only) from the upper tabs
1. Click "Click to view value"
1. Example results will show the CommandId, a resultCode of 0 for success and a TextResult of "root".