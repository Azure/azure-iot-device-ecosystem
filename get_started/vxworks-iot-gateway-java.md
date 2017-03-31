---
platform: vxworks 
device: iot gateway
language: java
---

Run a simple Java sample on IoT Gateway device running VxWorks
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build the Sample](#Build)
-   [Step 4: Run the Sample](#Run)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to build and run the Java sample application. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Prepare your development environment][devbox-setup]
-   Computer with Git client installed and access to the
    [azure-iot-sdks](https://github.com/Azure/azure-iot-sdks) GitHub public repository.
-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   Make sure desktop is ready as per instructions given on [Prepare your development environment][devbox-setup].

<a name="Build"></a>
# Step 3: Build the sample

If you have successfully [prepared your development environment][devbox-setup] the IoT device SDK and all samples should be built and ready to use.

-   Build the client library along with the **samples** :
	```
	{IoT device SDK root}/java/device/>mvn install -DskipTests
	```

    On build completion, you will see summary of what got built.    

<a name="Run"></a>
# Step 3: Run the sample

-	Now start a new instance of the [Device Explorer][device-explorer], select or create a new device, obtain and note the connection string for the device, and begin monitoring under the Data tab.

-	Open an SFTP-Client (Port 22) and login to the IoT Gateway 

		User: boschrexroth
		Password: boschrexroth

-	First navigate to the folder containing the executable jar files you have build in the section before. 
-	Pick the sample jar you wish to run and transfer it to the OEM-Partition from the IoT Gateway.
-   Open the file `jvm.xml` in the folder `/OEM/ProjectDataProtected` in an text editor.
-   Change the jar file in the line 
  
		<vm runable="true" type="release" app="/SYSTEM/jvm/app/felix.jar">

	to the sample file copied before.
-   Also add the arguments `{connection string}`, `{number of requests to send}`, e.g. needed from your sample as `<option>....</option> ` in this file.

-	As final step oben the hosts file in the folder` /OEM/ProjectDataProtected` and add the host ip and host name from your IoT Hub.

-	If you restart now your IoT Gateway, the simple Java sample will run on the IoT Gateway device running VxWorks.


[devbox-setup]: java-devbox-setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md
[how-to-build-a-java-app-from-scratch]: https://azure.microsoft.com/documentation/articles/iot-hub-java-java-getstarted/