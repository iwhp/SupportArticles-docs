---
title: OOBE Fails with Something Went Wrong Error
description: Helps resolve the Something went wrong error during the Windows Out of Box Experience (OOBE).
manager: dcscontentpm
ms.date: 04/02/2025
ms.topic: troubleshooting
ms.reviewer: kaushika, erikje, v-lianna
ms.custom:
- pcy:Configuration and Management\Managing Devices with Intune
- sap:WinComm User Experience
---
# Windows 365 Link fails to load OOBE with error "Something went wrong"

This article helps resolve the error "Something went wrong" during the Windows Out of Box Experience (OOBE).

When you first turn on Windows 365 Link, OOBE is loaded to guide you through the process of joining the device to your Microsoft Entra tenant and enrolling the device into Intune management. If a failure occurs, you might encounter the following error message:

> Something went wrong.  
Looks like we can't connect to the URL for your organization's MDM terms of use. Try again, or contact your system administrator with the problem information from this page.

This error commonly occurs because you aren't configured for automatic enrollment in mobile device management (MDM) with Intune. For configuration details, see [Automatically enroll Windows 365 Link in Intune](/windows-365/link/intune-automatic-enrollment).
