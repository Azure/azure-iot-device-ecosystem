---
platform: rasbian linux
device: optiga tpm slb 9670 iridium board
language: c
---

Use a Infineon OPTIGA&trade; TPM SLx 9670 on a Raspberry Pi 3B running Raspbian Linux OS to test Azure Device Provisioning Service for Azure IoT Edge
===
---

## Create and provision an IoT Edge device with a Infineon OPTIGA&trade; TPM SLx 9670 on a Raspberry Pi 3B running Raspbian Linux OS.

Azure IoT Edge devices can be automatically provisioned using the [Device Provisioning Service](https://docs.microsoft.com/en-us/azure/iot-dps/index). If you're unfamiliar with the process of autoprovisioning, review the [autoprovisioning concepts](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-auto-provisioning) before continuing. 

This article shows you how to test autoprovisioning on a IoT Edge device with the following steps: 

-   Install Raspbian Linux OS (<https://raspberrypi.org>)
-   Enable Support for Infineon OPTIGA&trade; TPM SLx 9670 on Raspbian using a device tree overlay
-   Create an instance of IoT Hub Device Provisioning Service (DPS).
-   Create an individual enrollment for the device
-   Install the IoT Edge runtime and connect the device to IoT Hub

The steps in this article are meant for testing purposes.

## Prerequisites

-   A Raspberry Pi Model 3 including necessary accessories.
-   [Infineon IRIDIUM9670 Board](https://www.infineon.com/cms/en/product/evaluation-boards/iridium9670-tpm2.0-linux/)
-   An active IoT Hub and IoT Hub Device Provisioning Service (DPS)

### Supported Hardware

The following Infineon OPTIGA&trade; TPMs are supported and are available as Infineon IRIDIUM9670 Evaluation-Boards.

-   OPTIGA&trade; TPM SLB 9670 - for standard applications (TPM2.0 - Firmware Version 7.85)
-   OPTIGA&trade; TPM SLM 9670 - for industrial & demanding applications (TPM2.0 - Firmware Version  13.11)
-   OPTIGA&trade; TPM SLI 9670 - for automotive applications (TPM2.0 - Firmware Version  13.11)

In the following guide SLx 9670 will be used as a placeholder and includes all of the above.


## Install Raspbian on Raspberry Pi and Enable Support for Infineon OPTIGA&trade; TPM SLx 9670

In this section, you will install and configure Raspbian on a Raspberry Pi and enable support for the TPM, so that you can use it for testing how automatic provisioning works with IoT Edge.

1.  Download latest Raspbian (2018-11) and flash onto SD Card
   (see <https://www.raspberrypi.org/documentation/installation/installing-images/README.md> for details)
2.  Plugin IRIDIUM9670 Board on Raspberry Pi Header.
  -   The chips must be facing the outside of the Raspberry Pi.
  -   Pin 1 of the IRIDIUM9670 must align with Pin 1 of the Raspberry Pi.
  -   Pin 1 is also marked by a rectangular solder pad on the IRIDIUM9670 board.
3.  Plugin SD Card, Monitor, Keyboard, Mouse into Raspberry Pi and power it up.
4.  Follow basic Raspberry Pi Setup instructions, especially Wifi and User Password.
5.  Use 'raspi-setup' to enable SSH and SPI.
6.  Update your system with `sudo apt update && sudo apt upgrade`.
7.  Install mandatory packages via `sudo apt install git build-essential`.

### Enable Support for Infineon OPTIGA&trade; TPM SLx 9670
- Install latest kernel via `sudo rpi-update`.
- Edit */boot/config.txt* and add the following line:

        dtoverlay=tpm-slb9670

    (this tpm-slb9670 overlay applies to SLB 9670, SLI 9670 and SLM 9670).

-   Reboot your Raspberry Pi and check that */dev/tpm0* is available.


### Collect TPM data in Raspbian

On the Raspberry Pi, build a C SDK tool that you can use to retrieve the device's **Registration ID** and **Endorsement Key**. 

1.  On a Command line Window on the Raspberry Pi follow the following steps to install and build the Azure IoT device SDK for C. ([Set up a Linux development environment](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux)).

        sudo apt-get update
        sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
    Clone the [latest release](https://github.com/Azure/azure-iot-sdk-c/releases/tag/LTS_02_2020_Ref01) of SDK to your local machine using the tag name you found:

        git clone -b <lts_mm_yyyy> https://github.com/Azure/azure-iot-sdk-c.git
        cd azure-iot-sdk-c
        git submodule update --init
    
2. Run the following commands to build an C SDK tool that retrieves your device provisioning information. 

       mkdir azure-iot-sdk-c/cmake
       cd azure-iot-sdk-c/cmake
       cmake -Duse_prov_client:BOOL=ON ..
       cd provisioning_client/tools/tpm_device_provision
       make
       sudo ./tpm_device_provision
   *Note: These steps generate RSA Endorsement(EK) and SRK primary keys which may take 30s and up to 60s to generate new keys. For quicker provisioning using Infineon pre-provisioned EK and real deployment, please refer to [Edge Device Provisioning with Infineon Endorsement Key to Microsoft Azure DPS](https://www.infineon.com/sec/login?ret=https%3A%2F%2Fwww.infineon.com%2Fcms%2Fen%2Fproduct%2Fsecurity-smart-card-solutions%2Foptiga-embedded-security-solutions%2Foptiga-tpm%2F%23!documents%2Fdocument-group-myInfineon-65) for provisioning steps and automation scripts.* 

3. Copy the values for **Registration ID** and **Endorsement Key**. You use these values to create an individual enrollment for your device in DPS(example as below). 

       Registration Id:
       examplezw5g3fthisisexamplethisisexampleoqk2vaqkaggca
       
       Endorsement Key:
       AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAoDkgNpncAPwEGTRjPE0bptyh/drshy+80na3example0sGrAX9YtZIbmEmfYEnVXkGNztulPeoqNRONvf+Vuexample1ubEHexampleSM5W/dm7QScSGXbnMLUe/I7YhvexamplenJ5/exampleW72bAMGPBocS4OFIlB+95yCexampletSueexampleXbM6xaYhHXdyTaCjD/yTsc4NQlthZ1Cb2examplejrRexamplevHxO94f080EJjmZE5kS0zexample4eHwNBAyGKNGFTIBYy4brVb0g+fkG9FZcEzPJIM7KxDnTmM7bm+55VrrckHsrlOTWSgkeimxU7w==
## Set up the IoT Hub Device Provisioning Service

Create a new instance of the IoT Hub Device Provisioning Service in Azure, and link it to your IoT hub. You can follow the instructions in [Set up the IoT Hub DPS](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision).

After you have the Device Provisioning Service running, copy the value of  [**ID Scope**](https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-device#create-the-device-registration-software) from the overview page. You use this value when you configure the IoT Edge runtime. 

## Create a DPS enrollment

Retrieve the provisioning information from your device, and use that to create an individual enrollment in Device Provisioning Service. 

When you create an enrollment in DPS, you have the opportunity to declare an **Initial Device Twin State**. In the device twin, you can set tags to group devices by any metric you need in your solution, like region, environment, location, or device type. These tags are used to create [automatic deployments](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-deploy-monitor). 


1.  In the [Azure portal](https://portal.azure.com), and navigate to your instance of IoT Hub Device Provisioning Service. 

2.  Under **Settings**, select **Manage enrollments**. 

3.  Select **Add individual enrollment** then complete the following steps to configure the enrollment:  

   1.  For **Mechanism**, select **TPM**. 
   
   2.  Provide the **Endorsement key** and **Registration ID** that you copied from your device.
   
   3.  Select **True** to declare that this device is an IoT Edge device. 
   
   4.  Choose the linked **IoT Hub** that you want to connect your device to. You can choose multiple hubs, and the device will be assigned to one of them according to the selected allocation policy. 
   
   5.  Provide an ID for your device if you'd like. You can use device IDs to target an individual device for module deployment. If you don't provide a device ID, the registration ID is used.
   
   6.  Add a tag value to the **Initial Device Twin State** if you'd like. You can use tags to target groups of devices for module deployment. For example: 

            json
            {
               "tags": {
                  "environment": "test"
               },
               "properties": {
                  "desired": {}
               }
            }
   

   7.  Select **Save**. 

Now that an enrollment exists for this device, the IoT Edge runtime can automatically provision the device during installation. 

## Install the IoT Edge runtime

The IoT Edge runtime is deployed on all IoT Edge devices. Its components run in containers, and allow you to deploy additional containers to the device so that you can run code at the edge.  

Retrieve your DPS [**ID Scope**](https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-device#create-the-device-registration-software) and device **Registration ID** before beginning.  Make sure to configure the IoT Edge runtime for automatic, not manual, provisioning. For Raspberry Pi we use ARM32v7/armhf. See [Install the Azure IoT Edge runtime on Linux (ARM32v7/armhf)](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux-arm).
Ensure that the following items have been installed

   1.  Install the repository configuration for **[Raspbian Stretch](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#register-microsoft-key-and-software-repository-feed)** ,copy the generated list and install Microsoft GPG public key.

   2.  Install the [**Moby engine**](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#install-a-container-runtime) container runtime.

   3.  Install the [**Azure IoT Edge Security Daemon**](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#install-the-azure-iot-edge-security-daemon).

   4. Go to [**Option 2: Automatic provisioning**](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux#option-2-automatic-provisioning) and modify the **config.yaml** file according to the **TPM attestation** section.
## Give IoT Edge access to the TPM

In order for the IoT Edge runtime to automatically provision your device, it needs access to the TPM. 

You can give TPM access to the IoT Edge runtime by overriding the systemd settings so that the **iotedge** service has root privileges. If you don't want to elevate the service privileges, you can also use the following steps to manually provide TPM access. 

1. Find the file path to the TPM hardware module on your device and save it as a local variable. 

        tpm=$(sudo find /sys -name dev -print | fgrep tpm0 | sed 's/.\{4\}$//')


2. Create a new rule that will give the IoT Edge runtime access to tpm0. 

        sudo touch /etc/udev/rules.d/tpmaccess.rules

3. Open the rules file. 

        sudo nano /etc/udev/rules.d/tpmaccess.rules

4. Copy the following access information into the rules file. 

        input
        # allow iotedge access to tpm0
        KERNEL=="tpm0", SUBSYSTEM=="tpm", GROUP="iotedge", MODE="0660"
   
5. Save and exit the file. 

6. Trigger the udev system to evaluate the new rule. 

        /bin/udevadm trigger $tpm

7. Verify that the rule was successfully applied.

        ls -l /dev/tpm0

     Successful output looks like the following:

        output
        crw-rw---- 1 root iotedge 10, 224 Jul 20 16:27 /dev/tpm0

    If you don't see that the correct permissions have been applied, try rebooting your machine to refresh udev. 

## Restart the IoT Edge runtime

Restart the IoT Edge runtime so that it picks up all the configuration changes that you made on the device. 

    sudo systemctl restart iotedge

Check to see that the IoT Edge runtime is running. 

    sudo systemctl status iotedge

If you see provisioning errors, it may be that the configuration changes haven't taken effect yet. Try restarting the IoT Edge daemon again. 

    sudo systemctl daemon-reload

Or, try restarting your platform to see if the changes take effect on a fresh start. 

## Verify successful installation

If the runtime started successfully, you can go into your IoT Hub and see that your new device was automatically provisioned. Now your device is ready to run IoT Edge modules. 

Check the status of the IoT Edge Daemon.

    cmd/sh
    systemctl status iotedge

Examine daemon logs.

    cmd/sh
    journalctl -u iotedge --no-pager --no-full

List running modules.

    cmd/sh
    iotedge list


You can verify that the individual enrollment that you created in Device Provisioning Service was used. Navigate to your Device Provisioning Service instance in the Azure portal. Open the enrollment details for the individual enrollment that you created. Notice that the status of the enrollment is **assigned** and the device ID is listed. 

## Next steps

The Device Provisioning Service enrollment process lets you set the device ID and device twin tags at the same time as you provision the new device. You can use those values to target individual devices or groups of devices using automatic device management. Learn how to [Deploy and monitor IoT Edge modules at scale using the Azure portal](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-deploy-monitor) or [using Azure CLI](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-deploy-monitor-cli).
