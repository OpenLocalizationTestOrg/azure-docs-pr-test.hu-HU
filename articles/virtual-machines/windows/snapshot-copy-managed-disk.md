---
title: "egy-egy példányát az Azure kezelt lemez a biztonságimásolat-aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy példányát az Azure Managed lemez toouse vissza felfelé vagy hibaelhárítási lemez ad ki."
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
ms.openlocfilehash: 2f33dbbee624bcd813f3c7c3e3401072d0933714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Felügyelt pillanatképek használatával Azure felügyelt lemezként tárolt virtuális merevlemez másolatának létrehozása
Pillanatképet készít, egy felügyelt lemezt a biztonsági mentéshez vagy hello pillanatképből kezelt lemez létrehozása, és mellékelje tooa teszt virtuális gép tootroubleshoot. Felügyelt pillanatkép egy virtuális gép kezelt lemez teljes-időpontban másolatát. Azt a virtuális merevlemez csak olvasható másolatát hozza létre, és alapértelmezés szerint tárolja azt, Standard szintű felügyelt lemezes. Felügyelt lemezek kapcsolatos további információkért lásd: [Azure felügyelt lemezekhez – áttekintés](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

További információk a díjszabásról: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/managed-disks/). 

## <a name="before-you-begin"></a>Előkészületek
Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját. Futtatás hello parancs tooinstall követően.

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).

## <a name="copy-hello-vhd-with-a-snapshot"></a>Másolja a VHD hello egy pillanatképet
Használja a hello Azure-portálon vagy a PowerShell tootake hello kezelt lemez pillanatképet.

### <a name="use-azure-portal-tootake-a-snapshot"></a>Az Azure portál tootake pillanatkép használata 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a bal felső hello kezdődően **új** keresse meg a **pillanatkép**.
3. Hello pillanatkép paneljén kattintson **létrehozása**.
4. Adjon meg egy **neve** hello pillanatkép.
5. Válasszon ki egy létező [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md#resource-groups) vagy egy új hello típusnév. 
6. Válasszon egy Azure-adatközpontban helyét.  
7. A **forráslemez**, válassza ki a kezelt lemez toosnapshot hello.
8. Jelölje be hello **fiók típusa** toouse toostore hello pillanatkép. Ajánlott **Standard_LRS** kivéve, ha esetleg szükség lenne rá egy nagy teljesítményű lemezen tárolja.
9. Kattintson a **Create** (Létrehozás) gombra.

### <a name="use-powershell-tootake-a-snapshot"></a>PowerShell tootake pillanatkép használata
hello következő lépések bemutatják, hogyan tooget hello VHD lemezre másolt toobe hello pillanatkép-konfigurációk létrehozása és pillanatképet hello lemez hello New-AzureRmSnapshot parancsmaggal<!--Add link toocmdlet when available-->. 

1. Egyes paraméterek megadása 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  Cserélje le a paraméterértékeket hello:
  -  "contoso.com" hello VM erőforrás csoporttal.
  -  "southeastasia" hello földrajzi helyét, a felügyelt pillanatkép tárolása az. <!---How do you look these up? -->
  -  "ContosoMD_datadisk1" hello nevű hello VHD lemez, amelyet az toocopy.
  -  "ContosoMD_datadisk1_snapshot1" hello a nevét, toouse szánt hello új pillanatkép.

2. Hello VHD lemez toobe másolt beolvasása.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. Hozzon létre hello pillanatkép-konfigurációkat. 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. Hello pillanatképet.

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
Ha toouse hello pillanatkép toocreate tervezze meg a felügyelt lemez, és csatlakoztassa a virtuális gép, amelyet a toobe magas hajt végre, használja a hello paramétert `-AccountType Premium_LRS` a New-AzureRmSnapshot hello paranccsal. hello paraméter hello pillanatképet hoz, hogy a prémium felügyelt lemezként tárolja. Prémium szintű kezelt lemezek drágább szabvány. Ezért mindenképpen valóban szükség van prémium, hogy a paraméter használata előtt.


