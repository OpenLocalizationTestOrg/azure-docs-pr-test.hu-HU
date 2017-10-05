---
title: "Hozzon létre egy Azure felügyelt lemezt biztonsági másolatát mentése |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy Azure felügyelt lemezt biztonsági vagy a lemez problémák elhárításához."
documentationcenter: 
author: cwatson-cat
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 2/9/2017
ms.author: cwatson
ms.openlocfilehash: a7527b12f4f0d2b45713a0c0109d81ff51293fd8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Felügyelt pillanatképek használatával Azure felügyelt lemezként tárolt virtuális merevlemez másolatának létrehozása
Pillanatkép készítése a biztonsági mentéshez kezelt lemez vagy hozhat létre egy felügyelt lemezt a pillanatkép, és csatlakoztassa azt a teszt virtuális gép a hibaelhárítás. Felügyelt pillanatkép egy virtuális gép kezelt lemez teljes-időpontban másolatát. Azt a virtuális merevlemez csak olvasható másolatát hozza létre, és alapértelmezés szerint tárolja azt, Standard szintű felügyelt lemezes. Felügyelt lemezek kapcsolatos további információkért lásd: [Azure felügyelt lemezekhez – áttekintés](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

További információk a díjszabásról: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/managed-disks/). 

## <a name="before-you-begin"></a>Előkészületek
Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e a legújabb verzióját a AzureRM.Compute PowerShell-modult. A következő parancsot a telepítéshez.

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).

## <a name="copy-the-vhd-with-a-snapshot"></a>Pillanatkép virtuális merevlemez másolása
Az Azure portálon vagy a PowerShell segítségével készítsen pillanatképet a kezelt lemezre.

### <a name="use-azure-portal-to-take-a-snapshot"></a>Készítsen pillanatképet az Azure portál használatával 

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Kezdési bal felső sarokban, kattintson a **új** keresse meg a **pillanatkép**.
3. A pillanatkép paneljén kattintson **létrehozása**.
4. Adjon meg egy **neve** a pillanatkép.
5. Válasszon ki egy létező [Erőforráscsoportot](../../azure-resource-manager/resource-group-overview.md#resource-groups), vagy adja meg egy új csoport nevét. 
6. Válasszon egy Azure-adatközpontban helyét.  
7. A **forráslemez**, válassza ki a felügyelt lemezt pillanatkép.
8. Válassza ki a **fiók típusa** használni a pillanatkép tárolására. Ajánlott **Standard_LRS** kivéve, ha esetleg szükség lenne rá egy nagy teljesítményű lemezen tárolja.
9. Kattintson a **Create** (Létrehozás) gombra.

### <a name="use-powershell-to-take-a-snapshot"></a>Pillanatkép készítése a PowerShell használatával
A következő lépések bemutatják a lekérése a VHD lemez másolni, hozzon létre a pillanatkép-konfigurációkat és pillanatképet készít, a lemez a New-AzureRmSnapshot parancsmaggal<!--Add link to cmdlet when available-->. 

1. Egyes paraméterek megadása 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  Cserélje le a paraméterértékeket:
  -  a virtuális gép erőforráscsoporttal "contoso.com".
  -  az a földrajzi hely, ahol azt szeretné, hogy a felügyelt pillanatkép tárolt "southeastasia". <!---How do you look these up? -->
  -  "ContosoMD_datadisk1" nevű, a VHD lemez, amelyeket másolni kíván.
  -  Az új pillanatkép használni kívánt "ContosoMD_datadisk1_snapshot1" néven.

2. A VHD lemez másolandó beolvasása.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. A pillanatkép-konfigurációk létrehozása. 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. A pillanatkép elkészítésére.

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
Ha azt tervezi, a pillanatkép segítségével kezelt lemez létrehozása, és csatlakoztassa a virtuális gépek magas végrehajtása szükséges, használja a paraméterrel `-AccountType Premium_LRS` a New-AzureRmSnapshot paranccsal. A paraméter a pillanatkép hoz, hogy prémium felügyelt lemezként tárolja. Prémium szintű kezelt lemezek drágább szabvány. Ezért mindenképpen valóban szükség van prémium, hogy a paraméter használata előtt.


