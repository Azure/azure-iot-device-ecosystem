Introduction
===
---

## About this document

Thank you for your interest in certifying your IoT device(s) for Azure Certified for IoT program. This document provides an overview of new web-based service called Azure IoT certification service that allows device manufacturers to reduce the engineering processes and automate the certification processes. 

## Azure IoT certification service (AICS)

AICS is a web-based workflow that connects to IoT Hub to streamline the certification processes. Traditionally, device manufacturers are required to follow the [instructions](http://aka.ms/certfaq) to get IoT device certified by sending Microsoft a package of screenshot of device explorer to show the messages from/to IoT Hub instances and build logs from using Azure IoT device SDK. While the device manufacturer still does need to build its own firmware or software that includes the connection strings to the IoT Hub instances, AICS eliminates the need of instantiation of IoT Hub under customer’s Azure subscription.

AICS is also expanding to support additional IoT Hub primitives such as device twin and direct methods on top of existing bi-directional telemetry messages from device-to-cloud and cloud-to-device. 

Additionally, AICS can validate against the software code without the usage of Azure IoT device SDK. This means that device manufacturers who do not use Azure IoT device SDK to build an app to establish connectivity to IoT Hub, but instead use customized app using another SDK is now capable of testing the IoT Hub connectivity.

Lastly, AICS is integrating with the existing Azure IoT device catalog experience for device manufacturer engineers to navigate through seamless UI. AICS also allows device manufacturers to easily see the test results with detail logs.

**Feedbacks?**

Please provide feedbacks on AICS to [iotcertdisc@microsoft.com](mailto:iotcertdisc@microsoft.com)

