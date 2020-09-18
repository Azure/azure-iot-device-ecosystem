### Welcome and services document ###
# Azure Certified Device program: #
# Getting started with the preview portal #
Updated: 17 Sept 2020
  

### Hello! ###

Welcome to the new Azure Certified Device portal. Here you will be able to certify your devices for one or more of our certifications:

- [Azure Certified Device](https://aka.ms/acdrequirements "Azure Certified Device requirements") (baseline program)
- [IoT Plug and Play](https://aka.ms/acdiotpnprequirements "IoT Plug and Play requirements") (incremental program)
- [Edge Managed](https://aka.ms/acdedgemanagedrequirements "Edge Managed requirements") (incremental program)
 
*While the [Azure Certified Device portal](https://certify.azure.com "Azure Certified Device portal") will continue to evolve with new features over the coming months, theses certifications are Generally Available, so you can confidently certify your devices now.*

If you have previously certified devices with the Azure IoT Device Certification program, we will **soon share more information with our partners** about our transition plans from our older portal to this newer portal, including how device builders can manage their previously created projects.

For more information on the Azure Certified Device program, please see [Getting Started with the Azure Certified Device program](https://aka.ms/certlearnmore "Getting Started with the Azure Certified Device program"). If you have additional questions or feedback on the certifications or on the Azure Certified Device portal, please contact us at [iotcert@microsoft.com](mailto:iotcert@microsoft.com). Thanks!

-The Azure Certified Device team

## Overview of certification process ##

Certifying a device in our Azure Certified Device program consists of four main activities. These include:

1) Providing hardware capability information

2) Validating device functionality

3) Submitting and completing the review

4) Publishing to the Azure Certified Device Catalog (optional)


## Activity 1 - Providing hardware capability information ("Device details") ##

One of the core purposes of providing device hardware information is to improve customer confidence as they make purchasing decisions. Your device—including hardware features and other distinct details—can be distinguishable, discoverable and searchable on the Azure Certified Device Catalog.

The product information provided during the certification process falls into four distinct categories:

1) Device information

2) *Get started* guide

3) Marketing details

4) Additional industry certifications (optional)

### Starting the project ###

On the main projects screen after signing in to the Azure Certified Device portal, click the "Create new project" button above the table area:

![Image of the Create new project button](./images/create_new_project.png)

This will launch a window to collect basic information about your project. This information can be changed later within the device details areas). 

![Image of the Create new project window](./images/create_new_project_window.png)

**In this window, the following fields are required:**

- *Project name:* This is an internal name for your project that would not be visible to customers on the Azure Certified Device Catalog
- *Device name:* This is the customer-facing name for your device
- *Device type:* Currently, we only support certifying finished products. Soon we will add the ability to certify solution-ready dev kits.
- *Device class:* This is the form factor or classification that best represents your device. We currently list Gateway, Sensor, and Other. If you select other, you able the ability to describe your device class in your own terms. Over time, we may continue to add new values to this list, particularly as we continue to monitor feedback from our partners.
- *Certifications:* This is where you specify which certification(s) you wish to achieve for your device. As Azure Certified Device is a required baseline, the toggle button is not editable. Currently, IoT Plug and Play and Edge Managed are mutually exclusive. As we evolve our existing programs and add new programs, the selections and functionality in this area will likewise change.

After you click the "Create" button, the new project will be saved and visible in the projects table. To edit the project, click on the Project name in the table. This will launch the project summary page where you can begin entering more details about your device:

![Image of the project details page](./images/Device_details_section.png)

### Input device details ###

This area allows you to provide information on the core hardware capabilities of your device, such as device name, description, processor, operating system, connectivity options, hardware interfaces, industry protocols, physical dimensions, and more. While many of the fields are optional, most of this information will be made available to potential customers on the Azure Certified Device Catalog if you choose to publish your device after it has been certified.

![Image of the project details page](./images/Device_details_menu.png)

#### Basics: ####
This area shows the same fields that were initially requested when creating the project: Project name, Device name, Device type, Device class.

#### Certifications: ####
Similarly, this area shows the selected certifications when the project was first created. We currently have three certifications generally available: Azure Certified Device (required), IoT Plug and Play (optional), and Edge Managed (optional). More information on these programs can be found in the links at the top of this document.

