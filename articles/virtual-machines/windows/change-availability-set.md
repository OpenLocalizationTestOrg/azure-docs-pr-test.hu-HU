---
title: "a virtuális gépek rendelkezésre állási csoport aaaChange |} Microsoft Docs"
description: "Ismerje meg, hogyan toochange hello rendelkezésre állási készlet használata az Azure PowerShell és hello Resource Manager üzembe helyezési modellben a virtuális gépek."
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 44c90f90-bc9a-4260-a36f-5465e2a1ef94
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/15/2016
ms.author: drewm
ms.openlocfilehash: 3b1cc010a6d4c4883f2e34da9cfca4372aec92cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-availability-set-for-a-windows-vm"></a><span data-ttu-id="6db5c-103">Hello rendelkezésre állási csoportban, a Windows virtuális gépek módosítása</span><span class="sxs-lookup"><span data-stu-id="6db5c-103">Change hello availability set for a Windows VM</span></span>
<span data-ttu-id="6db5c-104">hello lépések ismertetik, hogyan toochange hello rendelkezésre állási csoport a virtuális gépek Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="6db5c-104">hello following steps describe how toochange hello availability set of a VM using Azure PowerShell.</span></span> <span data-ttu-id="6db5c-105">A virtuális gép nem vehető tooan rendelkezésre állási csoport a létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="6db5c-105">A VM can only be added tooan availability set when it is created.</span></span> <span data-ttu-id="6db5c-106">Rendelés toochange hello rendelkezésre állási csoport toodelete kell, és hozza létre újra a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="6db5c-106">In order toochange hello availability set, you need toodelete and recreate hello virtual machine.</span></span> 

## <a name="change-hello-availability-set-using-powershell"></a><span data-ttu-id="6db5c-107">Módosítsa a hello rendelkezésre állási csoportot a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="6db5c-107">Change hello availability set using PowerShell</span></span>
1. <span data-ttu-id="6db5c-108">Hello hello VM toobe módosította a következő kulcs adatok rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6db5c-108">Capture hello following key details from hello VM toobe modified.</span></span>
   
    <span data-ttu-id="6db5c-109">Hello virtuális gép neve</span><span class="sxs-lookup"><span data-stu-id="6db5c-109">Name of hello VM</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
   
    <span data-ttu-id="6db5c-110">Virtuálisgép-mérettel</span><span class="sxs-lookup"><span data-stu-id="6db5c-110">VM Size</span></span>
   
    ```powershell
    $vm.HardwareProfile.VmSize
    ```
   
    <span data-ttu-id="6db5c-111">Hálózati elsődleges hálózati adapter és a választható hálózati adapterek, ha vannak ilyenek, a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="6db5c-111">Network primary network interface and optional network interfaces if they exist on hello VM</span></span>
   
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```
   
    <span data-ttu-id="6db5c-112">Az operációsrendszer-lemez profil</span><span class="sxs-lookup"><span data-stu-id="6db5c-112">OS Disk Profile</span></span>
   
    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```
   
    <span data-ttu-id="6db5c-113">Minden egyes adatlemez lemez profilok</span><span class="sxs-lookup"><span data-stu-id="6db5c-113">Disk profiles for each data disk</span></span> 
   
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```
   
    <span data-ttu-id="6db5c-114">Virtuálisgép-bővítmény</span><span class="sxs-lookup"><span data-stu-id="6db5c-114">VM extensions installed</span></span> 
   
    ```powershell
    $vm.Extensions
    ```
2. <span data-ttu-id="6db5c-115">Hello virtuális gép törlése, hello lemezek vagy hello hálózati törlése nélkül.</span><span class="sxs-lookup"><span data-stu-id="6db5c-115">Delete hello VM without deleting any of hello disks or hello network interfaces.</span></span>
   
    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```
3. <span data-ttu-id="6db5c-116">Hozzon létre hello rendelkezésre állási csoportot, ha még nem létezik</span><span class="sxs-lookup"><span data-stu-id="6db5c-116">Create hello availability set if it does not already exist</span></span>
   
    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```
4. <span data-ttu-id="6db5c-117">Hozza létre újra hello hello új rendelkezésre állási csoportot használó virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="6db5c-117">Recreate hello VM using hello new availability set</span></span>
   
    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>
   
    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]
   
    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 
   
    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 
5. <span data-ttu-id="6db5c-118">Az adatlemezek és kiterjesztések hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="6db5c-118">Add data disks and extensions.</span></span> <span data-ttu-id="6db5c-119">További információkért lásd: [adatlemez csatolása tooVM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Resource Manager-sablonok bővítményei](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span><span class="sxs-lookup"><span data-stu-id="6db5c-119">For more information, see [Attach Data Disk tooVM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Extensions in Resource Manager templates](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions).</span></span> <span data-ttu-id="6db5c-120">Az adatlemezek és bővítmények adhatók toohello VM PowerShell vagy az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="6db5c-120">Data disks and extensions can be added toohello VM using PowerShell or Azure CLI.</span></span>

## <a name="example-script"></a><span data-ttu-id="6db5c-121">A példaként megadott parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="6db5c-121">Example Script</span></span>
<span data-ttu-id="6db5c-122">hello következő parancsfájl egy példát biztosít a hello szükséges információk összegyűjtéséhez törlése hello eredeti virtuális gép, és azt, majd újbóli létrehozása egy új rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="6db5c-122">hello following script provides an example of gathering hello required information, deleting hello original VM and then recreating it in a new availability set.</span></span>

```powershell
    #set variables
    $rg = "demo-resource-group"
    $vmName = "demo-vm"
    $newAvailSetName = "demo-as"
    $outFile = "C:\temp\outfile.txt"

    #Get VM Details
    $OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

    #Output VM details toofile
    "VM Name: " | Out-File -FilePath $outFile 
    $OriginalVM.Name | Out-File -FilePath $outFile -Append

    "Extensions: " | Out-File -FilePath $outFile -Append
    $OriginalVM.Extensions | Out-File -FilePath $outFile -Append

    "VMSize: " | Out-File -FilePath $outFile -Append
    $OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

    "NIC: " | Out-File -FilePath $outFile -Append
    $OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

    "OSType: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

    "OS Disk: " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

    if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
    }

    #Remove hello original VM
    Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

    #Create new availability set if it does not exist
    $availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
    if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
    }

    #Create hello basic configuration for hello replacement VM
    $newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
    Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

    #Add Data Disks
    foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
    }

    #Add NIC(s)
    foreach ($nic in $OriginalVM.NetworkInterfaceIDs) {
        Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic
    }

    #Create hello VM
    New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## <a name="next-steps"></a><span data-ttu-id="6db5c-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6db5c-123">Next steps</span></span>
<span data-ttu-id="6db5c-124">Adja hozzá a további tárhely tooyour VM további hozzáadásával [adatlemez](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6db5c-124">Add additional storage tooyour VM by adding an additional [data disk](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

