---
title: Compatibility with user accounts ending with the dollar sign
description: Starting in Windows 7/2008R2, there are potential compatibility issues with using domain user accounts ending with the dollar sign ($).  Managed service accounts are identified by ending in a dollar sign ($) and there can be confusion on a system when setting a service to run under an account that ends with the dollar sign ($).
ms.date: 01/15/2025
manager: dcscontentpm
audience: itpro
ms.topic: troubleshooting
ms.reviewer: kaushika
ms.custom:
- sap:active directory\user,computer,group,and object management
- pcy:WinComm Directory Services
---
# Compatibility with user accounts ending with the dollar sign ($)

This article describes the potential compatibility issues in using domain user accounts ending with the dollar sign ($) as a service account.

_Original KB number:_ &nbsp; 2666116

## Summary

Starting in Windows 7/2008R2, there are potential compatibility issues with using domain user accounts ending with the dollar sign ($) as a service account. Managed service accounts are identified by ending in a dollar sign ($). The system may evaluate the account as a managed service account and block the change.

## More information

In the services console, if you enter a user account in a trusted domain that ends in the dollar sign ($), it will warn you that "The name provided is not a properly formed account name." This occurs because managed service accounts can't be used in a trusted domain.

[Managed Service Accounts](https://technet.microsoft.com/library/ff641731%28ws.10%29.aspx)