#### Review: ####
This area provides a read-only overview of the full set of device details that been entered.

#### Product details: ####
Here you specify generally-applicable information for your device, including supported Operating system(s), Additional product details, and optional Internal Comments for the Azure Certified Device team to review.

Importantly, the *Product details* area is where you also enter information about specific "components" for your device. This is an key **new concept** that we have introduced in the Azure Certified Device portal: *we have added the ability to denote multiple, separate hardware products (“components”) that make up your device.* We have added this functionality to enable the device builder to better promote devices that come with additional hardware, and to better enable customers and solution developers to find the right product to integrate into their solution. ***Every project submitted for certification will require at least one component (which in many cases will be the full product itself)***. Further suggestions on how to use this new functionality are covered in the next section of this document.

*The “Add a component” feature is available on the Product details tab:*

![Add a component link](./images/Add_a_component_link.png)

*This action enables new form fields for the component:*

![Component details section](./images/Component_details_section.png)

*After completing the relevant fields for the component, the component can be saved using the “Save Product Details” button at the bottom of the page:*

![Save Product Details button](./images/Save_Product_Details_button.png)

*For each component you add to your project, you can further tailor the hardware capabilities it supports. To further configure your component with these properties, click the **Edit** link by the component name:*

![Edit Component button](./images/component_edit.png)

*Hardware capability information can be provided for each component:*

![Edit Component button](./images/component_selection_area.png)

These sections include:

- *General* hardware details like processors and secure hardware
- *Connectivity* options and interfaces
- Hardware *Accelerators* (if relevant for your device)
- *Sensors* (if relevant for your device)
- *Additional Specs* such as physical dimensions and storage/battery information

##### Component use requirements and recommendations: #####

Below are examples of a few scenarios, and how the new Component feature may (or may not) apply:

- If you are certifying a Finished Product that is one complete device, then your device information must include a single component, which is itself the **Customer Ready Product**. In this case, the Attachment method value must be Discrete.
- If you are certifying a Finished Product that includes a detachable peripheral, then your device information should include the **Customer Ready Product** and then, optionally, a **Peripheral** component that is identified as **detachable**
- If you are certifying a Finished Product that includes an integrated component you want to highlight to your customers – such as a System on Module (SoM) you may be using – your device information should include the Customer Ready Product and then, optionally, a **System on Module** component that is identified as **integrated**
- ***(Coming soon)*** If you are certifying a **Solution-Ready Dev Kit**, you likewise have the ability to denote components that range from **System on Module**, **Development Board**, and **Peripheral**.

Two additional important notes:

- A project can contain—at most—one Customer Ready Product component. If you are certifying a project with two independent devices, those devices should be certified separately. If you have any questions in this scenario, please contact our team at [iotcert@microsoft.com](mailto:iotcert@microsoft.com).
- It is primarily at *your discretion* on how to use—or not use—this new feature to promote your device's capabilities to potential customers.


### *Get Started* guide for your customers: ###

The second category of information we request from our device builders is a PDF document to simplify setup/configuration and management of your product—a ‘Get started’ guide. This furthers our mutual goal of making it simple for customers to connect and support devices on Azure.

We provide a number of Get Started templates from which to begin, depending on both the certifications sought and your preferred language to provide customer-ready samples.

