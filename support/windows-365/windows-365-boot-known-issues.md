---
title: Windows 365 Boot known issues
description: Learn about known issues with Windows 365 Boot, including workarounds and updated fixes.
manager: dcscontentpm
ms.date: 03/31/2025
ms.topic: troubleshooting
ms.reviewer: elluthra, erikje
ms.custom:
- pcy:Windows 365 Boot
- sap:WinComm User Experience
ms.collection:
- M365-identity-device-management
- tier2
---

# Windows 365 Boot known issues

This article provides solutions to the recent known issues with [Windows 365 Boot](/windows-365/enterprise/windows-365-boot-overview).

## Wi-Fi connectivity

Wi-Fi connections that require captive browser-based authentication or consent aren't supported.

User-based Wi-Fi profiles that require the user to sign in to connect to Wi-Fi aren't supported. New users who never signed in before converting the device to a Windows 365 Boot device can't use the device to connect to the network.

## VPN support

Windows 365 Boot only supports VPN clients that don't require application installation. App-based VPN clients that require users to sign in aren't supported because users can't interact with the VPN client from a Windows 365 Boot device.

## Exiting the session on sleep or closing the device

The user isn't completely signed out after closing the laptop lid. When they open the laptop, they see a black screen or the app trying to disconnect. Eventually, it signs the user out of the session.

## Duplicate dialogs for some shortcut and sticky keys

Duplicate dialogs might be displayed for the physical device and the Cloud PC. This issue can occur for:

- Some shortcut keys, like <kbd>Win</kbd>+<kbd>G</kbd> or sticky keys.
- Enabling accessibility key settings, like High contrast, Num keys, or Toggle keys.

## Windows 365 Boot isn't supported in Kiosk mode

Windows 365 Boot isn't currently supported in Kiosk mode on Windows.

## Restricted access to the physical device

Windows 365 Boot doesn't completely restrict the user from accessing the physical device. For more information, see [Restrict user access to Windows 365 Boot physical device](/windows-365/enterprise/windows-365-boot-restrict-user-access-physical-device).

## Other sign-in options besides username/password are displayed on the sign-in screen

Windows 365 Boot is used with the Shared PC configuration service provider (CSP). The primary supported sign-in method is username/password.

### Solution

