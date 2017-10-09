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
# <a name="increase-hello-size-of-a-data-disk-attached-tooa-windows-vm"></a><span data-ttu-id="8f067-103">Hello méretének növelését adatlemezt csatolni tooa Windows virtuális gép</span><span class="sxs-lookup"><span data-stu-id="8f067-103">Increase hello size of a data disk attached tooa Windows VM</span></span>

<span data-ttu-id="8f067-104">Ha tooincrease hello mérete hello adatok csatlakoztatott lemez tooyour virtuális gép van szüksége, növelheti a PowerShell-lel hello mérete.</span><span class="sxs-lookup"><span data-stu-id="8f067-104">If you need tooincrease hello size of hello data disk attached tooyour virtual machine, you can increase hello size using PowerShell.</span></span> <span data-ttu-id="8f067-105">Miután hello adatlemez hello Azure virtuális gép beállításainál hello méretének növeléséhez akkor is tooallocate hello új szabad területre van szükség hello VM belül.</span><span class="sxs-lookup"><span data-stu-id="8f067-105">After you increase hello size of hello data disk in hello Azure VM settings, you also need tooallocate hello new disk space within hello VM.</span></span>


## <a name="use-powershell-tooincrease-hello-size-of-a-managed-data-disk"></a><span data-ttu-id="8f067-106">Használja a felügyelt adatok lemez Powershell tooincrease hello mérete</span><span class="sxs-lookup"><span data-stu-id="8f067-106">Use Powershell tooincrease hello size of a managed data disk</span></span>

