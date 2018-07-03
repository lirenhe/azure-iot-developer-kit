---
title: "Get started"
permalink: /docs/get-started/
excerpt: "How to quickly install and set up your development environment to use the DevKit."
variable:
  - platform: windows
    name: Windows
  - platform: macos
    name: macOS
last_modified_at: 2018-03-12
---

For first-time users of the MXChip IoT DevKit (a.k.a. DevKit), follow these quick steps to:
- Prepare your development environment.
- Send temperature and humidity data from built-in DevKit sensors to the Azure IoT Hub.

If you have already done this, you can try more samples from the [Projects Catalog]({{"/docs/projects/" | absolute_url }}) or build your own IoT application.

{% include toc icon="columns" %}

## What you learn

* How to connect the DevKit to a wireless access point.
* How to install the development environment.
* How to create an IoT Hub and register a device for the DevKit.
* How to collect sensor data by running a sample application on the DevKit.
* How to send the DevKit sensor data to your IoT hub.

## What you need

* An MXChip IoT DevKit. [Get it now](https://aka.ms/iot-devkit-purchase){:target="_blank"}.
* A computer running Windows 10 or macOS 10.10+.
* An active Azure subscription. [Activate a free 30-day trial Microsoft Azure account](https://azure.microsoft.com/en-us/free/).

![Required hardware]({{"/assets/images/getting-started/hardware.jpg" | absolute_url }})

## Prepare your hardware

To connect the DevKit to your computer:

1. Connect the Micro-USB end to the DevKit.
2. Connect the USB end to your computer.
3. The green LED for power confirms the connection.

![Hardware connections]({{"/assets/images/getting-started/connect.jpg" | absolute_url }})

## Configure Wi-Fi

IoT projects rely on internet connectivity. Use AP Mode on the DevKit to configure and connect to Wi-Fi.

1. Hold down button B, push and release the reset button, and then release button B. Your DevKit enters AP mode for configuring the Wi-Fi connection. The screen displays the service set identifier (SSID) of the DevKit and the configuration portal IP address:
  ![Reset button, button B, and SSID]({{"/assets/images/getting-started/wifi-ap.jpg" | absolute_url }})

2. Use a Web browser on a different Wi-Fi enabled device (computer or mobile phone) to connect to the DevKit SSID displayed in the previous step. If it asks for a password, leave it empty.
  ![Network info and Connect button]({{"/assets/images/getting-started/connect-ssid.png" | absolute_url }})

3. Open **192.168.0.1** in the browser. Select the Wi-Fi network that you want the DevKit to connect to, type the password for the Wi-Fi conection, and then click **Connect**.
  ![Password box and Connect button]({{"/assets/images/getting-started/wifi-portal.png" | absolute_url }})

4. The DevKit reboots in a few seconds. You then see the Wi-Fi name and assigned IP address on the screen of the DevKit:
  ![Wi-Fi name and IP address]({{"/assets/images/getting-started/wifi-ip.jpg" | absolute_url }})

**Note:** After  asuccessful Wi-Fi connection, the currently-installed and latest available version of the DevKit's firmware is displayed on the DevKit screen. If the DevKit is not running on the latest available version, follow the [firmware upgrading guide]({{"/docs/firmware-upgrading/" | absolute_url }}) to install the latest version.
{: .notice--info}

## Install development environment

We recommend [Azure IoT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) extension for Visual Studio Code to develop on the IoT DevKit.

Azure IoT Workbench provides an integrated experience to develop IoT solutions. It helps both on device and cloud development using Azure IoT and other services. You can watch this [Channel9 video](https://channel9.msdn.com/Shows/Internet-of-Things-Show/IoT-Workbench-extension-for-VS-Code) to have an overview of what it does.

Follow these steps to prepare the development environment for IoT DevKit:

1. Download and install [Arduino IDE](https://www.arduino.cc/en/Main/Software). It provides the necessary toolchain for compiling and uploading Arduino code.
  * Windows: Use Windows Installer version
  * macOS: Drag and drop the Arduino into `/Applications`
  * Ubuntu: Unzip it into `$HOME/Downloads/arduino-1.8.5`

1. Install [Visual Studio Code](https://code.visualstudio.com/), a cross platform source code editor with powerful developer tooling, like IntelliSense code completion and debugging.

1. Look for **Azure IoT Workbench** in the extension marketplace and install it.
  ![Install IoT Workbench]({{"/assets/images/getting-started/install-workbench.png" | absolute_url }})
  Together with the IoT Workbench, other dependent extensions will be installed.

1. Open **File > Preference > Settings** and add following lines to configure Arduino.
  * Windows:
    ```json
    "arduino.path": "C:\\Program Files (x86)\\Arduino",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```
  * macOS:
    ```json
    "arduino.path": "/Application",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```
  * Ubuntu:
    ```json
    "arduino.path": "/home/{username}/Downloads/arduino-1.8.5",
    "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
    ```

1. Click `F1` to open the command palette, type and select **Arduino: Board Manager**. Search for **AZ3166** and install the latest version.
  ![Install DevKit SDK]({{"/assets/images/getting-started/install-sdk.png" | absolute_url }})
  
As a fallback, you can follow the [manual steps]({{"/docs/installation/" | absolute_url }}) to install the environment.

## ST-Link configuration

[ST-Link/V2](http://www.st.com/en/development-tools/st-link-v2.html) is the USB interface that IoT DevKit uses to communicate with your development machine. Follow the platform specific steps to allow the machine access to your device.

### Windows

Download and install USB driver from [STMicro](http://www.st.com/en/development-tools/stsw-link009.html).

### macOS

No driver is required for macOS.

### Unbutu

Run the following in terminal and logout and login for the group change to take effect:
```bash
# Copy the default rules. This grants permission to the group 'plugdev'
sudo cp ~/.arduino15/packages/AZ3166/tools/openocd/0.10.0/linux/contrib/60-openocd.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules

# Add yourself to the group 'plugdev'
# Logout and log back in for the group to take effect
sudo usermod -a -G plugdev $(whoami)
```

Now you are all set with preparing and configuring your development environment. Let us build a "Hello World" sample for IoT: sending temperature telemetry data to Azure IoT Hub.

## Build your first project

1. Make sure your DevKit is **not connected** to your computer. Start VS Code first, and then connect the DevKit to your computer.

1. In the bottom right status bar, check the **MXCHIP AZ3166** is shown as selected board and serial port with **STMicroelectronics** is used.
  ![Select board and serial port]({{"/assets/images/getting-started/select-board.png" | absolute_url }})

1. Click `F1` to open the command palette, type and select **IoT Workbench: Examples**. Then select **IoT DevKit** as board.

1. In the pop-up page, scroll down and click **Open Sample** on Get Started tile. Also selects the default path download the sample.
  ![Open sample]({{"/assets/images/getting-started/open-sample.png" | absolute_url }})

1. In the new opened project window, click `F1` to open the command palette, type and select **IoT Workbench: Cloud**, then select **Azure Provision**.

1. Follow the step by step guide to finish provisioning your Azure IoT Hub and creating the device.
  ![Cloud provision]({{"/assets/images/getting-started/cloud-provision.png" | absolute_url }})

1. Click `F1` to open the command palette, type and select **IoT Workbench: Device**, then select **Config Device Settings > Select IoT Hub Device Connection String**.

1. On IoT DevKit, hold down button **A**, push and release the **reset** button, and then release button **A**. Your DevKit enters configuration mode and saves the connection string.
  ![Set connection string]({{"/assets/images/getting-started/connection-string.png" | absolute_url }})

1. Click `F1` again, type and select **IoT Workbench: Device**, then select **Device Upload**.
  ![Verification and upload of the Arduino sketch]({{"/assets/images/getting-started/arduino-upload.png" | absolute_url }})

The DevKit reboots and starts running the code.

**Note:** If there is errors or interruptions, you can always recover by running the command again.
{: .notice--info}

## Test the project

Click the power plug icon on the status bar to open the Serial Monitor:
  ![Open serial monitor]({{"/assets/images/mini-solution/connect-iothub/serial-monitor.png" | absolute_url }})

The sample application is running successfully when you see the following results:

* The Serial Monitor displays the message sent to the IoT Hub.
* The LED on the MXChip IoT DevKit is blinking.

![Final output in VS Code]({{"/assets/images/mini-solution/connect-iothub/result-serial-output.png" | absolute_url }})

You can use [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) to monitor device-to-cloud (D2C) messages in IoT Hub.

1. Log in [Azure portal](https://portal.azure.com), find the IoT Hub you created.
  ![azure-portal-iot-hub]({{"/assets/images/mini-solution/connect-iothub/azure-iot-hub-portal.png" | absolute_url }})

1. In the **Shared access policies pane**, click the **iothubowner policy**, and write down the Connection string of your IoT hub.
  ![azure-portal-iot-hub-conn-string]({{"/assets/images/mini-solution/connect-iothub/azure-portal-conn-string.png" | absolute_url }})

1. Expand **AZURE IOT HUB DEVICES** on the bottom left corner.
  ![azure-iot-toolkit-iot-hub-devices]({{"/assets/images/mini-solution/connect-iothub/azure-iot-toolkit-devices.png" | absolute_url }})

1. Click **Set IoT Hub Connection String** in context menu.
  ![azure-iot-toolkit-iot-hub-conn-string]({{"/assets/images/mini-solution/connect-iothub/azure-iot-toolkit-conn-string.png" | absolute_url }})

1. Click **IoT: Start monitoring D2C message** in context menu.

1. In **OUTPUT** pane, you can see the incoming D2C messages to the IoT Hub.
  ![azure-iot-toolkit-output-console]({{"/assets/images/mini-solution/connect-iothub/azure-iot-toolkit-console.png" | absolute_url }})

## Problems and feedback

If you encounter problems, you can refer to [FAQs]({{"/docs/faq/" | absolute_url }}) or reach out to us from [Gitter channel](https://gitter.im/Microsoft/azure-iot-developer-kit){:target="_blank"}.

{% include feedback.html tutorial="get-started" %}

## Next Steps

You have successfully connected an MXChip IoT DevKit to your IoT hub, and you have sent the captured sensor data to your IoT hub. 
Check our [Projects Catalog]({{"/docs/projects/" | absolute_url }}) for more samples you can build with the DevKit and Azure multiple services.
