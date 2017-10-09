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
# <a name="change-hello-availability-set-for-a-windows-vm"></a>Hello rendelkezésre állási csoportban, a Windows virtuális gépek módosítása
hello lépések ismertetik, hogyan toochange hello rendelkezésre állási csoport a virtuális gépek Azure PowerShell használatával. A virtuális gép nem vehető tooan rendelkezésre állási csoport a létrehozásakor. Rendelés toochange hello rendelkezésre állási csoport toodelete kell, és hozza létre újra a virtuális gép hello. 

## <a name="change-hello-availability-set-using-powershell"></a>Módosítsa a hello rendelkezésre állási csoportot a PowerShell használatával
1. Hello hello VM toobe módosította a következő kulcs adatok rögzítéséhez.
   
    Hello virtuális gép neve
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
    $vm.Name
    ```
   
    Virtuálisgép-mérettel
   
    ```powershell
    $vm.HardwareProfile.VmSize
    ```
   
    Hálózati elsődleges hálózati adapter és a választható hálózati adapterek, ha vannak ilyenek, a virtuális gép hello
   
    ```powershell
    $vm.NetworkProfile.NetworkInterfaces[0].Id
    ```
   
    Az operációsrendszer-lemez profil
   
    ```powershell
    $vm.StorageProfile.OsDisk.OsType
    $vm.StorageProfile.OsDisk.Name
    $vm.StorageProfile.OsDisk.Vhd.Uri
    ```
   
    Minden egyes adatlemez lemez profilok 
   
    ```powershell
    $vm.StorageProfile.DataDisks[<index>].Lun
    $vm.StorageProfile.DataDisks[<index>].Vhd.Uri
    ```
   
    Virtuálisgép-bővítmény 
   
    ```powershell
    $vm.Extensions
    ```
2. Hello virtuális gép törlése, hello lemezek vagy hello hálózati törlése nélkül.
   
    ```powershell
    Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
    ```
3. Hozzon létre hello rendelkezésre állási csoportot, ha még nem létezik
   
    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
    ```
4. Hozza létre újra hello hello új rendelkezésre állási csoportot használó virtuális gépek
   
    ```powershell
    $vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>
   
    Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]
   
    Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 
   
    New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
    ``` 
5. Az adatlemezek és kiterjesztések hozzáadása. További információkért lásd: [adatlemez csatolása tooVM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Resource Manager-sablonok bővítményei](../windows/template-description.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#extensions). Az adatlemezek és bővítmények adhatók toohello VM PowerShell vagy az Azure parancssori felület használatával.

## <a name="example-script"></a>A példaként megadott parancsfájlt
hello következő parancsfájl egy példát biztosít a hello szükséges információk összegyűjtéséhez törlése hello eredeti virtuális gép, és azt, majd újbóli létrehozása egy új rendelkezésre állási csoport.

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

## <a name="next-steps"></a>Következő lépések
Adja hozzá a további tárhely tooyour VM további hozzáadásával [adatlemez](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

