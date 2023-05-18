# Integrating Workspace ONE UEM with Android

Workspace ONE UEM powered by AirWatch providess you with a robust set of mobility management solutions for enrolling, securing, configuring, and managing your Android device deployment. Through the Workspace ONE UEM console, you have several tools and features at your disposal for managing the entire life cycle of corporate and employee owned devices.

The guide explains how to integrate Workspace ONE UEM as your Enterprise Mobility Manager (EMM) with Android devices.

##Key Terms for Android

These key terms associated with Android will help you in understandang how to configure and deploy settings to your users.

-   **Work Profile**– Work Profile mode, also known as Profile Owner, creates a dedicated container on your device for only business applications and content. Work Profile mode allows organizations to manage the business data and applications but not have access to the user's personal data and apps. The Android apps are denoted with a briefcase icon so they are distinguishable from the personal apps. 
-   **Work Managed**– Work Managed mode, also referred to as Device Owner or Fully Managed Mode, locks the whole device. Users will have access to corporate apps and no access to personal apps through the Google Playstore.
-   **Corporate Owned Personally Enabled** – Corporate Owned Personally (COPE) refers to company-owned devices, similar to Work Managed Device, but users receive a Work Profile to access corporate applications. They still have access to their personal Google Play Store outside of the Work Profile. COPE is available on Android 8.0 or later deivces only. 
-   **Managed Google Account** – Refers to the Google account registered to the device used for Android and provides Android app management through Google Play. This account is managed by the domain that manages your Android configuration.
-   **Managed Google Play Account** - For organizations that want to set up Android but do not have G Suite Accounts or Managed Google Accounts.
-   **Google Service Account** – The Google Service Account is a special Google account that is used by applications to access Google APIs recommended for G Suite customers.
-   **EMM Token** – Unique ID that Workspace ONE UEM uses to connect the Workspace ONE UEM console to the Managed Google Account.
-   **Managed Google Domain** – Domain claimed for enabling Android associated with your enterprise.
-   **Google Domain Setup** – Google process for claiming a managed Google domain.
-   **AirWatch Relay** – The Workspace ONE UEM application admins use to bulk enroll Android Devices into Workspace ONE UEM.
-   **NFC Bump** – A communication technology that allows devices to exchange information by placing them next to each other - known as a 'bump'. This is done while using the AirWatch Relay app to pass information from the parent device to the child device.
-   **AOSP/Closed Network** – Android Open Source Project or Closed Network refers to Android devices without Google Mobile Services (GMS) and Console environments with no access to Google. No Google account is created with this enrollment mode.
-   **User-based enrollment** - When a device is enrolled, the Google account that is created is the same across all devices enrolled by this employee. This enrollment method is ideal for when you assign employees to devices with no staging involved.
-   **Device-based enrollment** - The generated Google acccount is unique to each device enrolled by the same user. This is ideal for a staging device or dedicated devices.
-   **Cap and Grow** - Cap and Grow allows you to continue using your current device deployment as you make the transition from Android (Legacy) to Android Enterprise. Any new device rollouts can be enrolled into Android Enterprise and be managed with older devices.

## Requirements for Using Android With Workspace ONE UEM

Before deploying Android devices, consider the following pre-requisites, requirements for enrollment, supporting materials, and helpful suggestions from the Workspace ONE UEM team.

### Supported Operating Systems

Android 5.X.X (Lollipop)

Android 6.X.X

Android 7.X.X

Android 8.X.X

Android 9.X.X

**Note:** LG Service Application is no longer supported on LG devices running Android 9 and later with Android (Legacy) deployments. If you are using LG devices on Android 9 or later using the Android Legacy enrollment method, consider migrating to Android Enterprise.

Android 10.X.X

Android 11.X.X

Android 12.X.X

Android 13.X.X

**Note:** Customers will experience an updated privacy conscious feature set when a COPE enrolled device is upgraded from Android 10 to Android 11. A summary of the key features and functionality of COPE devices can be found in Understanding Android Device Modes.

