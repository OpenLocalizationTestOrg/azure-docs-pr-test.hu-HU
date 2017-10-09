---
title: aaaExpand adatlemezt csatolni tooa Windows Azure-ban |} Microsoft Docs
description: "Bontsa ki, amely csatolt tooa Windows virtuális gépet a PowerShell használatával adatlemezt hello méretét."
services: virtual-machines-windows
documentationcenter: na
author: cynthn
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cynthn
ms.openlocfilehash: b16ad0da9cff9dfffc9dc9ec7dd72891e7ddd745
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="increase-hello-size-of-a-data-disk-attached-tooa-windows-vm"></a>Hello méretének növelését adatlemezt csatolni tooa Windows virtuális gép

Ha tooincrease hello mérete hello adatok csatlakoztatott lemez tooyour virtuális gép van szüksége, növelheti a PowerShell-lel hello mérete. Miután hello adatlemez hello Azure virtuális gép beállításainál hello méretének növeléséhez akkor is tooallocate hello új szabad területre van szükség hello VM belül.


## <a name="use-powershell-tooincrease-hello-size-of-a-managed-data-disk"></a>Használja a felügyelt adatok lemez Powershell tooincrease hello mérete

felügyelt adatlemezt, a következő PowerShell-parancsmagok használata hello tooincrease hello mérete:

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [Get-AzureRMReseourceGroup](/powershell/module/azurerm.resources/get-azurermresourcegroup) | [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [STOP-AzureRMVM](/powershell/module/azurerm.compute/stop-azurermvm)                        | [Frissítés-AzureRmDisk](/powershell/module/azurerm.compute/Update-AzureRmDisk) |
 | [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm)             |
<br>

hello alábbi parancsfájl bemutatja hello Virtuálisgép-adatok beolvasása, hello adatlemez kiválasztása és hello új méret megadása.

```powershell
# Select resource group

    $rg = Get-AzureRMResourceGroup | Out-GridView `
        -Title "Select hello resource group" `
        -PassThru

    $rgName = $rg.ResourceGroupName

# Select hello VM

    $vm = Get-AzureRMVM -ResourceGroupName $rgName `
        | Out-GridView `
            -Title "Select a VM" `
             -PassThru

# Select data disk

    $disk = $vm.StorageProfile.DataDisks | Out-GridView `
        -Title "Select a data disk" `
        -PassThru

# Specify a larger size for hello data disk

    $size =  Read-Host `
        -Prompt "New size in GB"

# Stop and Deallocate VM prior tooresizing data disk

    $vm | Stop-AzureRMVM -Force

# Set hello new disk size

    $diskUpdateConfig = New-AzureRmDiskUpdateConfig -DiskSizeGB $size

# Update hello configuration in Azure

    $managedDisk = Get-AzureRmResource -ResourceId $disk.ManagedDisk.Id
    Update-AzureRmDisk -DiskName $managedDisk.ResourceName -ResourceGroupName $managedDisk.ResourceGroupName -DiskUpdate $diskUpdateConfig

# Start hello VM

    Start-AzureRmVM -ResourceGroupName $rgName -VMName $vm.name
```

## <a name="use-powershell-tooincrease-hello-size-of-an-unmanaged-data-disk"></a>PowerShell tooincrease hello méret egy nem felügyelt adatok lemez használata

nem felügyelt adatok lemezek tárfiókokban, a következő PowerShell-parancsmagok használata hello tooincrease hello mérete:

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [Get-AzureRMStorageAccount](/powershell/module/azurerm.storage/get-azurermstorageaccount) | [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [STOP-AzureRMVM](/powershell/module/azurerm.compute/stop-azurermvm)                       | [Set-AzureRmVMDataDisk](/powershell/module/azurerm.compute/set-azurermvmdatadisk) |
| [Frissítés-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm)                   | [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm)             |

<br>

hello alábbi parancsfájl bemutatja hello VM és a tárolási fiók adatait, hello adatlemez kiválasztása és a hello új méret megadása.

```powershell

# Select Azure Storage Account

    $storageAccount =
        Get-AzureRMStorageAccount | Out-GridView `
            -Title "Select Azure Storage Account" `
            -PassThru

    $rgName = $storageAccount.ResourceGroupName

# Select hello VM

    $vm = Get-AzureRMVM `
    -ResourceGroupName $rgName | Out-GridView `
            -Title "Select a VM …" `
            -PassThru

# Select Data Disk tooresize

    $disk =
        $vm.DataDiskNames | Out-GridView `
            -Title "Select a data disk tooresize" `
            -PassThru


# Specify a larger data disk size

    $size =  Read-Host `
        -Prompt "New size in GB"

# Stop and Deallocate VM prior tooresizing data disk

    $vm | Stop-AzureRMVM -Force

# Set hello new disk size

    Set-AzureRmVMDataDisk -VM $vm -Name "$disk" `
        -DiskSizeInGB $size

# Update hello configuration in Azure

    Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Start hello VM
    Start-AzureRmVM -ResourceGroupName $rgName `
    -VMName $vm.name

```

## <a name="allocate-hello-unallocated-disk-space"></a>Hello nem lefoglalt lemezterület foglalása

Miután végzett hello meghajtó nagyobb, tooallocate hello új szabad területtel rendelkező hello VM belül kell. tooallocate hello terület, csatlakoztathatja toohello virtuális gép használata a Lemezkezelés (Diskmgmt.msc beépülő modult). Vagy, ha engedélyezte a Rendszerfelügyeleti webszolgáltatások és a virtuális gép hello tanúsítvány létrehozása után, távoli PowerShell tooinitialize hello lemez használható. Egy egyéni parancsprogramok futtatására szolgáló bővítmény is használhatja:

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

hello parancsfájl tartalmazza a kód tooincrease hello meghajtó foglalási toohello maximális méret hello lemezek hasonlót:

```powershell
$driveLetter= "F"

$MaxSize = (Get-PartitionSupportedSize -DriveLetter $driveLetter).sizeMax

Resize-Partition -DriveLetter $driveLetter -Size $MaxSize
```

## <a name="next-steps"></a>Következő lépések
- [További információ a lemezek és a VHD-k](../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
