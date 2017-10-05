---
title: "Adatlemez csatlakoztatása a Windows Azure-ban PowerShell használatával |} Microsoft Docs"
description: "Új vagy meglévő adatlemez csatolása egy Windows-VM PowerShell használata a Resource Manager üzembe helyezési modellel hogyan."
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
ms.openlocfilehash: 486e6a27fa28ec63001d824fe9f59c03a7aea5a7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="attach-a-data-disk-to-a-windows-vm-using-powershell"></a><span data-ttu-id="c6075-103">Adatlemez csatlakoztatása egy Windows-VM PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="c6075-103">Attach a data disk to a Windows VM using PowerShell</span></span>

<span data-ttu-id="c6075-104">Ez a cikk bemutatja, hogyan új és meglévő lemez csatolása virtuális géphez Windows PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="c6075-104">This article shows you how to attach both new and existing disks to a Windows virtual machine using PowerShell.</span></span> <span data-ttu-id="c6075-105">Ha a virtuális Gépet felügyelt lemezt használ, csatolhat felügyelt adatlemeznek.</span><span class="sxs-lookup"><span data-stu-id="c6075-105">If your VM uses managed disks, you can attach additional managed data disks.</span></span> <span data-ttu-id="c6075-106">Nem felügyelt adatlemezek is csatolhat egy virtuális gép által használt nem felügyelt lemezek tárfiókokban.</span><span class="sxs-lookup"><span data-stu-id="c6075-106">You can also attach unmanaged data disks to a VM that uses unmanaged disks in a storage account.</span></span>

<span data-ttu-id="c6075-107">Mielőtt ezt megtehetné, tekintse át az alábbi tippek:</span><span class="sxs-lookup"><span data-stu-id="c6075-107">Before you do this, review these tips:</span></span>
* <span data-ttu-id="c6075-108">A virtuális gép mérete csatolhat hány adatlemezek szabályozza.</span><span class="sxs-lookup"><span data-stu-id="c6075-108">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="c6075-109">További információkért lásd: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c6075-109">For details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="c6075-110">A prémium szintű storage, szüksége lesz egy prémium szintű Storage engedélyezve van, például a DS-méretek és GS sorozatnak virtuális gép Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="c6075-110">To use Premium storage, you'll need a Premium Storage enabled VM size like the DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="c6075-111">Ezek a virtuális gépek lemezeit mind prémium és Standard storage-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="c6075-111">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="c6075-112">Prémium szintű storage bizonyos régiókban érhető el.</span><span class="sxs-lookup"><span data-stu-id="c6075-112">Premium storage is available in certain regions.</span></span> <span data-ttu-id="c6075-113">További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c6075-113">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c6075-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="c6075-114">Before you begin</span></span>
<span data-ttu-id="c6075-115">Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e a legújabb verzióját a AzureRM.Compute PowerShell-modult.</span><span class="sxs-lookup"><span data-stu-id="c6075-115">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="c6075-116">A következő parancsot a telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="c6075-116">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="c6075-117">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c6075-117">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a><span data-ttu-id="c6075-118">Egy üres adatlemez hozzáadása a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="c6075-118">Add an empty data disk to a virtual machine</span></span>

<span data-ttu-id="c6075-119">Ez a példa bemutatja, hogyan egy üres adatlemez hozzáadása egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="c6075-119">This example shows how to add an empty data disk to an existing virtual machine.</span></span>

### <a name="using-managed-disks"></a><span data-ttu-id="c6075-120">Felügyelt lemezekkel</span><span class="sxs-lookup"><span data-stu-id="c6075-120">Using managed disks</span></span>

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

### <a name="using-unmanaged-disks-in-a-storage-account"></a><span data-ttu-id="c6075-121">Nem felügyelt lemezek tárfiókokban használata</span><span class="sxs-lookup"><span data-stu-id="c6075-121">Using unmanaged disks in a storage account</span></span>

```powershell
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
```


### <a name="initialize-the-disk"></a><span data-ttu-id="c6075-122">A lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="c6075-122">Initialize the disk</span></span>

<span data-ttu-id="c6075-123">Üres lemez hozzáadása után kell inicializálnia.</span><span class="sxs-lookup"><span data-stu-id="c6075-123">After you add an empty disk, you need to initialize it.</span></span> <span data-ttu-id="c6075-124">A lemez inicializálása, jelentkezzen be egy virtuális Gépet, és használja a Lemezkezelést.</span><span class="sxs-lookup"><span data-stu-id="c6075-124">To initialize the disk, you can log in to a VM and use disk management.</span></span> <span data-ttu-id="c6075-125">Vannak engedélyezve a Rendszerfelügyeleti webszolgáltatások és a tanúsítványt a virtuális gép létrehozása után, a távoli PowerShell használatával végezze el a lemez inicializálását.</span><span class="sxs-lookup"><span data-stu-id="c6075-125">If you enabled WinRM and a certificate on the VM when you created it, you can use remote PowerShell to initialize the disk.</span></span> <span data-ttu-id="c6075-126">Egy egyéni parancsprogramok futtatására szolgáló bővítmény is használhatja:</span><span class="sxs-lookup"><span data-stu-id="c6075-126">You can also use a custom script extension:</span></span> 

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
<span data-ttu-id="c6075-127">A parancsfájl tartalmazza a kódot, hogy a lemezek inicializálása hasonlót:</span><span class="sxs-lookup"><span data-stu-id="c6075-127">The script file can contain something like this code to initialize the disks:</span></span>

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


## <a name="attach-an-existing-data-disk-to-a-vm"></a><span data-ttu-id="c6075-128">Adatok meglévő lemez csatolása a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="c6075-128">Attach an existing data disk to a VM</span></span>

<span data-ttu-id="c6075-129">Egy virtuális géphez, amely felügyelt adatok lemezként is csatolhat egy meglévő VHD-t.</span><span class="sxs-lookup"><span data-stu-id="c6075-129">You can also attach an existing VHD as a managed data disk to a virtual machine.</span></span> 

### <a name="using-managed-disks"></a><span data-ttu-id="c6075-130">Felügyelt lemezekkel</span><span class="sxs-lookup"><span data-stu-id="c6075-130">Using managed disks</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c6075-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c6075-131">Next steps</span></span>

<span data-ttu-id="c6075-132">Hozzon létre egy [pillanatkép](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="c6075-132">Create a [snapshot](snapshot-copy-managed-disk.md).</span></span>