If you have Windows Hello for Business enabled for the Windows 365 Boot device, you can use Microsoft Intune to disable it. For more information, see [Enable security keys for Windows sign-in](/azure/active-directory/authentication/howto-authentication-passwordless-security-key-windows#enable-security-keys-for-windows-sign-in).

If the Windows 365 Boot physical device lets users sign in using a convenience PIN, you can turn it off. For more information, see [AllowPINLogon](/windows/client-management/mdm/policy-csp-credentialproviders#allowpinlogon).

## Default credential provider is set to Security Key on the sign-in screen

When the **Use security keys for sign-in** policy setting is enabled, it can be configured to be the default credential provider. This policy might result in the user seeing a sign-in for the physical device.

### Solution

Check to see if you have the policy setting configured in Microsoft Intune. For more information on how to check, see [Enable security keys for Windows sign-in](/azure/active-directory/authentication/howto-authentication-passwordless-security-key-windows#enable-security-keys-for-windows-sign-in).

If it's set, exclude your Windows 365 Boot devices from the policy.

## Local device has background apps and previous policy configurations that impact the user's Windows 365 Boot experience

Windows 365 Boot uses "clean" Windows 11 devices that don't have preconfigured applications or policies assigned to the device.

### Solution

Reset the device to a "clean" state. For more information, see [Windows 365 Boot physical device requirements](/windows-365/enterprise/windows-365-boot-physical-device-requirements).

## Single sign-on users see a dialog to allow remote desktop connection during the connection attempt

When using [single sign-on](/entra/identity/enterprise-apps/what-is-single-sign-on), users are prompted to authenticate to Microsoft Entra ID and allow the Remote Desktop connection when launching a connection to a new Cloud PC. Microsoft Entra remembers up to 15 devices for 30 days before prompting again.

### Solution

If you see this dialog, select **Yes** to connect.

## Users can't launch the web browser to sign in to a Wi-Fi network

Windows 365 Boot is designed for Ethernet connections or Wi-Fi connections managed through the [WiFi CSP](/windows/client-management/mdm/wifi-csp).

### Solution

Configure the Windows 365 Boot physical device's Wi-Fi profile through Microsoft Intune. For more information, see [Add Wi-Fi settings for Windows 10/11 devices in Microsoft Intune](/mem/intune/configuration/wi-fi-settings-windows).

## Users see a black screen after using the Disconnect/Sign-out/Lock command from the Cloud PC

This known issue is under investigation.

### Solution

Select <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> and select the **Sign out** option.

## Microsoft Teams calls have poor performance

### Solution

Make sure Microsoft Teams optimizations are used as explained in [Microsoft Teams on Cloud PC](/windows-365/enterprise/teams-on-cloud-pc).

## Camera access is denied in the Cloud PC

Camera permissions must be granted to the Azure Virtual Desktop (HostApp) application to use your Windows 365 Boot configured device's camera in Microsoft Teams.

### Solution

1. [Remove Windows 365 Boot from the physical device](troubleshoot-windows-365-boot.md#remove-windows-365-boot-from-the-physical-device).
2. On the physical device, open **Settings** > **Privacy & Security** > **Camera** > **Let apps access your camera**.
3. Set **Azure Virtual Desktop (HostApp)** to **On**.
4. [Add Windows 365 Boot back onto the physical device](troubleshoot-windows-365-boot.md#add-windows-365-boot-back-to-the-physical-device).

## Users can still interact with physical device features like Settings, Task Manager, and Notifications

Users are currently blocked from accessing most features on their Windows 365 Boot physical devices. However, to assist with troubleshooting, some features aren't blocked.

### Solution

To learn how to restrict user access to the physical device, see [Restrict user access to Windows 365 Boot physical device](/windows-365/enterprise/windows-365-boot-restrict-user-access-physical-device).

## Users are disconnected from the Cloud PC after being idle for too long

Windows 365 Boot physical devices might sign out users because of screen idle policies applied to the physical device or Cloud PC.

### Solution

To use an Intune device configuration profile, change or configure the [DeviceLock CSP policy (MaxInactivityTimeDeviceLock)](/windows/client-management/mdm/policy-csp-devicelock#maxinactivitytimedevicelock). Make these changes for both the physical device and the Cloud PC.

## Users see multiple authentication dialogs even if single sign-on is enabled

### Solution

Check the Windows 365 Provisioning policy associated with the Cloud PC to see if [single sign-on](/entra/identity/enterprise-apps/what-is-single-sign-on) is enabled. If it isn't, enable single sign-on for the provisioning policy. This policy change requires reprovisioning the Cloud PC.

Your Conditional Access policies might also cause other authentication dialogs. For more information, see [Troubleshooting sign-in problems with Conditional Access](/azure/active-directory/conditional-access/troubleshoot-conditional-access).

When using single sign-on, users are prompted to:

- Authenticate to Microsoft Entra ID.
- Allow the Remote Desktop connection when launching a connection to a new Cloud PC.

Microsoft Entra remembers up to 15 devices for 30 days before prompting again.

If you see this dialog, select **Yes** to connect.

## Users see a black screen after signing in

Windows 365 Boot is configured by using the [CloudDesktop CSP](/windows/client-management/mdm/policy-csp-clouddesktop#boottocloudmode).

### Solution

If you configured your device for Windows 365 Boot using the [Windows 365 Boot Guided Scenario](/windows-365/enterprise/windows-365-boot-guide):

1. Sign in to the [Microsoft Intune admin center](https://go.microsoft.com/fwlink/?linkid=2109431), and select **Devices** > **Configuration profiles**.
2. Search for the device configuration profile that contains "Windows 365 Boot Device Configuration Policy" in its name.
3. Make sure that the "Cloud Desktop" configuration is configured with the **Windows 365 Boot Mode** setting set to **Enable Windows 365 Boot Desktop**.
4. Select **Device assignment status** and make sure that the configuration policy was successfully applied to it.
5. If the check-in status isn't successful, see [Troubleshooting policies and profiles in Microsoft Intune](../mem/intune/device-configuration/troubleshoot-policies-in-microsoft-intune.md).

If the problem persists, reinstall the physical device's operating system as explained in [Windows 365 Boot physical device requirements](/windows-365/enterprise/windows-365-boot-physical-device-requirements).

## Users see the local PC desktop when responding to authentication dialogs

Windows 365 Boot is configured through the [Windowslogon CSP](/windows/client-management/mdm/policy-csp-windowslogon#overrideshellprogram).

### Solution

If you configured your device for Windows 365 Boot using the [Windows 365 Boot Guided Scenario](/windows-365/enterprise/windows-365-boot-guide):

1. Sign in to the [Microsoft Intune admin center](https://go.microsoft.com/fwlink/?linkid=2109431), select > **Devices** > **Configuration profiles**.
2. Search for the device configuration profile that contains "Windows 365 Boot Device Configuration Policy" in its name.
3. Make sure that the "Windows Logon" configuration is configured with the **Override Shell Program** setting set to **Apply Lightweight shell**.
4. Select **Device assignment status** and make sure that the configuration policy was successfully applied to it.
5. If the check-in status isn't successful, see [Troubleshooting policies and profiles in Microsoft Intune](../mem/intune/device-configuration/troubleshoot-policies-in-microsoft-intune.md).

If the problem persists, reinstall the physical device's operating system as explained in [Windows 365 Boot physical device requirements](/windows-365/enterprise/windows-365-boot-physical-device-requirements).

## Error message: Windows 365 can't connect to the resources it uses to run properly

### Solution

Check your device's network connection and try again. If you use an Azure network connection (ANC) for your Cloud PC, check the ANC status as explained in [Azure network connection status](/windows-365/enterprise/health-checks#azure-network-connection-status).

For more information, see [Troubleshoot Cloud PC connection errors](/windows-365/enterprise/connection-errors).

## Error message: Something is preventing you from using the Windows 365 app

### Solution

For more information, see [Troubleshoot Windows 365 Boot](/windows-365/enterprise/troubleshoot-windows-365-boot).

## Users can't connect to their Cloud PC and continue to wait at the transition screen

### Solution

Make sure that the device has applications that support Windows 365 Boot.

If the applications meet the minimum version requirements, collect diagnostic logs and [contact Microsoft support](troubleshoot-windows-365-boot.md#contact-microsoft-support).

## Error message: Remote Desktop Gateway server is temporarily unavailable

### Solution

This issue might be a transient issue because of network congestion. Try signing in again after waiting some time. If the issue persists, collect diagnostic logs and [contact Microsoft support](troubleshoot-windows-365-boot.md#contact-microsoft-support).

## Error message: You need to be assigned a Cloud PC

Windows 365 Boot requires users to have a Windows 365 Cloud PC provisioned for them.

### Solution

Create and assign a Windows 365 provisioning policy. For more information, see [Create provisioning policies](/windows-365/enterprise/create-provisioning-policy).

## An error screen occurs when users sign in to a device for the first time

This error screen occurs when a device is removed from and then re-enrolled in Windows 365 Boot mode. The original registration hasn't completed uninstallation yet.

### Solution

Users should try signing in a second time on the device. In most instances, they should be able to connect to their Windows 365 Cloud PC. If they still see the error screen, contact Microsoft support with the displayed correlation ID.

## Resources created by the Windows 365 Boot guided scenario are shown as "not applicable" in Microsoft Intune

Resources created by the Windows 365 Boot guided scenario can be applied to both:

- Microsoft Entra joined devices.
- Microsoft Entra hybrid joined devices in tenants where certain workloads are switched to Microsoft Intune. For Microsoft Entra hybrid joined devices, Windows Update policies, device configuration, and client app workloads must be switched to Microsoft Intune.

### Solution

To determine if your device is Microsoft Entra hybrid joined, see [Using the Azure portal](/azure/active-directory/devices/howto-hybrid-join-verify#using-the-azure-portal). If your device is Microsoft Entra hybrid joined, see [co-management workloads](/mem/configmgr/comanage/workloads) to see what workloads are configured in your environment.

## Users can't reset their password on the Windows 365 Boot device

If the user needs to reset their password, it isn't possible on their Windows 365 Boot device.

### Solution

Users should reset their password on another non-Windows 365 Boot configured device.

## Error message: The Provider app couldn't be found

The user tries to connect to their Cloud PC but receives the following error message:

> Can't connect to Cloud PC from this device  
> The provider app could not be found. Try signing in from another device.  
> To resolve this issue here, contact support.

This error occurs in the following scenarios:

- The provider app is uninstalled.
- The provider app isn't available or installed on the end user's physical device.
- The provider app is installed at the user scope manually before the device is set up for Boot.

### Solution

The end user can use one of these options:

- Sign in from another device.
- Wait for the app to install on the physical device.
- Contact the user's IT administrators and ask them to push the app to the device.
- If you suspect the app is installed in the user scope, use the following steps:

    1. Contact the user's IT administrators to remove the device from Boot mode by removing it from the device group.
    2. Uninstall the provider app installed in the user scope.
    3. Put the device back into the Boot mode device group (the system scope apps should be delivered by Microsoft Intune if set up through a guided scenario).

If the issue persists, contact support.

## Can't share the physical device's local settings screen while using Microsoft Teams on a Cloud PC

If a user has a local setting screen (like Local Bluetooth settings) open on their physical device, they can't share their screen on a video call in Microsoft Teams.

### Solution

If the user must share a local settings screen, use a different tool like Quick Assist.

## Can't return to the Cloud PC after signing in to the physical device 

### Solution

If the user still doesn't return to the Cloud PC sign-in screen after signing out of the physical device, restart the physical device.  

## Next steps

For more information, see [Troubleshoot Windows 365 Boot](/windows-365/enterprise/troubleshoot-windows-365-boot).
