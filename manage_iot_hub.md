# Manage IoT Hub Devices

Before a device can communicate with IoT Hub, you must add details of that device to the IoT Hub device identity registry. When you add a device to your IoT Hub device identity registry, the hub generates the connection string that the device must use when it establishes its secure connection to your hub. You can also use the device identity registry to disable a device and prevent it from connecting to your hub.

To add devices to your IoT hub and manage those devices, you can use either of:

- The Command-line [Azure CLI](#azure-cli) tool
- The Windows-only, graphical [Azure IOT Explorer](#pnp-explorer)

Use either of these tools to generate a device-specific connection string that you can copy and paste in the source code of the application running on your device. Both tools are available in this [repository][lnk-this-repo].
 
> Note: While IoT Hub supports multiple authentication schemes for devices, both these tools generate a pre-shared key to use for authentication.

> Note: You must have an IoT hub running in Azure before you can provision your device. The document [Set up IoT Hub][setup-iothub] describes how to set up an IoT hub.

You can also use both of these tools to monitor the messages that your device sends to an IoT hub and send commands to you your devices from IoT Hub.

<a name="azure-cli"></a>
## Use Azure CLI and Azure CLI extensions to provision a device

The Azure IoT extension for Azure CLI aims to accelerate the development, management and automation of Azure IoT solutions. It does this via addition of rich features and functionality to the official Azure CLI.

Install [Azure CLI and CLI Extensions ](https://github.com/azure/azure-iot-cli-extension)

You must have at least v2.0.76, which you can verify with

     az --version

To provision a new device:

- Get the connection string for your IoT hub. See [Set up IoT Hub][setup-iothub] for more details.

*Note:- Use the IoT hub connection string to avoid the session login via "az login"*

- Create new device on your Hub by running below command:

        az iot hub device-identity create --device-id {device_id} --login {iothub_cs}

- Run the below command to get the connection string. Copy and save the device connection string information for later use
       
        az iot hub device-identity show-connection-string --device-id {device_id} --login {iothub_cs}
       
- The samples in this repository use connection strings in the format 

        HostName=<iothub-name>.azure-devices.net;DeviceId=<device-name>;SharedAccessKey=<device-key>

For further information about using the Azure CLI Extension to perform tasks such as create/disabling a device, monitoring a device, and sending commands to a device, please refer the below links:

- [Working with the device identity registry](https://docs.microsoft.com/en-us/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest)
- [Working with monitoring devices](https://docs.microsoft.com/en-us/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-monitor-events)

<a name="pnp-explorer"></a>
## Azure IOT Explorer for Windows

**Download a pre-built version**

Go to the [Releases](https://github.com/Azure/azure-iot-explorer/releases) tab, download the installer corresponding to your platform and install.

**Run it locally and build it yourself**

1.  Open a Node capable command prompt
2.  Clone the repo: `git clone https://github.com/Azure/azure-iot-explorer.git`
3.  Run: `npm install`
4.  Run: `npm start`
    - A new tab in your default browser will be opened automatically pointing to the locally running site.
5.  [optional] Stop step 4 then run: `npm run build` and then run: `npm run electron`.
    - The electron app will spin up using the bits generated in the dist folder.

If you'd like to package the app yourself, please refer to the [FAQ](https://github.com/Azure/azure-iot-explorer/wiki/FAQ).

Please click [here](https://github.com/Azure/azure-iot-explorer#features) to know about more features of Azure IoT Explorer such Configure and Manage IoT hub devices.

[setup-iothub]: setup_iothub.md
[lnk-this-repo]: https://github.com/Azure/azure-iot-sdks

