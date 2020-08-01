<!---
title: Ear SoM firmware updating                    # the article title to show on the browser tab
description: Walks a user through the ear SoM firmware update process for Project Santa Cruz Private Preview (July 2020). 
author: elqu20      # the author's GitHub ID - will be auto-populated if set in settings.json
ms.author: v-elqu     # the author's Microsoft alias (if applicable) - will be auto-populated if set in settings.json
ms.date: {@date}           # the date - will be auto-populated when template is first applied
ms.topic: reference  # the type of article
--->
# Ear SoM firmware updating

## Prerequisites:

-	Host PC.
-	Project Santa Cruz Devkit
-	[Onboarding](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/getting_started/onboarding_july_2020.md) completion.
-	[Devkit setup](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/getting_started/devkit_unboxing_setup_july_2020.md) completion.
-	[OOBE](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/getting_started/oobe_july_2020.md) completion.

## Ear SoM Firmware Update Procedure: 

1. Connect the Azure Ear SoM to the carrier board of the devkit. 

1. Plug in and power on the devkit. There are three LEDs on the Azure Ear SoM, labeled L01, L02, and L03. Upon powering on the devkit, L01 and L02 will turn green, and L02 will flash on and off for several seconds, indicating that the SoM authentication is in progress. After authentication is complete, all three LEDs will turn blue. 

    ![ear_som_led](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_ear_som_led.png)
 
1. On your PC, login to the [Azure Portal](https://ms.portal.azure.com/#home) and click All resources under the Azure services section of the portal homepage. 

    ![azure_services](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_azure_services_all_resources.png)

1. On the All resources page, click on the name of the IoT Hub that was provisioned to your devkit during the OOBE process. 

    ![all_resources](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_all_resources.png)

1. On the left side of the IoT Hub page, click on IoT Edge under Automatic Device Management. On the IoT Edge devices page, find the device ID of your devkit and click to open. 

    ![iot_hub](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_iot_hub.png)

1. Click Set Modules at the top of your devkit’s IoT Edge device page.

    ![set_modules](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_set_modules.png)

1. Find speechclient on the IoT Edge Modules page and click to open it. 

    ![speechclient](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_speechclient.png)

1. Select stopped under the Desired Status drop-down menu and click Update.

    ![desired_status_stopped](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_desired_status_stopped.png)

1. Back on the IoT Edge Modules page, click devmmclient. This will open the IoT Edge Module Details page for devmmclient. 

    ![devmmclient](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_devmmclient.png)

1. Click Direct method on the devmmclient Iot Edge Module Details page.

    ![direct_method](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_direct_method.png)

1. On the Direct method page, set Method Name to UpgradeDev and enter the following into the Payload box:
    ```console
    {
    “PID_VID”:”<id>”,
    “PKG_URL”:”<encoded url>”,
    “TASK_ID”:”<integer>”
    }
    ```
    The PID_VID is the USB VID and PID of the Ear SoM Codec, which is currently VID045ePID0671. The TASK_ID is a sequence ID, which can be set to any integer. The PKG_URL is a Base64 encoded URL for the firmware package update, which will be available to you through Private Preview. If you cannot access the PKG_URL for firmware updates, please contact ____.

    After entering the Method Name and Payload information, click Invoke Method to run the firmware update. If the PKG_URL is invalid, you will receive a success message, but the codec will reject the invalid package and keep the current firmware version. 

    ![upgrade_dev](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_upgrade_dev.png)

1. Click Direct method on the IoT Edge Module Details page to open the Direct method page for devmmclient a second time. Set the Method Name to QueryDevInfo, remove any settings from the Payload box, and click Invoke Method. The new firmware version can be seen in the Result box at the bottom of the page. 

    ![query_dev_info](https://github.com/Azure/AI-at-Edge-Preview/blob/main/user_guides/updates/article_images/firmware_query_dev_info.png)

1. Return to the Set Modules page and select speechclient to open the speechclient Update IoT Edge Module page again. Select running under the Desired Status drop-down menu and click Update. 

Congratulations! The firmware of the Azure Ear SoM has been successfully updated, and the device may be used normally. 
