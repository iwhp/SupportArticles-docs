---
title: Access is denied when you promote domain controller
description: Provides a solution to fix an error (Access is denied) that occurs when you create NTDS Settings object.
ms.date: 01/15/2025
manager: dcscontentpm
audience: itpro
ms.topic: troubleshooting
ms.reviewer: kaushika, shamilprem
ms.custom:
- sap:active directory\dcpromo and the installation or removal of domain controllers
- pcy:WinComm Directory Services
---
# "Access is denied" error when you try to create NTDS Settings object

This article provides a solution to fix an error (Access is denied) that occurs when you promote Windows Server 2012 R2 or later domain controllers in an existing domain.

_Original KB number:_ &nbsp; 3207962

## Symptoms

When you try to promote Windows Server 2012 R2 or later domain controllers in an existing domain, the operation fails with an "Access is denied" error. This issue occurs even when the user is a member of the Domain Admins or Enterprise Admins group.

In this situation, the administrator sees the following error message:

Title: Windows Security  
Message Text: Network Credentials  
> The operation failed because: Active Directory Domain Services could not configure the computer account \<hostname>$ to the remote Active Directory Domain Controller account \<fully qualified name of helper DC>. "Access is denied"

The failure occurs when adding the NTDS Settings object for the new Domain Controller, returning the following error message:  

The operation failed because:  

Active Directory Domain Services could not create the NTDS Settings object for this Active Directory Domain Controller CN=NTDS Settings,CN=TEST-DC,CN=Servers,CN=mysite,CN=Sites,CN=Configuration,DC=domain,DC=com on the remote AD DC **DCName.ChildDomain.domain.com**. Ensure the provided network credentials have sufficient permissions.
> "Access is denied."

Additionally, the DCPromo.log file shows the following errors:  

2705DateTime[INFO]  
> Error - Active Directory Domain Services could not create the NTDS Settings object for this Active Directory Domain Controller CN=NTDS Settings,CN=TEST-DC,CN=Servers,CN=mysite,CN=Sites,CN=Configuration,DC=domain,DC=com on the remote AD DC **DCName.ChildDomain.domain.com**. Ensure the provided network credentials have sufficient permissions. (5)  

DateTime[INFO] EVENTLOG (Error): NTDS General / Internal Processing: 1168
Internal error: An Active Directory Domain Services error has occurred.

Additional Data

Error value (decimal):  
> -1073741823  

Error value (hex):  
> c0000001  

Internal ID:
30017c6

...  
DateTime[INFO] NtdsInstall for `ChildDomain.domain.com` returned 5  
DateTime [INFO] DsRolepInstallDs returned 5  
DateTime [ERROR] Failed to install to Directory Service (5)  
DateTime[ERROR] DsRolepFinishSysVolPropagation (Abort Promote) failed with 8001  
DateTime[WARNING] Failed to abort system volume installation (8001)  
DateTime[INFO] Starting service NETLOGON  
DateTime[INFO] Configuring service NETLOGON to 2 returned 0  
DateTime[INFO] The attempted domain controller operation has completed  

Where the errors map to the following:

> Error code: 0xc0000001  
> Symbolic name: STATUS_UNSUCCESSFUL  
> Error description: {Operation Failed} The requested operation was unsuccessful.

> Error code: 0x5  
> Symbolic name: ERROR_ACCESS_DENIED  
> Error description: Access is denied.

:::image type="content" source="media/access-denied-create-ntds-settings-object/error-mappings.png" alt-text="Error mappings that contain error code, symbolic name, error description, and header.":::

## Cause

This issue occurs because the Add/Remove Replica In Domain permission is missing for the Domain Admins and Enterprise Admins groups on the domain partition of the domain.

## Resolution

To resolve this issue, follow these steps:  

1. Verify that all the steps and conditions in the "Resolution" section of Knowledge Base article [2002413](https://support.microsoft.com/help/2002413) are true for your environment.

2. If domain controller promotion still fails even after you make sure that the user also has the SeEnableDelegationPrivilege permission, check ADSIEdit.msc to verify the user's effective permissions for the domain partition:  

   1. Click Start, click Run, and then type adsiedit.msc.
   2. Expand Default naming context, right-click *DC=domain,DC=com*, and then click **Properties**.
   3. On the **Security** tab, click the **Advanced** button.
   4. On the Effective Access tab, enter the user or group name of the user who is performing the operation that's failing in DCPromo.
   5. Confirm whether the Add/remove replica in domain control access permission has been granted.

      :::image type="content" source="media/access-denied-create-ntds-settings-object/add-remove-replica-in-domain.png" alt-text="Add/remove replica in domain control access permission.":::

3. If the Add/Remove Replica In Domain permission is missing for the user or group, add it by using ADSIEdit.msc:  

   1. Click Start, click Run, and then type adsiedit.msc.
   2. Expand Default naming context, right-click *DC=domain,DC=com*, and then click **Properties**.
   3. On the **Security** tab, click the **Advanced** button.
   4. On the Permissions tab, add the Add/remove replica in domain control access permission for the desired user or group as follows:

      Type: Allow  
      Applies to: This object only  

## More information

> [!NOTE]
> there could be additional reasons why a domain controller promotion or demotion fails with an "Access is denied" error. For more information, see [KB 2002413](https://support.microsoft.com/help/2002413).
