---
title: "Alakítsa át a Windows rendszerű virtuális gép nem felügyelt lemezekből felügyelt - Azure felügyelt lemezek |} Microsoft Docs"
description: "A Windows virtuális gépek átalakítása nem felügyelt lemezekről történő által kezelt lemezeken a Resource Manager üzembe helyezési modellben a PowerShell használatával"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: cynthn
ms.openlocfilehash: 54afcf1e37f696979bfe270a473c72aedf20dc43
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a><span data-ttu-id="c2b1b-103">Alakítsa át a Windows rendszerű virtuális gép nem felügyelt lemezekből felügyelt</span><span class="sxs-lookup"><span data-stu-id="c2b1b-103">Convert a Windows virtual machine from unmanaged disks to managed disks</span></span>

<span data-ttu-id="c2b1b-104">Ha meglévő Windows virtuális gépek (VM), a nem felügyelt lemezeket használó, a virtuális gépek keresztül felügyelt lemezeket használni átválthat a [Azure felügyelt lemezek](managed-disks-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-104">If you have existing Windows virtual machines (VMs) that use unmanaged disks, you can convert the VMs to use managed disks through the [Azure Managed Disks](managed-disks-overview.md) service.</span></span> <span data-ttu-id="c2b1b-105">Ez a folyamat az operációsrendszer-lemez és a mellékelt adatok lemezzel alakítja át.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-105">This process converts both the OS disk and any attached data disks.</span></span>

<span data-ttu-id="c2b1b-106">Ez a cikk bemutatja, hogyan alakítsa át a virtuális gépek Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-106">This article shows you how to convert VMs by using Azure PowerShell.</span></span> <span data-ttu-id="c2b1b-107">Ha szeretné telepíteni vagy frissíteni, lásd: [Azure PowerShell telepítése és konfigurálása](/powershell/azure/install-azurerm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="c2b1b-107">If you need to install or upgrade it, see [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c2b1b-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c2b1b-108">Before you begin</span></span>


* <span data-ttu-id="c2b1b-109">Felülvizsgálati [kezelt lemezek az áttelepítés megtervezése](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span><span class="sxs-lookup"><span data-stu-id="c2b1b-109">Review [Plan for the migration to Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a><span data-ttu-id="c2b1b-110">Egypéldányos virtuális gépek átalakítása</span><span class="sxs-lookup"><span data-stu-id="c2b1b-110">Convert single-instance VMs</span></span>
<span data-ttu-id="c2b1b-111">Ez a szakasz bemutatja, hogyan adhat alakítsa át a egypéldányos Azure virtuális gépek nem felügyelt lemezekből felügyelt.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-111">This section covers how to convert single-instance Azure VMs from unmanaged disks to managed disks.</span></span> <span data-ttu-id="c2b1b-112">(Ha a virtuális gépek rendelkezésre állási beállítása, a következő szakaszban talál.)</span><span class="sxs-lookup"><span data-stu-id="c2b1b-112">(If your VMs are in an availability set, see the next section.)</span></span> 

1. <span data-ttu-id="c2b1b-113">A virtuális gép felszabadítása használatával a [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-113">Deallocate the VM by using the [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet.</span></span> <span data-ttu-id="c2b1b-114">Az alábbi példa felszabadítja a nevű virtuális gép `myVM` az erőforráscsoport neve `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="c2b1b-114">The following example deallocates the VM named `myVM` in the resource group named `myResourceGroup`:</span></span> 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. <span data-ttu-id="c2b1b-115">A virtuális gép átalakítása felügyelt lemezek használatával a [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-115">Convert the VM to managed disks by using the [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet.</span></span> <span data-ttu-id="c2b1b-116">A következő folyamat alakítja át az előző VM, beleértve az operációs rendszer lemezének és adatlemezeket:</span><span class="sxs-lookup"><span data-stu-id="c2b1b-116">The following process converts the previous VM, including the OS disk and any data disks:</span></span>

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. <span data-ttu-id="c2b1b-117">Indítsa el a virtuális Gépet felügyelt lemezekre átalakítás után [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="c2b1b-117">Start the VM after the conversion to managed disks by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span></span> <span data-ttu-id="c2b1b-118">A következő példa az előző virtuális gép újraindul:</span><span class="sxs-lookup"><span data-stu-id="c2b1b-118">The following example restarts the previous VM:</span></span>

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="c2b1b-119">Alakítsa át a virtuális gépek rendelkezésre állási csoportba</span><span class="sxs-lookup"><span data-stu-id="c2b1b-119">Convert VMs in an availability set</span></span>

<span data-ttu-id="c2b1b-120">Ha az átalakítani kívánt virtuális gépek lemezek rendelkezésre állási csoportba, először alakítsa át a rendelkezésre állási csoportot egy felügyelt rendelkezésre állási csoportba.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-120">If the VMs that you want to convert to managed disks are in an availability set, you first need to convert the availability set to a managed availability set.</span></span>

1. <span data-ttu-id="c2b1b-121">A rendelkezésre állási csoportot használatával alakítsa át a [frissítés-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-121">Convert the availability set by using the [Update-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet.</span></span> <span data-ttu-id="c2b1b-122">Az alábbi példa frissíti a rendelkezésre állási csoportot elnevezett `myAvailabilitySet` az erőforráscsoport neve `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="c2b1b-122">The following example updates the availability set named `myAvailabilitySet` in the resource group named `myResourceGroup`:</span></span>

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  <span data-ttu-id="c2b1b-123">Ha helyezkedik el a régió, ahol a rendelkezésre állási beállítása csak 2 felügyelt tartalék tartományok rendelkezik, de a nem felügyelt tartalék tartományok száma: 3, ez a parancs megjeleníti hiba hasonló "a tartalék tartományok megadott számának 3 1 és 2 között kell lennie."</span><span class="sxs-lookup"><span data-stu-id="c2b1b-123">If the region where your availability set is located has only 2 managed fault domains but the number of unmanaged fault domains is 3, this command shows an error similar to "The specified fault domain count 3 must fall in the range 1 to 2."</span></span> <span data-ttu-id="c2b1b-124">A hiba megoldásához frissítse a tartalék tartomány 2 és a frissítés `Sku` való `Aligned` az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c2b1b-124">To resolve the error, update the fault domain to 2 and update `Sku` to `Aligned` as follows:</span></span>

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. <span data-ttu-id="c2b1b-125">Felszabadítani, és alakítsa át a virtuális gépek a rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-125">Deallocate and convert the VMs in the availability set.</span></span> <span data-ttu-id="c2b1b-126">Az alábbi parancsfájlt minden virtuális gép felszabadítja a használatával a [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) -parancsmag használatával konvertálja [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), és újraindítja a [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="c2b1b-126">The following script deallocates each VM by using the [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, converts it by using [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), and restarts it by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

  ```powershell
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
     Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  }
  ```


## <a name="troubleshooting"></a><span data-ttu-id="c2b1b-127">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="c2b1b-127">Troubleshooting</span></span>

<span data-ttu-id="c2b1b-128">Ha nem sikerül átalakítás során, vagy az előző konverzió problémák miatt sikertelen állapotban van a virtuális gépek, futtassa a `ConvertTo-AzureRmVMManagedDisk` újra a parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-128">If there is an error during conversion, or if a VM is in a failed state because of issues in a previous conversion, run the `ConvertTo-AzureRmVMManagedDisk` cmdlet again.</span></span> <span data-ttu-id="c2b1b-129">Egy egyszerű újra általában feloldja a problémát.</span><span class="sxs-lookup"><span data-stu-id="c2b1b-129">A simple retry usually unblocks the situation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c2b1b-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2b1b-130">Next steps</span></span>

[<span data-ttu-id="c2b1b-131">Standard szintű felügyelt lemez konvertálása premium</span><span class="sxs-lookup"><span data-stu-id="c2b1b-131">Convert standard managed disks to premium</span></span>](convert-disk-storage.md)

<span data-ttu-id="c2b1b-132">Tegye meg a virtuális gépek csak olvasható másolatát a használatával [pillanatképek](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="c2b1b-132">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

