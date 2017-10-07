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
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a>A Windows virtuális gép átalakítása nem felügyelt lemezek toomanaged lemezekből

Ha meglévő Windows virtuális gépek (VM), a nem felügyelt lemezeket használó, hello virtuális gépek felügyelt toouse lemezek keresztül hello átválthat [Azure felügyelt lemezek](managed-disks-overview.md) szolgáltatás. Ez a folyamat hello operációsrendszer-lemez és a mellékelt adatok lemezzel alakítja át.

Ez a cikk bemutatja, hogyan tooconvert virtuális gépek Azure PowerShell használatával. Ha tooinstall kell, vagy frissíteni, lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Előkészületek


* Felülvizsgálati [hello áttelepítés tervezése tooManaged lemezek](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Egypéldányos virtuális gépek átalakítása
Ez a szakasz ismerteti, hogyan tooconvert egypéldányos Azure virtuális gépek nem felügyelt kódból felügyelt lemezek, toomanaged lemezek. (Ha a virtuális gépek rendelkezésre állási csoportba, lásd: hello következő szakaszt.) 

1. Hello virtuális gép felszabadítása hello segítségével [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) parancsmag. hello alábbi példa felszabadítja a hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`: 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. Alakítsa át a hello VM toomanaged lemezeket hello segítségével [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) parancsmag. a következő folyamat konvertálja hello előző VM, beleértve hello operációsrendszer-lemez és adatlemezeket hello:

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. Hello VM indítása után hello átalakítás toomanaged lemezek használatával [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm). a következő példa újraindítást hello előző VM hello:

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a>Alakítsa át a virtuális gépek rendelkezésre állási csoportba

Ha hello virtuális gépeket, amelyet a rendelkezésre állási csoportok tooconvert toomanaged lemezek vannak, akkor először kell tooconvert hello rendelkezésre állási készlet tooa felügyelt rendelkezésre állási csoportot.

1. Átalakítás hello rendelkezésre állási csoport hello segítségével [frissítés-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) parancsmag. a következő példa frissítések hello rendelkezésre állási csoport elnevezett hello `myAvailabilitySet` nevű hello erőforráscsoportban `myResourceGroup`:

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Ha hello terület, ahol a rendelkezésre állási csoport található csak 2 felügyelt tartalék tartományok viszont nem felügyelt tartalék tartományok hello száma: 3, ez a parancs megjeleníti hiba hasonló túl "hello megadva tartalék tartományainak száma 3 hello tartomány 1 too2 kell lennie." tooresolve hello hiba, a frissítés hello tartalék tartomány too2 és a frissítés `Sku` túl`Aligned` az alábbiak szerint:

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Felszabadítani, és konvertálja hello hello rendelkezésre állási csoport virtuális gépe. hello következő parancsfájl felszabadítja a virtuális gépek hello segítségével [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) -parancsmag használatával konvertálja [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), és újraindítja a [Start-AzureRmVM ](/powershell/module/azurerm.compute/start-azurermvm):

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


## <a name="troubleshooting"></a>Hibaelhárítás

Ha nem sikerül átalakítás során, vagy az előző konverzió problémák miatt sikertelen állapotban van a virtuális gépek, futtassa a hello `ConvertTo-AzureRmVMManagedDisk` újra a parancsmagot. Egy egyszerű újra általában feloldja hello helyzet.


## <a name="next-steps"></a>Következő lépések

[Standard szintű felügyelt lemez toopremium átalakítása](convert-disk-storage.md)

Tegye meg a virtuális gépek csak olvasható másolatát a használatával [pillanatképek](snapshot-copy-managed-disk.md).

