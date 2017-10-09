---
title: "az Azure Managed lemez a biztonsági mentést aaaCopy |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy példányát az Azure Managed lemez toouse vissza felfelé vagy hibaelhárítási lemez ad ki."
documentationcenter: 
author: squillace
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 2/6/2017
ms.author: rasquill
ms.openlocfilehash: 41b91c2d68eb5be9c493a66be5f7d085a70450d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a>Felügyelt pillanatképek használatával Azure felügyelt lemezként tárolt virtuális merevlemez másolatának létrehozása
Pillanatkép készítése a biztonsági mentéshez felügyelt lemez vagy hello pillanatképből kezelt lemez létrehozása, és mellékelje tooa teszt virtuális gép tootroubleshoot. Felügyelt pillanatkép egy virtuális gép kezelt lemez teljes-időpontban másolatát. Azt a virtuális merevlemez csak olvasható másolatát hozza létre, és alapértelmezés szerint tárolja azt, Standard szintű felügyelt lemezes. 

További információk a díjszabásról: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/managed-disks/). <!--Add link tootopic or blog post that explains managed disks. -->

Használjon vagy hello Azure portál vagy hello Azure CLI 2.0 tootake hello kezelt lemez pillanatképet.

## <a name="use-azure-cli-20-tootake-a-snapshot"></a>Azure CLI 2.0 tootake pillanatkép használata

> [!NOTE] 
> hello alábbi példa telepíteni kell hello Azure CLI 2.0 és az Azure-fiókjával bejelentkezik.

hello következő lépések bemutatják, hogyan tooobtain, és hajtsa végre a megfelelő felügyelt OS pillanatképe lemez hello segítségével `az snapshot create` hello parancsot `--source-disk` paraméter. hello alábbi példa feltételezi, hogy van-e a virtuális gépek nevű `myVM` hello a felügyelt operációsrendszer-lemezzel létrehozott `myResourceGroup` erőforráscsoportot.

```azure-cli
# take hello disk id with which toocreate a snapshot
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create -g myResourceGroup --source "$osDiskId" --name osDisk-backup
```

hello kimeneti hasonlóan kell kinéznie:

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Copy",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/disks/osdisk_6NexYgkFQU",
    "storageAccountId": null
  },
  "diskSizeGb": 30,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/osDisk-backup",
  "location": "westus",
  "name": "osDisk-backup",
  "osType": "Linux",
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-06T21:27:10.172980+00:00",
  "type": "Microsoft.Compute/snapshots"
}
```

## <a name="use-azure-portal-tootake-a-snapshot"></a>Az Azure portál tootake pillanatkép használata 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. -Től kezdődően hello bal felső sarokban kattintson **új** keresse meg a **pillanatkép**.
3. Hello pillanatkép paneljén kattintson **létrehozása**.
4. Adjon meg egy **neve** hello pillanatkép.
5. Válasszon ki egy létező [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md#resource-groups) vagy egy új hello típusnév. 
6. Válasszon egy Azure-adatközpontban helyét.  
7. A **forráslemez**, válassza ki a kezelt lemez toosnapshot hello.
8. Jelölje be hello **fiók típusa** toouse toostore hello pillanatkép. Ajánlott **Standard_LRS** kivéve, ha esetleg szükség lenne rá egy nagy teljesítményű lemezen tárolja.
9. Kattintson a **Create** (Létrehozás) gombra.

Ha toouse hello pillanatkép toocreate tervezze meg a felügyelt lemez, és csatlakoztassa a virtuális gép, amelyet a toobe magas hajt végre, használja a hello paramétert `--sku Premium_LRS` a hello `az snapshot create` parancsot. Ez létrehoz hello pillanatkép, prémium felügyelt lemezként tárolja. Prémium szintű felügyelt lemez jobb teljesítményt, mert SSD-meghajtót (SSD), de drágább, mint a Standard lemezek (HDD).


