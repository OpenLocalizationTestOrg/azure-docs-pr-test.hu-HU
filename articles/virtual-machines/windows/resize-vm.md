---
title: aaaUse PowerShell tooresize egy Windows Azure-ban |} Microsoft Docs
description: "A Windows hello Resource Manager üzembe helyezési modellel, az Azure Powershell használatával létrehozott virtuális gépek méretét."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: a4a80f3bc99911e4f1a095f0ce63aca00fa50694
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-windows-vm"></a>A Windows virtuális gép átméretezése
Ez a cikk bemutatja, hogyan tooresize egy Windows virtuális gép létrehozása hello Resource Manager üzembe helyezési modellel Azure Powershell használatával.

Miután létrehozta a virtuális gép (VM), méretezhető hello VM felfelé vagy lefelé hello Virtuálisgép-méretet módosításával. Néhány esetben először hello VM kell felszabadítani. Ez akkor fordulhat elő, ha hello új mérete nem érhető el a virtuális gép hello jelenleg üzemeltető hello hardver fürt.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Egy Windows virtuális gép nem a rendelkezésre állási csoportok átméretezése
1. Ahol hello virtuális gép tárolása hello hardver fürtön elérhető hello Virtuálisgép-méretek listázása. 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. Ha hello szükséges méret szerepel, futtassa a következő parancsok tooresize hello VM hello. Ha hello szükséges méret nem szerepel a listában, nyissa meg toostep 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. Ha hello szükséges méret nem szerepel a listában, futtassa a következő parancsok toodeallocate hello VM, méretezze át, és indítsa újra a virtuális gép hello hello.
   
    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [!WARNING]
> Virtuális gép hello felszabadítása dinamikus IP-címek hozzárendelése a virtuális gép toohello kiadását. az operációs rendszer hello és adatlemezek nem érintettek. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Egy Windows virtuális gép rendelkezésre állási csoportba átméretezése
Ha hello új egy rendelkezésre állási csoportot a virtuális gép mérete nem érhető el a hello hardver fürt hello tartalmazó virtuális gép, majd hello rendelkezésre állási csoport virtuális gépeinek kell toobe tooresize hello virtuális gép felszabadítása. Előfordulhat, hogy szükség tooupdate hello méretétől, más virtuális gépek hello rendelkezésre állási csoportban, miután egy virtuális gép át lett méretezve. tooresize a virtuális gépek rendelkezésre állási csoportba, hajtsa végre a lépéseket követve hello.

1. Ahol hello virtuális gép tárolása hello hardver fürtön elérhető hello Virtuálisgép-méretek listázása.
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. Ha hello szükséges méret szerepel, futtassa a következő parancsok tooresize hello VM hello. Ha nem szerepel, folytassa a toostep 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. Hello szükséges méret nem szerepel a listában, folytassa a következő lépéseket toodeallocate hello hello rendelkezésre állási csoport virtuális gépeinek, méretezze át a virtuális gépek és újra kell indítania őket.
4. Állítsa le virtuális gépeinek hello rendelkezésre állási csoportot.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
   } 
   ```
5. Átméretezése, és indítsa újra a hello virtuális gépek hello rendelkezésre állási készlet.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $newSize = "<newVmSize>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
     $vm.HardwareProfile.VmSize = $newSize
     Update-AzureRmVM -ResourceGroupName $rg -VM $vm
     Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
   }
   ```

## <a name="next-steps"></a>Következő lépések
* További méretezhetőséget, a virtuális gép több példányának futtatása, és kiterjesztése. További információkért lásd: [automatikus méretezése a Windows-alapú gépek egy virtuálisgép-méretezési csoportban lévő](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

