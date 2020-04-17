---
platform: raspbian
device: spyrus microsdhc enhanced
language: c
---

Run a simple AMQP sample on SPYRUS microSDHC Enhanced device running on a Raspberry Pi 3 with Raspbian
===
---

# Table of Contents
elect the appropriate SPYRUS device. In this case we are using a Rosetta 
-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build and Run the Sample](#Build)
-   [Tips](#tips)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect the SPYRUS microSDHC Enhanced device running on a Raspberry Pi 3 with Raspbian with Azure IoT SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering your IoT device
-   Build and deploy Azure IoT SDK on device

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process:

-   [Setup your IoT hub][lnk-setup-iot-hub]
-   [Provision your device and get its credentials][lnk-manage-iot-hub]
-   SPYRUS microSDHC Enhanced device.
-   [Download the SPYRUS Azure IoT SDK](https://developer.spyrus.com/login/registration/)

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device

-   The SPYRUS Azure IoT SDK comes with a command line utility for determining that your Hardware is working correctly. To test your SPYRUS Rosetta microSD hardware on a Debian Linux environment:
and mounted on the platform and that the communication with the SMARTIO file is established.

    1.  Insert Rosetta microSD.
    2.  If drive does not auto mount, you may need to configure your system automount settings or you can manually mount it with:
								
			 sudo mount /dev/sd(x) /media/(username)/Spyrus 
        , where x is the SD’s assigned drive letter.
    3.  Check the SD’s mount point and for the SMART_IO.CRD file with:
    
			ls –a /media/(username)/Spyrus

    If the SMART_IO.CRD file is found, you are now ready to run the smartio_test. The smartio_test is included in the SPYRUS PKCS#11 SDK for Rosetta microSD. It is used to display version information about your Rosetta microSD. You may need to set the smart_io binary to executable with:

	    sudo chmod +x smart_io

     An example of the expected output is shown below.

    ![](media/spyrus-microsdhc-enhanced/smartio_linux.png)

-   Once you have determined that your hardware is working properly, you will need to import the HMAC private key onto your Rosetta Smart Card. The HMAC key used during the SASTokenCreate process is the known in the code as the Device Key. This is the key that needs to be programmed into the Rosetta device as the HMAC key.
The Device Key is obtained by Base64 decoding the Primary Key shown for the device when it is added to the IOT Hub using iothub-explorer (on Linux) or DeviceExplorer (Windows). 
On Linux the iothub-explorer will display the Primary Key in the authentication details:-

		  authentication: 

		  SymmetricKey:

		  primaryKey:   O7/Gkm1Hlb7zs4B9qqxFLXVJOOR31WrCorsbuERAHcE=

		  secondaryKey: n0dQ9awh9bu3ZqN2/CESqOwr510kumrYuv59pqn14Sk=

    The Primary key is also visible in the IOT Hub:

    ![](media/spyrus-microsdhc-enhanced/device_portal.png)   

    To make it easy to integrate this key into the rosparse scripts, a simple program PrimaryKeyToDeviceKey has been written. This takes the Primary Key in Base64 form and outputs the decoded Device Key in hex form suitable for direct cut & paste into the required rosparse script. This program is included in the bin directory of the SPYRUS Azure IoT SDK.

	    I:\ IOT\ Release>PrimaryKeyToDeviceKey.exe "F1Ya8SBfI3fbDia7t3fRDA/BxujdVs9xntUnJOSN+qM="
		Device Key=17 56 1A F1 20 5F 23 77 DB 0E 26 BB B7 77 D1 0C 0F C1 C6 E8 DD 56 CF
		71 9E D5 27 24 E4 8D FA A3

    To load this key onto your Rosetta Smart Card, we have provided a Windows application called rosparse and some scripts that will need to be updated with your device's key.

    To do this follow these steps:-

    1.  Locate the appropriate script for your device type.
        In this example, we will use a SPYRUS Rosetta microSD in Enhanced mode. 
        The appropriate script would be found in the folder Device Setup Scripts\ SRS3_MicroSDHC_SPYCOS3\FIPS. The script is called Azure_IOT_SDK_SetupDevice.txt.
    2.  Using an editor, open Azure_IOT_SDK_SetupDevice.txt.
    3.  Locate the following section:-

            //
            // Replace the 32 bytes following a0 20 with the Base 64 decoded 
            // Primary Key for the device added to the IOT Hub.
            //
            Import SHA2 HMAC Key onto token,  
            90 5A 80 00 22 
            a0 20 
            "17 56 1A F1 20 5F 23 77 DB 0E 26 BB B7 77 D1 0C 
            0F C1 C6 E8 DD 56 CF 71 9E D5 27 24 E4 8D FA A3", sw.

    4.  Replace the 32 byte key value (in double quotes above) with the Device Key value obtained in the section above titled “Obtain Device Key”. 
    5.  Save the changes and exit the editor.

    The rosparse utility only runs on Windows platforms. So the programming of the device will require a Windows host even if the intended deployment is Linux or Raspberry PI. 

    1.  Insert the Rosetta Device
    2.  Start an Command Prompt
    3.  Change directory to the Device Setup Scripts\ SRS3_MicroSDHC_SPYCOS3\FIPS folder
    4.  Run the command “rosparseSD Azure_IOT_SDK_SetupDevice.txt”
    5.  Select the appropriate SPYRUS device. In this case we are using a Rosetta microSDHC mounted at the drive letter E:\ so we will select that reader.

        ![](media/spyrus-microsdhc-enhanced/reader_list.png)

    6.  Check the command output for errors.  When programming the device an end user does not need to understand the values returned by the programming process, but, however, they do need to be able to identify success or failure. 

    The output will differ based on whether or not the device is in FIPS mode or Non-FIPS mode. To determine if your device is in FIPS mode, you can pass the’GetFIPSMode’ file into the rosparse script on the command line:

        rosparseSD.exe < GetFIPSMode.txt

    Your output should look something like this:

        Check FIPS
        <<  90 28 00 00 01
        >>  01 90 00
        0.004122s

    If the third line of output is “>>  01 90 00”, then your device is in FIPS mode. If it is “>>  00 90 00”, then your device is in Non-FIPS mode.

    Success is indicated by a string  “>> 90 00” where  “>>” represents a return value and “90 00” indicates the command executed successfully.

    Listed below is the output from programming a Rosetta microSDHC Enhanced device. Two values are in double quotes. These represent the two most important steps:- logging into the user account and importing of the HMAC key. The decrypted result received over the secure channel must be 90 00. Other values would indicate failure to setup the device properly. The rest of the output can be safely ignored. It is useful only for contacting SPYRUS for support if you fail to program your device successfully.

        I:\ Device Setup Scripts\SRS3_MicroSDHC_SPYCOS3\FIPS>rosparseSD.exe < Azure_IOT_SDK_SetupDevice.txt
        Spyrus Inc microSDHC 0

        Get FIPS
        <<  90 28 00 00 01
        >>  01 90 00
        0.237648s

        Select SPEX Directory
        <<  00 A4 01 00 02 10 DD
        >>  6A 82
        0.003888s

        Delete directory
        <<  90 E4 01 00 00
        >>  69 83
        0.002180s

        Create SPEX Directory
        <<  90 E0 00 00 20 10 DD 0D 00 00 00 00 00 00 00 00
        <<  00 00 00 00 00 01 02 03 04 05 06 07 08 09 0A 0B
        <<  0C 0D 0E 0F 00
        >>  90 00
        0.067122s

        Create PIN File
        <<  90 E0 00 00 11 21 C4 01 00 80 00 0F 0F 0F 0F 0F
        <<  0F 0F 0F 0F 0F 00
        >>  90 00
        0.046476s

        Initialize PIN File
        <<  00 08 00 00 00
        >>  90 00
        0.878148s

        Start ECDHAES Channel...
          Manage Channel Command:
            >>  90 86 02 00
            <<  61 65
          Get Response:
            >>  00 C0 00 00 65
            <<  72 20 EC D5 DF 3C 0F 01 1A A7 7F 6F 27 33 25 FF
            <<  10 66 E0 64 E0 D4 25 CC 2E B5 2D C5 00 12 96 4F
            <<  EE 4B 73 41 04 D3 12 58 C9 B8 E6 00 A5 41 3A 27
            <<  23 51 1A A3 CE 5A 9D A3 00 CE 66 8D D3 BA 10 9E
            <<  B8 59 2E B1 B4 53 83 01 4B 99 6A 5D 0F EA 93 44
            <<  B7 49 C5 3F 5F 93 D3 DA 1C 6B F5 A4 C6 2A CE 23
            <<  CD 2B A7 29 7E 90 00
          Nonce:
             EC D5 DF 3C 0F 01 1A A7 7F 6F 27 33 25 FF 10 66
             E0 64 E0 D4 25 CC 2E B5 2D C5 00 12 96 4F EE 4B
          Card Public:
             04 D3 12 58 C9 B8 E6 00 A5 41 3A 27 23 51 1A A3
             CE 5A 9D A3 00 CE 66 8D D3 BA 10 9E B8 59 2E B1
             B4 53 83 01 4B 99 6A 5D 0F EA 93 44 B7 49 C5 3F
             5F 93 D3 DA 1C 6B F5 A4 C6 2A CE 23 CD 2B A7 29
             7E
          Host Public:
             04 60 FD E2 F0 B3 DA 4A 4E 5C 2F 81 2B A8 1D 3C
             A5 21 F8 05 01 80 49 B5 AD 45 D4 EA 4C 07 63 7E
             C5 3B 6B 81 55 A3 B6 49 54 4A 59 80 29 E7 F6 18
             42 34 93 BB 45 5A FD C8 D5 F9 DC 4E 2B 5F 7E 50
             CD
          Host Private:
             F0 E7 D8 31 45 C9 7A 0A 6E EC D1 23 F7 E6 6A E2
             5E BF BF B1 A9 7E 93 96 B0 D8 CA EE E7 A1 6B 1F
          Secret:
             6D 11 AD E9 0E 45 7B D8 1B 2A 9A B5 40 85 3A 94
             53 AF 99 09 68 BD E9 07 CD 18 AC 42 30 40 04 23
          Key:
             38 7A BD E0 F1 58 13 1D 60 30 A6 58 F0 00 1D CE
             03 3E FD B8 83 E2 2D 9B 9B 7F 03 F9 43 00 91 EB
          Params (Key Confirmation || host public):
             74 20 60 F1 DE 4D 10 A8 8F F4 F8 13 95 35 EB 63
             16 5D 76 6E 32 0B 6D 78 58 1F 4E 17 0F 96 DC B9
             77 52 73 41 04 60 FD E2 F0 B3 DA 4A 4E 5C 2F 81
             2B A8 1D 3C A5 21 F8 05 01 80 49 B5 AD 45 D4 EA
             4C 07 63 7E C5 3B 6B 81 55 A3 B6 49 54 4A 59 80
             29 E7 F6 18 42 34 93 BB 45 5A FD C8 D5 F9 DC 4E
             2B 5F 7E 50 CD

          Authenticate Channel Command:
            >>  90 8C 02 00 65 74 20 60 F1 DE 4D 10 A8 8F F4 F8
            >>  13 95 35 EB 63 16 5D 76 6E 32 0B 6D 78 58 1F 4E
            >>  17 0F 96 DC B9 77 52 73 41 04 60 FD E2 F0 B3 DA
            >>  4A 4E 5C 2F 81 2B A8 1D 3C A5 21 F8 05 01 80 49
            >>  B5 AD 45 D4 EA 4C 07 63 7E C5 3B 6B 81 55 A3 B6
            >>  49 54 4A 59 80 29 E7 F6 18 42 34 93 BB 45 5A FD
            >>  C8 D5 F9 DC 4E 2B 5F 7E 50 CD
        Send over AES Encrypted Channel...
          Command Before Encrypting
            00 20 00 01 06 4D 6F 73 61 69 63
          Padded Command Before Encrypting
            00 20 00 01 06 4D 6F 73 61 69 63 80 00 00 00 00
          IV Counter = 1
          IV = AD 07 D3 45 1E CA BF 33 44 A2 17 77 89 FC 24 A3
        AESTxChannel: "Logon USER"
        <<  94 C2 00 00 10 04 5F F8 52 EF 36 F4 D0 0C 94 1E
        <<  14 7D EE 6E 2D
        >>  61 10
        0.280154s

        Receive over AES Encrypted Channel...
        AESRxChannel: get response
        <<  00 C0 00 00 10
          Response Before Decrypting
            C7 22 7F 0D 2F 70 71 87 1D E7 78 FF 1E 5E C9 67
          IV Counter = 2
          IV = D3 4E D4 8E 9E 33 F1 76 62 06 AF 99 51 9C EC 88
          Response After Decrypting (padded)
            90 00 80 00 00 00 00 00 00 00 00 00 00 00 00 00
        Response (decrypted):
        >>  "90 00"
        0.003750s

        Send over AES Encrypted Channel...
          Command Before Encrypting
            90 0A 00 FF 14 31 66 62 38 34 66 61 31 64 62 38
            33 35 64 37 35 64 65 65 61
          Padded Command Before Encrypting
            90 0A 00 FF 14 31 66 62 38 34 66 61 31 64 62 38
            33 35 64 37 35 64 65 65 61 80 00 00 00 00 00 00
          IV Counter = 3
          IV = 10 37 0D DB 8E AD 99 B7 40 C2 87 76 FF 56 C6 69
        AESTxChannel:Load new PIN USER(1)
        <<  94 C2 00 00 20 10 5B 79 98 D3 4F EE 2B 8D 14 97
        <<  40 75 56 9B D2 06 15 C4 AE 42 FD 6F 4D 43 83 B7
        <<  2B 6E 46 36 5A
        >>  61 10
        0.007280s

        Receive over AES Encrypted Channel...
        AESRxChannel: get response
        <<  00 C0 00 00 10
          Response Before Decrypting
            21 24 6E B9 34 E4 C2 89 C0 DE 89 3D D7 1E EB 65
          IV Counter = 4
          IV = CC 9B 6C B0 4E 8C 27 81 9E 07 93 71 CB 80 AA B3
          Response After Decrypting (padded)
            90 00 80 00 00 00 00 00 00 00 00 00 00 00 00 00
        Response (decrypted):
        >>  90 00
        0.003654s

        Send over AES Encrypted Channel...
          Command Before Encrypting
            90 24 00 01 06 4D 6F 73 61 69 63
          Padded Command Before Encrypting
            90 24 00 01 06 4D 6F 73 61 69 63 80 00 00 00 00
          IV Counter = 5
          IV = B4 EB 63 75 7C 69 51 25 F6 C2 87 69 CC 9E 45 EB
        AESTxChannel:Change PIN USER(1)
        <<  94 C2 00 00 10 D4 71 FC 84 83 3B 20 9D B8 69 F7
        <<  71 4D CD 4B 93
        >>  61 10
        0.377542s

        Receive over AES Encrypted Channel...
        AESRxChannel: get response
        <<  00 C0 00 00 10
          Response Before Decrypting
            99 C4 80 8A E8 46 16 2E 21 E5 48 EB 04 61 A3 02
          IV Counter = 6
          IV = 38 33 25 27 4D 31 AB 4B 96 3E D8 E0 46 74 46 75
          Response After Decrypting (padded)
            90 00 80 00 00 00 00 00 00 00 00 00 00 00 00 00
        Response (decrypted):
        >>  90 00
        0.003620s

        Send over AES Encrypted Channel...
          Command Before Encrypting
            00 20 00 01 14 31 66 62 38 34 66 61 31 64 62 38
            33 35 64 37 35 64 65 65 61
          Padded Command Before Encrypting
            00 20 00 01 14 31 66 62 38 34 66 61 31 64 62 38
            33 35 64 37 35 64 65 65 61 80 00 00 00 00 00 00
          IV Counter = 7
          IV = FD B7 66 14 2D 4F 51 13 E7 B0 10 43 2F 99 ED B2
        AESTxChannel: Logon USER - ExpRsp 0x9000
        <<  94 C2 00 00 20 3F 1E 69 BD 35 A2 01 34 81 EC 64
        <<  8D A3 33 C8 5F F8 75 64 F1 C4 62 A7 85 E3 AB 26
        <<  FF 38 14 25 A3
        >>  61 10
        0.282399s

        Receive over AES Encrypted Channel...
        AESRxChannel: get response
        <<  00 C0 00 00 10
          Response Before Decrypting
            B8 29 A9 CF 75 4A 35 31 DF 07 29 0D 48 51 35 6F
          IV Counter = 8
          IV = 7E A3 2D 7D 60 B3 B9 3F E0 0B 4A 8D A8 3F 53 B8
          Response After Decrypting (padded)
            90 00 80 00 00 00 00 00 00 00 00 00 00 00 00 00
        Response (decrypted):
        >>  90 00
        0.003795s

        Create a test Key File
        <<  90 E0 00 00 11 21 D3 10 00 80 80 0F 0F 0F 0F 0F
        <<  0F 02 12 0F 0F 70
        >>  90 00
        0.047339s

        Send over AES Encrypted Channel...
          Command Before Encrypting
            90 5A 80 00 22 A0 20 17 56 1A F1 20 5F 23 77 DB
            0E 26 BB B7 77 D1 0C 0F C1 C6 E8 DD 56 CF 71 9E
            D5 27 24 E4 8D FA A3
          Padded Command Before Encrypting
            90 5A 80 00 22 A0 20 17 56 1A F1 20 5F 23 77 DB
            0E 26 BB B7 77 D1 0C 0F C1 C6 E8 DD 56 CF 71 9E
            D5 27 24 E4 8D FA A3 80 00 00 00 00 00 00 00 00
          IV Counter = 9
          IV = F1 C2 D1 7D 25 9F DB 3A DC 98 B8 2E 5E 67 F8 51
        AESTxChannel:"Import SHA2 HMAC Key onto token"
        <<  94 C2 00 00 30 CC 0C 81 9F 6E 57 5A 90 79 E0 80
        <<  D4 21 05 92 A5 B2 BC E3 C2 78 88 18 1D 21 54 0B
        <<  A8 9C 27 55 75 C9 BE D8 3C A1 44 07 03 5D 65 06
        <<  60 8C 3B 54 3F
        >>  61 10
        0.047542s

        Receive over AES Encrypted Channel...
        AESRxChannel: get response
        <<  00 C0 00 00 10
          Response Before Decrypting
            4F 32 71 16 E1 9F 6B 9D 27 7F B9 AC 67 C4 C8 4F
          IV Counter = 10
          IV = 44 4F B4 58 9F FC C5 DD 72 19 0C 79 92 5B E4 7F
          Response After Decrypting (padded)
            90 00 80 00 00 00 00 00 00 00 00 00 00 00 00 00
        Response (decrypted):
        >>  "90 00"
        0.003861s

        End ECDHAES Channel...

<a name="Build"></a>
# Step 3: Build and Run the sample

<a name="Load"></a>
## 3.1 Build SDK and sample

-   Open a PuTTY session and connect to the device.

-   Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by issuing the following commands from the command line on your board:

        sudo apt-get update

        sudo apt-get install -y curl libcurl4-openssl-dev uuid-dev uuid g++ make cmake git unzip openjdk-7-jre libpcsclite-dev 

-   Download the Microsoft Azure IoT Device SDK for C to the board by issuing the following command on the board::

        git clone --recursive https://github.com/Azure/azure-iot-sdk-c.git

-   Copy RosettaIOT.h to azure-iot-sdks/c/c-utility/inc/azure_c_shared_utility

-   Copy RosettaIOT.c to azure-iot-sdks/c/c-utility/src

-   Edit the following files using any text editor of your choice:

    1.  Edit the CMakeLists.txt file for aziotsharedutil (azure-iot-sdks/c/c-utility)

        Changes required are as follows:-
        a.  Add PCSC include directory
        include_directories(/usr/include/PCSC)
        b.  Add RosettaSDLIB2 directory if intention is to use Rosetta microSDHC
        e.g. Include_directories(/usr/share/SPYRUS/IOT/RosettaSDLib2/include)
        The path to the library differs on each target platform.
        c.  Add ./src/RosettaIOT.c to source_c_files
        d.  Add ./inc/azure_c_shared_utility/RosettaIOT.h to source_h_files
        e.  Add pcsclite and dl to target_link_libraries

        An example of this modified file is included in the IOT SDK Rosetta Support\SDK Modifications directory of the SPYRUS Azure IoT SDK, but additional modifications may be required as the Microsoft Code changes occur.

    2.  Edit platform_linux.c (azure-iot-sdks\c\c-utility\adapters) file. 

    -    Find the following line in the platform_init function:

             return tlsio_openssl_init();

    -   Replace that line with the following code:

            // Initialize Rosetta Device
            result = rosetta_platform_init();

    3.  Edit sastoken.c (azure-iot-sdks\c\c-utility\src)

    -   Find the following line in the construct_sas_token function:

            size_t outLen = BUFFER_length(decodedKey);
            unsigned char* outBuf = BUFFER_u_char(decodedKey);

    -   Replace that line with the following code:

            unsigned char hmac[64];
            DWORD hmacSize = 64;
            int hmacStatus = rosetta_hmacSHA2256((unsigned char *)inBuf, inLen, hmac, &hmacSize);
            if (hmacStatus == 0) {
                BUFFER_enlarge(hash, 32);
                unsigned char *hmac_ptr = BUFFER_u_char(hash);
                if (hmac_ptr) {
                    memcpy(hmac_ptr, hmac, 32);
                }
            } else {
                LogError("Unable to build the SAS token. rosetta_hmacSHA2256 failed. errCode=%d", hmacStatus);
                STRING_delete(result);
                result = NULL;
                STRING_delete(toBeHashed);
                BUFFER_delete(hash);
                BUFFER_delete(decodedKey);
                return result;
            }

    -   Find the following lines in the construct_sas_token function:
   
                    if ((HMACSHA256_ComputeHash(outBuf, outLen, inBuf, inLen, hash) != HMACSHA256_OK) ||
                        ((base64Signature = Base64_Encoder(hash)) == NULL) ||
                        ((urlEncodedSignature = URL_Encode(base64Signature)) == NULL) ||
                        (STRING_copy(result, "SharedAccessSignature sr=") != 0) ||
                        (STRING_concat(result, scope) != 0) ||
                        (STRING_concat(result, "&sig=") != 0) ||
                        (STRING_concat_with_STRING(result, urlEncodedSignature) != 0) ||
                        (STRING_concat(result, "&se=") != 0) ||
                        (STRING_concat(result, tokenExpirationTime) != 0) ||
                        (STRING_concat(result, "&skn=") != 0) ||
                        (STRING_concat(result, keyname) != 0))

    -   Replace those lines with the following code:

                     if (((base64Signature = Base64_Encoder(hash)) == NULL) ||
                        ((urlEncodedSignature = URL_Encode(base64Signature)) == NULL) ||
                        (STRING_copy(result, "SharedAccessSignature sr=") != 0) ||
                        (STRING_concat(result, scope) != 0) ||
                        (STRING_concat(result, "&sig=") != 0) ||
                        (STRING_concat_with_STRING(result, urlEncodedSignature) != 0) ||
                        (STRING_concat(result, "&se=") != 0) ||
                        (STRING_concat(result, tokenExpirationTime) != 0) ||
                        (STRING_concat(result, "&skn=") != 0) ||
                        (STRING_concat(result, keyname) != 0))

        An example of these modified files is included in the IOT SDK Rosetta Support\SDK Modifications\SASToken directory of the SPYRUS Azure IoT SDK, but additional modifications may be required as the Microsoft Code changes occur.


    4.  Edit iothub_client_sample_amqp.c (azure-iot-sdk-c/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp.c)


    -   Find the following place holder for IoT connection string:

            static const char* connectionString = "[device connection string]";

    -   Replace the above placeholder with device connection string you obtained in [Step 1](#Prerequisites) and save the changes.

-   Add Rosetta microSDHC Support

    To enable support for Rosetta microSDHC the following steps must be taken:-

       1.  In RosettaIOT.c uncomment the #define for _USE_ROSETTALIB

          #define _USE_ROSETTASDLIB 1

       2.  Install the Rosetta microSDHC support library header files and libraries. 

            This library is provided as zip file called SPYRUS_IOT_Install.zip. 

            Unzip this file and copy the SPYRUS directory to:

		 /usr/share

    It is worth noting that at this point in time the code is either compiled for Rosetta microSDHC or Rosetta USB/Smartcard. It does not support all device types at runtime. This feature maybe added in the future.

-   Build the SDK using following command.

        sudo ./azure-iot-sdk-c/build_all/linux/build.sh

## 3.2 Send Device Events to IoT Hub:

-   Run the sample by issuing following command:


        ~/azure-iot-sdk-c/cmake/iotsdk_linux/iothub_client/samples/iothub_client_sample_amqp/iothub_client_sample_amqp


-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to observe the messages IoT Hub receives from the application.

## 3.3 Receive messages from IoT Hub

-   See [Manage IoT Hub][lnk-manage-iot-hub] to learn how to send cloud-to-device messages to the application.

<a name="tips"></a>
# Tips

- If you just want to build the serializer samples, run the following commands:

  ```
  cd ./c/serializer/build/linux
  make -f makefile.linux all
  ```

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
[setup-devbox-linux]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-setup-iot-hub]: ../setup_iothub.md
[lnk-manage-iot-hub]: ../manage_iot_hub.md

