---
title: GPIO-Based Interrupt Resources
author: windows-driver-content
description: Drivers for peripheral devices that send interrupts to general-purpose I/O (GPIO) pins acquire GPIO interrupts as abstract Windows interrupt resources.
MSHAttr:
- 'PreferredSiteName:MSDN'
- 'PreferredLib:/library/windows/hardware'
ms.assetid: 65031C43-917D-4665-BD7F-97D3DDA0A918
---

# GPIO-Based Interrupt Resources


Drivers for peripheral devices that send interrupts to general-purpose I/O (GPIO) pins acquire GPIO interrupts as abstract Windows interrupt resources. [Kernel-mode driver framework](https://msdn.microsoft.com/library/windows/hardware/ff544296) (KMDF) drivers receive these resources through their [*EvtDevicePrepareHardware*](https://msdn.microsoft.com/library/windows/hardware/ff540880) event callback functions. [User-mode driver framework](https://msdn.microsoft.com/library/windows/hardware/ff560442) (UMDF) drivers receive them through their [**IPnpCallbackHardware2::OnPrepareHardware**](https://msdn.microsoft.com/library/windows/hardware/ff556766) methods.

Peripheral device drivers that use GPIO-based interrupt resources can ignore low-level implementation details, such as whether an interrupt is generated by a GPIO pin instead of by an interrupt controller or by an interrupt pin on a processor chip.

A GPIO-based interrupt is a resource of type **CmResourceTypeInterrupt**. The configuration parameters for this interrupt are contained in the **u.Interrupt** member of the [**CM\_PARTIAL\_RESOURCE\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/hardware/ff541977) structure that describes the interrupt resource. To connect an interrupt service routine (ISR) to an interrupt, a UMDF or KMDF driver supplies both the [raw and translated](https://msdn.microsoft.com/library/windows/hardware/ff544561) descriptions of the interrupt resource to an interrupt-creation method.

The KMDF driver for a peripheral device calls the [**WdfInterruptCreate**](https://msdn.microsoft.com/library/windows/hardware/ff547345) method to connect an ISR to the interrupt from the device. One of the input parameters to this method is a pointer to a [**WDF\_INTERRUPT\_CONFIG**](https://msdn.microsoft.com/library/windows/hardware/ff552347) structure that contains configuration information for the interrupt. For more information, see [Handling Hardware Interrupts](https://msdn.microsoft.com/library/windows/hardware/ff543281).

The UMDF driver for a peripheral device calls the [**IWDFDevice3::CreateInterrupt**](https://msdn.microsoft.com/library/windows/hardware/hh451208) method to connect an ISR to the interrupt from the device. One of the input parameters to this method is a pointer to a [**WUDF\_INTERRUPT\_CONFIG**](https://msdn.microsoft.com/library/windows/hardware/ff552347) structure that contains configuration information for the interrupt. UMDF support for interrupts is available starting with Windows 8. For more information, see [Accessing Hardware and Handling Interrupts](https://msdn.microsoft.com/library/windows/hardware/hh439560).

If a peripheral device driver uses more than one GPIO interrupt resource, this driver must be aware of the order in which these resources appear in the raw and translated resource lists that are supplied as input parameters to the *EvtDevicePrepareHardware* function or **OnPrepareHardware** method. The resources in these lists appear in the order in which they are described in the platform firmware, which must match the order that is expected by the driver.

 

 


--------------------

