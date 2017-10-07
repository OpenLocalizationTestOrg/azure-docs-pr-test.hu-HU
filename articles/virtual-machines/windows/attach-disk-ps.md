---
title: "aaaAttach egy adatok lemez tooa Windows Azure-ban PowerShell használatával |} Microsoft Docs"
description: "Hogyan tooattach új vagy meglévő adatok lemezre tooa Windows VM PowerShell használatával hello Resource Manager üzembe helyezési modellben."
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
ms.date: 02/07/2017
ms.author: cynthn
ms.openlocfilehash: 12ffdd4ced791ba0948047d3af24ad73e36c7ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="attach-a-data-disk-tooa-windows-vm-using-powershell"></a>Csatlakoztassa a adatok lemez tooa Windows virtuális gép PowerShell használatával

Ez a cikk bemutatja, hogyan tooattach meglévő és új lemezek tooa Windows virtuális gépet a PowerShell használatával. Ha a virtuális Gépet felügyelt lemezt használ, csatolhat felügyelt adatlemeznek. Nem felügyelt adatok lemezek tooa tárfiókokban nem felügyelt lemezeket használó virtuális gép is csatolható.

Mielőtt ezt megtehetné, tekintse át az alábbi tippek:
* hello mérete hello virtuális gép csatolhat hány adatlemezek határozza meg. További információkért lásd: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* prémium szintű storage toouse, szüksége lesz egy prémium szintű Storage engedélyezve van, mint a DS-méretek és GS sorozatnak virtuális gép hello Virtuálisgép-méretet. Ezek a virtuális gépek lemezeit mind prémium és Standard storage-fiókok használatával. Prémium szintű storage bizonyos régiókban érhető el. További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="before-you-begin"></a>Előkészületek
Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját. Futtatás hello parancs tooinstall követően.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).


## <a name="add-an-empty-data-disk-tooa-virtual-machine"></a>Az üres adatok lemez tooa virtuális gép hozzáadása

Ez a példa bemutatja, hogyan tooadd egy üres adatok lemezre tooan meglévő virtuális gépet.

### <a name="using-managed-disks"></a>Felügyelt lemezekkel

```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'West Central US' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Empty -DiskSizeGB 128

$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-unmanaged-disks-in-a-storage-account"></a>Nem felügyelt lemezek tárfiókokban használata

```powershell
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
```


### <a name="initialize-hello-disk"></a>Hello lemez inicializálása

Üres lemez hozzáadása után tooinitialize kell azt. tooinitialize hello lemez, bejelentkezhet tooa virtuális Gépet, és használja a Lemezkezelés eszközben. Ha engedélyezte a Rendszerfelügyeleti webszolgáltatások és a virtuális gép hello tanúsítvány létrehozása után, a távoli PowerShell tooinitialize hello lemez is használhatja. Egy egyéni parancsprogramok futtatására szolgáló bővítmény is használhatja: 

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
hello parancsfájl tartalmazza a kód tooinitialize hello lemezek hasonlót:

```powershell
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-tooa-vm"></a>Egy meglévő adatok lemez tooa VM csatolása

Egy meglévő VHD-t is csatolhat a felügyelt adatok lemez tooa virtuális gépként. 

### <a name="using-managed-disks"></a>Felügyelt lemezekkel

```powershell
$rgName = 'myRG'
$vmName = 'ContosoMdPir3'
$location = 'West Central US' 
$storageType = 'PremiumLRS'
$dataDiskName = $vmName + '_datadisk2'
$dataVhdUri = 'https://mystorageaccount.blob.core.windows.net/vhds/managed_data_disk.vhd' 

$diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location $location -CreateOption Import -SourceUri $dataVhdUri -DiskSizeGB 128

$dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk2.Id -Lun 2

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Következő lépések

Hozzon létre egy [pillanatkép](snapshot-copy-managed-disk.md).
