---
title: EVT_NET_ADAPTER_CREATE_RXQUEUE callback function
topic_type:
- apiref
api_name:
- PFN_NET_ADAPTER_CREATE_RXQUEUE
api_location:
- netadapter.h
api_type:
- UserDefined
---

# EVT_NET_ADAPTER_CREATE_RXQUEUE callback function


[!include[NetAdapterCx Beta Prerelease](../netcx-beta-prerelease.md)]

The client driver's implementation of the *EVT_NET_ADAPTER_CREATE_RXQUEUE* event callback function that sets up a receive queue.

Syntax
------

```cpp
EVT_NET_ADAPTER_CREATE_RXQUEUE EvtNetAdapterCreateRxqueue;

NTSTATUS EvtNetAdapterCreateRxqueue(
  _In_    NETADAPTER       Adapter,
  _Inout_ PNETRXQUEUE_INIT RxQueueInit
)
{ ... }

typedef EVT_NET_ADAPTER_CREATE_RXQUEUE PFN_NET_ADAPTER_CREATE_RXQUEUE;
```

Parameters
----------

*Adapter* [in]  
The network adapter object that the client created in a prior call to [**NetAdapterCreate**](netadaptercreate.md).

*RxQueueInit* [in, out]  
A pointer to a NetAdapterCx-allocated **NETRXQUEUE_INIT** structure. For more information, see the Remarks section.

Return value
------------

If the operation is successful, the callback function must return STATUS_SUCCESS, or another status value for which NT_SUCCESS(status) equals TRUE. Otherwise, an appropriate [NTSTATUS](https://msdn.microsoft.com/library/windows/hardware/ff557697) error code.

Remarks
-------

To register an *EVT_NET_ADAPTER_CREATE_RXQUEUE* callback function, the client driver must call [**NetAdapterCreate**](netadaptercreate.md).

The **NETRXQUEUE_INIT** structure is an opaque structure that is defined and allocated by NetAdapterCx, similar to [WDFDEVICE_INIT](https://msdn.microsoft.com/library/windows/hardware/ff546951).

In this callback, the client driver might call [**NetRxQueueInitGetQueueId**](netrxqueueinitgetqueueid.md) to retrieve the identifier of the receive queue to set up.

Next, the client calls [**NetRxQueueCreate**](netrxqueuecreate.md) to allocate a queue.  If the client provides a non-zero value in the **AllocationSize** member of the [**NET_RXQUEUE_CONFIG**](net-rxqueue-config.md) structure, [**NetRxQueueCreate**](netrxqueuecreate.md) allocates the receive buffers.  The client should not use the buffers until after [**NetRxQueueCreate**](netrxqueuecreate.md) has returned.  If [**NetRxQueueCreate**](netrxqueuecreate.md) fails, the *EVT_NET_ADAPTER_CREATE_RXQUEUE* callback function should return an error code.

To retrieve the ring buffer associated with a given queue, call [**NetRxQueueGetRingBuffer**](netrxqueuegetringbuffer.md).

Example
-----

>[!TIP]
> This example uses DMA allocation for the receive queue. It is assumed that the example code previously declared a context for its NETADAPTER object and included a WDFDMAENABLER object in the context, which will now be retrieved in **EvtAdapterCreateRxQueue** to be used for receive buffer DMA allocation. For more info about receive queue DMA allocation, see [NetRxQueueInitSetDmaAllocatorConfig](netrxqueueinitsetdmaallocatorconfig.md).
>
> Error handling code has been excised from this example for brevity and clarity.

```cpp
NTSTATUS
EvtAdapterCreateRxQueue(
    _In_ NETADAPTER netAdapter,
    _Inout_ PNETRXQUEUE_INIT rxQueueInit)
{
    NTSTATUS status = STATUS_SUCCESS;

    NET_RXQUEUE_CONFIG rxConfig;
    NET_RXQUEUE_CONFIG_INIT(
        &rxConfig,
        EvtRxQueueAdvance,
        EvtRxQueueSetNotificationEnabled,
        EvtRxQueueCancel);

    // Specify buffer size required per packet so the OS can preallocate

    rxConfig.AlignmentRequirement = 64;
    rxConfig.AllocationSize = NIC_MAX_PACKET_SIZE + FRAME_CRC_SIZE + RSVD_BUF_SIZE;

    // Initialize the per-packet context

    NET_PACKET_CONTEXT_ATTRIBUTES myRxContextAttributes;
    NET_PACKET_CONTEXT_ATTRIBUTES_INIT_TYPE(&myRxContextAttributes, MY_RXQUEUE_PACKET_CONTEXT);

    // Add the context attributes to the queue

    status = NetRxQueueInitAddPacketContextAttributes(rxQueueInit, &myRxContextAttributes);

    // Retrieve the WDFDMAENABLER from the NETADAPTER's context to opt in to DMA allocation

    MY_NET_ADAPTER_CONTEXT *adapterContext = GetMyNetAdapterContext(netAdapter);
    WDFDMAENABLER dmaEnabler = adapterContext->dmaEnabler;

    // Specify that the OS use the WDFDMAENABLER to allocate the receive buffers

    NET_RXQUEUE_DMA_ALLOCATOR_CONFIG dmaAllocatorConfig;
    NET_RXQUEUE_DMA_ALLOCATOR_CONFIG_INIT(
        &dmaAllocatorConfig,
        dmaEnabler);
    
    NetRxQueueInitSetDmaAllocatorConfig(
        &rxQueueInit,
        &dmaAllocatorConfig);

    // Create the receive queue

    status = NetRxQueueCreate(
        rxQueueInit,
        &rxAttributes,
        &rxConfig,
        &netAdapter->RxQueue);

     return status;
}
```

Requirements
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Target platform</p></td>
<td align="left">Universal</td>
</tr>
<tr class="even">
<td align="left"><p>Minimum KMDF version</p></td>
<td align="left"><p>1.21</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Minimum NetAdapterCx version</p></td>
<td align="left"><p>1.0</p></td>
</tr>
<tr class="even">
<td align="left"><p>Header</p></td>
<td align="left">NetAdapter.h</td>
</tr>
<tr class="odd">
<td align="left"><p>IRQL</p></td>
<td align="left"><p>PASSIVE_LEVEL</p></td>
</tr>
</tbody>
</table>

 

 





