---
title: "az egy VHD-ről az Azure-ban felügyelt lemezes aaaCreate |} Microsoft Docs"
description: "Hozzon létre egy felügyelt lemezes virtuális merevlemez jelenleg egy Azure storage-fiók használatával hello Resource Manager üzembe helyezési modellben."
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
ms.openlocfilehash: 77adaac5419186ff85039fe2c4752f021aa5e448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a>Felügyelt lemezek tárfiókokban nem felügyelt lemezekből létrehozása

Felügyelt lemezes egy meglévő adatlemez vagy egy operációsrendszer-lemez, amely jelenleg egy Azure storage-fiókok hozhatók létre. Üres lemez, amely használható a virtuális gépek új adatlemezt is létrehozhat. 

## <a name="before-you-begin"></a>Előkészületek
Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját. Futtatás hello parancs tooinstall követően.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a>Az Azure-tárfiók olyan virtuális merevlemezről kezelt lemez létrehozása

Hello példában azt hozhat létre egy felügyelt lemezes virtuális Merevlemezt egy lemezt, és rendelje hozzá toohello paraméter **$ 1. lemez** toouse később. 

hello felügyelt lemezes létrejön hello **nyugati-US** helyét, nevű erőforráscsoport **myResourceGroup**. hello lemez neve lehet **myDisk** és a rendszer automatikusan létrehozza a VHD-fájl nevű **myDisk.vhd** azt korábban feltöltött nevű tooa tárfiók **mystorageaccount**. prémium szintű helyileg redundáns tárolás (LRS) hello felügyelt lemezes létrejön. StandardLRS és PremiumLRS is csak hello **- AccountType** lehetőségeit által kezelt lemezeken. 

1.  Egyes paraméterek beállítása

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. Hello adatok lemez létrehozása. 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a>Hozzon létre egy üres adatlemez felügyelt lemezes

Hello példában azt hozzon létre egy üres adatlemez felügyelt lemezként, és rendelje hozzá toohello paraméter **$dataDisk2** toouse később. Egy üres adatlemez kell inicializálni toobe toohello VM-naplózás és diskmgmt.msc használatával vagy [Rendszerfelügyeleti webszolgáltatások és egy parancsfájl segítségével távolról](attach-disk-ps.md#initialize-the-disk), ha fut a virtuális gép csatlakoztatott tooa.

hello üres adatlemez létrejön hello **nyugati középső Régiójában** helyét, nevű erőforráscsoport **myResourceGroup**. hello lemez neve lehet **myEmptyDataDisk**. prémium szintű helyileg redundáns tárolás (LRS) hello üres lemez létrejön. StandardLRS és PremiumLRS is csak hello **- AccountType** lehetőségeit által kezelt lemezeken.

Ebben a példában hello lemez mérete 128 GB-os, de kell kiválasztani, amely megfelel a virtuális gépen futó alkalmazások hello igényeinek méretet.

1.  Egyes paraméterek beállítása

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. Hello adatok lemez létrehozása.
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a>Következő lépések   
- Ha már van egy virtuális Gépet, akkor [adatlemezzel](attach-disk-portal.md).
