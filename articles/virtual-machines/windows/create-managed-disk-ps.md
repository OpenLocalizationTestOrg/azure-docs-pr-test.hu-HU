---
title: "Hozzon létre egy felügyelt lemezes az egy VHD-ről az Azure-ban |} Microsoft Docs"
description: "Hozzon létre egy felügyelt lemezes virtuális Merevlemezt, amely jelenleg egy Azure storage-fiók használata a Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
ms.author: cynthn
ms.openlocfilehash: c03ebf73f1090b595149daf2eb3e274b05822f4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a>Felügyelt lemezek tárfiókokban nem felügyelt lemezekből létrehozása

Felügyelt lemezes egy meglévő adatlemez vagy egy operációsrendszer-lemez, amely jelenleg egy Azure storage-fiókok hozhatók létre. Üres lemez, amely használható a virtuális gépek új adatlemezt is létrehozhat. 

## <a name="before-you-begin"></a>Előkészületek
Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e a legújabb verzióját a AzureRM.Compute PowerShell-modult. A következő parancsot a telepítéshez.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a>Az Azure-tárfiók olyan virtuális merevlemezről kezelt lemez létrehozása

A példa azt hozhat létre egy felügyelt lemezes virtuális Merevlemezt egy lemezt, és rendelje hozzá a paraméter **$ 1. lemez** későbbi használat céljából. 

A felügyelt lemezes létrejön a **nyugati-US** helyét, nevű erőforráscsoport **myResourceGroup**. A lemez neve lehet **myDisk** és a rendszer automatikusan létrehozza a VHD-fájl nevű **myDisk.vhd** azt korábban feltöltött nevű tárfiók **mystorageaccount**. A felügyelt lemezes prémium helyileg redundáns tárolás (LRS) létrejön. Az egyetlen StandardLRS és PremiumLRS **- AccountType** lehetőségeit által kezelt lemezeken. 

1.  Egyes paraméterek beállítása

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. Az adatok lemez létrehozása. 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a>Hozzon létre egy üres adatlemez felügyelt lemezes

A példa azt hozzon létre egy üres adatlemez felügyelt lemezként, és rendelje hozzá a paraméter **$dataDisk2** későbbi használat céljából. Egy üres adatlemez inicializált jelentkezik be a virtuális gép és diskmgmt.msc használatával kell vagy [Rendszerfelügyeleti webszolgáltatások és egy parancsfájl segítségével távolról](attach-disk-ps.md#initialize-the-disk), miután a virtuális gép csatlakozik.

Az üres adatlemez létrejön a **nyugati középső Régiójában** helyét, nevű erőforráscsoport **myResourceGroup**. A lemez neve lehet **myEmptyDataDisk**. Az üres lemez prémium helyileg redundáns tárolás (LRS) létrejön. Az egyetlen StandardLRS és PremiumLRS **- AccountType** lehetőségeit által kezelt lemezeken.

Ebben a példában a lemez mérete 128 GB-os, de a méretet, amely megfelel a virtuális gépen futó alkalmazások igényeinek megfelelően kell kiválasztani.

1.  Egyes paraméterek beállítása

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. Az adatok lemez létrehozása.
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a>Következő lépések   
- Ha már van egy virtuális Gépet, akkor [adatlemezzel](attach-disk-portal.md).
