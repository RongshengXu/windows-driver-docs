---
title: DeviceD1 and DeviceD2
author: windows-driver-content
description: DeviceD1 and DeviceD2
ms.assetid: 88e9e1bf-c797-4e00-b540-1b2f8d48300a
keywords: ["DeviceD1", "DeviceD2"]
ms.author: windowsdriverdev
ms.date: 06/16/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# DeviceD1 and DeviceD2


## <a href="" id="ddk-deviced1-and-deviced2-kg"></a>


The **DeviceD1** and **DeviceD2** members of [**DEVICE\_CAPABILITIES**](https://msdn.microsoft.com/library/windows/hardware/ff543095) indicate whether the device hardware supports these device power states. Each is a single bit, which is set if the device supports the state and is clear if the device does not support the state. The operating system assumes that all devices support the D0 and D3 [device power states](device-power-states.md).

 

 




