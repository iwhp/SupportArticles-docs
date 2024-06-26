--- 
title: Inventory owner not allowed when processing movements
description: You may receive the inventory owner is not allowed error. This issue occurs because currently warehouse management processes support only inventory that is owned by the legal entity.
author: Mirzaab 
ms.date: 06/24/2021 
ms.topic: troubleshooting 
# ms.search.form:  
audience: Application User 
ms.reviewer: kamaybac 
ms.search.region: Global 
ms.author: mirzaab 
ms.search.validFrom: 2021-06-24 
ms.dyn365.ops.version: 10.0.20 
ms.custom: sap:Warehouse management
--- 
# Inventory owner not allowed when processing movements in the warehouse app

## Symptoms

When processing movements in the Warehouse Management mobile app, you may receive the following error message:

> The inventory owner %1 is not allowed in this process.

## Cause

This occurs because the **Owner** tracking dimension is missing when the Warehouse Management mobile app is used to make movements. A regular inventory transfer journal from the Supply Chain Management client appears to work as intended and can be posted only if the **Owner** dimension is filled in.

## Resolution

Microsoft has evaluated this issue and has determined that it's a feature limitation. Currently, warehouse management processes support only inventory that is owned by the legal entity.
