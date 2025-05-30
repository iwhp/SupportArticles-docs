---
title: Use Rsop.msc to gather computer policy
description: Describes how to use the Resultant Set of Policy utility to gather only computer-specific policy information.
ms.date: 01/15/2025
manager: dcscontentpm
audience: itpro
ms.topic: troubleshooting
ms.reviewer: kaushika, TIMTHO
ms.custom:
- sap:group policy\group policy management (gpmc or gpedit)
- pcy:WinComm Directory Services
---
# Use Resultant Set of Policy logging to gather computer policy information

This article describes how to use the Resultant Set of Policy utility (Rsop.msc) to gather only computer-specific policy information.

_Applies to:_ &nbsp; Windows Server (All supported versions)  
_Original KB number:_ &nbsp; 312321

## Use Rsop.msc

When using the Resultant Set of Policy utility, you can gather only computer-specific policy information:

1. Click **Start**, click **Run**, type *mmc* in the **Open** box, and then click **OK**.
2. Click **File**, click **Add/Remove Snapin**, and then click **Add** in the **Add/Remove** dialog box.
3. Click **Resultant Set of Policy**, click **Add**, and then click **Close** in the **Add/Remove Standalone Snapin** dialog box.
4. Click **OK** in the **Add/Remove** dialog box. The Resultant Set of Policy snapin is displayed in the MMC.
5. Click **Generate RSOP data** on the **Action** menu.
6. Click **Next**, click **Logging Mode**, and then click **Next**.
7. Click either **This Computer** or **Another Computer**, and then type the computer name.
8. Click **Select a specific user**, and then click the blank space that is below the listed users. This has the same effect as clicking **Do not display user policy settings in the results**.
9. Click **Next**, and then click **Next**. Only computer-specific settings are displayed.

## Data collection

If you need assistance from Microsoft support, we recommend you collect the information by following the steps mentioned in [Gather information by using TSS for Group Policy issues](../../windows-client/windows-troubleshooters/gather-information-using-tss-group-policy.md).

## Disclaimer

Starting with Microsoft Windows Vista Service Pack 1 (SP1), the Resultant Set of Policies (RSOP) report does not show all Microsoft Group Policy settings. To see the full set of Microsoft Group Policy settings applied for a computer or a user, use the command-line tool *gpresult*.
