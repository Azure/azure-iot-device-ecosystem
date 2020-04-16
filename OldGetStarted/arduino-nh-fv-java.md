---
platform: arduino
device: nh-fv
language: java
---
Run a simple java sample on NH-FV Series device running Arduino
===
---

# Table of Contents

-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Configure the Device](#Build)
-   [Step 4: Control NH using field list](#Control)
-   [Next Steps](#NextSteps)


<a name="Introduction"></a>
# Introduction

This document describes how to connect NH-FV Series device running JAVA
with Azure IoT Hub. This multi-step process includes:

-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Configure the device
-   Control NH using field list

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub](https://catalog.azureiotsolutions.com/docs?title=Azure/azure-iot-device-ecosystem/setup_iothub)

-   [Provision your device and get its credentials](https://catalog.azureiotsolutions.com/docs?title=Azure/azure-iot-device-ecosystem/manage_iot_hub)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   NH-FV Series device[Firmware Version 1.12 or later]

<a name="Build"></a>
# Step 3: Configure the Device

## 3.1 Installation Procedure and Flowchart

1.  Install this product on a level surface. If necessary, use the accessories (rubber foot, adhesive seal, support base) if needed when
installing.

2.  Connect the AC Adaptor to this product Using the cable clamp will prevent the power cord from being unintentionally pulled out. (Refer to **1.5 About AC Adaptor**)

3.  Connect the LAN cable to this product.

4.  Set up the network environment configuration. Refer to **3.2 Network Setup -- 3.3 Clock Setup**  on how to setup the network.

5.  Set up the detailed settings after the network environment has been configured

6.  Azure IoT Hub **3.4 Azure IoT HUB**

7.  Azure IoT Central **3.5 Azure IoT Central**

8.  This product is now ready to be used.

## 3.2 Network Setup

The default IP address for this product is: `192.168.10.1`

Subnet mask is: `255.255.255.0`

The IP address change should be done by logging in to the web browser of the personal computer (henceforth PC) to change the setup. Log into the personal computer before changing the Network settings, so that the personal computer can communicate with this product. Refer to **Login** for the login method.

Various setups for this product is done by logging in from a web browser. In order to log in, the IP address for this product is entered into the address portion of the web browser.

 ![](./media/PATLITE_NH-FV/image1.PNG)

Attention： When the login screen is displayed, enter "patlite" in the password field, then click the "Logging In" button. The default password is set to "patlite". Be sure to change the password to prevent any security breaching.

When the login screen is displayed, go to the upper right of the screen where **Please Select Your Language** is located to select the preferred language.

 ![](./media/PATLITE_NH-FV/image2.PNG)

Select the preferred language from the pull down menu in the upper right of the login screen. (Refer to　above.)

The currently selectable languages available are **Japanese** and **English**. Once selected, the language will be displayed on each screen in the Web setup tool.

When the login screen is displayed, go to the upper right of the screen where **Please Select Your Language** is located to select the preferred language. Enter **patlite** in the password field, then click the **Logging In** button.

The default password is set to "patlite." Be sure to change the password to prevent any security breaching.

2.  Setting the IP Address

After logging in, the web setup tool will be executed and the **System Setup** screen will be displayed.

1.  Click the **Setup Menu** in the Setup Table Entry on the left-hand side of the setup screen.

2.  Click **System Setup** from the tree menu. The System Setup Screen is displayed. (Refer to **Figure 2.7.2--2**)

    ![](./media/PATLITE_NH-FV/image3.PNG)

    In the System Setup Screen, the network can be changed.

    **Setup Method:**

3.  Enter the Main Unit IP address.

4.  Set up the net mask, default gateway, etc. if needed.

5.  Click the **Set** button to activate the setup.

    ![](./media/PATLITE_NH-FV/image4.PNG)

6.  In order to activate the setting parameters, click the **Network Reboot**\" button.

7.  Rebooting the network takes about 20 seconds. If no action occurs
after time elapses, click To the Login screen to log in, again.

    ![](./media/PATLITE_NH-FV/image5.PNG)

**Setup Confirmation:**

If the web browser address is reflecting the changed value of the IP address after clicking **To the Login screen**, the setup of the new IP address has been  uccessful. However, in cases where the preset value of other networks had been changed, be sure to enter the proper IP Address value where it was moved to in order to verify it in the system setup screen.

Verify that the IP Address has changed (Below example shows
**192.168.10.1**)

## 3.3 Clock Setup

The PC clock time is reflected in this product when logged in again.

**[Setup Method]**

1.  Compare the columns between the **NH Monitor Time** and the **Host Computer Time**.

2.  Click the **Manually Setup Clock** button to synchronize the time with the PC which is logged in.

    ![](./media/PATLITE_NH-FV/image6.PNG)

## 3.4 Azure IoT Hub

1.  Click **Setup Menu** in the menu to open the tree menu.

2.  Click **Cloud Connection Configuration** in the tree menu to move to the settings screen.

3.  Enter the device's connection string value in the **Connection string** field.

4.  Click the **Settings** button to apply the settings.

5.  Click **Logout** in the tree menu and close the browser.

    ![](./media/PATLITE_NH-FV/image7.png)

    ![](./media/PATLITE_NH-FV/image8.png)

## 3.5 Azure IoT Central

1.  Click **Setup Menu** in the menu to open the tree menu.

2.  Click **Cloud Connection Configuration** in the tree menu to move to the settings screen.

3.  Enter numerical values in the **Scope ID** field, the **Device ID** field, and the **Primary Key** field.

4.  Click the **Settings** button to apply the settings.

5.  Click **Logout** in the tree menu and close the browser.

    ![](./media/PATLITE_NH-FV/image7.png)

    ![](./media/PATLITE_NH-FV/image9.png)

<a name="Control"></a>
# Step 4: Field Name List

List and description of field names for controlling NH-FV.

Choose the appropriate method depending on the control application.

## 4.1 Device Twin

|No.|Field name |Value|Description|
| - | --------- |---- | --------- |
| 1 |led_red / led_red_c|0: OFF, 1: ON, 2: Flashing 1, 3: Flashing 2|LED Unit (red)|
| 2 |led_yellow / led_yellow_c|0: OFF, 1: ON, 2: Flashing 1, 3: Flashing 2|LED Unit (yellow)|
| 3 |led_green / led_green_c|0: OFF, 1: ON, 2: Flashing 1, 3: Flashing 2|LED Unit (green)|
| 4 |led_blue / led_blue_c|0: OFF, 1: ON, 2: Flashing 1, 3: Flashing 2|LED Unit (blue)|
| 5 |led_white / led_white_c|0: OFF, 1: ON, 2: Flashing 1, 3: Flashing 2|LED Unit (white)|
| 6 |buz_pattern / buz_pattern_c|0: Stop, 1 to 4 : Pattern 1 to Pattern 4|Buzzer control|
| 7 |sound_pattern / sound_pattern_c| Channel 1 to 70 |Audio channel|
| 8 |digital_output /digital_output_c|0: OFF, 1: ON|Digital output|


## 4.2 Direct Method / Cloud-to-device Message

| No.  | Field name | Value         | Description                                                  |
| ---- | ---------- | ------------- | ------------------------------------------------------------ |
| 1    | alert      | "6 digits"    | Controls the signal light and buzzer.<br>Set patterns in order: R (red) → Y (yellow) → G (green) → B (blue) → C (white)→ Z (buzzer).<br>[RYGBC] - 0: OFF, 1: ON, 2: flashing 1, 3: flashing 2, 9: no change<br>[Z] - 0: No sound, 1 to 4: Buzzer Pattern 1 to 4, 9: No change |
| 2    | led        | "5 digits"    | Controls the signal light.<br>Set patterns in order: R (red) → Y (yellow) → G (green) → B (blue) → C (white).<br/>[RYGBC] - 0: OFF, 1: ON, 2: flashing 1, 3: flashing 2, 9: no change |
| 3    | alert_do   | 0: OFF, 1: ON | Controls digital output.                                     |
| 4    | clear      | 1             | Turn off all signal lights and stop playing the current channel. |
| 5    | sound      | 1 to 70       | Play specified audio channel.                                |
| 6    | repeat     | 0 to 255      | Play the audio channel defined by the [sound] instruction the number of times specified here. |

### 4.3 Device-to-cloud Message

| NO.  | Field Name    | Value       | Description                                                  |
| ---- | ------------- | ----------- | ------------------------------------------------------------ |
| 1    | clear_switch  | "on"        | Notify when the Clear switch is pressed                      |
| 2    | input_state_1 | "on", "off" | Notify when there is a change in state in the digital input 1. |
| 3    | input_state_2 | "on", "off" | Notify when there is a change in state in the digital input 2. |
| 4    | input_state_3 | "on", "off" | Notify when there is a change in state in the digital input 3. |
| 5    | input_state_4 | "on", "off" | Notify when there is a change in state in the digital input 4. |
| 6    | red_state     | "0" to "3"  | Notify when there is a change in state in the red LED unit.<br>"0": OFF, "1": ON, "2": Flashing 1, "3": Flashing 2 |
| 7    | yellow_state  | "0" to "3"  | Notify when there is a change in state in the yellow LED unit.<br>"0": OFF, "1": ON, "2": Flashing 1, "3": Flashing 2 |
| 8    | green_state   | "0" to "3"  | Notify when there is a change in state in the green LED unit.<br>"0": OFF, "1": ON, "2": Flashing 1, "3": Flashing 2 |
| 9    | blue_state    | "0" to "3"  | Notify when there is a change in state in the blue LED unit.<br>"0": OFF, "1": ON, "2": Flashing 1, "3": Flashing 2 |
| 10   | white_state   | "0" to "3"  | Notify when there is a change in state in the white LED unit.<br>"0": OFF, "1": ON, "2": Flashing 1, "3": Flashing 2 |
| 11   | buzzer_state  | "0" to "4"  | Notify when there is a change in the buzzer pattern.<br>"0": Stop, "1" to "4" : Pattern 1 to Pattern 4 |
| 12   | sound_state   | "0" to "70" | Notify when there is a change in the audio channel.          |
| 13   | output_state  | "on", "off" | Notify when there is a change in state in the digital  output. |

