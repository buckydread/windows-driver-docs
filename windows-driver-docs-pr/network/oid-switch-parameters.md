---
title: OID_SWITCH_PARAMETERS
author: windows-driver-content
description: A Hyper-V extensible switch extension issues an object identifier (OID) query request of OID_SWITCH_PARAMETERS to obtain the configuration data of the extensible switch.
ms.assetid: F2CA0BE5-ED21-4ACF-B26A-4F512D4B15C7
ms.author: windowsdriverdev
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
keywords: 
 -OID_SWITCH_PARAMETERS Network Drivers Starting with Windows Vista
ms.localizationpriority: medium
---

# OID\_SWITCH\_PARAMETERS


A Hyper-V extensible switch extension issues an object identifier (OID) query request of OID\_SWITCH\_PARAMETERS to obtain the configuration data of the extensible switch.

If the OID query request completes successfully, the **InformationBuffer** member of the [**NDIS\_OID\_REQUEST**](https://msdn.microsoft.com/library/windows/hardware/ff566710) structure contains a pointer to an [**NDIS\_SWITCH\_PARAMETERS**](https://msdn.microsoft.com/library/windows/hardware/hh598220) structure.

Remarks
-------

When the extension processes the returned [**NDIS\_SWITCH\_PARAMETERS**](https://msdn.microsoft.com/library/windows/hardware/hh598220) structure, it must not assume that the various string members of the **NDIS\_SWITCH\_PARAMETERS** structure, such as **SwitchName**, are null-terminated. The data types for these string members are type-defined by the [**IF\_COUNTED\_STRING**](https://msdn.microsoft.com/library/windows/hardware/hh451419) structure. The extension must determine the string length from the value of the **Length** member of this structure.

**Note**  If the string is null-terminated, the **Length** member must not include the terminating null character.

 

### Return Status Codes

The underlying miniport edge of the extensible switch completes the OID query request of OID\_SWITCH\_PARAMETERS and returns one of the following status codes.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>Status Code</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>NDIS_STATUS_SUCCESS</p></td>
<td><p>The OID request completed successfully.</p></td>
</tr>
<tr class="even">
<td><p>NDIS_STATUS_INVALID_LENGTH</p></td>
<td><p>The length of the information buffer is too small to return the OID_SWITCH_PARAMETERS structure for an OID query request. The underlying miniport edge of the extensible switch sets the <strong>DATA.QUERY_INFORMATION.BytesNeeded</strong> member in the [<strong>NDIS_OID_REQUEST</strong>](https://msdn.microsoft.com/library/windows/hardware/ff566710) structure to the minimum buffer size that is required.</p></td>
</tr>
<tr class="odd">
<td><p>NDIS_STATUS_FAILURE</p></td>
<td><p>The request failed for other reasons.</p></td>
</tr>
</tbody>
</table>

 

Requirements
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Version</p></td>
<td><p>Supported in NDIS 6.30 and later.</p></td>
</tr>
<tr class="even">
<td><p>Header</p></td>
<td>Ntddndis.h (include Ndis.h)</td>
</tr>
</tbody>
</table>

## See also


****
[**NDIS\_OID\_REQUEST**](https://msdn.microsoft.com/library/windows/hardware/ff566710)

[**NDIS\_SWITCH\_PARAMETERS**](https://msdn.microsoft.com/library/windows/hardware/hh598220)

[**NdisFOidRequest**](https://msdn.microsoft.com/library/windows/hardware/ff561830)

 

 