**Note:** If your organization requires more time to complete testing, there are two options to delay your devices upgrading to Android 11. See Manage System Updates for Android Devices.

If your devices do not support Google Play EMM Integration, refer to Android (Legacy) deployment or use AOSP/Closed Network configuration.

For more information on AOSP/Closed Network, see Understanding Android Device Modes.

### Android GO Support

Workspace ONE UEM supports devices running Android GO in Work Managed mode only. For these, all device management capabilities for the Work Managed mode are supported with the exception of the following:
 
- Workspace ONE Launcher
- Product Provisioning features that require accessing or modifying files or directories on the device 
    - Files/Actions - Only Reboot and Run Intent Actions are supported
    - Conditions - All Conditions except Launcher are supported
    - Event/Actions - All Actions except Apply Custom Settings are supported

### Network Requirements for Android

End-user devices must be able to reach certain endpoints for access to apps and services. The Network Requirements for Android is a list of known endpoints for current and past versions of enterprise management APIs.

To reach all the endpoints successfully, a direct connection is required. If the devices are connected behind a proxy, the direct communication is not possible and certain functions fail.

|Destination Host|Ports|Purpose|
|----------------|-----|-------|
|play.google.com,android.com,google-analytics.com, \*.googleusercontent.com,\*gstatic.com,\*gvt1.com*, \*ggpht.com,dl.google.com,dl-ssl.google.com, android.clients.google.com,\*gvt2.com,\*gvt3.com|TCP/443TCP,UDP/5228-5230|Google Play and updatesgstatic.com,* googleusercontent.com - contains User Generated Content (e.g. appicons in the store)*gvt1.com, *.ggpht, dl.google.com,dl-ssl.google.com,android.clients.google.com -Download apps and updates, PlayStore APIs, gvt2.com and gvt3.com are usedfor Play connectivity monitoring fordiagnostics.|
|*.googleapis.com|TCP/443|EMM/Google APIs/PlayStore APIs|
|accounts.google.com, accounts.google.\[country\]|TCP/443|Authentication For accounts.google.[country], use your local top-level domain for [country]. For example, for Australia use accounts.google.com.au, and for United Kingdom use accounts.google.co.uk.|
|fcm.googleapis.com, fcm-xmpp.googleapis.com|TCP/443,5228-5230|Firebase Cloud Messaging (e.g. Find My Device, EMM Console <-> DPC communication, like pushing configs).This does not work with proxies (see details [here](https://firebase.google.com/docs/cloud-messaging/concept-options#messaging-ports-and-your-firewall)).|
|pki.google.com, clients1.google.com|TCP/443|Certificate Revocation list checks for Google-issued certificates|
|clients2.google.com, clients3.google.com. clients4.google.com, clients5.google.com, clients6.google.com|TCP/443|Domains shared by various Google backend services such as crash reporting, Chrome Bookmark Sync, time sync (tlsdate), and many others|
|omahaproxy.appspot.com|TCP/443|Chrome updates|
|android.clients.google.com|TCP/443|CloudDPC download URL used in NFC provisioning|
|connectivitycheck.android.com www.google.com|TCP/443|Connectivity check prior to CloudDPC v470 Android connectivity check starting with N MR1 requires https://www.google.com/generate \_204 to be reachable, or for the given WiFi network to point to a reachable PAC file. Also required for AOSP devices running Android 7.0 or later.|
|www.google.com, www.google.com/generate\_204| |AOSP devices runnning Android 7.0 or later|
|android-safebrowsing.google.com, safebrowsing.google.com|TCP/443|Android application verification.|

### Device Services Proxy Requirements

The Workspace ONE UEM Device Services application uses Google's SafetyNet Attestation API to verify the integrity of Android devices and ensure they are not compromised. To do so, it makes outbound API calls to Google servers. In On-Premise environments, organizations may choose to only allow the Device Services application to make outbound connections via a proxy. In these cases, besides configuring the proxy settings at the application level via the Workspace ONE UEM Console, customers must also configure this outbound proxy at the system level for the Windows server that hosts the Device Services application. If the Windows server is unable to make outbound connections to the required Google endpoints, SafetyNet Health Attestation will fail.

### Firewall Rules for Consoles

If an EMM console is located on-premise, the destinations below need to be reachable from the network in order to create a Managed Google Play Enterprise and to access the ​Managed Google Play iFrame​.

These requirements reflect current Google Cloud requirements and are subject to change.

|Destination Host|Ports|Purpose|
|----------------|-----|-------|
|play.google.com, www.google.com|TCP/443|Google Play Store Play Enterprise re-enroll|
|fonts.googleapis.com\*, .gstatic.com|TCP/443|iFrame JS, Google fonts, User Generated Content (e.g. appicons in the store)|
|accounts.youtube.com, accounts.google.com, accounts.google.com.\*|TCP/443|Account Authentication, Country-specific account authdomains|
|apis.google.com, ajax.googleapis.com|TCP/443|GCM, other Google web services, and iFrame JS|
|clients1.google.com, payments.google.com, google.com|TCP/443|App approval|
|ogs.google.com|TCP/443|iFrame UI elements|
|notifications.google.com|TCP/443|Desktop/Mobile Notifications|

### Enrollment Requirements

Each Android device in your organization's deployment must be enrolled before it can communicate with Workspace ONE UEM and access internal content and features. The following information is required prior to enrolling your device.

If an email domain is associated with your environment – If using Auto Discovery:

-   **Email address** – This is your email address associated with your organization. For example, **JohnDoe@acme.com**.
-   **Credentials** – This **username** and **password** allows you to access your Workspace ONE UEM environment. These credentials may be the same as your network directory services or may be uniquely defined in the Workspace ONE UEM console.

If an email domain is not associated with your environment - If not using Auto Discovery:

If a domain is not associated with your environment, you are still prompted to enter your email address. Since auto discovery is not enabled, you are then prompted for the following information:

-   **Group ID** – The Group ID associates your device with your corporate role and is defined in the Workspace ONE UEM console.
-   **Credentials** – This unique user name and password pairing allows you to access your AirWatch environment. These credentials may be the same as your network directory services or may be uniquely defined in the Workspace ONE UEM console .

To download the Workspace ONE Intelligent Hub and subsequently enroll an Android device, you need to complete one of the following:

-   Navigate to https://www.getwsone.com and follow the prompts.
-   Download Workspace ONE Intelligent Hub from the Google Play Store.

### Enrollment Restrictions for Android

Enrollment restrictions allows you to provision enrollment such as restricting enrollment to known users, user groups, and number of enrolled devices allowed.

These options are available by navigating to **Groups & Settings** > **All Settings** > **Devices & Users** > **General** > **Enrollment** and choosing the **Restrictions** tab allows you to customize enrollment restriction policies by organization group and user group roles.

You can create enrollment restrictions based on:

-   Android manufacturer and model to ensure only approved devices are enrolled into Workspace ONE UEM. When an Android device is enrolled, smart group and enrollment restriction criteria is updated to include the new make and model of the device.

    **Note:** Some devices are manufactured by other vendors. You can create a policy with the actual manufacturer of the device for policies to come into effect.The following are some ways to identify the device manufacture:

    -   Navigate to the **About** page in device settings.
    -   With an adb command: `adb shell getprop | grep "manufacturer"`.
-   Blacklist or whitelist devices by UDID, IMEI, and serial number.

    **Note:** When enrolling Android 10 or later devices into Work Profile mode, the devices are held in a pending status until the UEM console is able to retrieve the IMEI or Serial Number from the the devices to see if they are whitelisted or black listed. Until this is verified, the device will not be fully enrolled nor any work data sent until enrollment is complete.

## Understanding Android Device Modes

Android’s built-in management features enable IT admins to fully manage devices used exclusively for work.

Android offers several modes depending on the ownership of the device being used within your organization:

-   **Work Profile**: Creates a dedicated space on the device for only work applications and data. This is the ideal deployment for Bring Your Own Device (BYOD) applications.
-   **Work Managed Device**: Allows Workspace ONE UEM and IT admin to control the entire device and enforce an extended range of policy controls unavailable to work profiles, but restricts the device to only corporate use
    -   **Corporate Owned Personally Enabled**: Refers to company-owned devices, similar to Work Managed Device, but is provisioned with a Work Profile which uses both personal and corporate use.
    -   **Work Managed Device Without Google Play Services**: If you are using Workspace ONE UEM on Android Open Source Project (AOSP) devices, non- GMS devices, or using closed networks within your organization, you can enroll your Android devices using the Work Managed Device enrollment flow without Google Play Services

### Work Profile Mode Functionality

Applications in the Work Profile are differentiated by a red briefcase icon, called badged applications, and are shown in a unified launcher with the user's personal applications. For example, your device shows both a personal icon for Google Chrome and a separate icon for Work Chrome denoted by the badge. From an end-user perspective, it looks like two different applications, but the application is only installed once with business data stored separately from personal data.

The Workspace ONE Intelligent Hub is badged and exists only within the Work Profile data space. There is no control over personal applications and the Workspace ONE Intelligent Hub does not have access to personal information.

There are a handful of system applications that are included with the Work Profile by default such as Work Chrome, Google Play, Google settings, Contacts, and Camera – which can be hidden using a restrictions profile.

Certain settings show the separation between personal and work configurations. Users see separate configurations for the following settings:

-   **Credentials** – View corporate certificates for user authentication to managed devices.
-   **Accounts** – View the Managed Google Account tied to the Work Profile.
-   **Applications** – Lists all applications installed on the device.
-   **Security** – Shows device encryption status.

### Work Managed Device Mode Functionality

When devices are enrolled in Work Managed Device mode, a true corporate ownership mode is created. Workspace ONE UEM controls the entire device and there is no separation of work and personal data.

Important things to note for the Work Managed mode are:

-   The homescreen does not show badged applications like Work Profile mode.
-   Users have access to various pre-loaded applications upon activation of the device. Additional applications can only be approved and added through the Workspace ONE UEM console.
-   The Workspace ONE Intelligent Hub is set as the device administrator in the security settings and cannot be disabled.
-   Unenrolling the device from Work Managed mode prompts device factory reset.

### Work Managed Device Without Google Play Services

If you are using Workspace ONE UEM on Android Open Source Project (AOSP) devices, non- GMS devices, or using closed networks within your organization, you can enroll your Android devices using the Work Managed Device enrollment flow without Google Play Services. You can host apps on your organization's intranet and use OEM specific enrollment methods for deployment.

You will need to specify in the UEM console that you are using AOSP/Closed Network during Android EMM Registration.

Things to consider when using Work Managed Device Without Google Play Services on AOSP/Closed Network deployments:

-   If you have already setup Android at a top Organization Group and want to deploy AOSP/ Closed network at a specific child Organization Group only, the UEM console admin has an option to specify that out of box enrollments at the child Organization Group will not have a managed Google account. For more information, see Enrollment Settings in the Android EMM Registration.
-   If you are deploying devices using Workspace ONE UEM 1907 and below, there is no UEM console configuration required.
-   If you are deploying devices using Workspace ONE UEM 1908 and higher, you must configure the settings in the Android EMM Registration page.
-   The supported enrollment methods are:
    -   QR Code
    -   StageNow for Zebra devices
    -   Honeywell Enterprise Provisioner for Honeywell devices
-   Enrollment through Workspace ONE Intelligent Hub identifier is not supportet on AOSP devices.
-   Public Auto Update profile is not supported. This profile is specifically for public apps and will not function on devices on AOSP or closed networks
-   Factory Reset Protection profile is not supported.
-   Internal apps (hosted in the Workspace ONE UEM console) will deploy silently to the AOSP/ Closed network devices.
-   Work Managed devices enrolled without a managed Google account should not be assigned any public apps and should not be considered in public app assignment device counts.
-   OS Version & OEM requirements for Work Managed Device without Google Play Services:
    -   AOSP (non-GMS)
        -   Zebra and Honeywell - Must be on an OS version that supports StageNow or Honeywell Enterprise Provisioner enrollment.
        -   Other OEMs - Not supported unless OEM develops support for it via a client like StageNow or by allowing users to access QR Code enrollment.
    -   Closed Network
        -   Zebra and Honeywell - Android 7.0 and higher or must be on an OS version that supports StageNow (also 7.0 or higher) or Honeywell Enterprise Provisioner enrollment.
        -   Other OEMs - Android 7.0 or higher since QR Code enrollment is the only supported method.
-	When a Work Managed device is configured without Google Play Services, Workspace ONE Intelligent Hub needs to be configured to use AWCM instead Firebase Cloud messaging.  Without this update, devices will not receive push notifications from the console.

### Corporate Owned Personally Enabled (COPE) Mode

When devices are enrolled using COPE mode, you still control the entire device. The unique capability with COPE mode is that it allows you to enforce two separate sets of policies, such as restrictions, for the device and inside a Work profile.

COPE mode is only available on Android 8.0+ devices. If you enroll Android devices below Android 8.0, the device automatically enrolls as Fully Managed Device.

There are some caveats to consider when enrolling devices into COPE mode:

-   For new enrollments, using Android 11 must use Workspace ONE Intelligent Hub 20.08 for Android and Workspace ONE UEM console 2008. For specific information, see [Changes to Corporate Owned Personally Enabled (COPE) in Android 11](https://kb.vmware.com/s/article/79915?lang=en_US).

-   Pin Based encryption and Workspace ONE UEM Single Sign On by using SDK is not supported for Corporate Owned Personally Enabled devices. A work passcode can be enforced to ensure that the use of work applications requires the use of a passcode.
-   Single user staging and Multi-user staging are not supported for COPE enrollments.
-   Internal applications (hosted in Workspace ONE UEM) and public applications deployed to COPE devices are shown in the application Catalog within the Work Profile.
-   Similar to Work Profile only enrollments, Corporate Owned Personally Enabled devices provide users the option to disable the Work Profile (for example, if the user is on vacation). When the Work Profile is disabled, the work applications no longer present notifications and cannot be launched. The status (Enabled or Disabled) of the Work Profile is presented to the admin on the Device Details page. When the Work Profile is disabled, the latest application and profile information cannot be retrieved from the Work Profile.
-   The Workspace ONE Intelligent Hub exists in the Fully Managed and the Work Profile sections of the Corporate Owned Personally Enabled device. By existing both inside and outside the Work Profile, management policies can be applied within the Work Profile and the entire device. However, the Workspace ONE Intelligent Hub is only visible within the Work Profile.
-   When push notifications are sent to the device, the Workspace ONE Intelligent Hub outside the Work Profile is temporarily available for the user to view messages, ensuring that critical messages reach the user even if the Work Profile is temporarily disabled.
-   Assigned profiles can be viewed through the Workspace ONE Intelligent Hub in the Work Profile.
-   Compliance policies for application management (such as block/ remove applications) are only supported for applications within the Work Profile. Applications can be blacklisted on the device (outside the Work Profile) by using Application Control profiles.
-   On Android 11+ COPE devices, you can choose to Enterprise Wipe devices instead of performing a full Device Wipe. You can still use the Device Wipe command to perform a full device wipe. When you do an Enterprise Wipe, the device deletes the Work Profile and returns ownership of the device to the user. The users personal data is untouched.
-   Product Provisioning is not supported on COPE enrollments.
-   **Android 11 Specific Changes**:
    -   Internal applications (hosted by Workspace ONE UEM) can no longer be pushed on the personal side of the device. Both internal apps (as private apps) and public apps must be deployed to the Work profile only.
        -   Any other functionality such as Compliance Rules that rely on Internal applications will also no longer be supported.
    -   The enrollment method afw\#hub will no longer be supported.
        -   Consider using QR code or Zero Touch enrollment instead.
    -   If your organization requires more time to complete testing, there are two options to delay your devices upgrading to Android 11. 
        For specific information, see [Changes to Corporate Owned Personally Enabled (COPE) in Android 11](https://kb.vmware.com/s/article/79915?lang=en_US).
