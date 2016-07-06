---
title: Miscellaneous Network Mini-Redirector Routines
author: windows-driver-content
description: Miscellaneous Network Mini-Redirector Routines
ms.assetid: bad5847c-8ac5-4e63-a0a5-3bbf13424928
keywords: ["mini-redirectors WDK , miscellaneous routines", "connection IDs WDK network redirectors", "buffering state WDK network redirectors"]
---

# Miscellaneous Network Mini-Redirector Routines


A few other routines implemented by a network mini-redirector deal with buffering state management and connection IDs. The buffering state management routines can be used by a network mini-redirector to handle various buffering schemes implemented in the redirector and the server and to communicate any changes in buffering state. For example, the [**MRxCompleteBufferingStateChangeRequest**](https://msdn.microsoft.com/library/windows/hardware/ff549850) routine is used by the SMB redirector as part of the mechanism to send an oplock break to the server.

Connection IDs can be used for multiplexing multiple connections. It is unnecessary for a network mini-redirector to support connection IDs, so [**MRxGetConnectionId**](https://msdn.microsoft.com/library/windows/hardware/ff550687) is optional.

The following table lists the routines that can be implemented by a network mini-redirector for miscellaneous operations.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Routine</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">[<strong>MRxCompleteBufferingStateChangeRequest</strong>](https://msdn.microsoft.com/library/windows/hardware/ff549850)</td>
<td align="left"><p>RDBSS calls this routine to notify the network mini-redirector that a buffering state change request has been completed. For example, the SMB redirector uses this routine to send an oplock break response or to close the handle on an oplock break if the file is no longer in use. Byte range locks that need to be flushed out to the server are passed to the network mini-redirector in the <strong>LowIoContext.ParamsFor.Locks.LockList</strong> member of the RX_CONTEXT.</p></td>
</tr>
<tr class="even">
<td align="left">[<strong>MRxComputeNewBufferingState</strong>](https://msdn.microsoft.com/library/windows/hardware/ff549856)</td>
<td align="left"><p>RDBSS calls this routine to request that the network mini-redirector compute a new buffering state change.</p></td>
</tr>
<tr class="odd">
<td align="left">[<strong>MRxGetConnectionId</strong>](https://msdn.microsoft.com/library/windows/hardware/ff550687)</td>
<td align="left"><p>RDBSS calls this routine to request that a network mini-redirector return a connection ID for the connection which can be used for handling multiple sessions. If connection IDs are supported by the network mini-redirector, then the returned connection ID is appended to the connection structures stored in the name table. RDBSS considers the connection ID as an opaque blob, and does a byte-by-byte comparison of the connection ID blob while looking up the net-name table for a given name.</p></td>
</tr>
</tbody>
</table>

 

 

 


--------------------
[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[ifsk\ifsk]:%20Miscellaneous%20Network%20Mini-Redirector%20Routines%20%20RELEASE:%20%285/9/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")

