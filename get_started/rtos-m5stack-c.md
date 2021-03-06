---
platform: RTOS
device: M5Stack IoT Kit
language: c
---

Run a simple C sample on M5Stack IoT Kit device running RTOS
===
---


# Table of Contents
- [Introduction](#Introduction)
- [Step 1: Prerequisites](#Prerequisites)
- [Step 2: SDK and Tools Preparation](#Tools_prepare)
- [Step 3: Configuring and building](#Config_build)
- [Step 4: Result shows](#Results)
- [Trouble Shoot](#TroubleShoot)
- [Next Steps](#NextSteps)


<a name="Introduction"></a>
Introduction
------------------------------

M5Stack is an ESP32-based development kit that integrates screens, buttons, Wi-Fi, Bluetooth, TF reader, buzzer, built-in battery, SPI bus, I2C bus.
![](media/m5stack_starter_kit/PIC2.png)

Azure cloud is one of wonderful cloud that could collect data from lot device or push data to lot device,for more details, click https://www.azure.cn/home/features/iot-hub/

 **Aim:** This page would guide you connecting your device(ESP32 or lot device with ESP32) to Azure by MQTT protocol, and then send data to Azure,receive message from Azure.Main workflow:
![](media/m5stack_starter_kit/PIC18.png)


<a name="Prerequisites"></a>
# Step 1: Prerequisites
 ------------------------------
- **ubuntu environment** for building your demo.
- **M5Stack IoT Kit** for running the demo.  
![](media/m5stack_starter_kit/PIC3.png)


<a name="Tools_prepare"></a>
# Step 2: SDK and Tools Preparation
 ------------------------------
 
  **iothub-explorer install**
 
 - The iothub-explorer tool enables you to provision, monitor, and delete devices in your IoT hub. It runs on any operating system where Node.js is available.

- Download and install Node.js from here.  https://nodejs.org/en/
- From a command line (Command Prompt on Windows, or Terminal on Mac OS X), execute the following:
  ```
    npm install -g iothub-explorer
  ```
##### if success, you can get version information like:
```shell
$ node -v
v6.9.5
$ iothub-explorer -V
1.1.6
```
if failed,please click http://thinglabs.io/workshop/esp8266/setup-azure-iot-hub/

### SDK get
You can get AZURE-SDK from https://github.com/ustccw/AzureESP32. This SDK can implement that connect ESP32 to Azure by MQTT protocol. You can get IDF-SDK from https://github.com/espressif/esp-idf  
this SDK can make ESP32 work well  

### Compiler get
Follow the guide: http://esp-idf.readthedocs.io/en/latest/linux-setup.html

<a name="Config_build"></a>
# Step 3: Configuring and building
 ------------------------------
###  Update Variables
[/examples/project_template/user/iothub_client_sample_mqtt.c](#)

Update the connectionString variable to the device-specific connection string you got earlier from the Setup Azure IoT step:
```
static const char* connectionString = '[azure connection string]'
```
The azure connection string contains Hostname, DeviceId, and SharedAccessKey in the format:
```
"HostName=<host_name>;DeviceId=<device_id>;SharedAccessKey=<device_key>"
 ```
### Config your Wifi
 ```
 make menuconfig
 ```
 choose example configuration to **set Wifi SSID and Password!**

### Build your demo and flash to ESP32
 ```
 $make flash
 ```
 if failed,try:
 - make sure that ESP32 had connect to PC by serial port
 - make sure you flash to correct serial port
 - try type command:
   > sudo usermod -a -G dialout $USER

<a name="Results"></a>
# Step 4: Result shows
 ------------------------------
login iothub-explorer,and monitor events:
```
iothub-explorer monitor-events AirConditionDevice_001 --login '<connection-string>' --login 'HostName=youriothub-ms-lot-hub.azure-devices.cn;SharedAccessKeyName=iothubowner;SharedAccessKey=zMeLQ0JTlZXVcHBVOwRFVmlFtcCz+CtbDpUPBWexbIY='
```
-  Restart ESP32 after bin had flashed,you would see the ESP32 send data to lothub-explorer by minicom,and iothub-explorer would receive data!
- At the same time,you can send message to ESP32 by iothub-explorer until you send a quit message

![](media/m5stack_starter_kit/ReceiveMessageFromDevice.png)

<a name="TroubleShoot"></a>
Trouble Shoot
 ------------------------------
 - close some firewall settings
 - build failed,try:
   - git submodule init
   - git submodule update
   - export your compiler path
   - export your SDK path
   - get start with http://espressif.com/en/support/download/documents?keys=&field_type_tid%5B%5D=13
   - make sure you had input correct device connect-string which get from Part 3

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
[lnk-setup-iot-hub]: ../../setup_iothub.md
[lnk-manage-iot-hub]: ../../manage_iot_hub.md
