---
platform: linux
device: inrouter 915
language: python
---

Run a simple PYTHON sample on inrouter 915 device running linux
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

This document describes how to connect InRouter 915 device running Linux with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][setup-devbox-python]
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   InRouter 915 device.

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
- the URL for the device is https://www.inhand.com.cn/products/industrial-4g-router.html
Connect the InRouter 9XX using the ssh or telnet with putty. 

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK on ubuntu machine
###3.1.1 Setup the development environment

- sudo apt-get update
- sudo apt-get install -y curl libcurl4-openssl-dev build-essential cmake-3.6 git python2.7-dev libboost-python-dev

### 3.1.2 Install the Cross Compilation Toolchain
- tar -xjvf am335x-arm-unknown-linux-uclibc-V1.1.tar.bz2 -C /opt
- export PATH=$PATH:/opt/buildroot-2012.05/output/host/usr/bin

### 3.1.3 Clone github repository

-  git clone --recursive ttps://github.com/Azure/azure-iot-sdk-python.git


### 3.1.4 cross compile azure iot sdk for python

- cd azure-iot-sdk-python
- touch  AZURE_IOT_IR9_CROSS_COMPILE.cmake


The content is as follows:
    
    SET(IR9_CROSS_COMPILE_PATH                                                "/opt/buildroot-2012.05/output/host/usr")
    SET(IR9_SYSTEM_PATH "/root/INOS/IR9/system")
    SET(CMAKE_SYSTEM_NAME Linux)
    SET(CMAKE_SYSTEM_VERSION 1)
    SET(CMAKE_C_COMPILER) ${IR9_CROSS_COMPILE_PATH}/bin/arm-linux-gcc)
    SET(CMAKE_CXX_COMPILER  	                                    ${IR9_CROSS_COMPILE_PATH}/bin/arm-linux-g++)

    SET(CMAKE_FIND_ROOT_PATH	"${IR9_SYSTEM_PATH}/"               "${IR9_SYSTEM_PATH}/python/"                                    "\${IR9_SYSTEM_PATH}/install/IR9-ti-am335x/usr/lib/")
    INCLUDE_DIRECTORIES("${IR9_SYSTEM_PATH}/python/" 	            "${IR9_SYSTEM_PATH}/libuuid/include"
    "${IR9_SYSTEM_PATH}/openssl2/include" 	                        "${IR9_SYSTEM_PATH}/curl/include")

    SET(Boost_INCLUDE_DIR ${IR9_SYSTEM_PATH}/boost)
    SET(Boost_LIBRARY_DIR ${IR9_SYSTEM_PATH}/boost/stage/lib/)

    SET(PYTHON_INCLUDE_DIR ${IR9_SYSTEM_PATH}/python/Include/)
    SET(PYTHON_LIBRARY                                              ${IR9_SYSTEM_PATH}/python/libpython2.7.so) 

    SET(OPENSSL_ROOT_DIR ${IR9_SYSTEM_PATH}/openssl2/)
    SET(OPENSSL_INCLUDE_DIR                                         ${IR9_SYSTEM_PATH}/openssl2/include/)
    SET(OPENSSL_CRYPTO_LIBRARY                                      ${IR9_SYSTEM_PATH}/openssl2/libcrypto.so)
    SET(OPENSSL_SSL_LIBRARY                                         ${IR9_SYSTEM_PATH}/openssl2/libssl.so)

    SET(CURL_INCLUDE_DIR ${IR9_SYSTEM_PATH}/curl/include/)
    SET(CURL_LIBRARY                                                ${IR9_SYSTEM_PATH}/curl/lib/.libs/libcurl.so)

    SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
    SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
    SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

- build project:

    ./build_all/linux/build.sh	--toolchain-file                         AZURE_IOT_IR9_CROSS_COMPILE.cmake 

## 3.2 Running the Sample

### 3.2.1 Use "tftp" command copy files from ubuntu tftpboot

directory to target device
    
    cd  /tmp
    tftp -gr iothub_client.so 10.5.16.172(tftp server ip         address)
    tftp -gr iothub_service_client.so 10.5.16.172
    tftp -gr iothub_client_mock.so 10.5.16.172
    tftp -gr iothub_service_client_mock.so 10.5.16.172
    tftp -gr  iothub_client_sample.py 10.5.16.172
    tftp -gr  iothub_client_args.py 10.5.16.172
    tftp -gr  iothub_client_cert.py 10.5.16.172
    Tftp -gr ca-certificates.crt /etc/ssl/certs/


export PYTHONPATH=$PATH:/tmp


### 3.2.2 Send Device Events to IoT Hub

####  Run the sample application using the following command

- For MQTT protocol:

    python iothub_client_sample.py -p mqtt -c 
    “HostName=freetest.azure-    devices.cn;DeviceId=InRouter9XX;SharedAccessKey=czuSbRBED6jpxvfS6Kj9GSACofJ0qL45b+U9Sk3qOcI=”

- For HTTP protocol:

    python iothub_client_sample.py -p http -c 
“HostName=freetest.azure-devices.cn;DeviceId=InRouter9XX;SharedAccessKey=czuSbRBED6jpxvfS6Kj9GSACofJ0qL45b+U9Sk3qOcI=”

- See Manage IoT Hub to learn how to observe the messages IoT Hub receives from the application.Verify that the confirmation messages show an OK. If not, then you may have incorrectly copied the devce connection string


### 3.2.2 Receive messages from IoT Hub

- See Manage IoT Hub to learn how to send cloud-to-device messages to the application.

<a name="NextSteps"></a>
# Next Steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer]
-   [Save IoT Hub messages to Azure data storage]
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub]
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub]
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning]
-   [Remote monitoring and notifications with Logic Apps]   
