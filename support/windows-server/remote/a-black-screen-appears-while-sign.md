---
title: A black screen appears while sign-in
description: Provides a solution to an issue that a black screen may appear while sign-in by using remote desktop.
ms.date: 01/15/2025
manager: dcscontentpm
audience: itpro
ms.topic: troubleshooting
ms.reviewer: kaushika
ms.custom:
- sap:remote desktop services and terminal services\deployment,configuration,and management of remote desktop services infrastructure
- pcy:WinComm User Experience
---
# A black screen may appear while sign-in by using remote desktop

This article provides a solution to an issue that a black screen may appear while sign-in by using remote desktop.

_Applies to:_ &nbsp; Windows Server 2003  
_Original KB number:_ &nbsp; 555840

## Summary

The following knowledge base can help you to resolve the issue of a black screen may appear while sign-in by using remote desktop.

## Symptoms

While sign in into a remote server by using remote desktop, the following issues may occur:

1. A slow sign-in process.

2. A black screen appears for a while, until the regular desktop appears.

## Resolution

1. Disable the use of **Bitmap Caching** on the Remote Desktop Protocol (RDP) client.

2. Verify that the server, client, and the network equipment using the "**MTU**" size.

## More information

Configure bitmap caching

Troubleshooting Specific Remote Desktop problems.

[!INCLUDE [Community Solutions Content Disclaimer](../../includes/community-solutions-content-disclaimer.md)]
