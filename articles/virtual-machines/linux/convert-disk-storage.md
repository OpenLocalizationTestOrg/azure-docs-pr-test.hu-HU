---
title: "aaaConvert Azure kezelt lemezek tárolási szabványos toopremium, és ez fordítva is igaz |} Microsoft Docs"
description: "Tooconvert Azure kezelésének lemezek tárolási szabványos toopremium, és ez fordítva is igaz, az Azure parancssori felület használatával."
services: virtual-machines-linux
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 261d77202f7fd381085c4e25211a5d0026f43b01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a>Azure átalakítása felügyelt lemezek tárolási szabványos toopremium, és ez fordítva is igaz

Kezelt lemezek két tárolási lehetőségeket kínál: [prémium](../../storage/storage-premium-storage.md) (SSD-alapú) és [szabványos](../../storage/storage-standard-storage.md) (HDD-alapú). Ez lehetővé teszi tooeasily kapcsoló minimális állásidővel, a teljesítmény igények alapján hello két beállításai között. Ez a funkció nem felügyelt lemezek nem érhető el. Azonban úgy is könnyen [toomanaged lemezek konvertálása](convert-unmanaged-to-managed-disks.md) tooeasily kapcsoló két hello-beállítások között.

Ez a cikk bemutatja, hogyan tooconvert kezeli-e lemezek a standard toopremium, és ez fordítva is igaz Azure parancssori felület használatával. Ha tooinstall kell, vagy frissíteni, lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli.md). 

## <a name="before-you-begin"></a>Előkészületek

* hello átalakításhoz hello virtuális gép újraindítása szükséges, ezért ütemezése hello áttelepítését a lemezek már meglévő karbantartási időszak alatt történjen. 
* Használatakor a nem felügyelt lemezek, először [toomanaged lemezek konvertálása](convert-unmanaged-to-managed-disks.md) toouse Ez a cikk tooswitch hello két tárolási lehetőségek között. 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a>Az összes hello Convert által kezelt lemezeken a virtuális gépek a szabványos toopremium, és ez fordítva is igaz

A következő példa hello, megmutatjuk, hogyan tooswitch összes lemezt a virtuális gépek a szabványos toopremium tárolási hello. toouse prémium által kezelt lemezeken, a virtuális Gépet kell használnia egy [Virtuálisgép-méretet](sizes.md) , amely támogatja a prémium szintű storage. Ebben a példában is, amely támogatja a prémium szintű storage tooa mérete vált.

 ```azurecli

#resource group that contains hello virtual machine
rgName='yourResourceGroup'

#Name of hello virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard toopremium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate hello VM before changing hello size of hello VM
az vm deallocate --name $vmName --resource-group $rgName

#Change hello VM size tooa size that supports premium storage 
#Skip this step if converting storage from premium toostandard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update hello sku of all hello data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update hello sku of hello OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a>Alakítsa át a felügyelt lemezes szabványos toopremium, és ez fordítva is igaz

A fejlesztési és tesztelési célú munkaterheléshez érdemes lehet standard és premium lemezek tooreduce toohave keverékével az költsége. Ez csak jobb teljesítményt igénylő hello lemezek toopremium tárolási frissítésével végezheti el. A következő példa hello, megmutatjuk, hogyan tooswitch egyetlen lemezt a virtuális gépek szabványos toopremium tárolóból, és ez fordítva is igaz. toouse prémium által kezelt lemezeken, a virtuális Gépet kell használnia egy [Virtuálisgép-méretet](sizes.md) , amely támogatja a prémium szintű storage. Ebben a példában is, amely támogatja a prémium szintű storage tooa mérete vált.

 ```azurecli

#resource group that contains hello managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard toopremium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get hello parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate hello VM before changing hello size of hello VM
az vm deallocate --ids $vmId 

#Change hello VM size tooa size that supports premium storage 
#Skip this step if converting storage from premium toostandard
az vm resize --ids $vmId --size $size

# Update hello sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="next-steps"></a>Következő lépések

Tegye meg a virtuális gépek csak olvasható másolatát a használatával [pillanatképek](snapshot-copy-managed-disk.md).

