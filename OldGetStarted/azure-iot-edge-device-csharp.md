---
platform: azure iot edge
device: edge device
language: csharp
---

Deploy Azure Stack HCI and WAC, Azure IoT Edge
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step1: Deploy Azure Stack HCI, install WAC](#DeployAzureStackHCIinstallWAC)
-   [Step2: Deploy Azure IoT Edge on Azure Stack HCI](#DeployAzureIoTEdgeonAzureStackHCI)

<a name="Introduction"></a>
# Introduction

This topics provides step-by-step instructions to deploy [Azure Stack HCI](https://azure.microsoft.com/en-us/overview/azure-stack/hci/) and [Azure
IoT Edge](https://azure.microsoft.com/en-us/services/iot-edge/). Leverage your Azure Stack HCI investment to run key virtual applications and workloads in a highly available, resilient fashion on hardware designed for Branch office and edge scenarios with industry-leading support for 2 node configurations including: [Nestedresiliency](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/nested-resiliency), [USB thumb drive clusterwitness](https://docs.microsoft.com/en-us/windows-server/failover-clustering/file-share-witness), and browser-based administration via [Windows Admin Center](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/understand/windows-admin-center). You can find Azure Stack HCI solutions from your preferred hardware vendors from the [Azure Stack HCIcatalog](https://www.microsoft.com/en-us/cloud-platform/azure-stack-hci-catalog?Hardware-partners=Cisco).

<a name="DeployAzureStackHCIinstallWAC"></a>
# Step1: Deploy Azure Stack HCI, install WAC

Step by Step guide to deploy Azure Stack HCI

1.  Install Windows Server 2019 Datacenter (follow guidance above in network
    connectivity for Clustering)

2.  Add Roles and Features

3.  Setup Failover Clustering and enable a Cluster Witness

4.  Setup Storage Spaces Direct

5.  [Install Windows Admin Center](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/deploy/install)
    (WAC)

<a name="DeployAzureIoTEdgeonAzureStackHCI"></a>
# Step 2: Deploy Azure IoT Edge on Azure Stack HCI

1.  [Create a VM on your Azure Stack HCI using Windows Admin Center](https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/use/manage-virtual-machines#create-a-new-virtual-machine)

    (For supported OS versions, VM types, processor architectures and system
    requirements, click
    [here](https://docs.microsoft.com/en-us/azure/iot-edge/support))

2.  If you do not already have an Azure account, get your free account [here](https://azure.microsoft.com/en-in/free/)

3.  [Create an Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart#create-an-iot-hub) in the Azure Portal

4.  [Register an IoT Edge device](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart#register-an-iot-edge-device) in the Azure Portal

    *(The IoT Edge “device” is the Windows or Linux VM running on your Azure
    Stack HCI installation)*

5.  [Install and start the IoT Edge runtime](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart#install-and-start-the-iot-edge-runtime) on the VM you created in step 1

    *(You will need the device string created in step 4 above to connect the
    runtime to your Azure IoT Hub)*

6.  [Deploy a module to IoT Edge](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart#deploy-a-module)

    *(Pre-built modules can be sourced and deployed from the* [IoT Edge Modules
    section of the Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules)*)*