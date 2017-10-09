---
title: "aaaConvert Azure kezelt lemezek tárolási szabványos toopremium, és ez fordítva is igaz |} Microsoft Docs"
description: "Hogyan tooconvert Azure által kezelt lemezeken szabványos toopremium, és ez fordítva is igaz, Azure PowerShell használatával."
services: virtual-machines-windows
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 11f35cde216e91c0599d3619682686e8eb162fad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="93dd7-103">Azure átalakítása felügyelt lemezek tárolási szabványos toopremium, és ez fordítva is igaz</span><span class="sxs-lookup"><span data-stu-id="93dd7-103">Convert Azure managed disks storage from standard toopremium, and vice versa</span></span>

<span data-ttu-id="93dd7-104">Kezelt lemezek két tárolási lehetőségeket kínál: [prémium](../../storage/storage-premium-storage.md) (SSD-alapú) és [szabványos](../../storage/storage-standard-storage.md) (HDD-alapú).</span><span class="sxs-lookup"><span data-stu-id="93dd7-104">Managed disks offers two storage options: [Premium](../../storage/storage-premium-storage.md) (SSD-based) and [Standard](../../storage/storage-standard-storage.md) (HDD-based).</span></span> <span data-ttu-id="93dd7-105">Ez lehetővé teszi tooeasily kapcsoló minimális állásidővel, a teljesítmény igények alapján hello két beállításai között.</span><span class="sxs-lookup"><span data-stu-id="93dd7-105">It allows you tooeasily switch between hello two options with minimal downtime based on your performance needs.</span></span> <span data-ttu-id="93dd7-106">Ez a funkció nem felügyelt lemezek nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="93dd7-106">This capability is not available for unmanaged disks.</span></span> <span data-ttu-id="93dd7-107">Azonban úgy is könnyen [toomanaged lemezek konvertálása](convert-unmanaged-to-managed-disks.md) tooeasily kapcsoló két hello-beállítások között.</span><span class="sxs-lookup"><span data-stu-id="93dd7-107">But you can easily [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) tooeasily switch between hello two options.</span></span>

<span data-ttu-id="93dd7-108">Ez a cikk bemutatja, hogyan tooconvert felügyelt lemezek a standard toopremium, és ez fordítva is igaz Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="93dd7-108">This article shows you how tooconvert managed disks from standard toopremium, and vice versa by using Azure PowerShell.</span></span> <span data-ttu-id="93dd7-109">Ha tooinstall kell, vagy frissíteni, lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/install-azurerm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="93dd7-109">If you need tooinstall or upgrade it, see [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="93dd7-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="93dd7-110">Before you begin</span></span>

* <span data-ttu-id="93dd7-111">hello átalakításhoz hello virtuális gép újraindítása szükséges, ezért ütemezése hello áttelepítését a lemezek már meglévő karbantartási időszak alatt történjen.</span><span class="sxs-lookup"><span data-stu-id="93dd7-111">hello conversion requires a restart of hello VM, so schedule hello migration of your disks storage during a pre-existing maintenance window.</span></span> 
* <span data-ttu-id="93dd7-112">Használatakor a nem felügyelt lemezek, először [toomanaged lemezek konvertálása](convert-unmanaged-to-managed-disks.md) toouse Ez a cikk tooswitch hello két tárolási lehetőségek között.</span><span class="sxs-lookup"><span data-stu-id="93dd7-112">If you are using unmanaged disks, first [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) toouse this article tooswitch between hello two storage options.</span></span> 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="93dd7-113">Az összes hello Convert által kezelt lemezeken a virtuális gépek a szabványos toopremium, és ez fordítva is igaz</span><span class="sxs-lookup"><span data-stu-id="93dd7-113">Convert all hello managed disks of a VM from standard toopremium, and vice versa</span></span>

<span data-ttu-id="93dd7-114">A következő példa hello, megmutatjuk, hogyan tooswitch összes lemezt a virtuális gépek a szabványos toopremium tárolási hello.</span><span class="sxs-lookup"><span data-stu-id="93dd7-114">In hello following example, we show how tooswitch all hello disks of a VM from standard toopremium storage.</span></span> <span data-ttu-id="93dd7-115">toouse prémium által kezelt lemezeken, a virtuális Gépet kell használnia egy [Virtuálisgép-méretet](sizes.md) , amely támogatja a prémium szintű storage.</span><span class="sxs-lookup"><span data-stu-id="93dd7-115">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="93dd7-116">Ebben a példában is, amely támogatja a prémium szintű storage tooa mérete vált.</span><span class="sxs-lookup"><span data-stu-id="93dd7-116">This example also switches tooa size that supports premium storage.</span></span>

```powershell
# Name of hello resource group that contains hello VM
$rgName = 'yourResourceGroup'

# Name of hello your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'

# Premium capable size
# Required only if converting storage from standard toopremium
$size = 'Standard_DS2_v2'
$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Stop and deallocate hello VM before changing hello size
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

# Change hello VM size tooa size that supports premium storage
# Skip this step if converting storage from premium toostandard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in hello resource group of hello VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong toohello selected VM, convert toopremium storage
foreach ($disk in $vmDisks)
{
    if ($disk.OwnerId -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="93dd7-117">Alakítsa át a felügyelt lemezes szabványos toopremium, és ez fordítva is igaz</span><span class="sxs-lookup"><span data-stu-id="93dd7-117">Convert a managed disk from standard toopremium, and vice versa</span></span>

<span data-ttu-id="93dd7-118">A fejlesztési és tesztelési célú munkaterheléshez érdemes lehet standard és premium lemezek tooreduce toohave keverékével az költsége.</span><span class="sxs-lookup"><span data-stu-id="93dd7-118">For your dev/test workload, you may want toohave mixture of standard and premium disks tooreduce your cost.</span></span> <span data-ttu-id="93dd7-119">Ez csak jobb teljesítményt igénylő hello lemezek toopremium tárolási frissítésével végezheti el.</span><span class="sxs-lookup"><span data-stu-id="93dd7-119">You can accomplish it by upgrading toopremium storage, only hello disks that require better performance.</span></span> <span data-ttu-id="93dd7-120">A következő példa hello, megmutatjuk, hogyan tooswitch egyetlen lemezt a virtuális gépek szabványos toopremium tárolóból, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="93dd7-120">In hello following example, we show how tooswitch a single disk of a VM from standard toopremium storage, and vice versa.</span></span> <span data-ttu-id="93dd7-121">toouse prémium által kezelt lemezeken, a virtuális Gépet kell használnia egy [Virtuálisgép-méretet](sizes.md) , amely támogatja a prémium szintű storage.</span><span class="sxs-lookup"><span data-stu-id="93dd7-121">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="93dd7-122">Ebben a példában is, amely támogatja a prémium szintű storage tooa mérete vált.</span><span class="sxs-lookup"><span data-stu-id="93dd7-122">This example also switches tooa size that supports premium storage.</span></span>

```powershell

$diskName = 'yourDiskName'
# resource group that contains hello managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'PremiumLRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get hello ARM resource tooget name and resource group of hello VM
$vmResource = Get-AzureRmResource -ResourceId $disk.OwnerId
$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Stop and deallocate hello VM before changing hello storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

# Change hello VM size tooa size that supports premium storage
# Skip this step if converting storage from premium toostandard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Update hello storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a><span data-ttu-id="93dd7-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="93dd7-123">Next steps</span></span>

<span data-ttu-id="93dd7-124">Tegye meg a virtuális gépek csak olvasható másolatát a használatával [pillanatképek](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="93dd7-124">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

