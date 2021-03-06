---
title: Driver distribution expansion
description: Creating a floor or ceiling for a driver submission in order to change its distribution.
ms.assetid: 
ms.author: 
ms.date: 
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# Limiting driver distribution by Windows versions

IHVs, OEMs, and ODMs often need to change the distribution of a driver in Windows Update to a specific range of Windows versions. For example, a driver might:

* Have known problems in a specified range of versions.

* Need to be deployed to fix issues in past versions of Windows while a current driver is distributed to current versions.

* Extend its distribution to earlier, current or newer Windows versions.

These distribution ranges are defined by a **floor** and **ceiling**. A floor describes the earliest Windows version the driver will be distributed to, and a ceiling marks the latest. By adding a floor, you can expand your driver’s distribution. By adding a ceiling, you can restrict it. Floors and ceilings are specified in [shipping labels](https://docs.microsoft.com/windows-hardware/drivers/dashboard/manage-driver-distribution-by-submission) within the Hardware Dev Center Dashboard for the following driver submission formats:

* .HLKX
* .HCKX
* .CAB

> [!NOTE]
> Only Administrators, Shipping Label Owners, and Shipping Label Promoters can set floors and ceilings for driver submissions.

## Floor and ceiling types

Floors and ceilings are applied to newer or earlier or current version of Windows OS.
There are two types of floors and ceilings supported by the HDC Dashboard:

| Floor/ceiling type | Description |
| -- | -- |
| OS release-based | <ul><li>Used to expand or restrict the driver to additional Windows versions other than the certified Windows OS version.</li><li>Meant for drivers released to the public.</li></ul> |
| Build number-based | <ul><li>Used when developing drivers for newer and unreleased versions of Windows. </li><li>Available for Microsoft co-engineering partners only.</li></ul> |

## Setting floors and ceilings for your driver distribution

1. Create a shipping label and enter your details for the label’s name, publisher and targeting. For more information, see [Publish a driver to Windows Update](https://docs.microsoft.com/en-us/windows-hardware/drivers/dashboard/publish-a-driver-to-windows-update).

2. In **Select PNPs**, select the Hardware ID and operating system combinations you wish to publish to. Note that a floor can be set for each Hardware ID, but a ceiling is applied to all IDs within the same shipping label. Additionally, the oldest operating system you select will automatically be selected as the floor for your label. 

### Adding a floor

1. You can add a floor (OS based driver expansion) for your driver’s distribution by clicking **Please expand operating systems for driver’s distribution here**, located below the label’s targeting information. 
    ![A screen showing the dashboard screen to add a floor to a driver.](images/driver-floor-screen.png)

    When you click the link, a list of operating systems appears that you can extend driver coverage to.

    ![An image showing the operating systems available to expand coverage to.](images/driver-floor-screen-2.png)

2.	After selecting the operating systems, you wish to expand coverage to, click Submit

### Adding a ceiling

1.	Below the CHID information, you can add a ceiling by selecting **Please specify a ceiling OS**, and selecting an operating system.

    ![A screenshot of the ceiling options available to driver submissions.](images/driver-ceiling-screen.png)

2. Select **Publish**

> [!NOTE]
> Note the following when adding a ceiling
> * The ceiling option is enabled only for Hardware IDs that target Windows 10, and only after clicking Publish in the **Select PNPs** selection area.

> * The ceiling you select should be higher than the PNP selections you made.

## FAQ

**When would I need to specify a floor or ceiling for a version of Windows that has been released?**

*To prevent a pre-existing driver from being distributed to future versions of Windows. For example, while developing a replacement driver targeting an unreleased version of Windows.*

**When would I need to specify a floor and/or ceiling for a co-engineering driver? (coming soon)**

*You may need to specify a version of Windows that is currently in development as a floor If your driver contains a dependency contained in it.*

**How can I target a Windows version that is older than my driver’s certification?**

*You can specify an earlier version of Windows as a floor; enabling the driver to target both that version and later of Windows.*
