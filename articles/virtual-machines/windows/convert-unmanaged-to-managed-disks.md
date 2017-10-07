---
title: "a Windows rendszerű virtuális gép nem felügyelt kódból felügyelt aaaConvert lemezek toomanaged lemezek - Azure kezelt lemezek |} Microsoft Docs"
description: "Hogyan tooconvert egy Windows virtuális gép nem felügyelt lemezek toomanaged lemezek hello Resource Manager üzembe helyezési modellben a PowerShell használatával"
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
ms.openlocfilehash: e8ed8694b0e776d22df26261e2fc8340bfe5cafa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a><span data-ttu-id="24ef6-103">A Windows virtuális gép átalakítása nem felügyelt lemezek toomanaged lemezekből</span><span class="sxs-lookup"><span data-stu-id="24ef6-103">Convert a Windows virtual machine from unmanaged disks toomanaged disks</span></span>

<span data-ttu-id="24ef6-104">Ha meglévő Windows virtuális gépek (VM), a nem felügyelt lemezeket használó, hello virtuális gépek felügyelt toouse lemezek keresztül hello átválthat [Azure felügyelt lemezek](managed-disks-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="24ef6-104">If you have existing Windows virtual machines (VMs) that use unmanaged disks, you can convert hello VMs toouse managed disks through hello [Azure Managed Disks](managed-disks-overview.md) service.</span></span> <span data-ttu-id="24ef6-105">Ez a folyamat hello operációsrendszer-lemez és a mellékelt adatok lemezzel alakítja át.</span><span class="sxs-lookup"><span data-stu-id="24ef6-105">This process converts both hello OS disk and any attached data disks.</span></span>

<span data-ttu-id="24ef6-106">Ez a cikk bemutatja, hogyan tooconvert virtuális gépek Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="24ef6-106">This article shows you how tooconvert VMs by using Azure PowerShell.</span></span> <span data-ttu-id="24ef6-107">Ha tooinstall kell, vagy frissíteni, lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/install-azurerm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="24ef6-107">If you need tooinstall or upgrade it, see [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="24ef6-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="24ef6-108">Before you begin</span></span>


* <span data-ttu-id="24ef6-109">Felülvizsgálati [hello áttelepítés tervezése tooManaged lemezek](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span><span class="sxs-lookup"><span data-stu-id="24ef6-109">Review [Plan for hello migration tooManaged Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a><span data-ttu-id="24ef6-110">Egypéldányos virtuális gépek átalakítása</span><span class="sxs-lookup"><span data-stu-id="24ef6-110">Convert single-instance VMs</span></span>
<span data-ttu-id="24ef6-111">Ez a szakasz ismerteti, hogyan tooconvert egypéldányos Azure virtuális gépek nem felügyelt kódból felügyelt lemezek, toomanaged lemezek.</span><span class="sxs-lookup"><span data-stu-id="24ef6-111">This section covers how tooconvert single-instance Azure VMs from unmanaged disks toomanaged disks.</span></span> <span data-ttu-id="24ef6-112">(Ha a virtuális gépek rendelkezésre állási csoportba, lásd: hello következő szakaszt.)</span><span class="sxs-lookup"><span data-stu-id="24ef6-112">(If your VMs are in an availability set, see hello next section.)</span></span> 

1. <span data-ttu-id="24ef6-113">Hello virtuális gép felszabadítása hello segítségével [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="24ef6-113">Deallocate hello VM by using hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet.</span></span> <span data-ttu-id="24ef6-114">hello alábbi példa felszabadítja a hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="24ef6-114">hello following example deallocates hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span> 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. <span data-ttu-id="24ef6-115">Alakítsa át a hello VM toomanaged lemezeket hello segítségével [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="24ef6-115">Convert hello VM toomanaged disks by using hello [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet.</span></span> <span data-ttu-id="24ef6-116">a következő folyamat konvertálja hello előző VM, beleértve hello operációsrendszer-lemez és adatlemezeket hello:</span><span class="sxs-lookup"><span data-stu-id="24ef6-116">hello following process converts hello previous VM, including hello OS disk and any data disks:</span></span>

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. <span data-ttu-id="24ef6-117">Hello VM indítása után hello átalakítás toomanaged lemezek használatával [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="24ef6-117">Start hello VM after hello conversion toomanaged disks by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span></span> <span data-ttu-id="24ef6-118">a következő példa újraindítást hello előző VM hello:</span><span class="sxs-lookup"><span data-stu-id="24ef6-118">hello following example restarts hello previous VM:</span></span>

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="24ef6-119">Alakítsa át a virtuális gépek rendelkezésre állási csoportba</span><span class="sxs-lookup"><span data-stu-id="24ef6-119">Convert VMs in an availability set</span></span>

<span data-ttu-id="24ef6-120">Ha hello virtuális gépeket, amelyet a rendelkezésre állási csoportok tooconvert toomanaged lemezek vannak, akkor először kell tooconvert hello rendelkezésre állási készlet tooa felügyelt rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="24ef6-120">If hello VMs that you want tooconvert toomanaged disks are in an availability set, you first need tooconvert hello availability set tooa managed availability set.</span></span>

1. <span data-ttu-id="24ef6-121">Átalakítás hello rendelkezésre állási csoport hello segítségével [frissítés-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="24ef6-121">Convert hello availability set by using hello [Update-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet.</span></span> <span data-ttu-id="24ef6-122">a következő példa frissítések hello rendelkezésre állási csoport elnevezett hello `myAvailabilitySet` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="24ef6-122">hello following example updates hello availability set named `myAvailabilitySet` in hello resource group named `myResourceGroup`:</span></span>

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  <span data-ttu-id="24ef6-123">Ha hello terület, ahol a rendelkezésre állási csoport található csak 2 felügyelt tartalék tartományok viszont nem felügyelt tartalék tartományok hello száma: 3, ez a parancs megjeleníti hiba hasonló túl "hello megadva tartalék tartományainak száma 3 hello tartomány 1 too2 kell lennie."</span><span class="sxs-lookup"><span data-stu-id="24ef6-123">If hello region where your availability set is located has only 2 managed fault domains but hello number of unmanaged fault domains is 3, this command shows an error similar too"hello specified fault domain count 3 must fall in hello range 1 too2."</span></span> <span data-ttu-id="24ef6-124">tooresolve hello hiba, a frissítés hello tartalék tartomány too2 és a frissítés `Sku` túl`Aligned` az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="24ef6-124">tooresolve hello error, update hello fault domain too2 and update `Sku` too`Aligned` as follows:</span></span>

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. <span data-ttu-id="24ef6-125">Felszabadítani, és konvertálja hello hello rendelkezésre állási csoport virtuális gépe.</span><span class="sxs-lookup"><span data-stu-id="24ef6-125">Deallocate and convert hello VMs in hello availability set.</span></span> <span data-ttu-id="24ef6-126">hello következő parancsfájl felszabadítja a virtuális gépek hello segítségével [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) -parancsmag használatával konvertálja [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), és újraindítja a [Start-AzureRmVM ](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="24ef6-126">hello following script deallocates each VM by using hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, converts it by using [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), and restarts it by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

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


## <a name="troubleshooting"></a><span data-ttu-id="24ef6-127">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="24ef6-127">Troubleshooting</span></span>

<span data-ttu-id="24ef6-128">Ha nem sikerül átalakítás során, vagy az előző konverzió problémák miatt sikertelen állapotban van a virtuális gépek, futtassa a hello `ConvertTo-AzureRmVMManagedDisk` újra a parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="24ef6-128">If there is an error during conversion, or if a VM is in a failed state because of issues in a previous conversion, run hello `ConvertTo-AzureRmVMManagedDisk` cmdlet again.</span></span> <span data-ttu-id="24ef6-129">Egy egyszerű újra általában feloldja hello helyzet.</span><span class="sxs-lookup"><span data-stu-id="24ef6-129">A simple retry usually unblocks hello situation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="24ef6-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24ef6-130">Next steps</span></span>

[<span data-ttu-id="24ef6-131">Standard szintű felügyelt lemez toopremium átalakítása</span><span class="sxs-lookup"><span data-stu-id="24ef6-131">Convert standard managed disks toopremium</span></span>](convert-disk-storage.md)

<span data-ttu-id="24ef6-132">Tegye meg a virtuális gépek csak olvasható másolatát a használatával [pillanatképek](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="24ef6-132">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

