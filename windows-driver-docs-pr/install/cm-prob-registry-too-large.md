---
title: CM_PROB_REGISTRY_TOO_LARGE
description: CM_PROB_REGISTRY_TOO_LARGE
ms.assetid: 8870ea57-4ae4-48a0-9d56-c5d0da8e1525
keywords:
- CM_PROB_REGISTRY_TOO_LARGE
ms.author: windowsdriverdev
ms.date: 04/20/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
ms.localizationpriority: medium
---

# CM_PROB_REGISTRY_TOO_LARGE

This function is reserved for system use.





The registry is too large.

### Error Code

49

### Display Message (Windows XP and later versions of Windows)

"Windows cannot start new hardware devices because the system hive is too large (exceeds the Registry Size Limit). (Code 49)"

### Recommended Resolution (Windows XP and later versions of Windows)

Set the environment variable DEVMGR_SHOW_NONPRESENT_DEVICES to 1. This causes Device Manager to display installed devices that are currently not present. Use Device Manager to remove these devices. If the registry is still too large, reinstall Windows.

 

 





