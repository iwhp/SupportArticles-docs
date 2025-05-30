---
title: Troubleshoot connection errors
description: Troubleshoot connection errors in Windows 365.
manager: dcscontentpm
ms.date: 01/20/2025
ms.topic: troubleshooting
ms.reviewer: traceyadams, erikje
ms.custom:
- pcy:Session connectivity\Users are not able to make a connection
- sap:WinComm User Experience
ms.collection:
- M365-identity-device-management
- tier2
---
# Troubleshoot Cloud PC connection errors

The following errors can occur when connecting to a Cloud PC.

<a name='errors-when-connecting-to-an-azure-active-directory-azure-ad-joined-cloud-pc'></a>

## Errors when connecting to a Microsoft Entra joined Cloud PC

### The logon attempt failed

### Potential cause 1

Either the Cloud PC or the user's physical device denied PKU2U protocol requests. The PKU2U protocol is only triggered in the following cases:

- The Cloud PC is Microsoft Entra joined.
- The user is connecting from the Windows desktop client.
- The user's physical device is Microsoft Entra registered, Microsoft Entra joined, or Microsoft Entra hybrid joined to the same organization as the Cloud PC.

### Possible solution

Turn on PKU2U protocol requests on both the Cloud PC and the user's physical device:

1. [Create a filter for all Cloud PCs](/windows-365/enterprise/create-filter#create-a-filter-for-all-cloud-pcs).
2. Create a device configuration policy [using the settings catalog](/mem/intune/configuration/settings-catalog).
3. On the **Configuration settings** page, search for and select **Network Security Allow PKU2U Authentication Requests** > **Allow**.

    :::image type="content" source="./media/connection-errors/allow-pku2u.png" alt-text="Screenshot showing the Network Security Allow PKU2U Authentication Requests option set to Allow.":::

4. On the **Assignments** page, select **Add all devices** > **Edit filter** > **Include filtered devices in assignment**, and then select the filter you created for all Cloud PCs.
5. On the **Assignments** page, also select a Microsoft Entra group containing the user or the user's physical device.
6. Complete the creation of the device configuration policy.

If you only manage the user's physical device through Group Policy or you don't manage the user's physical device, you (or the user) can manage this setting through the [Allow PKU2U authentication requests to this computer to use online identities](/windows/security/threat-protection/security-policy-settings/network-security-allow-pku2u-authentication-requests-to-this-computer-to-use-online-identities) policy.

### Potential cause 2

[Per-user multifactor authentication](/azure/active-directory/authentication/howto-mfa-userstates) is turned on for the user account. Because it blocks sign-in, per-user multifactor authentication isn't supported for users connecting to Microsoft Entra joined Cloud PCs.

### Possible solution

[Remove per-user multifactor authentication](/azure/active-directory/devices/howto-vm-sign-in-azure-ad-windows#mfa-sign-in-method-required) for all users connecting to Cloud PCs. Then, [set a Microsoft Entra Conditional Access policy](/windows-365/enterprise/set-conditional-access-policies) and assign it to the appropriate users.

## Specific connection errors

### We couldn't connect because there are currently no available resources

### Potential cause

There might be a resource issue on your Cloud PC.

### Possible solution

Sign in to [Windows 365](https://windows365.microsoft.com), select the cog icon next to the Cloud PC, and then select **Restart**.

### We couldn't connect to the gateway because of an error. If this keeps happening, ask your admin or tech support for help.

### Potential cause

This error can be caused by network configuration settings, like:

- Custom DNS settings
- Network Virtual Appliance blocking
- Network security group configuration
- Resource locks
- Blocks on required endpoints

### Possible solution

Review the settings and confirm that they aren't interfering with connections.

### The remote PC ended your session. If this keeps happening, contact your network administrator for assistance. Error code: 0x3

### Potential cause

This error can occur when the Cloud PC's processor is over-utilized.

### Possible solution

If the issue persists, sign in to [Windows 365](https://windows365.microsoft.com), select the cog icon next to the Cloud PC, and then select **Restart**.

### Connection Attempt timed out, Please try again, or An error occurred while accessing this resource

If you encounter these errors, make sure that you don't have a configured Cloud Service Provider (CSP) or Group Policy Object (GPO) that blocks remote desktop connections.

Intune CSP policy:

  Settings catalog: **Administrative Templates\\Windows Components\\Remote Desktop Services\\Remote Desktop Session Host\\Connections**

  **Allow users to connect remotely by using Remote Desktop Services**

GPO configuration path:

  **Computer Configuration\\Windows Components\\Remote Desktop Services\\Remote Desktop Session Host\\Connections**

  **Allow users to connect remotely by using Remote Desktop Services**

If you continue to experience issues, run the **Inspect Connection** option within the Windows App or the **Troubleshoot** option under **Manage my Cloud PC** located under the three dots.

## Other connection error causes

Some other possible causes for Cloud PC connection failures include:

### Out-of-date third-party VPN client versions

### Possible solution

Update VPN clients to the most up-to-date versions.

<a name='signing-in-to-the-cloud-pc-with-azure-active-directory-only-user-accounts'></a>

### Signing in to the Cloud PC with Microsoft Entra ID-only user accounts

### Possible solution

Windows 365 is currently a Microsoft Entra hybrid join device, requiring users to sign in with their on-premises Active Directory account.

### Using a client PC with Remote Credential Guard enabled

### Possible solution

Remote Credential Guard requires connectivity to the on-premises Active Directory Domain Controller on the client PC used to access the Cloud PC. This connection is only possible using a VPN solution. Using a KDC proxy isn't currently available for Windows 365.

### Azure WireServer may be blocked

Windows 365 Cloud PCs require access to Azure communication channels.

Make sure that IP address 168.63.129.16 is reachable through any security software installed on the Cloud PC or gateway devices used in the virtual network connected to your Azure network connection (ANC).

For more information, see [What is IP Address 168.63.129.16](/azure/virtual-network/what-is-ip-address-168-63-129-16).

## Other troubleshooting steps

### Move the Cloud PC to a new organizational unit (OU) with no group policies

Connection problems might be caused by settings delivered by group policies. To test this possible cause, you can move the Cloud PC to a separate OU that's blocked from receiving group policies.

### On-premises GPO may affect a Cloud PC's provisioning or behavior

Settings delivered by group policies might cause connection problems. To test this problem, you can move the Cloud PC to a separate OU that's blocked from receiving group policies.

### Intermittent disconnections

If users encounter intermittent connection issues, consider the following:

### Possible solution

- Make sure that SSL inspection/Termination is disabled for networks used by your Cloud PCs. For more information, see [Proxy server guidelines for Azure Virtual Desktop]( /azure/virtual-desktop/proxy-server-support).
- When using third-party software web gateways, make sure forced tunnel exceptions are in place for critical Azure Virtual Desktop (AVD) traffic. For more information, see [Optimizing RDP Connectivity for Windows 365]( https://techcommunity.microsoft.com/discussions/windows365discussions/optimizing-rdp-connectivity-for-windows-365/3554327).

## Next steps

[Review other troubleshooting steps](/windows-365/enterprise/troubleshooting)