<span data-ttu-id="8f067-107">felügyelt adatlemezt, a következő PowerShell-parancsmagok használata hello tooincrease hello mérete:</span><span class="sxs-lookup"><span data-stu-id="8f067-107">tooincrease hello size of a managed data disk, use hello following PowerShell cmdlets:</span></span>

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [<span data-ttu-id="8f067-108">Get-AzureRMReseourceGroup</span><span class="sxs-lookup"><span data-stu-id="8f067-108">Get-AzureRMReseourceGroup</span></span>](/powershell/module/azurerm.resources/get-azurermresourcegroup) | [<span data-ttu-id="8f067-109">Get-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="8f067-109">Get-AzureRMVM</span></span>](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [<span data-ttu-id="8f067-110">STOP-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="8f067-110">Stop-AzureRMVM</span></span>](/powershell/module/azurerm.compute/stop-azurermvm)                        | [<span data-ttu-id="8f067-111">Frissítés-AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="8f067-111">Update-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/Update-AzureRmDisk) |
 | [<span data-ttu-id="8f067-112">Start-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="8f067-112">Start-AzureRmVM</span></span>](/powershell/module/azurerm.compute/start-azurermvm)             |
<br>

<span data-ttu-id="8f067-113">hello alábbi parancsfájl bemutatja hello Virtuálisgép-adatok beolvasása, hello adatlemez kiválasztása és hello új méret megadása.</span><span class="sxs-lookup"><span data-stu-id="8f067-113">hello following script will walk you through getting hello VM information, selecting hello data disk and specifying hello new size.</span></span>

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

## <a name="use-powershell-tooincrease-hello-size-of-an-unmanaged-data-disk"></a><span data-ttu-id="8f067-114">PowerShell tooincrease hello méret egy nem felügyelt adatok lemez használata</span><span class="sxs-lookup"><span data-stu-id="8f067-114">Use PowerShell tooincrease hello size of an unmanaged data disk</span></span>

<span data-ttu-id="8f067-115">nem felügyelt adatok lemezek tárfiókokban, a következő PowerShell-parancsmagok használata hello tooincrease hello mérete:</span><span class="sxs-lookup"><span data-stu-id="8f067-115">tooincrease hello size of unmanaged data disks in a storage account, use hello following PowerShell cmdlets:</span></span>

|                                                                    |                                                            |
|--------------------------------------------------------------------|------------------------------------------------------------|
| [<span data-ttu-id="8f067-116">Get-AzureRMStorageAccount</span><span class="sxs-lookup"><span data-stu-id="8f067-116">Get-AzureRMStorageAccount</span></span>](/powershell/module/azurerm.storage/get-azurermstorageaccount) | [<span data-ttu-id="8f067-117">Get-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="8f067-117">Get-AzureRMVM</span></span>](/powershell/module/azurerm.compute/get-azurermvm)                 |
| [<span data-ttu-id="8f067-118">STOP-AzureRMVM</span><span class="sxs-lookup"><span data-stu-id="8f067-118">Stop-AzureRMVM</span></span>](/powershell/module/azurerm.compute/stop-azurermvm)                       | [<span data-ttu-id="8f067-119">Set-AzureRmVMDataDisk</span><span class="sxs-lookup"><span data-stu-id="8f067-119">Set-AzureRmVMDataDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmdatadisk) |
| [<span data-ttu-id="8f067-120">Frissítés-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="8f067-120">Update-AzureRmVM</span></span>](/powershell/module/azurerm.compute/update-azurermvm)                   | [<span data-ttu-id="8f067-121">Start-AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="8f067-121">Start-AzureRmVM</span></span>](/powershell/module/azurerm.compute/start-azurermvm)             |

<br>

<span data-ttu-id="8f067-122">hello alábbi parancsfájl bemutatja hello VM és a tárolási fiók adatait, hello adatlemez kiválasztása és a hello új méret megadása.</span><span class="sxs-lookup"><span data-stu-id="8f067-122">hello following script will walk you through getting hello VM and storage account information, selecting hello data disk and specifying hello new size.</span></span>

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

## <a name="allocate-hello-unallocated-disk-space"></a><span data-ttu-id="8f067-123">Hello nem lefoglalt lemezterület foglalása</span><span class="sxs-lookup"><span data-stu-id="8f067-123">Allocate hello unallocated disk space</span></span>

<span data-ttu-id="8f067-124">Miután végzett hello meghajtó nagyobb, tooallocate hello új szabad területtel rendelkező hello VM belül kell.</span><span class="sxs-lookup"><span data-stu-id="8f067-124">Once you have made hello drive larger, you need tooallocate hello new unallocated space from within hello VM.</span></span> <span data-ttu-id="8f067-125">tooallocate hello terület, csatlakoztathatja toohello virtuális gép használata a Lemezkezelés (Diskmgmt.msc beépülő modult).</span><span class="sxs-lookup"><span data-stu-id="8f067-125">tooallocate hello space, you can connect toohello VM use Disk Management (diskmgmt.msc).</span></span> <span data-ttu-id="8f067-126">Vagy, ha engedélyezte a Rendszerfelügyeleti webszolgáltatások és a virtuális gép hello tanúsítvány létrehozása után, távoli PowerShell tooinitialize hello lemez használható.</span><span class="sxs-lookup"><span data-stu-id="8f067-126">Or, if you enabled WinRM and a certificate on hello VM when you created it, you can use remote PowerShell tooinitialize hello disk.</span></span> <span data-ttu-id="8f067-127">Egy egyéni parancsprogramok futtatására szolgáló bővítmény is használhatja:</span><span class="sxs-lookup"><span data-stu-id="8f067-127">You can also use a custom script extension:</span></span>

```powershell
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

<span data-ttu-id="8f067-128">hello parancsfájl tartalmazza a kód tooincrease hello meghajtó foglalási toohello maximális méret hello lemezek hasonlót:</span><span class="sxs-lookup"><span data-stu-id="8f067-128">hello script file can contain something like this code tooincrease hello drive allocation toohello maximum size hello disks:</span></span>

```powershell
$driveLetter= "F"

$MaxSize = (Get-PartitionSupportedSize -DriveLetter $driveLetter).sizeMax

Resize-Partition -DriveLetter $driveLetter -Size $MaxSize
```

## <a name="next-steps"></a><span data-ttu-id="8f067-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f067-129">Next Steps</span></span>
- [<span data-ttu-id="8f067-130">További információ a lemezek és a VHD-k</span><span class="sxs-lookup"><span data-stu-id="8f067-130">Learn more about disks and VHDs</span></span>](../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