The templates are available at our [Get started templates](https://aka.ms/GSTemplate "Get started templates") GitHub location:

![Get started GitHub location](./images/Get_Started_template_location.PNG)

- If you are certifying for the Azure Certified Device certification only, use a template in the "Azure-Certified-Device" folder
- If you are certifying for IoT Plug and Play, use the template in the "IoT-Plug-and-Play" folder
- If you are certifying for Edge Managed, use a template in the "Edge-Managed" folder

*You may only submit **one** Get Started guide for your device.*

### Marketing details: ###

In this area, you will need to provide customer-ready marketing information for your device. These fields in particular will be showcased on the Azure Certified Device Catalog if you choose to publish your device after it is certified.

A new addition to our preview portal is the ability for our device builders to list multiple distributors in which your device can be purchases. This is found in the “Add a distributor” section on the Marketing details page. A minimum of one distributor is required, with a maximum of five distributors.

### Industry Certifications your device has achieved (Optional): ###

If you so choose, you can promote additional Industry Certifications you may have received for your device. By default, we provide a list of many of the most common Industry Certifications, though if your product has achieved a certification not in our list, you can specify a custom string value once you select “Other (please specify)”.


## Activity 2 - Validating device functionality (“Connect & test”) ##

### Automated testing ###
The second major phase of the certification process (though it can be done in any order) involves testing your device for adherence to our certification requirements.

*To begin the testing phase, click the "Connect & test" link on the project summary page:*
![Connect and test link](./images/connect_and_test_link.PNG)

*Depending on the certification(s) selected, you will be required to run one or more sets of tests to validate your device functionality.*

![Connect and test page](./images/connect_and_test.PNG)

For testing of all certifications, you are required to connect a device to IoT Hub using the Device Provisioning Service (DPS). DPS supports connectivity options of Symmetric keys, X.509 certification, and a Trusted Platform Module (TPM). 

For information on connecting your device to Azure IoT Hub with Device Provisioning Service (DPS) visit [provisioning devices overview](https://aka.ms/acddpsinfo "Device Provisioning Service overview").

*After configuring your device with DPS, confirm the connection by clicking the "Connect" button at the bottom of the page. Upon successful connection, you can proceed to the testing phase by clicking the "Next" button:*

![Connect and Test connected](./images/connected.PNG)

Testing specifics on the next few pages will vary by program. Please refer to the certification requirement documents linked at the top of this page for more information on what is being validated, plus refer to other resources in our [Getting Started with Azure Certified Device program](https://certlearnmore "Getting Started with the Azure Certifed Device program") for more information.

Upon completion of automated testing, you will see the Testing status, as well as access to Log files. All automated tests must pass before you will be able to proceed on to the next stage.

### Important Note about Manual validation ###
***While you will be able to complete the online certification process for Iot Plug and Play and Edge Managed without having to submit your device for manual review, you may be contacted by a Azure Certified Device team member for further device validation beyond what is tested through our automation service.***

## Activity 3 - Submitting and completing the review (“Review & certify”) ##
Once you have completed all of the mandatory fields in the **Device details** area and successfully passed the automated testing in the **Connect & test** process, you can now notify the Azure Certified Device team that you are ready for certification review.

*Once ready, click "Submit for review" on the project page:*

![Review and Certify link](./images/review_and_certify.PNG)

You will see a confirmation dialog before the Azure Certified Device team is notified to complete the review. Once a device has been submitted, **devices details can no longer be edited through the portal**. Likewise, once a device is approved, all device details will be read-only. You will need to contact the Azure Certified Device team at [iotcert@microsoft.com](mailto:iotcert@microsoft.com) to request further changes.

![Start Certification review dialog](./images/start_certification_review.PNG)

*Once the project is submitted, the project summary page will indicate the project is Under Certification Review:*

![Under Review](./images/review_and_certify_under_review.PNG)

In the case your project is not approved, you will be able to make changes to the project details and then re-submit the device for certification once ready. Email will be sent to the email address in the Company profile will information on why the project was not approved. 

### Once approved ###
Once your project has been reviewed and approved, you will receive a notification to the email address under the Company profile. The email will include a set of files including the Azure Certified Device badge, badge usage guidelines, and other information on how to amplify the message that your device is certified. Congratulations!

## Activity 4 - Publishing to the Azure Certified Device Catalog (“Publish to catalog”) ##

After your device has been certified, you can optionally publish your device details to the Azure Certified Device Catalog for a world of customers to see.

*To publish your device, return to the project summary page and click Publish to Device Catalog:*
![Publish to Catalog](./images/publish_to_catalog.PNG)

*You will received a confirmation dialog before the device is published:*
![Publish to Catalog confirmation](./images/publish_to_catalog_confirm.PNG)

You will receive notification to the email address in the Company profile once the device is on the Azure Certified Device Catalog. **If you need to make further edits to the device details after the device is published, please send an email to the Azure Certified Device team.**
