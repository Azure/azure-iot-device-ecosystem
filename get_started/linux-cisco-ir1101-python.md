---
platform: linux
device: cisco ir1101
language: python
---

Run a simple PYTHON sample on Cisco IR1101 device running Linux
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

This document describes how to connect Cisco Industrial Router IR1101 device running Linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-python]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   Cisco Industrial Router IR1101
-   A build machine with [Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) and [ioxclient](https://developer.cisco.com/docs/iox/#!iox-resource-downloads) installed

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

Cisco IOx is an application hosting environement for Cisco gateways. It is based on Linux and can run Docker containers.

-   Follow the guide on how to [configure Cisco IOx on Cisco IR1101](https://www.cisco.com/c/en/us/td/docs/routers/access/1101/software/configuration/guide/b_IR1101config/b_IR1101config_chapter_010001.html) 
-	You will need to get access to Cisco IOx Local Manager to deploy applications on the platform.
- 	If you do not want to use Local Manager, this step can be skipped if you are using a device provisioned with Cisco Kinetic Gateway Mananagement Module (GMM) or Field Network Director (FND). In this case IOx is automatically activated and the application can be uploaded and executed from the management application.

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

- 	As listed in the prerequisites you'll need access to a build machine that can build Docker images, with ioxclient installed.

-   In order to run these tests in the most simple way, we will build a Ubuntu-based Docker container with the Microsoft Python SDK.

-   The scripts are part of the Microsoft Python SDK hosted on a GitHub repo and will be cloned during the Docker image build process. 

- On your developement machine create a Dockerfile that contains everything you'll need. The advantage is that none of this will need to be installed on your computer as this will go straight in the Docker image:

		FROM multiarch/qemu-user-static:x86_64-aarch64 as qemu
		FROM arm64v8/ubuntu:20.04

		COPY --from=qemu /usr/bin/qemu-aarch64-static /usr/bin

		RUN apt-get update && apt-get install -y openssh-server curl \
			libcurl4-openssl-dev build-essential cmake git python2.7-dev \
			libboost-python-dev joe
		RUN pip3 install azure-iot-device

		RUN mkdir /var/run/sshd
		RUN echo 'root:root' | chpasswd

		RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
		RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config


		RUN apt-get clean && \
    	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

		RUN mkdir /root/.ssh

		RUN git clone --recursive https://github.com/Azure/azure-iot-sdk-python.git

		EXPOSE 22

		CMD ["/usr/sbin/sshd", "-D"]

-   Build the Docker image with the following command:

		docker build -t ubuntu-with-ssh-aarch64 .

-   Build the IOx application from the image:

		ioxclient docker package -n ubuntu-with-ssh-aarch64 ubuntu-with-ssh-aarch64 .

-   Install the IOx application on your Cisco IR1101 `ubuntu-with-ssh-aarch64.tar` using local manager, and activate the application. This configuration is outside the scope of the document and is covered in the Cisco documentation, [such as in this example](https://www.cisco.com/c/en/us/support/docs/routers/1101-industrial-integrated-services-router/214383-build-and-deploy-a-docker-iox-package-fo.html).

-   When the container runs enter the container console from the IR1101 CLI using:

		ir1101# app-hosting connect appid ubuntu session /bin/bash                                                                                                                          

- 	When in the Docker container running on IR1101, navigate to samples folder by executing following command:

        cd /azure-iot-sdk-python/azure-iot-device/samples/async-hub-scenarios

-  Set up the environement variable to reflect your Azure IoT connection string:

		export IOTHUB_DEVICE_CONNECTION_STRING="HostName=...;DeviceId=...;SharedAccessKey=..."

## 3.2 Send Device Events to IoT Hub:

-   Run a sample application using the following command, for example to send messaged to Azure IoT Hub you can use:
 
   	    python3 send_message.py

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

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
[setup-devbox-python]: https://github.com/Azure/azure-iot-device-ecosystem/blob/master/get_started/python-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

