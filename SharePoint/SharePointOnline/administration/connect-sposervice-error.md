---
title: Can't connect to SharePoint Online
description: Fixes an issue in which you receive an error message when you use the Connect-SPOService cmdlet.
author: Cloud-Writer
ms.author: meerak
manager: dcscontentpm
audience: ITPro
ms.topic: troubleshooting
ms.custom: 
  - sap:Sites\Other
  - CI 166999
  - CSSTroubleshoot
ms.reviewer: salarson
appliesto: 
  - SharePoint Online
search.appverid: MET150
ms.date: 12/17/2023
---
# Can't connect to SharePoint Online when you run Connect-SPOService

## Symptoms

When you run the `Connect-SPOService` cmdlet in SharePoint Online Management Shell, you receive the following error message:

> Connect-SPOService: Could not connect to SharePoint Online.

## Cause

By default, the `Connect-SPOService` cmdlet uses the legacy authentication. This issue might occur if you add an Active Directory Federation Services (AD FS) claim rule to block legacy authentication requests that don't originate from your expected IP range.

## Resolution

To resolve this issue, use the `ModernAuth` parameter included in SharePoint Online Management Shell version 16.0.22601.12000 and later versions. This parameter must be used together with the `AuthenticationUrl` parameter.  

Here's an example of the cmdlet:

```powershell
$creds = Get-Credential
Connect-SPOService -Credential $creds -Url https://tenant-admin.sharepoint.com -ModernAuth $true -AuthenticationUrl https://login.microsoftonline.com/organizations 
```

**Note** Setting `AuthenticationUrl` to `https://login.microsoftonline.com/organizations` handles the redirection for federated tenants.

If the issue persists, follow the steps in [Errors when connecting to SharePoint Online Management Shell](/sharepoint/troubleshoot/administration/errors-connecting-to-management-shell).  
