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
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a>Azure átalakítása felügyelt lemezek tárolási szabványos toopremium, és ez fordítva is igaz

Kezelt lemezek két tárolási lehetőségeket kínál: [prémium](../../storage/storage-premium-storage.md) (SSD-alapú) és [szabványos](../../storage/storage-standard-storage.md) (HDD-alapú). Ez lehetővé teszi tooeasily kapcsoló minimális állásidővel, a teljesítmény igények alapján hello két beállításai között. Ez a funkció nem felügyelt lemezek nem érhető el. Azonban úgy is könnyen [toomanaged lemezek konvertálása](convert-unmanaged-to-managed-disks.md) tooeasily kapcsoló két hello-beállítások között.

Ez a cikk bemutatja, hogyan tooconvert felügyelt lemezek a standard toopremium, és ez fordítva is igaz Azure PowerShell használatával. Ha tooinstall kell, vagy frissíteni, lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Előkészületek

* hello átalakításhoz hello virtuális gép újraindítása szükséges, ezért ütemezése hello áttelepítését a lemezek már meglévő karbantartási időszak alatt történjen. 
* Használatakor a nem felügyelt lemezek, először [toomanaged lemezek konvertálása](convert-unmanaged-to-managed-disks.md) toouse Ez a cikk tooswitch hello két tárolási lehetőségek között. 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a>Az összes hello Convert által kezelt lemezeken a virtuális gépek a szabványos toopremium, és ez fordítva is igaz

A következő példa hello, megmutatjuk, hogyan tooswitch összes lemezt a virtuális gépek a szabványos toopremium tárolási hello. toouse prémium által kezelt lemezeken, a virtuális Gépet kell használnia egy [Virtuálisgép-méretet](sizes.md) , amely támogatja a prémium szintű storage. Ebben a példában is, amely támogatja a prémium szintű storage tooa mérete vált.

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
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a>Alakítsa át a felügyelt lemezes szabványos toopremium, és ez fordítva is igaz

A fejlesztési és tesztelési célú munkaterheléshez érdemes lehet standard és premium lemezek tooreduce toohave keverékével az költsége. Ez csak jobb teljesítményt igénylő hello lemezek toopremium tárolási frissítésével végezheti el. A következő példa hello, megmutatjuk, hogyan tooswitch egyetlen lemezt a virtuális gépek szabványos toopremium tárolóból, és ez fordítva is igaz. toouse prémium által kezelt lemezeken, a virtuális Gépet kell használnia egy [Virtuálisgép-méretet](sizes.md) , amely támogatja a prémium szintű storage. Ebben a példában is, amely támogatja a prémium szintű storage tooa mérete vált.

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

## <a name="next-steps"></a>Következő lépések

Tegye meg a virtuális gépek csak olvasható másolatát a használatával [pillanatképek](snapshot-copy-managed-disk.md).

