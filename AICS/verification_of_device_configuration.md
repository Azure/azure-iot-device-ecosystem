Verification of Device configuration
===
---

-   Please install python by following below command:

    **Debian or Ubuntu**

        sudo apt-get install python

    **Fedora**

        sudo dnf install python

    *This library also requires Python version 2.7.x. You can verify the current version installed in your environment using the following command:*
    
          python --version

-   Please install the below modules before you run the `platform_data.py`

    **Debian or Ubuntu** 

        sudo apt-get install python-requests
        sudo apt-get install python-netifaces

    **Fedora**

        sudo dnf install python-requests
        sudo dnf install python-netifaces

-   Download the SDK by issuing following command:

        git clone https://github.com/Azure/azure-iot-sdk-python.git

-   Navigate to tools folder by executing following command:

        cd azure-iot-sdk-python/Tools

-   Run the following command on the device
		
        python platform_data.py

    ![deviceinfo\_screenshot](images/python_modified_output.PNG)


## Share with the Azure IoT Certification team

1.  Go to [Partner Dashboard](https://catalog.azureiotsolutions.com/devices).
2.  Click on Upload icon at top-right corner of your device.

    ![Share\_Results\_upload\_icon](images/4_2_01.png)

3.  This will open an upload dialog. Browse your file(s) by clicking **Upload** button.

    ![Share\_Results\_upload\_dialog](images/4_2_02.png)

    You can upload multiple files for same device.

4.  Once you have uploaded all the files, click on **Submit for Review** button.

    ***Note:*** *Please contact iotcert team to change/remove the files once you submit them for review.*
 
## Troubleshooting

Please contact engineering support on <mailto:iotcert@microsoft.com> for help with troubleshooting.
