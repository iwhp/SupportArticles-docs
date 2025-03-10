---
title: Pre-installed Microsoft Store app is removed at first Windows logon
description: Provides a workaround for the issue in which a pre-installed Microsoft Store App is unexpectedly removed the first time that a user logs on
ms.date: 01/15/2025
manager: dcscontentpm
audience: itpro
ms.topic: troubleshooting
ms.reviewer: kaushika
ms.custom:
- sap:windows desktop and shell experience\modern,inbox and microsoft store apps
- pcy:WinComm User Experience
---
# Pre-installed Microsoft Store app is removed unexpectedly at first Windows logon

This article provides a workaround for the issue in which a pre-installed Microsoft Store App is unexpectedly removed the first time that a user logs on.

_Applies to:_ &nbsp; Windows 10, version 1903, Windows 10, version 1809  
_Original KB number:_ &nbsp; 4543142

## Symptoms

You use a DISM command to deploy a Microsoft Store app in Windows 10, version 1809 or a later version of Windows, and then you deploy the app on a computer. After a user logs on to the computer for the first time, the app is unexpectedly removed. Additionally, an Event ID 240 error is generated.

## Workaround

To work around this issue, add the `/Region:"All"` switch when you use the DISM command to deploy the app.
