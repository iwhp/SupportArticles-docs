---
title: Use the xray feature of TSS to identify known issues
description: Introduces the xray feature of TSS. This feature is used to identify known issues.
ms.date: 01/15/2025
manager: dcscontentpm
audience: itpro
ms.topic: troubleshooting
ms.reviewer: tdimli, ronsto, ravikiran.s, gipauli, warrenw
ms.custom:
- sap:support tools\xray
- pcy:WinComm Directory Services
---
# Identify known issues by using the xray feature of TSS

xray (all lowercase) is a diagnostic framework-based PowerShell feature of the [TroubleShootingScript (TSS)](introduction-to-troubleshootingscript-toolset-tss.md) toolset. The xray feature scans for known issues during data collection and creates reports with issue information and solutions. The xray feature displays the reports on the screen and also saves the reports in the dataset in a .zip file created by the TSS tool.

The xray feature is a dynamic feature with new versions released every week. It constantly updates its diagnostics to identify new problems and removes outdated ones to enhance performance and reduce runtime. TSS prompts you to update automatically when you run it. Be sure to keep TSS updated to get the latest features and fixes from TSS and xray. Otherwise, you might not able to detect some issues that were recently added to xray.

An administrator or support professional can review the report files to check if a known issue occurs.

## Download and run the xray feature

The xray feature can be downloaded as part of the [TSS package](https://aka.ms/getTSS).

When TSS is unzipped, there's an xray directory within the TSS directory.

You can also download xray as a standalone package by selecting this [link](https://aka.ms/getxray).

The xray feature runs by default. All you need to do is open the report, read it, and then check if a known issue occurs.

We recommend that you run xray as part of TSS. If you want to run xray directly (separately from TSS), run the following command:

```powershell
.\xray.ps1 -Area *
```

If you want to run it to look for a specific known issue, run the following command:

```powershell
.\xray.ps1 -Diagnostic <diagnostic name>
```

## Find the xray report

In the .zip file generated by TSS, or in the _psSDP*.zip_ file within the _TSS*.zip_ file, you can find these report files:

- _xray_ISSUES-FOUND\_\*.txt_ (known issue detected)
- _xray_INFO\_\*.txt_ (known issue with low impact detected)

> [!NOTE]
> It also generates the following two log files that should be ignored. They're only used by the xray team to improve diagnostics.
>
> - _xray_log\_\*.txt_
> - _xray_report\_\*.xml_


## Example scenario of using the xray report to resolve an issue

This section introduces an example xray report listing a known issue detected on a computer named DESKTOP_1234 and detailing how to resolve it. The filename is *xray_ISSUES-FOUND_231026-144320_ DESKTOP_1234.txt*.

```output
xray, v1.0.231018.0
Diagnostic check run on 231026-144320 UTC

**
** Issue 1	Found a potential issue (reported by net_smbcli_KB5027830):
**
Workstation Service is not running, this will prevent you from being able to connect to SMB shares.

This is most likely caused by the missing ComputerName value in registry:

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\ComputerName\ActiveComputerName

Please check this registry key and restore the missing ComnputerName value.

Example:
reg add HKLM\SYSTEM\CurrentControlSet\Control\ComputerName\ActiveComputerName /t REG_SZ /v ComputerName /d YourComputerName /f
```

In this report, the following text shows the issue:

```output
Workstation Service is not running, this will prevent you from being able to connect to SMB shares.
```

The following text shows the cause of the issue:

```output
This is most likely caused by the missing ComputerName value in registry:
 
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\ComputerName\ActiveComputerName
```

The following text shows the resolution for the issue:

```output
Please check this registry key and restore the missing ComnputerName value.

Example:
reg add HKLM\SYSTEM\CurrentControlSet\Control\ComputerName\ActiveComputerName /t REG_SZ /v ComputerName /d YourComputerName /f
```
