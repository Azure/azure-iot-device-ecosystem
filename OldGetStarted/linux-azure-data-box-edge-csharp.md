---
platform: edge linux
device: azure data box edge
language: csharp
---

Run a simple C# sample on Azure Data Box Edge device running Linux
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Manual Test for Azure Data Box Edge](#Manual)

<a name="Introduction"></a>
# Introduction

### About this document:

This document describes how to connect Azure Data Box Edge device running Linux with IoT Hub with Azure IoT Edge Runtime pre-installed and Device Management. This multi-step process includes:

**Azure Data Box Edge** is a storage solution that allows you to process data and send it over network to Azure. This article provides you an overview of the **Data Box Edge solution, benefits, key capabilities, and the scenarios** where you can deploy this device.

Data Box Edge uses a physical device supplied by Microsoft to accelerate the secure data transfer. The physical device resides in your premises and you write data to it using the **NFS and SMB protocols**.

Data Box Edge has all the gateway capabilities of Data Box Gateway. Data Box is additionally equipped with **AI-enabled edge computing capabilities** that help analyze, process, or filter data as it moves to **Azure block blob, page blob, or Azure Files**.

## Benefits

**Data Box Edge has the following benefits:**

-   *Transform data* - Enables analysis, processing, or filtering of data as it moves to Azure.
-   *Easy data transfer* - Makes moving data in and out of Azure storage as easy as working with a local network share.
-   *High performance* - Enables high-performance transfers to and from Azure.
-   *Fast access* - Caches most recent files for fast access of on-premises files.
-   *Limited bandwidth usage* - Data can be written to Azure even when the network is throttled to limit usage during peak business hours.

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-linux]
-   [Setup your IoT hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   [Sign up to IOT Hub](https://account.windowsazure.com/signup?offer=ms-azr-0044p)
-   [Get started](https://docs.microsoft.com/en-us/azure/databox-online/data-box-edge-deploy-prep#get-started) to deploy Data Box Edge

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Click [here](https://docs.microsoft.com/en-us/azure/databox-online/data-box-edge-deploy-prep) to prepare how to deploy Azure Data Box Edge.

-   This section describes how to install a Data Box Edge physical device. The installation procedure involves unpacking, rack mounting, and cabling the device.

-   Click [here](https://docs.microsoft.com/en-us/azure/databox-online/data-box-edge-deploy-install) to know how to Install Azure Data Box Edge.

<a name="Manual"></a>
# Step 3: Manual Test for Azure Data Box Edge

This section walks you through the test to be performed on the Edge devices running the Linux operating system such that it can qualify for Azure IoT Edge certification.

**Connect, set up, and activate Azure Data Box Edge:**

-   Click [here](https://docs.microsoft.com/en-us/azure/databox-online/data-box-edge-deploy-connect-setup-activate) to know how you can connect to, set up, and activate your Azure Data Box Edge device by using the local web UI.

**Transfer data with Azure Data Box Edge:**

-   Click [here](https://docs.microsoft.com/en-us/azure/databox-online/data-box-edge-deploy-add-shares) to know how to add and connect to shares on your Data Box Edge device. After you've added the shares, Data Box Edge can transfer data to Azure.

**Set up compute role:**

-   When the Edge compute role is set up on the Edge device, it creates two devices: an IoT device and an IoT Edge device. Both devices can be viewed in the IoT Hub resource. Click [here](https://docs.microsoft.com/en-us/azure/databox-online/data-box-edge-deploy-configure-compute#set-up-compute-role) to know set up the compute role on the device.


[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
